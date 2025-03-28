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

gme = yf.Ticker("GME")
gme_data = gme.history(period="max")
gme_data.head()

url = "https://www.macrotrends.net/stocks/charts/GME/gamestop/revenue"
html = requests.get(url).text
soup = BeautifulSoup(html, "html.parser")
tables = soup.find_all("table")

for table in tables:
    if "GameStop Quarterly Revenue" in str(table):
        df_gme = pd.read_html(str(table))[0]

df_gme.columns = ['Date', 'Revenue']
df_gme = df_gme[df_gme['Revenue'] != '']
df_gme.head()

import matplotlib.pyplot as plt

# Convert 'Date' columns to datetime
tesla_data.reset_index(inplace=True)
df['Date'] = pd.to_datetime(df['Date'])

# Merge datasets on nearest date
tesla_merged = pd.merge_asof(tesla_data[['Date', 'Close']], df, on='Date')

# Clean revenue
tesla_merged['Revenue'] = tesla_merged['Revenue'].str.replace('$','').str.replace(',','')
tesla_merged['Revenue'] = pd.to_numeric(tesla_merged['Revenue'])

# Plotting
fig, ax1 = plt.subplots(figsize=(12, 5))

ax1.set_title("Tesla Stock Price and Revenue")
ax1.set_xlabel("Date")
ax1.set_ylabel("Stock Price", color='blue')
ax1.plot(tesla_merged['Date'], tesla_merged['Close'], color='blue')
ax1.tick_params(axis='y', labelcolor='blue')

ax2 = ax1.twinx()
ax2.set_ylabel("Revenue", color='green')
ax2.plot(tesla_merged['Date'], tesla_merged['Revenue'], color='green')
ax2.tick_params(axis='y', labelcolor='green')

plt.show()

