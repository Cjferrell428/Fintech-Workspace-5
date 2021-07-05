Financial Planning with APIs and Simulations¶
In this Challenge, you’ll create two financial analysis tools by using a single Jupyter notebook:

Part 1: A financial planner for emergencies. The members will be able to use this tool to visualize their current savings. The members can then determine if they have enough reserves for an emergency fund.

Part 2: A financial planner for retirement. This tool will forecast the performance of their retirement portfolio in 30 years. To do this, the tool will make an Alpaca API call via the Alpaca SDK to get historical price data for use in Monte Carlo simulations.

You’ll use the information from the Monte Carlo simulation to answer questions about the portfolio in your Jupyter notebook.

# Import the required libraries and dependencies
import os
import requests
import json
import pandas as pd
from dotenv import load_dotenv
import alpaca_trade_api as tradeapi
from MCForecastTools import MCSimulation
​
%matplotlib inline
# Load the environment variables from the .env file
#by calling the load_dotenv function
load_dotenv()
True
Part 1: Create a Financial Planner for Emergencies
Evaluate the Cryptocurrency Wallet by Using the Requests Library
In this section, you’ll determine the current value of a member’s cryptocurrency wallet. You’ll collect the current prices for the Bitcoin and Ethereum cryptocurrencies by using the Python Requests library. For the prototype, you’ll assume that the member holds the 1.2 Bitcoins (BTC) and 5.3 Ethereum coins (ETH). To do all this, complete the following steps:

Create a variable named monthly_income, and set its value to 12000.

Use the Requests library to get the current price (in US dollars) of Bitcoin (BTC) and Ethereum (ETH) by using the API endpoints that the starter code supplies.

Navigate the JSON response object to access the current price of each coin, and store each in a variable.

Hint Note the specific identifier for each cryptocurrency in the API JSON response. The Bitcoin identifier is 1, and the Ethereum identifier is 1027.

Calculate the value, in US dollars, of the current amount of each cryptocurrency and of the entire cryptocurrency wallet.

# The current number of coins for each cryptocurrency asset held in the portfolio.
btc_coins = 1.2
eth_coins = 5.3
Step 1: Create a variable named monthly_income, and set its value to 12000.
monthly_income = 12000
# The monthly amount for the member's household income
monthly_income = 12000
​
Review the endpoint URLs for the API calls to Free Crypto API in order to get the current pricing information for both BTC and ETH.
# The Free Crypto API Call endpoint URLs for the held cryptocurrency assets
btc_url = "https://api.alternative.me/v2/ticker/Bitcoin/?convert=USD"
eth_url = "https://api.alternative.me/v2/ticker/Ethereum/?convert=USD"
Step 2. Use the Requests library to get the current price (in US dollars) of Bitcoin (BTC) and Ethereum (ETH) by using the API endpoints that the starter code supplied.
# Using the Python requests library, make an API call to access the current price of BTC
btc_response = requests.get(btc_url).json()
​
# Use the json.dumps function to review the response data from the API call
# Use the indent and sort_keys parameters to make the response object readable
print(json.dumps(btc_response, indent=4, sort_keys=True))
​
{
    "data": {
        "1": {
            "circulating_supply": 18745343,
            "id": 1,
            "last_updated": 1625091252,
            "max_supply": 21000000,
            "name": "Bitcoin",
            "quotes": {
                "USD": {
                    "market_cap": 653555226032,
                    "percent_change_1h": -0.183285576711663,
                    "percent_change_24h": -3.94439742460896,
                    "percent_change_7d": 6.96104947079976,
                    "percentage_change_1h": -0.183285576711663,
                    "percentage_change_24h": -3.94439742460896,
                    "percentage_change_7d": 6.96104947079976,
                    "price": 34771.0,
                    "volume_24h": 30843464544
                }
            },
            "rank": 1,
            "symbol": "BTC",
            "total_supply": 18745343,
            "website_slug": "bitcoin"
        }
    },
    "metadata": {
        "error": null,
        "num_cryptocurrencies": 1279,
        "timestamp": 1625091252
    }
}
# Using the Python requests library, make an API call to access the current price ETH
eth_response = requests.get(eth_url).json()
​
# Use the json.dumps function to review the response data from the API call
# Use the indent and sort_keys parameters to make the response object readable
print(json.dumps(eth_response, indent=4, sort_keys=True))
​
{
    "data": {
        "1027": {
            "circulating_supply": 116507158,
            "id": 1027,
            "last_updated": 1625091248,
            "max_supply": 0,
            "name": "Ethereum",
            "quotes": {
                "USD": {
                    "market_cap": 263567448019,
                    "percent_change_1h": -0.0502986125003234,
                    "percent_change_24h": 2.15261629813205,
                    "percent_change_7d": 20.3705238216434,
                    "percentage_change_1h": -0.0502986125003234,
                    "percentage_change_24h": 2.15261629813205,
                    "percentage_change_7d": 20.3705238216434,
                    "price": 2257.38,
                    "volume_24h": 28437170129
                }
            },
            "rank": 2,
            "symbol": "ETH",
            "total_supply": 116507158,
            "website_slug": "ethereum"
        }
    },
    "metadata": {
        "error": null,
        "num_cryptocurrencies": 1279,
        "timestamp": 1625091248
    }
}
Step 3: Navigate the JSON response object to access the current price of each coin, and store each in a variable.
# Navigate the BTC response object to access the current price of BTC
btc_price = btc_response['data']['1']['quotes']['USD']['price']
​
# Print the current price of BTC
print(f"The price for BTC is ${btc_price}")
​
The price for BTC is $34771.0
# Navigate the BTC response object to access the current price of ETH
eth_price = eth_response['data']['1027']['quotes']['USD']['price']
​
# Print the current price of ETH
print(f"The price for ETH is ${eth_price}")
​
The price for ETH is $2257.38
Step 4: Calculate the value, in US dollars, of the current amount of each cryptocurrency and of the entire cryptocurrency wallet.
coins
# Compute the current value of the BTC holding 
btc_value = btc_price * btc_coins
​
# Print current value of your holding in BTC
print(f"The current value of holding in BTC is ${btc_value}")
​
The current value of holding in BTC is $41725.2
coins
# Compute the current value of the ETH holding 
eth_value = eth_price * eth_coins
​
# Print current value of your holding in ETH
print(f"The current value of holding in ETH is ${eth_value}")
​
The current value of holding in ETH is $11964.114
# Compute the total value of the cryptocurrency wallet
# Add the value of the BTC holding to the value of the ETH holding
total_crypto_wallet = eth_value + btc_value
​
# Print current cryptocurrency wallet balance
print(f"The current value of cryptocurrency wallet is ${total_crypto_wallet}")
​
The current value of cryptocurrency wallet is $53689.314
Evaluate the Stock and Bond Holdings by Using the Alpaca SDK
In this section, you’ll determine the current value of a member’s stock and bond holdings. You’ll make an API call to Alpaca via the Alpaca SDK to get the current closing prices of the SPDR S&P 500 ETF Trust (ticker: SPY) and of the iShares Core US Aggregate Bond ETF (ticker: AGG). For the prototype, assume that the member holds 110 shares of SPY, which represents the stock portion of their portfolio, and 200 shares of AGG, which represents the bond portion. To do all this, complete the following steps:

In the Starter_Code folder, create an environment file (.env) to store the values of your Alpaca API key and Alpaca secret key.

Set the variables for the Alpaca API and secret keys. Using the Alpaca SDK, create the Alpaca tradeapi.REST object. In this object, include the parameters for the Alpaca API key, the secret key, and the version number.

Set the following parameters for the Alpaca API call:

tickers: Use the tickers for the member’s stock and bond holdings.

timeframe: Use a time frame of one day.

start_date and end_date: Use the same date for these parameters, and format them with the date of the previous weekday (or 2020-08-07). This is because you want the one closing price for the most-recent trading day.

Get the current closing prices for SPY and AGG by using the Alpaca get_barset function. Format the response as a Pandas DataFrame by including the df property at the end of the get_barset function.

Navigating the Alpaca response DataFrame, select the SPY and AGG closing prices, and store them as variables.

Calculate the value, in US dollars, of the current amount of shares in each of the stock and bond portions of the portfolio, and print the results.

Review the total number of shares held in both (SPY) and (AGG).
# Current amount of shares held in both the stock (SPY) and bond (AGG) portion of the portfolio.
spy_shares = 110
agg_shares = 200
​
Step 1: In the Starter_Code folder, create an environment file (.env) to store the values of your Alpaca API key and Alpaca secret key.
Step 2: Set the variables for the Alpaca API and secret keys. Using the Alpaca SDK, create the Alpaca tradeapi.REST object. In this object, include the parameters for the Alpaca API key, the secret key, and the version number.
# Set the variables for the Alpaca API and secret keys
alpaca_api_key = os.getenv("ALPACA_API_KEY")
alpaca_secret_key = os.getenv("ALPACA_SECRET_KEY")
​
# Create the Alpaca tradeapi.REST object
alpaca = tradeapi.REST(alpaca_api_key,alpaca_secret_key, api_version="v2")
​
Step 3: Set the following parameters for the Alpaca API call:
tickers: Use the tickers for the member’s stock and bond holdings.

timeframe: Use a time frame of one day.

start_date and end_date: Use the same date for these parameters, and format them with the date of the previous weekday (or 2020-08-07). This is because you want the one closing price for the most-recent trading day.

30
# Set the tickers for both the bond and stock portion of the portfolio
tickers = ['SPY', 'AGG']
​
# Set timeframe to 1D 
timeframe = "1D"
​
# Format current date as ISO format
# Set both the start and end date at the date of your prior weekday 
# This will give you the closing price of the previous trading day
# Alternatively you can use a start and end date of 2020-08-07
start_date = pd.Timestamp("2021-06-30", tz = 'America/New_York').isoformat()
end_date = pd.Timestamp("2021-06-30", tz = 'America/New_York').isoformat()
​
Step 4: Get the current closing prices for SPY and AGG by using the Alpaca get_barset function. Format the response as a Pandas DataFrame by including the df property at the end of the get_barset function.
portfolio_df
# Use the Alpaca get_barset function to get current closing prices the portfolio
# Be sure to set the `df` property after the function to format the response object as a DataFrame
portfolio_df = alpaca.get_barset(
    tickers,
    timeframe,
    start = start_date,
    end = end_date
).df
​
# Review the first 5 rows of the Alpaca DataFrame
portfolio_df
​
AGG	SPY
open	high	low	close	volume	open	high	low	close	volume
time										
2021-06-30 00:00:00-04:00	115.36	115.45	115.3	115.35	5984381	427.2	428.78	427.18	428.08	46776402
Step 5: Navigating the Alpaca response DataFrame, select the SPY and AGG closing prices, and store them as variables.
# Access the closing price for AGG from the Alpaca DataFrame
# Converting the value to a floating point number
agg_close_price = portfolio_df['AGG']['close']
​
# Print the AGG closing price
print(agg_close_price)
​
time
2021-06-30 00:00:00-04:00    115.35
Name: close, dtype: float64
# Access the closing price for SPY from the Alpaca DataFrame
# Converting the value to a floating point number
spy_close_price = portfolio_df['SPY']['close']
​
# Print the SPY closing price
print(spy_close_price)
​
time
2021-06-30 00:00:00-04:00    428.08
Name: close, dtype: float64
Step 6: Calculate the value, in US dollars, of the current amount of shares in each of the stock and bond portions of the portfolio, and print the results.
# Calculate the current value of the bond portion of the portfolio
agg_value = float(agg_close_price * agg_shares)
​
# Print the current value of the bond portfolio
print(f'The current value of the AGG shares is ${agg_value}')
​
The current value of the AGG shares is $23070.0
# Calculate the current value of the stock portion of the portfolio
spy_value = float(spy_close_price * spy_shares)
​
# Print the current value of the stock portfolio
print(f'The current value of the SPY shares is ${spy_value}')
​
The current value of the SPY shares is $47088.799999999996
# Calculate the total value of the stock and bond portion of the portfolio
total_stocks_bonds = float(agg_value + spy_value)
​
# Print the current balance of the stock and bond portion of the portfolio
print(f'The current balance of the stock and bonds portion is ${total_stocks_bonds}')
​
The current balance of the stock and bonds portion is $70158.79999999999
 
# Calculate the total value of the member's entire savings portfolio
# Add the value of the cryptocurrency walled to the value of the total stocks and bonds
total_portfolio = float(total_crypto_wallet + total_stocks_bonds)
​
# Print current cryptocurrency wallet balance
print(f'The current value of the total portfolio is ${total_portfolio}')
​
The current value of the total portfolio is $123848.11399999999
Evaluate the Emergency Fund
In this section, you’ll use the valuations for the cryptocurrency wallet and for the stock and bond portions of the portfolio to determine if the credit union member has enough savings to build an emergency fund into their financial plan. To do this, complete the following steps:

Create a Python list named savings_data that has two elements. The first element contains the total value of the cryptocurrency wallet. The second element contains the total value of the stock and bond portions of the portfolio.

Use the savings_data list to create a Pandas DataFrame named savings_df, and then display this DataFrame. The function to create the DataFrame should take the following three parameters:

savings_data: Use the list that you just created.

columns: Set this parameter equal to a Python list with a single value called amount.

index: Set this parameter equal to a Python list with the values of crypto and stock/bond.

Use the savings_df DataFrame to plot a pie chart that visualizes the composition of the member’s portfolio. The y-axis of the pie chart uses amount. Be sure to add a title.

Using Python, determine if the current portfolio has enough to create an emergency fund as part of the member’s financial plan. Ideally, an emergency fund should equal to three times the member’s monthly income. To do this, implement the following steps:

Create a variable named emergency_fund_value, and set it equal to three times the value of the member’s monthly_income of $12000. (You set this earlier in Part 1).

Create a series of three if statements to determine if the member’s total portfolio is large enough to fund the emergency portfolio:

If the total portfolio value is greater than the emergency fund value, display a message congratulating the member for having enough money in this fund.

Else if the total portfolio value is equal to the emergency fund value, display a message congratulating the member on reaching this important financial goal.

Else the total portfolio is less than the emergency fund value, so display a message showing how many dollars away the member is from reaching the goal. (Subtract the total portfolio value from the emergency fund value.)

Step 1: Create a Python list named savings_data that has two elements. The first element contains the total value of the cryptocurrency wallet. The second element contains the total value of the stock and bond portions of the portfolio.
savings_data
# Consolidate financial assets data into a Python list
savings_data = [total_crypto_wallet, total_stocks_bonds]
​
# Review the Python list savings_data
savings_data
    
[53689.314, 70158.79999999999]
Step 2: Use the savings_data list to create a Pandas DataFrame named savings_df, and then display this DataFrame. The function to create the DataFrame should take the following three parameters:
savings_data: Use the list that you just created.

columns: Set this parameter equal to a Python list with a single value called amount.

index: Set this parameter equal to a Python list with the values of crypto and stock/bond.

savings_df
# Create a Pandas DataFrame called savings_df 
savings_df = pd.DataFrame(
    savings_data,
    columns=['amount'],
    index= ['crypto', 'stock/bond'])
​
# Display the savings_df DataFrame
savings_df
​
amount
crypto	53689.314
stock/bond	70158.800
Step 3: Use the savings_df DataFrame to plot a pie chart that visualizes the composition of the member’s portfolio. The y-axis of the pie chart uses amount. Be sure to add a title.
# Plot the total value of the member's portfolio (crypto and stock/bond) in a pie chart
savings_df.plot(kind='pie', y='amount', title='Total value of portfolio')
​
<AxesSubplot:title={'center':'Total value of portfolio'}, ylabel='amount'>

Step 4: Using Python, determine if the current portfolio has enough to create an emergency fund as part of the member’s financial plan. Ideally, an emergency fund should equal to three times the member’s monthly income. To do this, implement the following steps:
Step 1. Create a variable named emergency_fund_value, and set it equal to three times the value of the member’s monthly_income of 12000. (You set this earlier in Part 1).

Step 2. Create a series of three if statements to determine if the member’s total portfolio is large enough to fund the emergency portfolio:

If the total portfolio value is greater than the emergency fund value, display a message congratulating the member for having enough money in this fund.

Else if the total portfolio value is equal to the emergency fund value, display a message congratulating the member on reaching this important financial goal.

Else the total portfolio is less than the emergency fund value, so display a message showing how many dollars away the member is from reaching the goal. (Subtract the total portfolio value from the emergency fund value.)

Step 4-1: Create a variable named emergency_fund_value, and set it equal to three times the value of the member’s monthly_income of 12000. (You set this earlier in Part 1).
emergency_fund_value
# Create a variable named emergency_fund_value
emergency_fund_value = monthly_income * 3
print(emergency_fund_value)
36000
Step 4-2: Create a series of three if statements to determine if the member’s total portfolio is large enough to fund the emergency portfolio:
If the total portfolio value is greater than the emergency fund value, display a message congratulating the member for having enough money in this fund.

Else if the total portfolio value is equal to the emergency fund value, display a message congratulating the member on reaching this important financial goal.

Else the total portfolio is less than the emergency fund value, so display a message showing how many dollars away the member is from reaching the goal. (Subtract the total portfolio value from the emergency fund value.)

# Evaluate the possibility of creating an emergency fund with 3 conditions:
if total_portfolio > emergency_fund_value:
    print('Congratulations you have enough money in the fund')
elif total_portfolio == emergency_fund_value:
    print('Congratualtions on reaching this important financial goal')
else:
    print(f'You are ${emergency_fund_value - total_portfolio} from reaching your goal')
​
​
Congratulations you have enough money in the fund
Part 2: Create a Financial Planner for Retirement
Create the Monte Carlo Simulation
In this section, you’ll use the MCForecastTools library to create a Monte Carlo simulation for the member’s savings portfolio. To do this, complete the following steps:

Make an API call via the Alpaca SDK to get 10 years of historical closing prices for a traditional 60/40 portfolio split: 60% stocks (SPY) and 40% bonds (AGG).

Run a Monte Carlo simulation of 500 samples and 30 years for the 60/40 portfolio, and then plot the results.The following image shows the overlay line plot resulting from a simulation with these characteristics. However, because a random number generator is used to run each live Monte Carlo simulation, your image will differ slightly from this exact image:

A screenshot depicts the resulting plot.

Plot the probability distribution of the Monte Carlo simulation. Plot the probability distribution of the Monte Carlo simulation. The following image shows the histogram plot resulting from a simulation with these characteristics. However, because a random number generator is used to run each live Monte Carlo simulation, your image will differ slightly from this exact image:
A screenshot depicts the histogram plot.

Generate the summary statistics for the Monte Carlo simulation.
Step 1: Make an API call via the Alpaca SDK to get 10 years of historical closing prices for a traditional 60/40 portfolio split: 60% stocks (SPY) and 40% bonds (AGG).
2
# Set start and end dates of 10 years back from your current date
# Alternatively, you can use an end date of 2020-08-07 and work 10 years back from that date 
start_date = pd.Timestamp('2011-06-30', tz='America/New_York').isoformat()
end_date = pd.Timestamp('2021-06-30', tz='America/New_York').isoformat()
​
5
# Use the Alpaca get_barset function to make the API call to get the 10 years worth of pricing data
# The tickers and timeframe parameters should have been set in Part 1 of this activity 
# The start and end dates should be updated with the information set above
# Remember to add the df property to the end of the call so the response is returned as a DataFrame
stocks_bonds_df = alpaca.get_barset(
    tickers,
    timeframe,
    start = start_date,
    end = end_date
).df
​
​
# Display both the first and last five rows of the DataFrame
stocks_bonds_df.head(5)
stocks_bonds_df.tail(5)
​
AGG	SPY
open	high	low	close	volume	open	high	low	close	volume
time										
2021-06-24 00:00:00-04:00	115.10	115.17	115.0450	115.07	6672880	424.89	425.5500	424.62	425.09	39863529
2021-06-25 00:00:00-04:00	115.11	115.13	114.7516	114.89	3987588	425.90	427.0943	425.55	426.57	50460394
2021-06-28 00:00:00-04:00	115.04	115.23	115.0400	115.16	5523690	427.17	427.6500	425.89	427.48	43937373
2021-06-29 00:00:00-04:00	115.06	115.25	115.0450	115.25	3469405	427.89	428.5600	427.13	427.68	32097572
2021-06-30 00:00:00-04:00	115.36	115.45	115.3000	115.35	5984381	427.20	428.7800	427.18	428.08	46776402
Step 2: Run a Monte Carlo simulation of 500 samples and 30 years for the 60/40 portfolio, and then plot the results.
_
# Configure the Monte Carlo simulation to forecast 30 years cumulative returns
# The weights should be split 40% to AGG and 60% to SPY.
# Run 500 samples.
MC_thirtyyear_cumulative = MCSimulation(
    portfolio_data = stocks_bonds_df,
    weights = [.60, .40],
    num_simulation = 500,
    num_trading_days = 252 * 30
)
​
# Review the simulation input data
MC_thirtyyear_cumulative.portfolio_data.head()
​
AGG	SPY
open	high	low	close	volume	daily_return	open	high	low	close	volume	daily_return
time												
2021-02-08 00:00:00-05:00	116.75	116.9285	116.7200	116.84	3923885	NaN	389.27	390.54	388.35	390.53	33986650	NaN
2021-02-09 00:00:00-05:00	116.94	116.9700	116.8215	116.86	4050208	0.000171	389.61	390.89	389.17	390.25	30861522	-0.000717
2021-02-10 00:00:00-05:00	116.97	117.0100	116.9200	117.00	3448438	0.001198	392.12	392.28	387.50	390.10	53797977	-0.000384
2021-02-11 00:00:00-05:00	117.03	117.0300	116.8000	116.87	3375374	-0.001111	391.24	391.69	388.10	390.73	38939025	0.001615
2021-02-12 00:00:00-05:00	116.67	116.7400	116.5418	116.58	3209765	-0.002481	389.85	392.90	389.77	392.69	39697380	0.005016
# Run the Monte Carlo simulation to forecast 30 years cumulative returns
MC_thirtyyear_cumulative.calc_cumulative_return()
​
Running Monte Carlo simulation number 0.
Running Monte Carlo simulation number 10.
Running Monte Carlo simulation number 20.
Running Monte Carlo simulation number 30.
Running Monte Carlo simulation number 40.
Running Monte Carlo simulation number 50.
Running Monte Carlo simulation number 60.
Running Monte Carlo simulation number 70.
Running Monte Carlo simulation number 80.
Running Monte Carlo simulation number 90.
Running Monte Carlo simulation number 100.
Running Monte Carlo simulation number 110.
Running Monte Carlo simulation number 120.
Running Monte Carlo simulation number 130.
Running Monte Carlo simulation number 140.
Running Monte Carlo simulation number 150.
Running Monte Carlo simulation number 160.
Running Monte Carlo simulation number 170.
Running Monte Carlo simulation number 180.
Running Monte Carlo simulation number 190.
Running Monte Carlo simulation number 200.
Running Monte Carlo simulation number 210.
Running Monte Carlo simulation number 220.
Running Monte Carlo simulation number 230.
Running Monte Carlo simulation number 240.
Running Monte Carlo simulation number 250.
Running Monte Carlo simulation number 260.
Running Monte Carlo simulation number 270.
Running Monte Carlo simulation number 280.
Running Monte Carlo simulation number 290.
Running Monte Carlo simulation number 300.
Running Monte Carlo simulation number 310.
Running Monte Carlo simulation number 320.
Running Monte Carlo simulation number 330.
Running Monte Carlo simulation number 340.
Running Monte Carlo simulation number 350.
Running Monte Carlo simulation number 360.
Running Monte Carlo simulation number 370.
Running Monte Carlo simulation number 380.
Running Monte Carlo simulation number 390.
Running Monte Carlo simulation number 400.
Running Monte Carlo simulation number 410.
Running Monte Carlo simulation number 420.
Running Monte Carlo simulation number 430.
Running Monte Carlo simulation number 440.
Running Monte Carlo simulation number 450.
Running Monte Carlo simulation number 460.
Running Monte Carlo simulation number 470.
Running Monte Carlo simulation number 480.
Running Monte Carlo simulation number 490.
0	1	2	3	4	5	6	7	8	9	...	490	491	492	493	494	495	496	497	498	499
0	1.000000	1.000000	1.000000	1.000000	1.000000	1.000000	1.000000	1.000000	1.000000	1.000000	...	1.000000	1.000000	1.000000	1.000000	1.000000	1.000000	1.000000	1.000000	1.000000	1.000000
1	0.993338	0.995301	1.004482	0.998676	0.995079	0.996310	1.001512	1.001356	1.002406	0.995615	...	1.001094	1.000803	0.998444	0.994620	1.003214	0.999677	0.996462	1.004827	1.004451	1.004642
2	0.996594	0.994118	1.000319	1.001366	0.993993	0.995845	1.004894	0.999882	1.004626	0.995557	...	1.008842	1.000709	1.003044	0.997922	1.001546	1.004948	0.994449	1.002364	1.007886	1.010352
3	0.992246	0.991565	0.996985	1.001163	0.992353	0.991584	1.001549	0.999086	1.004244	0.999057	...	1.002951	0.997935	1.002877	0.998032	1.003601	1.006715	0.987063	1.006493	1.003570	1.015349
4	0.995429	0.994762	1.000168	0.997852	0.989264	0.997149	0.996662	0.996858	1.005892	1.001397	...	0.998236	1.003077	1.003181	0.999860	0.999539	1.004032	0.981094	1.010744	1.000623	1.014119
...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...
7556	4.378813	7.860112	16.499052	5.981650	11.683602	9.441165	12.845285	11.504308	9.680834	4.910405	...	10.424746	15.683503	10.197896	9.495805	16.341111	10.957505	9.883762	10.671960	8.251169	12.667564
7557	4.383774	7.833851	16.510764	5.952713	11.647320	9.471056	12.789315	11.506174	9.694135	4.895379	...	10.427684	15.721748	10.179413	9.495623	16.292960	10.921240	9.908907	10.696033	8.223193	12.702227
7558	4.382296	7.797540	16.516783	5.974776	11.658474	9.467852	12.881799	11.532903	9.714217	4.895949	...	10.439752	15.662675	10.217783	9.420768	16.327766	10.902041	9.909617	10.665814	8.240732	12.668660
7559	4.394063	7.742328	16.497066	5.985351	11.654926	9.417289	12.860940	11.554007	9.713342	4.911928	...	10.529100	15.650476	10.208280	9.381874	16.241319	10.957114	9.899509	10.594695	8.253900	12.684889
7560	4.400410	7.781620	16.517318	6.006252	11.768502	9.410321	12.804239	11.537017	9.729920	4.900029	...	10.567600	15.564724	10.232006	9.441418	16.181976	10.982510	9.879489	10.606945	8.251055	12.689413
7561 rows × 500 columns

# Visualize the 30-year Monte Carlo simulation by creating an
# overlay line plot
MC_thirty_line_plot = MC_thirtyyear_cumulative.plot_simulation()
​

Step 3: Plot the probability distribution of the Monte Carlo simulation.
# Visualize the probability distribution of the 30-year Monte Carlo simulation 
# by plotting a histogram
MC_sim_distribution_plot = MC_thirtyyear_cumulative.plot_distribution()
​

Step 4: Generate the summary statistics for the Monte Carlo simulation.
# Generate summary statistics from the 30-year Monte Carlo simulation results
# Save the results as a variable
MC_summary_statistics = MC_thirtyyear_cumulative.summarize_cumulative_return()
​
​
# Review the 30-year Monte Carlo summary statistics
print(MC_summary_statistics)
count           500.000000
mean             10.330673
std               3.333905
min               3.664894
25%               8.031124
50%               9.836765
75%              11.956883
max              28.248669
95% CI Lower      5.468270
95% CI Upper     18.142471
Name: 7560, dtype: float64
Analyze the Retirement Portfolio Forecasts
Using the current value of only the stock and bond portion of the member's portfolio and the summary statistics that you generated from the Monte Carlo simulation, answer the following question in your Jupyter notebook:

What are the lower and upper bounds for the expected value of the portfolio with a 95% confidence interval?
total_stocks_bonds
# Print the current balance of the stock and bond portion of the members portfolio
print(total_stocks_bonds)
​
70158.79999999999
$
# Use the lower and upper `95%` confidence intervals to calculate the range of the possible outcomes for the current stock/bond portfolio
ci_lower_thirty_cumulative_return = MC_summary_statistics[8] * total_stocks_bonds
ci_upper_thirty_cumulative_return = MC_summary_statistics[9] * total_stocks_bonds
​
# Print the result of your calculations
print(f'There is a 95% chance that if you invest your total stock and bonds portfolio in the next thirty years your investment will fall between ${ci_lower_thirty_cumulative_return: .2f} and ${ci_upper_thirty_cumulative_return: .2f}')
​
There is a 95% chance that if you invest your total stock and bonds portfolio in the next thirty years your investment will fall between $ 383647.27 and $ 1272854.02
Forecast Cumulative Returns in 10 Years
The CTO of the credit union is impressed with your work on these planning tools but wonders if 30 years is a long time to wait until retirement. So, your next task is to adjust the retirement portfolio and run a new Monte Carlo simulation to find out if the changes will allow members to retire earlier.

For this new Monte Carlo simulation, do the following:

Forecast the cumulative returns for 10 years from now. Because of the shortened investment horizon (30 years to 10 years), the portfolio needs to invest more heavily in the riskier asset—that is, stock—to help accumulate wealth for retirement.

Adjust the weights of the retirement portfolio so that the composition for the Monte Carlo simulation consists of 20% bonds and 80% stocks.

Run the simulation over 500 samples, and use the same data that the API call to Alpaca generated.

Based on the new Monte Carlo simulation, answer the following questions in your Jupyter notebook:

Using the current value of only the stock and bond portion of the member's portfolio and the summary statistics that you generated from the new Monte Carlo simulation, what are the lower and upper bounds for the expected value of the portfolio (with the new weights) with a 95% confidence interval?

Will weighting the portfolio more heavily toward stocks allow the credit union members to retire after only 10 years?

# Configure a Monte Carlo simulation to forecast 10 years cumulative returns
# The weights should be split 20% to AGG and 80% to SPY.
# Run 500 samples.
MC_tenyear = MCSimulation(
    portfolio_data = stocks_bonds_df,
    weights = [.20, .80],
    num_simulation = 500,
    num_trading_days = 252 * 10)
​
# Review the simulation input data
MC_tenyear.portfolio_data.head()
​
AGG	SPY
open	high	low	close	volume	daily_return	open	high	low	close	volume	daily_return
time												
2021-02-08 00:00:00-05:00	116.75	116.9285	116.7200	116.84	3923885	NaN	389.27	390.54	388.35	390.53	33986650	NaN
2021-02-09 00:00:00-05:00	116.94	116.9700	116.8215	116.86	4050208	0.000171	389.61	390.89	389.17	390.25	30861522	-0.000717
2021-02-10 00:00:00-05:00	116.97	117.0100	116.9200	117.00	3448438	0.001198	392.12	392.28	387.50	390.10	53797977	-0.000384
2021-02-11 00:00:00-05:00	117.03	117.0300	116.8000	116.87	3375374	-0.001111	391.24	391.69	388.10	390.73	38939025	0.001615
2021-02-12 00:00:00-05:00	116.67	116.7400	116.5418	116.58	3209765	-0.002481	389.85	392.90	389.77	392.69	39697380	0.005016
# Run the Monte Carlo simulation to forecast 10 years cumulative returns
MC_tenyear.calc_cumulative_return()
​
Running Monte Carlo simulation number 0.
Running Monte Carlo simulation number 10.
Running Monte Carlo simulation number 20.
Running Monte Carlo simulation number 30.
Running Monte Carlo simulation number 40.
Running Monte Carlo simulation number 50.
Running Monte Carlo simulation number 60.
Running Monte Carlo simulation number 70.
Running Monte Carlo simulation number 80.
Running Monte Carlo simulation number 90.
Running Monte Carlo simulation number 100.
Running Monte Carlo simulation number 110.
Running Monte Carlo simulation number 120.
Running Monte Carlo simulation number 130.
Running Monte Carlo simulation number 140.
Running Monte Carlo simulation number 150.
Running Monte Carlo simulation number 160.
Running Monte Carlo simulation number 170.
Running Monte Carlo simulation number 180.
Running Monte Carlo simulation number 190.
Running Monte Carlo simulation number 200.
Running Monte Carlo simulation number 210.
Running Monte Carlo simulation number 220.
Running Monte Carlo simulation number 230.
Running Monte Carlo simulation number 240.
Running Monte Carlo simulation number 250.
Running Monte Carlo simulation number 260.
Running Monte Carlo simulation number 270.
Running Monte Carlo simulation number 280.
Running Monte Carlo simulation number 290.
Running Monte Carlo simulation number 300.
Running Monte Carlo simulation number 310.
Running Monte Carlo simulation number 320.
Running Monte Carlo simulation number 330.
Running Monte Carlo simulation number 340.
Running Monte Carlo simulation number 350.
Running Monte Carlo simulation number 360.
Running Monte Carlo simulation number 370.
Running Monte Carlo simulation number 380.
Running Monte Carlo simulation number 390.
Running Monte Carlo simulation number 400.
Running Monte Carlo simulation number 410.
Running Monte Carlo simulation number 420.
Running Monte Carlo simulation number 430.
Running Monte Carlo simulation number 440.
Running Monte Carlo simulation number 450.
Running Monte Carlo simulation number 460.
Running Monte Carlo simulation number 470.
Running Monte Carlo simulation number 480.
Running Monte Carlo simulation number 490.
0	1	2	3	4	5	6	7	8	9	...	490	491	492	493	494	495	496	497	498	499
0	1.000000	1.000000	1.000000	1.000000	1.000000	1.000000	1.000000	1.000000	1.000000	1.000000	...	1.000000	1.000000	1.000000	1.000000	1.000000	1.000000	1.000000	1.000000	1.000000	1.000000
1	0.991908	1.014580	0.995637	0.993925	1.005070	1.001798	1.012308	1.004155	1.006987	0.989928	...	0.990440	0.999368	1.003677	0.989554	1.005413	0.999113	1.000448	0.997570	0.988996	1.003951
2	0.993293	1.019369	0.994215	1.000654	0.998711	0.996889	0.997447	0.994361	1.013630	0.982234	...	0.984553	0.996196	1.017170	0.992226	1.004665	0.996313	0.995688	0.996226	0.983785	1.005083
3	0.989501	1.016527	0.982498	1.006492	1.001951	1.005960	1.000979	0.995244	1.007299	0.984167	...	0.989364	0.999745	1.013444	0.985771	1.006019	0.994278	1.001776	1.009579	0.982875	1.006603
4	0.989772	1.024061	0.974756	1.008997	0.996309	1.010968	1.005585	0.997830	1.010179	0.980061	...	0.982020	0.996285	1.009310	0.993066	1.001469	1.002261	1.002095	1.007812	0.986298	1.002991
...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...
2516	8.420726	11.611983	10.111095	5.676508	9.045846	4.787703	7.771865	3.714207	11.316326	4.046700	...	7.747175	5.955470	8.009011	8.954508	6.525826	4.308727	6.390663	3.606966	4.903014	8.438423
2517	8.420493	11.700496	10.153777	5.652570	8.935476	4.809465	7.724760	3.725057	11.341465	4.035712	...	7.752342	5.962784	8.067937	9.032318	6.644813	4.279975	6.390710	3.601290	4.931689	8.536809
2518	8.374867	11.670781	10.137370	5.639420	9.024120	4.806768	7.803944	3.706912	11.398489	4.089600	...	7.754330	6.066454	8.055008	9.055496	6.670966	4.288753	6.357184	3.630526	4.912383	8.659770
2519	8.354250	11.684080	10.125499	5.598915	8.966307	4.787661	7.836225	3.720125	11.392623	4.087329	...	7.813919	6.059155	7.983991	9.016677	6.688606	4.297790	6.377630	3.633434	4.972302	8.618798
2520	8.372470	11.577255	10.160723	5.636996	9.086293	4.806341	7.884128	3.732285	11.424093	4.134275	...	7.858838	5.962581	7.944218	9.019606	6.664226	4.340472	6.276894	3.677496	4.963272	8.607764
2521 rows × 500 columns

# Visualize the 10-year Monte Carlo simulation by creating an
# overlay line plot
MC_ten_line_plot = MC_tenyear.plot_simulation()
​

# Visualize the probability distribution of the 10-year Monte Carlo simulation 
# by plotting a histogram
MC_ten_distribution = MC_tenyear.plot_distribution()
​

MC_tenyear_summary
# Generate summary statistics from the 10-year Monte Carlo simulation results
# Save the results as a variable
MC_tenyear_summary = MC_tenyear.summarize_cumulative_return()
​
​
# Review the 10-year Monte Carlo summary statistics
print(MC_tenyear_summary)
count           500.000000
mean              6.505252
std               2.224549
min               2.249153
25%               4.908680
50%               6.138367
75%               7.859995
max              14.593989
95% CI Lower      3.067797
95% CI Upper     11.720403
Name: 2520, dtype: float64
Answer the following questions:
Question: Using the current value of only the stock and bond portion of the member's portfolio and the summary statistics that you generated from the new Monte Carlo simulation, what are the lower and upper bounds for the expected value of the portfolio (with the new weights) with a 95% confidence interval?
total_stocks_bonds
# Print the current balance of the stock and bond portion of the members portfolio
total_stocks_bonds
​
70158.79999999999
# Use the lower and upper `95%` confidence intervals to calculate the range of the possible outcomes for the current stock/bond portfolio
ci_lower_ten_cumulative_return = MC_tenyear_summary[8] * total_stocks_bonds
ci_upper_ten_cumulative_return = MC_tenyear_summary[9] * total_stocks_bonds
​
# Print the result of your calculations
print(f'There is a 95% chance that if you invest your total stock and bonds portfolio in the next thirty years your investment will fall between ${ci_lower_ten_cumulative_return: .2f} and ${ci_upper_ten_cumulative_return: .2f}')
​
There is a 95% chance that if you invest your total stock and bonds portfolio in the next thirty years your investment will fall between $ 215232.95 and $ 822289.44
Question: Will weighting the portfolio more heavily to stocks allow the credit union members to retire after only 10 years?
