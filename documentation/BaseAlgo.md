
BaseAlgo class is the main place to declare your algorithm's logic.  

## `def config(self)`
This method is called before any other methods (except for `__init__`),
and initializes parameters for this class. The attributes you can initialize are:
-  `self.interval`: The interval to run the algorithm. Choose from `1MIN`, `5MIN`, `15MIN`, `30MIN`, `1HR`, `1DAY`
-  `self.aggregations`: A list of intervals. Intervals in this list will be accessible by the algorithm at runtime in addition to the interval specified in `self.interval`
-  `self.watchlist`: List of assets this algorithm tracks. For stocks, use the ticker symbol, e.g. `TWTR`. For crypto, use the coin symbol preceded by an '@', e.g. `@BTC`. 

Any parameters set to None or an empty List will fall back to respective parameters set in the Trader class. 


## `def setup(self)`
Method called right before algorithm begins.
       

## `def main(self)`
Main method to run the algorithm. This method is executed at the interval specified in `self.interval`
   

## `def add_plugin(self, plugin: Plugin)`
Adds a plugin to the algorithm.
     
 
## `def buy( self, symbol: str = None, quantity: int = None, in_force: str = "gtc", extended: bool = False)`
Buys the specified asset.

When called, a limit buy order is placed with a limit price 5% higher than the current price.

- :param str? symbol: Symbol of the asset to buy. defaults to first symbol in watchlist
- :param float? quantity: Quantity of asset to buy. defaults to buys as many as possible
- :param str? in_force: Duration the order is in force. '{gtc}' or '{gtd}'. defaults to 'gtc'
- :param str? extended: Whether to trade in extended hours or not. defaults to False

:returns: The following Python dictionary
- id: str, ID of order
- symbol: str, symbol of asset

:raises Exception: There is an error in the order process.    

## `def sell(self, symbol: str = None, quantity: int = None, in_force: str = "gtc", extended: bool = False)`
Sells the specified asset.

When called, a limit sell order is placed with a limit price 5% lower than the current price.

- :param str? symbol:    Symbol of the asset to sell. defaults to first symbol in watchlist
- :param float? quantity:  Quantity of asset to sell defaults to sells all
- :param str? in_force:  Duration the order is in force. '{gtc}' or '{gtd}'. defaults to 'gtc'
- :param str? extended:  Whether to trade in extended hours or not. defaults to False

:returns: A dictionary with the following keys:
- id: str, ID of order
- symbol: str, symbol of asset

:raises Exception: There is an error in the order process.


## `def sell_all_options(self, symbol: str = None, in_force: str = "gtc")`
Sells all options of a stock

- :param str? symbol: symbol of stock. defaults to first symbol in watchlist

:returns: A list of dictionaries with the following keys:
- id: str, ID of order
- symbol: str, symbol of asset
    
## `def filter_option_chain(self, symbol=None, type=None, lower_exp=None upper_exp=None, lower_strike=None, upper_strike=None)`
    
Automatically buys an option that satisfies the criteria specified.

- :param str? symbol: Symbol of stock. defaults to first symbol in watchlist
- :param str? type: 'call' or 'put'
- :param lower_exp: Minimum expiration date of the option.
- :param upper_exp: Maximum expiration date of the option.
- :param float lower_strike: The minimum strike price of the option
- :param float upper_strike: The maximum strike price of the option


## `def get_option_chain_info(self, symbol: str = None)`
Returns metadata about a stock's option chain

- param str? symbol: symbol of stock. defaults to first symbol in watchlist

:returns: A dict with the following keys:
- exp_dates: List of expiration dates, in the format "YYYY-MM-DD"
- multiplier: Multiplier of the option, usually 100
   

## `def get_option_chain(self, symbol: str, date)`
Returns the option chain for the specified symbol and expiration date.

- :param str symbol: symbol of stock
- :param dt.datetime date: date of option expiration

:returns: A DataFrame with the following columns: 
- exp_date(datetime.datetime): The expiration date
- strike(float): Strike price
- type(str): 'call' or 'put'

The index is the {OCC} symbol of the option.
Note that the expiration date is not adjusted to the local time zone.

## `def get_option_market_data(self, symbol: str)`
Retrieves data of specified option.

- :param str? symbol: {OCC} symbol of option

:returns: A dictionary:
- price: price of option
- ask: ask price
- bid: bid price

## `def rsi(self, symbol: str = None, period: int = 14, interval: Interval = None, ref: str = "close", prices=None) -> np.array`
Calculate RSI

- :param str? symbol:     Symbol to perform calculation on. defaults to first symbol in watchlist
- :param int? period:     Period of RSI. defaults to 14
- :param str? interval:   Interval to perform the calculation. defaults to interval of algorithm
- :param str? ref:        'close', 'open', 'high', or 'low'. defaults to 'close'
- :param list? prices:    When specified, this function will use the values provided in the list to perform calculations and ignore other parameters. defaults to None

:returns: A list in numpy format, containing RSI values


## `def sma(self, symbol: str = None, period: int = 14, interval: Interval = None, ref: str = "close", prices=None) -> np.array`

Calculate SMA

- :param str? symbol:    Symbol to perform calculation on. defaults to first symbol in watchlist
- :param int? period:    Period of SMA. defaults to 14
- :param str? interval:  Interval to perform the calculation. defaults to interval of algorithm
- :param str? ref:       'close', 'open', 'high', or 'low'. defaults to 'close'
- :param list? prices:    When specified, this function will use the values provided in the list to perform calculations and ignore other parameters. defaults to None

:returns: A list in numpy format, containing SMA values
   

## `def ema(self, symbol: str = None, period: int = 14, interval: Interval = None, ref: str = "close", prices=None) -> np.array`

Calculate EMA
- :param str? symbol:    Symbol to perform calculation on. defaults to first symbol in watchlist
- :param int? period:    Period of EMA. defaults to 14
- :param str? interval:  Interval to perform the calculation. defaults to interval of algorithm
- :param str? ref:       'close', 'open', 'high', or 'low'. defaults to 'close'
- :param list? prices:    When specified, this function will use the values provided in the list to perform calculations and ignore other parameters. defaults to None

:returns: A list in numpy format, containing EMA values


## `def bbands(self, symbol: str = None, period: int = 14, interval: Interval = None, ref: str = "close", dev: float = 1.0, prices=None) -> Tuple[np.array, np.array, np.array]`

Calculate Bollinger Bands

- :param str? symbol:    Symbol to perform calculation on. defaults to first symbol in watchlist
- :param int? period:    Period of BBands. defaults to 14
- :param str? interval:  Interval to perform the calculation. defaults to interval of algorithm
- :param str? ref:       'close', 'open', 'high', or 'low'. defaults to 'close'
- :param float? dev:         Standard deviation of the bands. defaults to 1.0
- :param list? prices:    When specified, this function will use the values provided in the list to perform calculations and ignore other parameters. defaults to None
:returns: A tuple of numpy lists, each a list of BBand top, average, and bottom values


## `def crossover(self, prices_0, prices_1)`
Performs {crossover analysis} on two sets of price data

- :param list prices_0:  First set of price data.
- :param list prices_1:  Second set of price data
- :returns: 'True' if prices_0 most recently crossed over prices_1, 'False' otherwise

:raises Exception: If either or both price list has less than 2 values

## `def get_asset_quantity(self, symbol: str = None) -> float`
Returns the quantity owned of a specified asset.

Assets that are currently pending to be sold are not counted.

- :param str? symbol:  Symbol of asset. defaults to first symbol in watchlist
- :returns: Quantity of asset as float. 0 if quantity is not owned.

:raises:


## `def get_asset_cost(self, symbol: str = None) -> float`
Returns the average cost of a specified asset.

- :param str? symbol:  Symbol of asset. defaults to first symbol in watchlist

:returns: Average cost of asset. Returns None if asset is not being tracked.

:raises Exception: If symbol is not currently owned.
   

## `def get_asset_price(self, symbol: str = None) -> float`
Returns the current price of a specified asset.

- :param str? symbol: Symbol of asset. defaults to first symbol in watchlist

:returns:           Price of asset.

:raises Exception:  If symbol is not in the watchlist.
    
## `def get_asset_price_list( self, symbol: str = None, interval: str = None, ref: str = "close" )`
Returns a list of recent prices for an asset.

This function is not compatible with options.

- :param str? symbol:     Symbol of stock or crypto asset. defaults to first symbol in watchlist
- :param str? interval:   Interval of data. defaults to the interval of the algorithm
- :param str? ref:        'close', 'open', 'high', or 'low'. defaults to 'close'

:returns: List of prices
  

## `def get_asset_candle(self, symbol: str, interval=None) -> pd.DataFrame()`

Returns the most recent candle as a pandas DataFrame

This function is not compatible with options.

- :param str? symbol:  Symbol of stock or crypto asset. defaults to first symbol in watchlist

:returns: Price of asset as a dataframe with the following columns:
- open
- high
- low
- close
- volume

The index is a datetime object

:raises Exception: If symbol is not in the watchlist.


## `def get_asset_candle_list(self, symbol: str = None, interval=None) -> pd.DataFrame()`

Returns the candles of an asset as a pandas DataFrame

This function is not compatible with options.

:param str? symbol:  Symbol of stock or crypto asset. defaults to first symbol in watchlist
:returns: Prices of asset as a dataframe with the following columns:
- open
- high
- low
- close
- volume

The index is a datetime object

:raises Exception: If symbol is not in the watchlist.

## `def get_asset_returns(self, symbol=None) -> float`
Returns the return of a specified asset.

:param str? symbol:  Symbol of stock, crypto, or option. Options should be in {OCC} format.
                defaults to first symbol in watchlist
:returns: Return of asset, expressed as a decimal.

## `def get_asset_max_quantity(self, symbol=None)`
Calculates the maximum quantity of an asset that can be bought given the current buying power.

:param str? symbol:  Symbol of stock, crypto, or option. Options should be in {OCC} format.
    defaults to first symbol in watchlist
:returns: Quantity that can be bought.


## `def get_account_buying_power(self) -> float`
Returns the current buying power of the user

:returns: The current buying power as a float.

## `def get_account_equity(self) -> float`
Returns the current equity.

:returns: The current equity as a float.
    

## `def get_account_stock_positions(self) -> List`
Returns the current stock positions.

:returns: A list of dictionaries with the following keys:
- symbol
- quantity
- avg_price
    

## `def get_account_crypto_positions(self) -> List`
Returns the current crypto positions.

:returns: A list of dictionaries with the following keys:
- symbol
- quantity
- avg_price
  

## `def get_account_option_positions(self) -> List`
Returns the current option positions.

:returns: A list of dictionaries with the following keys:
    - symbol
    - quantity
    - avg_price
   

## `def get_watchlist(self) -> List`
Returns the current watchlist.
   

## `def get_stock_watchlist(self) -> List`
Returns the current watchlist.
   

## `def get_crypto_watchlist(self) -> List`
Returns the current watchlist.
   

## `def get_time(self)`
Returns the current hour and minute.

This returns the current time, which is different from the timestamp
on a ticker. For example, if you are running an algorithm every 5 minutes,
at 11:30am you will get a ticker for 11:25am. This function will return
11:30am.

:returns: The current time as a datetime object
  

## `def get_date(self)`
Returns the current date.

:returns: The current date as a datetime object
   

## `def get_datetime(self)`
Returns the current date and time.

This returns the current time, which is different from the timestamp
on a ticker. For example, if you are running an algorithm every 5 minutes,
at 11:30am you will get a ticker for 11:25am. This function will return
11:30am.

:returns: The current date and time as a datetime object
   

## `def get_option_position_quantity(self, symbol: str = None) -> bool`
Returns the number of types of options held for a stock.

:param str symbol:  Symbol of the stock to check
:returns: True if the user has an option for the specified symbol.
   



