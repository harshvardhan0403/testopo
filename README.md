# Install yfinance
!pip install yfinance

# Import and fetch Tesla stock data
import yfinance as yf

# Get Tesla stock data
tesla = yf.Ticker("TSLA")
tesla_data = tesla.history(period="max")

# Show data
tesla_data.head()
