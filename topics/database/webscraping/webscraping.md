# 🕸️ Web Scraping 101

## 📌 What is Web Scraping?

Web scraping is the process of automatically extracting data from websites instead of copying it manually.

A scraper:
1. Sends a request to a website
2. Receives the HTML content
3. Parses the HTML
4. Extracts specific data
5. Stores the data (CSV, JSON, Database)

---

## ⚙️ How the Web Works (Basics)

When you visit a website:
- Your browser sends a request
- The server returns HTML
- The browser renders it visually

Example HTML:
```html
<div class="product">
  <h2>iPhone 15</h2>
  <p>$999</p>
</div>
```

🧰 Tools You Need (Python)

Install:
```
pip install requests beautifulsoup4 lxml
```

Libraries:

```requests``` → fetch webpage

```BeautifulSoup``` → parse HTML

```lxml ```→ faster parser

🧪 Basic Example
```python
import requests
from bs4 import BeautifulSoup

url = "https://example.com"

# Step 1: Request page
response = requests.get(url)
html = response.text

# Step 2: Parse HTML
soup = BeautifulSoup(html, "html.parser")

# Step 3: Extract data
titles = soup.find_all("h2")

for t in titles:
    print(t.text)
```

🔍 Finding Elements (VERY IMPORTANT)

Use browser inspection:

1. Right-click → Inspect

2. Find HTML structure

3. Identify:
    - tag name
    - class name

Common methods:
``` python
soup.find("div", class_="product")
soup.find_all("li")
soup.select(".product-title")
```
🔁 Pagination (Multiple Pages)
```
for page in range(1, 101):
    url = f"https://example.com?page={page}"
```
🔗 Following Links (Deep Scraping)
```
link = item.find("a")["href"]
full_url = "https://example.com" + link
```
Then scrape the new page.

💾 Saving Data
CSV Example:
```python
import csv

with open("data.csv", "w") as f:
    writer = csv.writer(f)
    writer.writerow(["Title", "Price"])
```
⚠️ Common Problems
1. 403 Forbidden

Solution:
```
headers = {
    "User-Agent": "Mozilla/5.0"
}
requests.get(url, headers=headers)
```
2. JavaScript Websites

-  Problem:

    -   Data loads dynamically

    - Solution:

    - Use Selenium or Playwright

3. Website Blocking

- Avoid:

    - Too many requests

    -  No headers

- 🧠 Best Practices

    - Always inspect HTML first

    - Start with 1 page, then scale

    - Print results before saving

    - Keep code modular

⚖️ Ethics & Rules

- Check robots.txt

- Don’t overload servers

- Don’t scrape private data

🚀 Database Course Project Workflow

-   Inspect the website

-   Scrape 1 page

-   Extract data

- Handle pagination

- Visit detail pages

- Store results

- Insert into database

💡 Key Insight

Web scraping =

Crawler + Parser + Data Pipeline

✅ Minimal Setup