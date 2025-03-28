# Install yfinance
!pip install yfinance

# Import and fetch Tesla stock data
import yfinance as yf

# Get Tesla stock data
tesla = yf.Ticker("TSLA")
tesla_data = tesla.history(period="max")

# Show data
tesla_data.head()

# Install required libraries
!pip install requests beautifulsoup4

# Importing libraries
import requests
from bs4 import BeautifulSoup
import pandas as pd

# URL for Tesla revenue data
url = "https://www.macrotrends.net/stocks/charts/TSLA/tesla/revenue"

# Request the page
html = requests.get(url).text

# Parse HTML
soup = BeautifulSoup(html, "html.parser")

# Locate the correct table (Revenue table)
tables = soup.find_all("table")

# Extract revenue data
for table in tables:
    if "Tesla Quarterly Revenue" in str(table):
        df = pd.read_html(str(table))[0]

# Rename columns and display
df.columns = ['Date', 'Revenue']
df = df[df['Revenue'] != '']
df.head()

