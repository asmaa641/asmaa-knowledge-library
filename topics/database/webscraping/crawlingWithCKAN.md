# Data Crawling Using CKAN API (Data.gov)

## 🎯 Purpose
To retrieve dataset metadata from Data.gov using the CKAN API and prepare it for insertion into a relational database.

---

## 🧠 What is CKAN?

CKAN (Comprehensive Knowledge Archive Network) is an open-source data management system used by Data.gov.

It provides an API to access datasets in structured JSON format.

---

## 🔗 API Endpoint

```bash
https://catalog.data.gov/api/3/action/package_search
```
## ⚙️ Making API Requests in Python
Required Library

``
pip install requests
``

Basic Request Example
```
import requests

url = "https://catalog.data.gov/api/3/action/package_search"

params = {
    "rows": 10,
    "start": 0
}

response = requests.get(url, params=params)
data = response.json()

datasets = data["result"]["results"]
```
## 📊 Understanding API Response Structure

Each dataset is a dictionary containing:

| Field | Description |
|-------|------------|
| `title` | Dataset name |
| `notes` | Description |
| `organization` | Publishing organization |
| `tags` | List of tags |
| `resources` | File formats and URLs |

## 🧩 Extracting Data
```
for d in datasets:
    name = d["title"]
    description = d.get("notes", "")

    org = d.get("organization")
    org_name = org["title"] if org else "Unknown"

    tags = [t["name"] for t in d.get("tags", [])]
    formats = [r["format"] for r in d.get("resources", [])]
```
## ⚠️ Handling Missing Data

Use .get() to avoid errors:
```
d.get("notes", "")
d.get("tags", [])
d.get("resources", [])
```
👉 Prevents crashes when fields are missing.

## 🔁 Pagination (Fetching Multiple Pages)

To retrieve many datasets:
```
all_datasets = []

for page in range(0, 100):
    params = {
        "rows": 100,
        "start": page * 100
    }

    response = requests.get(url, params=params)
    data = response.json()

    datasets = data["result"]["results"]
    all_datasets.extend(datasets)
```
## 🎯 Data Mapping to Database
| API Field | Database Table | Column |
|----------|----------------|--------|
| `title` | Dataset | Name |
| `notes` | Dataset | Description |
| `organization.title` | Publishing_Organization | Name |
| `tags.name` | Tag | Tag_Name |
| `resources.format` | File_Resource | Format |
| `resources.url` | File_Resource | URL |

## ⚠️ Common Challenges
- ❌ Missing organization
    - org = d.get("organization")
- ❌ Empty tags/resources
    - d.get("tags", [])
- ❌ API rate limits
    - Avoid too many rapid requests

## 🧠 Key Concepts
- API returns structured JSON data
- No need for HTML parsing (unlike BeautifulSoup)
- Data must be cleaned and mapped before insertion
Pagination is required for large datasets
## 🚀 Why Use CKAN API Instead of Web Scraping?
| CKAN API | Web Scraping |
|----------|--------------|
| Structured JSON | Messy HTML |
| Reliable | Breaks easily |
| Faster to implement | More complex |

## 🧩 Summary

API Request → JSON Response → Extract Fields → Map to Database

## 🚀 Role in My Database Course - Project

This step is used to:

- Retrieve dataset metadata from Data.gov
- Populate:
- Publishing_Organization
- Dataset
- Tag
- File_Resource 

tables

