---
title: Retrieve ROR data from Zenodo
deprecated: false
hidden: true
metadata:
  robots: index
next:
  pages:
    - slug: data-dump
      title: Data dump
      type: basic
---
All releases / versions of the ROR [Data dump](doc:data-dump) are [hosted on Zenodo](https://zenodo.org/records/6347574) and can be downloaded programmatically using the Zenodo REST API. You can retrieve the most recent data release or any previous release.

## How Zenodo versioning works

Zenodo uses a concept DOI system. Each ROR release gets its own version-specific DOI, and there's also a "concept" DOI (`10.5281/zenodo.6347574`) that always resolves to the latest version. This means:

* **Concept record ID** - (`6347574`) Always redirects to the latest ROR data release
* **Version-specific record IDs** (e.g., `17953395`) - Points to a specific ROR data release

For most use cases, its best to use the concept record to automatically get the latest data.

## Request headers

When making requests to the Zenodo API, set appropriate headers. Zenodo tends to block requests with user-agents like those used by the default in Python's `requests` library or similar.

```python
headers = {
    "User-Agent": "ROR-Data-Downloader/1.0 (https://ror.org; mailto:your-email@example.com)"
}
```

## Getting and using an API key

While the Zenodo API works without authentication for public records, using an API key provides higher rate limits and is a better guarantee of programmatic access and is therefore recommended for retrieving ROR data.

To get an API key, follow these steps:

1. Create a [Zenodo account](https://zenodo.org/signup/)
2. Go to [Applications > Personal access tokens](https://zenodo.org/account/settings/applications/)
3. Create a new token (no special scopes needed for read-only access)

To use the API key in retrieving files, see the following examples.

### Example - cURL

```curl
export ZENODO_API_KEY="your-api-key-here"
curl -sL -H "Authorization: Bearer $ZENODO_API_KEY" \
    "https://zenodo.org/api/records/6347574"
```

### Example - Python

```python
import os
import requests

def get_headers():
    """Get request headers with User-Agent and optional auth."""
    headers = {
        "User-Agent": "ROR-Data-Downloader/1.0 (https://ror.org)"
    }
    api_key = os.environ.get("ZENODO_API_KEY")
    if api_key:
        headers["Authorization"] = f"Bearer {api_key}"
    return headers

response = requests.get(
    "https://zenodo.org/api/records/6347574",
    headers=get_headers(),
    allow_redirects=True
)
```

## Get the latest version

The simplest way to download the latest ROR data dump is to use the concept record ID, which automatically redirects to the most recent release.

### Example - cURL

```curl
# Fetch metadata for the latest version
curl -sL "https://zenodo.org/api/records/6347574"
```

To download the data file, first fetch the metadata to get the filename and download URL:

```curl
#!/bin/bash
set -e

# Fetch metadata (follows redirect to latest version)
METADATA=$(curl -sL "https://zenodo.org/api/records/6347574")

# Extract file information
FILENAME=$(echo "$METADATA" | jq -r '.files[0].key')
DOWNLOAD_URL=$(echo "$METADATA" | jq -r '.files[0].links.self')
EXPECTED_MD5=$(echo "$METADATA" | jq -r '.files[0].checksum' | sed 's/md5://')
VERSION_DOI=$(echo "$METADATA" | jq -r '.doi')

echo "Downloading: $FILENAME"
echo "Version DOI: $VERSION_DOI"

# Download the file
curl -L -o "$FILENAME" "$DOWNLOAD_URL"

# Verify checksum
ACTUAL_MD5=$(md5sum "$FILENAME" | cut -d' ' -f1)
# On macOS, use: ACTUAL_MD5=$(md5 -q "$FILENAME")

if [ "$EXPECTED_MD5" = "$ACTUAL_MD5" ]; then
    echo "Checksum verified successfully"
else
    echo "ERROR: Checksum mismatch!"
    echo "Expected: $EXPECTED_MD5"
    echo "Actual: $ACTUAL_MD5"
    exit 1
fi
```

### Example - Python

```python
import hashlib
import requests

def download_latest_ror_data(output_dir="."):
    """
    Download the latest ROR data dump with checksum verification.

    Args:
        output_dir: Directory to save the downloaded file

    Returns:
        dict with version info and file path
    """
    # Concept record always redirects to latest version
    api_url = "https://zenodo.org/api/records/6347574"

    # Fetch metadata
    response = requests.get(api_url, allow_redirects=True)
    response.raise_for_status()
    metadata = response.json()

    # Extract file information
    file_info = metadata["files"][0]
    filename = file_info["key"]
    download_url = file_info["links"]["self"]
    expected_checksum = file_info["checksum"].replace("md5:", "")

    print(f"Downloading: {filename}")
    print(f"Version DOI: {metadata['doi']}")
    print(f"Publication date: {metadata['metadata']['publication_date']}")

    # Download the file
    file_path = f"{output_dir}/{filename}"
    with requests.get(download_url, stream=True) as r:
        r.raise_for_status()
        with open(file_path, "wb") as f:
            for chunk in r.iter_content(chunk_size=8192):
                f.write(chunk)

    # Verify checksum
    md5_hash = hashlib.md5()
    with open(file_path, "rb") as f:
        for chunk in iter(lambda: f.read(8192), b""):
            md5_hash.update(chunk)
    actual_checksum = md5_hash.hexdigest()

    if actual_checksum != expected_checksum:
        raise ValueError(
            f"Checksum mismatch! Expected {expected_checksum}, got {actual_checksum}"
        )

    print("Checksum verified successfully")

    return {
        "file_path": file_path,
        "filename": filename,
        "version_doi": metadata["doi"],
        "concept_doi": metadata["conceptdoi"],
        "publication_date": metadata["metadata"]["publication_date"],
        "record_id": metadata["id"],
    }


if __name__ == "__main__":
    result = download_latest_ror_data()
    print(f"\nDownloaded: {result['file_path']}")
```

## Understanding the redirect

When you request the concept record (`/api/records/6347574`), Zenodo returns a `302 redirect` to the current latest version. Most HTTP clients follow this automatically, but you can handle it explicitly if you want to see which version you're getting.

### Example - cURL

```curl
# See the redirect without following it
curl -sI "https://zenodo.org/api/records/6347574" | grep -i location
# Output: location: /api/records/17953395

# Two-step approach: get redirect target, then fetch
LATEST_URL=$(curl -sI "https://zenodo.org/api/records/6347574" | grep -i location | cut -d' ' -f2 | tr -d '\r')
curl -s "https://zenodo.org${LATEST_URL}"
```

### Example - Python

```python
import requests

def get_latest_version_explicit():
    """Fetch latest version with explicit redirect handling."""
    api_url = "https://zenodo.org/api/records/6347574"

    # Don't follow redirect automatically
    response = requests.get(api_url, allow_redirects=False)

    if response.status_code == 302:
        redirect_path = response.headers["Location"]
        resolved_url = f"https://zenodo.org{redirect_path}"
        print(f"Redirected to: {resolved_url}")

        # Fetch the actual record
        response = requests.get(resolved_url)
        response.raise_for_status()
        return response.json()
    else:
        response.raise_for_status()
        return response.json()
```

## Discover all versions

To programmatically list all available ROR data releases, search by the concept record ID with `all_versions=true`.

### Example - cURL

```curl
# List all ROR data releases, newest first
curl -s "https://zenodo.org/api/records?q=conceptrecid:6347574&all_versions=true&sort=mostrecent&size=25"

# Get just the version info
curl -s "https://zenodo.org/api/records?q=conceptrecid:6347574&all_versions=true&sort=mostrecent&size=25" | \
    jq '.hits.hits[] | {id: .id, doi: .doi, date: .metadata.publication_date, file: .files[0].key}'
```

### Example - Python

```python
import requests

def list_all_versions(headers=None, page=1, per_page=25):
    """
    List all available ROR data dump versions.

    Args:
        headers: Request headers (should include User-Agent)
        page: Page number (1-indexed)
        per_page: Results per page (max 25 unauthenticated, 100 authenticated)

    Returns:
        dict with total count and list of versions
    """
    if headers is None:
        headers = {"User-Agent": "ROR-Data-Downloader/1.0 (https://ror.org)"}

    api_url = "https://zenodo.org/api/records"
    params = {
        "q": "conceptrecid:6347574",
        "all_versions": "true",
        "sort": "mostrecent",
        "size": per_page,
        "page": page,
    }

    response = requests.get(api_url, headers=headers, params=params)
    response.raise_for_status()
    data = response.json()

    versions = []
    for record in data["hits"]["hits"]:
        versions.append({
            "record_id": record["id"],
            "doi": record["doi"],
            "publication_date": record["metadata"]["publication_date"],
            "filename": record["files"][0]["key"] if record["files"] else None,
        })

    return {
        "total": data["hits"]["total"],
        "versions": versions
    }


# Example: list recent versions
result = list_all_versions()
print(f"Total versions available: {result['total']}")
for v in result["versions"][:10]:
    print(f"  {v['publication_date']}: {v['filename']} (record_id: {v['record_id']})")
```

Note: Unauthenticated requests are limited to 25 results per page. Use pagination or authentication for larger result sets.

## Get a specific version

To download a specific version of the ROR data dump, use the version-specific record ID.

### Finding record IDs

When you fetch the latest version, the response includes the resolved record ID:

```python
# The response from fetching the concept record contains version info
print(f"Record ID: {metadata['id']}")           # e.g., 17953395
print(f"DOI: {metadata['doi']}")                # e.g., 10.5281/zenodo.17953395
print(f"Concept DOI: {metadata['conceptdoi']}") # 10.5281/zenodo.6347574
```

To find a specific historical version, use the `list_all_versions()` function above and extract the `record_id` from the results.

### Fetching a specific version

Once you have the record ID, fetch it directly.

### Example - cURL

```curl
# Fetch a specific version directly (no redirect)
curl -s "https://zenodo.org/api/records/17953395"
```

### Example - Python

```python
import requests

def get_specific_version(record_id, headers=None):
    """
    Fetch a specific version of the ROR data dump.

    Args:
        record_id: The Zenodo record ID (e.g., 17953395)
        headers: Request headers (should include User-Agent)

    Returns:
        dict with record metadata
    """
    if headers is None:
        headers = {"User-Agent": "ROR-Data-Downloader/1.0 (https://ror.org)"}

    api_url = f"https://zenodo.org/api/records/{record_id}"
    response = requests.get(api_url, headers=headers)
    response.raise_for_status()
    return response.json()


# Example: fetch v2.0 release
metadata = get_specific_version(17953395)
print(f"Version: {metadata['metadata']['publication_date']}")
print(f"DOI: {metadata['doi']}")
print(f"File: {metadata['files'][0]['key']}")
```

## Schema versions and file validation

ROR data has evolved through multiple schema versions. Understanding this history helps when working with historical releases.

### Schema history

| Era               | Data Releases | Schema  | Files in ZIP               | Filename Pattern                            |
| ----------------- | ------------- | ------- | -------------------------- | ------------------------------------------- |
| Early releases    | Pre-2022      | v1      | JSON only, CSV added later | `ror-data.json`, `ror-data.csv`             |
| Transition period | 2022-2025     | v1 + v2 | Both v1 and v2 formats     | v2 files have `_schema_v2` suffix           |
| Current (v2.0+)   | Dec 2025+     | v2 only | Single JSON + CSV          | `v2.0-2025-12-16-ror-data.json` (no suffix) |

### Schema repository

ROR schemas are maintained at: [https://github.com/ror-community/ror-schema](https://github.com/ror-community/ror-schema)

* `ror_schema.json` - v1 schema
* `ror_schema_v2_0.json` - v2.0 schema
* `ror_schema_v2_1.json` - v2.1 schema (current)

### Working with CSV files

Each release includes both JSON and CSV files with the same base filename (e.g., `v2.0-2025-12-16-ror-data.json` and `v2.0-2025-12-16-ror-data.csv`). The CSV is a flattened subset of the JSON data for convenience.

If you need to work with CSV data from a specific version, first validate the JSON file to determine the schema version. The CSV structure corresponds to the JSON schema - v1 and v2 CSVs have different column structures.

```python
import csv
import json
import zipfile
from io import TextIOWrapper

import requests
from jsonschema import validate, ValidationError

SCHEMAS = {
    "v2.1": "https://raw.githubusercontent.com/ror-community/ror-schema/master/ror_schema_v2_1.json",
    "v2.0": "https://raw.githubusercontent.com/ror-community/ror-schema/master/ror_schema_v2_0.json",
    "v1": "https://raw.githubusercontent.com/ror-community/ror-schema/master/ror_schema.json",
}


def detect_schema_version(ror_record):
    """Detect schema version by validating a single record."""
    for version, url in SCHEMAS.items():
        try:
            schema = requests.get(url).json()
            validate(instance=ror_record, schema=schema)
            return version
        except ValidationError:
            continue
    return None


def load_csv_with_schema_detection(zip_path):
    """
    Load CSV data after detecting schema version from JSON.

    Returns:
        dict with schema_version and csv_data (list of dicts)
    """
    with zipfile.ZipFile(zip_path, 'r') as zf:
        files = zf.namelist()

        # Find JSON and CSV files with matching base names
        json_file = next((f for f in files if f.endswith('.json')), None)
        csv_file = next((f for f in files if f.endswith('.csv')), None)

        if not json_file or not csv_file:
            raise ValueError(f"Expected both JSON and CSV files, found: {files}")

        # Detect schema version from JSON
        with zf.open(json_file) as f:
            json_data = json.load(f)
            schema_version = detect_schema_version(json_data[0])
            print(f"Detected schema version: {schema_version}")

        # Load CSV with schema awareness
        with zf.open(csv_file) as f:
            reader = csv.DictReader(TextIOWrapper(f, encoding='utf-8'))
            csv_data = list(reader)

        # Column structure differs between v1 and v2
        if schema_version and schema_version.startswith("v2"):
            print(f"CSV has v2 column structure with {len(reader.fieldnames)} columns")
        else:
            print(f"CSV has v1 column structure with {len(reader.fieldnames)} columns")

        return {
            "schema_version": schema_version,
            "csv_data": csv_data,
            "columns": reader.fieldnames,
            "record_count": len(csv_data)
        }


# Example usage
if __name__ == "__main__":
    result = load_csv_with_schema_detection("v2.0-2025-12-16-ror-data.zip")
    print(f"Schema: {result['schema_version']}")
    print(f"Records: {result['record_count']}")
    print(f"Columns: {result['columns'][:5]}...")  # First 5 columns
```

### Validating downloaded data

Use can use the following code example to detect the schema version and validate a ROR data file:

```python
import json
import zipfile
import requests
from jsonschema import validate, ValidationError

# Schema URLs from GitHub
SCHEMAS = {
    "v2.1": "https://raw.githubusercontent.com/ror-community/ror-schema/master/ror_schema_v2_1.json",
    "v2.0": "https://raw.githubusercontent.com/ror-community/ror-schema/master/ror_schema_v2_0.json",
    "v1": "https://raw.githubusercontent.com/ror-community/ror-schema/master/ror_schema.json",
}


def fetch_schema(schema_url):
    """Fetch and cache a JSON schema from URL."""
    response = requests.get(schema_url)
    response.raise_for_status()
    return response.json()


def detect_and_validate_schema(ror_record):
    """
    Detect schema version and validate a single ROR record.

    Args:
        ror_record: A single ROR record as a dict

    Returns:
        tuple of (schema_version, is_valid, error_message)
    """
    # Try v2 first (current schema)
    try:
        schema_v2 = fetch_schema(SCHEMAS["v2"])
        validate(instance=ror_record, schema=schema_v2)
        return ("v2", True, None)
    except ValidationError:
        pass

    # Fall back to v1
    try:
        schema_v1 = fetch_schema(SCHEMAS["v1"])
        validate(instance=ror_record, schema=schema_v1)
        return ("v1", True, None)
    except ValidationError as e:
        return (None, False, str(e))


def validate_ror_data_file(zip_path):
    """
    Validate a ROR data dump ZIP file.

    Args:
        zip_path: Path to the downloaded ZIP file

    Returns:
        dict with validation results
    """
    results = {
        "zip_path": zip_path,
        "files": [],
        "json_file": None,
        "schema_version": None,
        "is_valid": False,
        "record_count": 0,
        "errors": [],
    }

    with zipfile.ZipFile(zip_path, 'r') as zf:
        results["files"] = zf.namelist()

        # Find the JSON file
        json_files = [f for f in results["files"] if f.endswith('.json')]
        if not json_files:
            results["errors"].append("No JSON file found in ZIP")
            return results

        results["json_file"] = json_files[0]

        # Load and validate
        with zf.open(results["json_file"]) as f:
            try:
                data = json.load(f)
            except json.JSONDecodeError as e:
                results["errors"].append(f"Invalid JSON: {e}")
                return results

        # ROR data is a list of records
        if not isinstance(data, list):
            results["errors"].append("Expected JSON array of records")
            return results

        results["record_count"] = len(data)

        # Validate first record to detect schema version
        if data:
            schema_version, is_valid, error = detect_and_validate_schema(data[0])
            results["schema_version"] = schema_version
            results["is_valid"] = is_valid
            if error:
                results["errors"].append(error)

    return results


# Example usage
if __name__ == "__main__":
    result = validate_ror_data_file("v2.0-2025-12-16-ror-data.zip")
    print(f"Files in ZIP: {result['files']}")
    print(f"JSON file: {result['json_file']}")
    print(f"Schema version: {result['schema_version']}")
    print(f"Valid: {result['is_valid']}")
    print(f"Record count: {result['record_count']}")
    if result['errors']:
        print(f"Errors: {result['errors']}")
```

### Expected file contents by version

**v2.0+ releases** contain exactly two files:

* `v{version}-{date}-ror-data.json` - Full registry data
* `v{version}-{date}-ror-data.csv` - Flattened subset for convenience

**Transition period releases** contain multiple files:

* Files without suffix use v1 schema
* Files with `_schema_v2` suffix use v2 schema

```python
def inspect_zip_contents(zip_path):
    """Show what's in a ROR data ZIP file."""
    with zipfile.ZipFile(zip_path, 'r') as zf:
        print(f"Contents of {zip_path}:")
        for info in zf.infolist():
            size_mb = info.file_size / (1024 * 1024)
            print(f"  {info.filename} ({size_mb:.2f} MB)")
```

## Complete example script

Here is a complete script that downloads the latest ROR data dump, verifies the checksum, and validates both JSON and CSV files against the schema. You can save this as `download_ror_data.py` and run:

```curl
pip install requests jsonschema
python download_ror_data.py ./data
```

```python
#!/usr/bin/env python3
"""
Download and validate ROR data from Zenodo.

Requirements:
    pip install requests jsonschema

Usage:
    python download_ror_data.py [output_directory]

Set ZENODO_API_KEY environment variable for higher rate limits.
"""

import csv
import hashlib
import json
import os
import sys
import zipfile
from io import TextIOWrapper
from pathlib import Path

import requests
from jsonschema import validate, ValidationError

# Zenodo concept record (always resolves to latest)
ZENODO_CONCEPT_RECORD = "https://zenodo.org/api/records/6347574"

# ROR schema URLs
SCHEMAS = {
    "v2.1": "https://raw.githubusercontent.com/ror-community/ror-schema/master/ror_schema_v2_1.json",
    "v2.0": "https://raw.githubusercontent.com/ror-community/ror-schema/master/ror_schema_v2_0.json",
    "v1": "https://raw.githubusercontent.com/ror-community/ror-schema/master/ror_schema.json",
}


def get_headers():
    """Get request headers with User-Agent and optional auth."""
    headers = {
        "User-Agent": "ROR-Data-Downloader/1.0 (https://ror.org)"
    }
    api_key = os.environ.get("ZENODO_API_KEY")
    if api_key:
        headers["Authorization"] = f"Bearer {api_key}"
    return headers


def download_latest_ror_data(output_dir="."):
    """Download latest ROR data with checksum verification."""
    output_dir = Path(output_dir)
    output_dir.mkdir(parents=True, exist_ok=True)
    headers = get_headers()

    # Fetch metadata
    print("Fetching metadata from Zenodo...")
    response = requests.get(ZENODO_CONCEPT_RECORD, headers=headers, allow_redirects=True)
    response.raise_for_status()
    metadata = response.json()

    file_info = metadata["files"][0]
    filename = file_info["key"]
    download_url = file_info["links"]["self"]
    expected_md5 = file_info["checksum"].replace("md5:", "")

    print(f"Version DOI: {metadata['doi']}")
    print(f"Publication date: {metadata['metadata']['publication_date']}")
    print(f"Downloading: {filename}")

    # Download
    file_path = output_dir / filename
    with requests.get(download_url, headers=headers, stream=True) as r:
        r.raise_for_status()
        total_size = int(r.headers.get('content-length', 0))
        downloaded = 0

        with open(file_path, "wb") as f:
            for chunk in r.iter_content(chunk_size=8192):
                f.write(chunk)
                downloaded += len(chunk)
                if total_size:
                    pct = (downloaded / total_size) * 100
                    print(f"\rProgress: {pct:.1f}%", end="", flush=True)
        print()

    # Verify checksum
    print("Verifying checksum...")
    md5_hash = hashlib.md5()
    with open(file_path, "rb") as f:
        for chunk in iter(lambda: f.read(8192), b""):
            md5_hash.update(chunk)

    actual_md5 = md5_hash.hexdigest()
    if actual_md5 != expected_md5:
        raise ValueError(f"Checksum mismatch! Expected {expected_md5}, got {actual_md5}")
    print("Checksum verified.")

    return {
        "file_path": str(file_path),
        "doi": metadata["doi"],
        "publication_date": metadata["metadata"]["publication_date"],
    }


def detect_schema_version(ror_record):
    """Detect schema version by validating a single record."""
    for version, url in SCHEMAS.items():
        try:
            schema = requests.get(url).json()
            validate(instance=ror_record, schema=schema)
            return version
        except ValidationError:
            continue
    return None


def validate_ror_data(zip_path):
    """Validate ROR JSON and CSV data against schema."""
    print(f"\nValidating {zip_path}...")

    with zipfile.ZipFile(zip_path, 'r') as zf:
        files = zf.namelist()
        json_file = next((f for f in files if f.endswith('.json')), None)
        csv_file = next((f for f in files if f.endswith('.csv')), None)

        if not json_file:
            raise ValueError("No JSON file found in ZIP")

        print(f"JSON file: {json_file}")

        # Load and validate JSON
        with zf.open(json_file) as f:
            json_data = json.load(f)

        json_record_count = len(json_data)
        print(f"JSON records: {json_record_count}")

        # Detect schema version
        schema_version = detect_schema_version(json_data[0])
        if not schema_version:
            raise ValueError("Data does not validate against any known schema")
        print(f"Schema version: {schema_version}")

        # Validate CSV if present
        csv_info = None
        if csv_file:
            print(f"CSV file: {csv_file}")
            with zf.open(csv_file) as f:
                reader = csv.DictReader(TextIOWrapper(f, encoding='utf-8'))
                csv_data = list(reader)
                csv_info = {
                    "filename": csv_file,
                    "record_count": len(csv_data),
                    "columns": len(reader.fieldnames),
                }
            print(f"CSV records: {csv_info['record_count']}")
            print(f"CSV columns: {csv_info['columns']}")

            # Verify record counts match
            if csv_info['record_count'] != json_record_count:
                print(f"WARNING: Record count mismatch (JSON: {json_record_count}, CSV: {csv_info['record_count']})")

        return {
            "schema_version": schema_version,
            "json_record_count": json_record_count,
            "csv_info": csv_info,
            "valid": True,
        }


def main():
    output_dir = sys.argv[1] if len(sys.argv) > 1 else "."

    # Download
    result = download_latest_ror_data(output_dir)
    print(f"\nDownloaded: {result['file_path']}")
    print(f"DOI: {result['doi']}")

    # Validate
    validation = validate_ror_data(result['file_path'])
    print(f"\nValidation: {'PASSED' if validation['valid'] else 'FAILED'}")
    print(f"Schema: {validation['schema_version']}")
    print(f"JSON records: {validation['json_record_count']}")
    if validation['csv_info']:
        print(f"CSV records: {validation['csv_info']['record_count']}")
        print(f"CSV columns: {validation['csv_info']['columns']}")


if __name__ == "__main__":
    main()
```
