# Amazon Price Tracker

A small Python web scraper that watches a product page on Amazon.ae, records the price over time into a CSV file, and is set up to send an email alert when the price drops below a target.

I built it as a hands-on project to practise web scraping, data cleaning and simple automation with a real product page (a data analyst themed t-shirt).

## What it does

1. Requests the product page using a browser style `User-Agent` header.
2. Parses the HTML with BeautifulSoup and extracts the product title, the price and the seller name.
3. Cleans the scraped text (strips whitespace and the currency prefix so the price becomes a plain number).
4. Stamps each record with the date it was collected.
5. Appends the record to `amazonDataset.csv`, so a price history builds up with every run.
6. Wraps everything in a `check_price()` function that can run in a loop, with an email function that fires when the price falls below the target.

## Output

Each run adds one row to the dataset:

| Column | Description |
|---|---|
| `title` | Product title as listed on Amazon |
| `price` | Price in AED, cleaned to a number |
| `daterecorded` | Date the data was collected |
| `seller` | Name of the seller |

A sample of the generated data is included in `amazonDataset.csv`. Note that the dataset is plain CSV, so if your copy is named `.xls` it will still open as a CSV file.

## Tech stack

Python, Requests, BeautifulSoup4, pandas, csv, smtplib, datetime, time, Jupyter Notebook

## Getting started

Clone the repository and install the dependencies:

```bash
git clone https://github.com/<your-username>/<your-repo-name>.git
cd <your-repo-name>
pip install requests beautifulsoup4 pandas jupyter
```

Open the notebook:

```bash
jupyter notebook Amazon_Web_Scraper.ipynb
```

Then run the cells in order:

1. Replace the `url` with the product page you want to track.
2. Run the scraping and cleaning cells to check the output looks right.
3. Run the cell that creates the CSV header **once only**. Running it again overwrites the file.
4. Use the append cell or the `check_price()` function to keep adding new rows.

## Setting your own User-Agent

The scraper sends a `User-Agent` header so that Amazon treats the request like it came from a normal browser. This value is specific to your browser and laptop, so the one in the notebook should be replaced with your own:

```python
headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/152.0.0.0 Safari/537.36 OPR/136.0.0.0 (Edition ms_store_gx)"}
```

To find yours, open https://httpbin.org/get in the same browser you normally use. The page returns a JSON response, and the value under `User-Agent` in the `headers` section is the string to copy into your script.

## Email alerts (optional)

The `send_mail()` function uses Gmail's SMTP server to notify you when the price drops. To use it, add your own sender address and an app password. Do not commit real credentials to GitHub. Store them in environment variables or a separate config file that is listed in `.gitignore`.

## Known limitations and next steps

- The scraped price is stored as text, so it should be converted with `float()` before being compared against the target price.
- `smtplib.sendmail()` needs a sender, a recipient and the message, so the recipient argument needs to be added.
- The loop currently waits one second between requests. A longer delay (for example once an hour or once a day) is more realistic for price tracking and kinder to the website.
- Amazon may change its page structure or block frequent requests, which would break the element IDs used for scraping.
- Possible extensions include plotting the price history with matplotlib, tracking several products at once, and scheduling the script with cron or Task Scheduler.

## Disclaimer

This project is for learning and personal use. Please respect Amazon's terms of service and robots.txt, and avoid sending frequent automated requests.
