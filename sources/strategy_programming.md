# Strategy Programming

## Account
### Description
Account is an object containing information about the account with which the current strategy is working.

The individual properties are:

-   **Account.AccountConnection**
    Name for the broker connection used (the name assigned under the account connection submenu)

-   **Account.AccountType**
    Type of account (live account, simulated account etc.)

-   **Account.Broker**
    Name/definition for the broker

-   **Account.BuyingPower**
    The current account equity in consideration of the leverage provided by the broker (IB leverages your account equity by a factor of 4, meaning that with 10000€ your buying power is equal to 40000€)

-   **Account.CashValue**
    Amount (double)

-   **Account.Currency**
    Currency in which the account is held

-   **Account.ExcessEquity**
    Excess

-   **Account.InitialMargin**
    Initial margin (depends on the broker, double)

-   **Account.InstrumentType**
    Type of trading instrument (type AgenaTrader.Plugins.InstrumentTypes)

-   **Account.IsDemo**
    True, if the account is a demo account

-   **Account.Name**
    Name of the account (should be identical to Account.AccountConnection)

-   **Account.OverNightMargin**
    Overnight margin (depends on the broker, double)

-   **Account.RealizedProfitLoss**
    Realized profits and losses (double)

### Example
```cs
Print("AccountConnection " + Account.AccountConnection);
Print("AccountType " + Account.AccountType);
Print("Broker " + Account.Broker);
Print("BuyingPower " + Account.BuyingPower);
Print("CashValue " + Account.CashValue);
Print("Currency " + Account.Currency);
Print("ExcessEquity " + Account.ExcessEquity);
Print("InitialMargin " + Account.InitialMargin);
Print("InstrumentTypes " + Account.InstrumentTypes);
Print("IsDemo " + Account.IsDemo);
Print("Name " + Account.Name);
Print("OverNightMargin " + Account.OverNightMargin);
Print("RealizedProfitLoss " + Account.RealizedProfitLoss);
```

## BarsSinceEntry()
### Description
The property "BarsSinceEntry" returns the number of bars that have occurred since the last entry into the market.

### Usage
```cs
BarsSinceEntry()
BarsSinceEntry(string strategyName)
```

For multi-bar strategies

```cs
BarsSinceEntry(int barsInProgressIndex, string strategyName, int entriesAgo)
```

### Parameter
|                     |                                                                                                           |
|---------------------|-----------------------------------------------------------------------------------------------------------|
| strategyName          | The strategy name (string) that has been used to clearly label the entry within an entry method.            |
| barsInProgressIndex | For *[Multibar*](#multibar)[*MultiBars*](#multibars) strategies. Index for the data series for which the entry order was executed. See [*BarsInProgress*](#barsinprogress), [*BarsInProgress*](#barsinprogress). |
| entriesAgo          | Number of entries in the past. A zero indicates the number of bars that have formed after the last entry. |

### Example
```cs
Print("The last entry was " + BarsSinceEntry() + " bars ago.");
```

## BarsSinceExit()
### Description
The property "BarsSinceExit" outputs the number of bars that have occurred since the last exit from the market.

### Usage
```cs
BarsSinceExit()
BarsSinceExit(string strategyName)
```

For multi-bar strategies
```cs
BarsSinceExit(int barsInProgressIndex, string strategyName, int exitsAgo)
```

### Parameter
|                     |                                                                                                                           |
|---------------------|---------------------------------------------------------------------------------------------------------------------------|
| strategyName          | The Strategy name (string) that has been used to clearly label the exit within the exit method.    |
| barsInProgressIndex | For *[Multibar*](#multibar)[*MultiBars*](#multibars) strategies. Index of the data series for which the exit order has been executed. See [*BarsInProgress*](#barsinprogress). |
| exitsAgo            | Number of exits that have occurred in the past. A zero indicates the number of bars that have formed after the last exit. |

### Example
```cs
Print("The last exit was " + BarsSinceExit() + " bars ago.");
```
## CancelAllOrders()
### Description
CancelAllOrders deletes all oders (cancel) managed by the strategy.
A cancel request is sent to the broker. Whether an or there is really deleted, can not be guaranteed. It may happen that an order has received a partial execution before it is deleted.
Therefore we recommend that you check the status of the order with [*OnOrderUpdate()*](#onorderupdate).

### Usage
```cs
CancelAllOrders()
```
### Parameter
None

### Example
```cs
protected override void OnBarUpdate()
{
   if (BarsSinceEntry() >= 30)
       CancelAllOrders();
}
```
## CancelOrder()
### Description
Cancel order deletes an order.

A cancel request is sent to the broker. There is no guarantee that the order will actually be deleted there. It may occur that the order receives a partial execution before it is deleted. Therefore we recommend that you check the status of the order with [*OnOrderUpdate()*](#onorderupdate).

### Usage
```cs
CancelOrder(IOrder order)
```

### Parameter
An order object of the type "IOrder"

### Example
```cs
private IOrder entryOrder = null;
private int barNumber = 0;
protected override void OnBarUpdate()
{
    // Place an entry stop at the high of the current bar
    if (entryOrder == null)
    {
        entryOrder = EnterLongStop(High[0], "stop long");
        barNumber = CurrentBar;
    }
    // Delete the order after 3 bars
    if (Position.MarketPosition == PositionType.Flat &&
    CurrentBar > barNumber + 3)
        CancelOrder(entryOrder);
}
```

## ChangeOrder()
### Description
Change order, as the name suggests, changes an order.

### Usage
```cs
ChangeOrder(IOrder iOrder, int quantity, double limitPrice, double stopPrice)
```

### Parameter
|            |                                          |
|------------|------------------------------------------|
| iOrder     | An order object of the type "IOrder"     |
| quantity   | Number of units to be ordered            |
| limitPrice | Limit price. Set this to 0 if not needed |
| stopPrice  | Stop price. Set this to 0 if not needed  |

### Example
```cs
private IOrder stopOrder = null;
protected override void OnBarUpdate()
{
// If the position is profiting by 10 ticks then set the stop to break-even
if (stopOrder != null 
    && Close[0] >= Position.AvgPrice + (10 * TickSize) 
        && stopOrder.StopPrice < Position.AvgPrice)
ChangeOrder(stopOrder, stopOrder.Quantity, stopOrder.LimitPrice, Position.AvgPrice);
}
```
## CreateIfDoneGroup()
### Description
If two orders are linked to one another via a CreateIfDoneGroup, it means that if the one order has been executed, the second linked order is activated.

### Usage
```cs
CreateIfDoneGroup(IEnumerable<IOrder> orders)
```

### Parameter
An order object of type IOrder as a list

### Example
```cs
private IOrder oEnterLong = null;
private IOrder oExitLong = null;


protected override void Initialize()
{
   IsAutomated = false;
}


protected override void OnBarUpdate()
{
   oEnterLong = SubmitOrder(0, OrderAction.Buy, OrderType.Market, DefaultQuantity, 0, 0, "ocoId","strategyName");
   oExitLong = SubmitOrder(0, OrderAction.Sell, OrderType.Stop, DefaultQuantity, 0, Close[0] * 1.1, "ocoId","strategyName");

   CreateIfDoneGroup(new List<IOrder> { oEnterLong, oExitLong });

   oEnterLong.ConfirmOrder();
}
```

## CreateOCOGroup()
### Description
If two orders are linked via a CreateOCOGroup, it means that once the one order has been executed, the second linked order is deleted.

### Usage
```cs
CreateOCOGroup(IEnumerable<IOrder> orders)
```

### Parameter
An order object of type IOrder as a list

### Example
```cs
private IOrder oEnterLong = null;
private IOrder oEnterShort = null;


protected override void Initialize()
{
   IsAutomated = false;
}


protected override void OnBarUpdate()
{
   oEnterLong = SubmitOrder(0, OrderAction.Buy, OrderType.Stop, DefaultQuantity, 0, Close[0] * 1.1, "ocoId","strategyName");
   oEnterShort = SubmitOrder(0, OrderAction.SellShort, OrderType.Stop, DefaultQuantity, 0, Close[0] * -1.1,"ocoId", "strategyName");

   CreateOCOGroup(new List<IOrder> { oEnterLong, oEnterShort });

   oEnterLong.ConfirmOrder();
   oEnterShort.ConfirmOrder();
}
```

## CreateOROGroup()
### Description
If two orders are linked via a CreateOROGroup, it means that once the one order has been executed, the order size of the second order is reduced by the order volume of the first order.
### Usage
```cs
CreateOROGroup(IEnumerable<IOrder> orders)
```

### Parameter
An order object of type IOrder as a list

### Example
```cs
private IOrder oStopLong = null;
private IOrder oLimitLong = null;


protected override void Initialize()
{
   IsAutomated = false;
}


protected override void OnBarUpdate()
{
   oStopLong = SubmitOrder(0, OrderAction.BuyToCover, OrderType.Stop, DefaultQuantity, 0, Close[0] * -1.1,"ocoId", "strategyName");
   oLimitLong = SubmitOrder(0, OrderAction.BuyToCover, OrderType.Limit, (int)(DefaultQuantity * 0.5), Close[0] * 1.1, 0, "ocoId", "strategyName");

   CreateOROGroup(new List<IOrder> { oLimitLong, oStopLong });
}
```


## DataSeriesConfigurable
## DefaultQuantity
### Description
Change order changes an order.

Default quantity defines the amount to be used in a strategy. Default quantity is set within the [*Initialize()*](#initialize) method.

### Usage
```cs
ChangeOrder(IOrder iOrder, int quantity, double limitPrice, double stopPrice)
```

### Parameter
An int value containing the amount (stocks, contracts etc.)

### Example
```cs
protected override void Initialize()
{
DefaultQuantity = 100;
}
```

## EnterLong()
### Description
Enter long creates a long position (buy).

If a signature not containing an amount is used, the amount is set by the [*DefaultQuantity*](#defaultquantity) or taken from the strategy dialog window.

See [*EnterLongLimit()*](#enterlonglimit), [*EnterLongStop()*](#enterlongstop), [*EnterLongStopLimit()*](#enterlongstoplimit).

### Usage
```cs
EnterLong()
EnterLong(string strategyName)
EnterLong(int quantity)
EnterLong(int quantity, string strategyName)

//For multi-bar strategies
EnterLong(int multibarSeriesIndex, int quantity, string strategyName)
```

### Parameter
|                     |                                                                                               |
|---------------------|-----------------------------------------------------------------------------------------------|
| strategyName          | An unambiguous name                                                                           |
| quantity            | The amount of stocks/contracts                                                                |
| multibarSeriesIndex | For [*Multibar*](#multibar), [*MultiBars*](#multibars) strategies.  Index of the data series for which the entry order is to be executed. See [*BarsInProgress*](#barsinprogress).  |

### Return Value
an order object of the type "IOrder"

### Example
```cs

// if the EMA14 crosses the SMA50 from below to above
// the ADX is rising its values
if (CrossAbove(EMA(14), SMA(50), 1) && Rising(ADX(20)))
    EnterLong("SMACrossesEMA");

```

## EnterLongLimit()
### Description
Enter long limit creates a limit order for entering a long position (buy).

If a signature not containing a set amount is used, the amount is set by the [*DefaultQuantity*](#defaultquantity) or taken from the strategy dialog window.

See [*EnterLong()*](#enterlong), [*EnterLongStop()*](#enterlongstop), [*EnterLongStopLimit()*](#enterlongstoplimit).

### Usage
```cs
EnterLongLimit(double limitPrice)
EnterLongLimit(double limitPrice, string strategyName)
EnterLongLimit(int quantity, double limitPrice)
EnterLongLimit(int quantity, double limitPrice, string strategyName)
```

For Multibar-Strategies
```cs
EnterLongLimit(int multibarSeriesIndex, bool liveUntilCancelled, int quantity, double limitPrice, string strategyName)
```

### Parameter
|             |                  |     
|---------------------|-------------|
| strategyName          | An unambiguous name |
| quantity            | Amount of stocks/contracts/etc.  |
| multibarSeriesIndex | For [*Multibar*](#multibar) and [*MultiBars*](#multibars) strategies. Index of the data series for which the entry order is to be executed. See [*BarsInProgress*](#barsinprogress). |
| limitPrice          | A double value for the limit price |
| liveUntilCancelled  | The order will not be deleted at the end of the bar, but will remain active until removed with [*CancelOrder*](#cancelorder) or until it reaches its expiry (see [*TimeInForce*](#timeinforce)). |

### Return Value
An order object of the type "IOrder"

### Example
```cs
// if the EMA14 crosses the SMA50 from below to above
// the ADX is rising its values
if (CrossAbove(EMA(14), SMA(50), 1) && Rising(ADX(20)))
    EnterLongLimit("SMACrossesEMA");
```

## EnterLongStop()
### Description
Enter long stop creates a limit order for entering a long position (buy).

If a signature not containing a set amount is used, the amount is set by the [*DefaultQuantity*](#defaultquantity) or taken from the strategy dialog window.

See [*EnterLong()*](#enterlong), [*EnterLongLimit()*](#enterlonglimit), [*EnterLongStopLimit()*](#enterlongstoplimit).

### Usage
```cs
EnterLongStop(double stopPrice)
EnterLongStop(double stopPrice, string strategyName)
EnterLongStop(int quantity, double stopPrice)
EnterLongStop(int quantity, double stopPrice, string strategyName)
```

For multi-bar strategies
```cs
EnterLongStop(int multibarSeriesIndex, bool liveUntilCancelled, int quantity, double stopPrice, string strategyName)
```

### Parameter
| |   |
|---------------------|-------------------------------------------------------------------------------------------|
| strategyName          | An unambiguous name    |
| quantity            | Amount of stocks or contracts etc.                                                                                                                                                    |
| multibarSeriesIndex | For [*Multibar*](#multibar) and [*MultiBars*](#multibars) strategies Index of the data series for which an entry order is to be executed. See [*BarsInProgress*](#barsinprogress).  |
| stopPrice           | A double value for the stop price                                                                                                                                                     |
| liveUntilCancelled  | The order will not be deleted at the end of the bar, but will remain active until deleted with the [*CancelOrder*](#cancelorder) command or until it reaches its expiry time (see [*TimeInForce*](#timeinforce)). |

### Return Value
An order object of the type "IOrder"

### Example
```cs
private IOrder stopOrder = null;
// Place an entry order at the high of the current bar
if (stopOrder == null)
    stopOrder = EnterLongStop(Low[0], "Stop Long");
```

## EnterLongStopLimit()
### Description
Enter long stop limit creates a buy stop limit order for entering a long position.

If a signature not containing a set amount is used, the amount is set by the [*DefaultQuantity*](#defaultquantity) or taken from the strategy dialog window.

See [*EnterLong()*](#enterlong), [*EnterLongLimit()*](#enterlonglimit), [*EnterLongStop()*](#enterlongstop).

### Usage
```cs
EnterLongStopLimit(double limitPrice, double stopPrice)
EnterLongStopLimit(double limitPrice, double stopPrice, string strategyName)
EnterLongStopLimit(int quantity, double limitPrice, double stopPrice)
EnterLongStopLimit(int quantity, double limitPrice, double stopPrice, string strategyName)
```

For multi-bar strategies
```cs
EnterLongStopLimit(int multibarSeriesIndex, bool liveUntilCancelled, int quantity, double limitPrice, double stopPrice, string strategyName)
```

### Parameter
|    |     |
|--------------|-------------------------|
| strategyName          | An unambiguous name       |
| quantity            | Amount of stocks or contracts to be ordered   |
| multibarSeriesIndex | For [*Multibar*](#multibar), [*MultiBars*](#multibars) strategies. Index of the data series for which the entry order is to be executed.  See [*BarsInProgress*](#barsinprogress).  |
| stopPrice           | A double value for the stop price |
| limitPrice          | A double value for the limit price |
| liveUntilCancelled  | The order will not be deleted at the end of the bar, but will remain active until canceled with the CancelOrder command or until it reaches its expiry (see [*TimeInForce*](#timeinforce)). |

### Return Value
An order object of the type "IOrder"

### Example
```cs
private IOrder stopOrder = null;
// Place an entry stop at the high of the current bar
// if the high is reached, a limit order will be placed 2 ticks above the high
if (stopOrder == null)
myEntryOrder = EnterLongStopLimit(High[0]+ (5*TickSize), High[0], "Stop Long Limit");
```

## EnterShort()
### Description
Enter short creates a market order for entering a short position (naked sell).

If a signature not containing a set amount is used, the amount is set by the [*DefaultQuantity*](#defaultquantity) or taken from the strategy dialog window.

See [*EnterShortLimit()*](#entershortlimit), [*EnterShortStop()*](#entershortstop)(entershortstop), [*EnterShortStopLimit()*](#entershortstoplimit).

### Usage
```cs
EnterShort()
EnterShort(string strategyName)
EnterShort(int quantity)
EnterShort(int quantity, string strategyName)
For multi-bar strategies
EnterShort(int multibarSeriesIndex, int quantity, string strategyName)
```

### Parameter
|                     |                                                                      |
|---------------------|----------------------------------------------------------------------|
| strategyName          | An unambiguous name                                                  |
| quantity            | Amount of stocks/contracts etc.                                      |
| multibarSeriesIndex | For [*Multibar*](#multibar), [*MultiBars*](#multibars) strategies                             
                       Index of the data series for which the entry order is to be executed  
                       See [*BarsInProgress*](#barsinprogress).                                               |

### Return Value
an order object of the type "IOrder"

### Example
```cs
// A short position will be placed if the last entry is 10 bars in the past and two SMAs have crossed each other
if (BarsSinceEntry() > 10 && CrossBelow(SMA(10), SMA(20), 1))
EnterShort("SMA cross entry");
```

## EnterShortLimit()
### Description
Enter short limit creates a limit order for entering a short position (naked short).

If a signature not containing a set amount is used, the amount is set by the [*DefaultQuantity*](#defaultquantity) or taken from the strategy dialog window.

See [*EnterShort()*](#entershort), [*EnterShortStop()*](#entershortstop), [*EnterShortStopLimit()*](#entershortstoplimit).

### Usage
```cs
EnterShortLimit(double limitPrice)
EnterShortLimit(double limitPrice, string strategyName)
EnterShortLimit(int quantity, double limitPrice)
EnterShortLimit(int quantity, double limitPrice, string strategyName)
```

For Multibar-Strategies
```cs
EnterShortLimit(int multibarSeriesIndex, bool liveUntilCancelled, int quantity, double limitPrice, string strategyName)
```

### Parameter
|                     |                                                                                                                                                                              |
|---------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| strategyName          | An unambiguous name                                                                                                                                                          |
| quantity            | Amount to be ordered                                                                                                                                                         |
| multibarSeriesIndex | For [*Multibar*](#multibar), [*MultiBars*](#multibars) strategies.                                                                                                                                    
                       Index of the data series for which the entry order is to be executed.                                                                                                         
                       See [*BarsInProgress*](#barsinprogress).                                                                                                                                                       |
| limitPrice          | A double value for the limit price                                                                                                                                           |
| liveUntilCancelled  | The order will not be deleted at the end of the bar, but will remain active until deleted with the CancelOrder command or until it reaches its expiry (see [*TimeInForce*](#timeinforce)). |

### Return Value
an order object of the type "IOrder"

### Example
```cs
// Enter a short position if the last entry is 10 bars in the past and two SMAs have crossed each other
if (BarsSinceEntry() > 10 && CrossBelow(SMA(10), SMA(20), 1))
EnterShortLimit("SMA cross entry");
```

## EnterShortStop()
### Description
Enter short stop creates a limit order for entering a short position.
If a signature not containing a set amount is used, the amount is set by the [*DefaultQuantity*](#defaultquantity) or taken from the strategy dialog window.
See [*EnterShort()*](#entershort), [*EnterShortLimit()*](#entershortlimit), [*EnterShortStopLimit()*](#entershortstoplimit).

### Usage
```cs
EnterShortStop(double stopPrice)
EnterShortStop(double stopPrice, string strategyName)
EnterShortStop(int quantity, double stopPrice)
EnterShortStop(int quantity, double stopPrice, string strategyName)
```

For multi-bar strategies
```cs
EnterShortStop(int multibarSeriesIndex, bool liveUntilCancelled, int quantity, double stopPrice, string strategyName)
```

### Parameter
|                     |                                                                                                               |
|---------------------|---------------------------------------------------------------------------------------------------------------|
| strategyName          | An unambiguous name                                                                                           |
| quantity            | Amount to be ordered                                                                                          |
| multibarSeriesIndex | For [*Multibar*](#multibar), [*MultiBars*](#multibars) strategies. Index of the data series for which the entry order is to be executed. See [*BarsInProgress*](#barsinprogress). |
| stopPrice           | A double value for the stop price                                                                             |
| liveUntilCancelled  | The order will remain active until canceled using the CancelOrder command or until it reaches its expiry time |

### Return Value
An order object of the type "IOrder"

### Example
```cs
private IOrder myEntryOrder = null;
// Place an entry stop at the low of the current bar
if (myEntryOrder == null)
myEntryOrder = EnterShortStop(Low[0], "stop short");
```

## EnterShortStopLimit()
### Description
Enter short stop limit creates a sell stop limit order for entering a short position.

If a signature not containing a set amount is used, the amount is set by the [*DefaultQuantity*](#defaultquantity) or taken from the strategy dialog window.

See [*EnterShort()*](#entershort), [*EnterShortLimit()*](#entershortlimit), [*EnterShortStop()*](#entershortstop).

### Usage
```cs
EnterShortStopLimit(double limitPrice, double stopPrice)
EnterShortStopLimit(double limitPrice, double stopPrice, string strategyName)
EnterShortStopLimit(int quantity, double limitPrice, double stopPrice)
EnterShortStopLimit(int quantity, double limitPrice, double stopPrice, string strategyName)
```

For multi-bar strategies

```cs
EnterShortStopLimit(int multibarSeriesIndex, bool liveUntilCancelled, int quantity, double limitPrice, double stopPrice, string strategyName)
```

### Parameter
|                     |                                                                                                                                                              |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| strategyName          | An unambiguous name                                                                                                                                          |
| quantity            | Amount to be ordered                                                                                                                                         |
| multibarSeriesIndex | For [*Multibar*](#multibar), [*MultiBars*](#multibars) strategies.                                                                                                                    
                       Index of the data series for which an entry order is to be placed.                                                                                            
                       See [*BarsInProgress*](#barsinprogress).                                                                                                                                       |
| stopPrice           | A double value for the stop price                                                                                                                            |
| limitPrice          | A double value for the limit price                                                                                                                           |
| liveUntilCancelled  | The order will not be deleted at the end of the bar, but will remain active until deleted using the CancelOrder command or until it reaches its expiry time. |

### Return Value
An order object of the type "IOrder"

### Example
```cs
private IOrder myEntryOrder = null;
// Place an entry stop at the low of the current bar; if the low is reached then place a limit order 2 ticks below the low
if (myEntryOrder == null)
myEntryOrder = EnterShortStopLimit(Low[0]-2*TickSize, Low[0], "stop short");
```

## EntriesPerDirection
### Description
Entries per direction defines the maximum number of entries permitted in one direction (long or short).

Whether the name of the entry signal is taken into consideration or not is defined within [*EntryHandling*](#entryhandling).

Entries per direction is defined with the [*Initialize()*](#initialize) method.

### Usage
**EntriesPerDirection**

### Parameter
An int value for the maximum entries permitted in one direction.

### Example
```cs
// Example 1
// If one of the two entry conditions is true and a long position is opened, then the other entry signal will be ignored
protected override void Initialize()
{
EntriesPerDirection = 1;
EntryHandling = EntryHandling.AllEntries;
}

protected override void OnBarUpdate()
{
if (CrossAbove(SMA(10), SMA(20), 1)
EnterLong("SMA Cross Entry");
if (CrossAbove(RSI(14, 3), 30, 1)
EnterLong("RSI Cross Entry");
}

// Example 2
// For each differently named entry signal, a long position will be opened
protected override void Initialize()
{
EntriesPerDirection = 1;
EntryHandling = EntryHandling.UniqueEntries;
}

protected override void OnBarUpdate()
{
if (CrossAbove(SMA(10), SMA(20), 1)
EnterLong("SMA Cross Entry");
if (CrossAbove(RSI(14, 3), 30, 1)
EnterLong("RSI Cross Entry");
}
```

## EntryHandling
### Description
Entry handling decides how the maximum number of entries permitted in one direction is interpreted ([*EntriesPerDirection*](#entriesperdirection)).

Entry handling is defined with the [*Initialize()*](#initialize) method.

**EntryHandling.AllEntries**

AgenaTrader continues to create entry orders until the maximum number of entries permitted (defined in [*EntriesPerDirection*](#entriesperdirection)) per direction (long or short) is reached, regardless of how the entry signals are named.

If entries per direction = 2, then enter long ("SMA crossover") and enter long ("range breakout") combined will reach the maximum number of long entries permitted.

**EntryHandling.UniqueEntries**

AgenaTrader continues to generate entry orders until the maximum number of entries (defined in entries per direction) in one direction (long or short) for the differently named entry signals has been reached.
If entries per direction = 2, then it is possible for two signals for enter long ("SMA crossover") *and* 2 signals for enter long ("range breakout") to be traded.

### Usage
**EntryHandling**

### Example
See [*EntriesPerDirection*](#entriesperdirection).

## ExcludeTradeHistoryInBacktest
## ExitLong()
### Description
Exit long creates a sell market order for closing a long position (sell).

If a signature not containing a set amount is used, the amount is set by [*DefaultQuantity*](#defaultquantity) or taken from the strategy dialog window.

See [*ExitLong()*], [*ExitLongLimit()*], [*ExitLongStop()*], [*ExitLongStopLimit()*].

### Usage
```cs
ExitLong()
ExitLong(int quantity)
ExitLong(string fromEntry signal)
ExitLong(string strategyName, string fromEntry signal)
ExitLong(int quantity, string strategyName, string fromEntry signal)
```

For multi-bar strategies
```cs
ExitLong(int multibarSeriesIndex, int quantity, string strategyName, string fromEntry signal)
```

### Parameter
|                     |                                                                      |
|---------------------|----------------------------------------------------------------------|
| strategyName          | An unambiguous name                                                  |
| quantity            | The quantity to be sold                                              |
| multibarSeriesIndex | For [*Multibar*](#multibar), [*MultiBars*](#multibars) strategies. Index of the data series for which the exit order is to be executed. See [*BarsInProgress*](#barsinprogress).   |
| fromEntry signal    | The name of the attached entry signal                                |

### Return Value
An order object of the type "IOrder"

### Example
```cs
// Enter if two SMAs cross each other
if (CrossAbove(SMA(10), SMA(20), 1))
EnterLong("SMA Cross Entry");

// Close position
if (CrossBelow(SMA(10), SMA(20), 1))
ExitLong();
```

## ExitLongLimit()
### Description
Exit long limit creates a sell limit order for closing a long position (i.e. for selling).

If a signature not containing a set amount is used, the amount is set by the [*DefaultQuantity*](#defaultquantity) or taken from the strategy dialog window.
See [*ExitLong()*], [*ExitLongLimit()*], [*ExitLongStop()*], [*ExitLongStopLimit()*].

### Usage
```cs
ExitLongLimit(double limitPrice)
ExitLongLimit(int quantity, double limitPrice)
ExitLongLimit(double limitPrice, string fromEntry signal)
ExitLongLimit(double limitPrice, string strategyName, string fromEntry signal)
ExitLongLimit(int quantity, double limitPrice, string strategyName, string fromEntry signal)
```

For multi-bar strategies
```cs
ExitLongLimit(int multibarSeriesIndex, bool liveUntilCancelled, int quantity, double limitPrice, string strategyName, string fromEntry signal)
```

### Parameter
|                     |                                                                                                                                                              |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| strategyName          | An unambiguous name                                                                                                                                          |
| fromEntry signal    | The name of the attached entry signal                                                                                                                        |
| quantity            | Order quantity to be sold                                                                                                                                    |
| multibarSeriesIndex | For [*Multibar*](#multibar), [*MultiBars*](#multibars) strategies. Index of the data series for which the exit order is to be executed. See [*BarsInProgress*](#barsinprogress).  |
| limitPrice          | A double value for the limit price                                                                                                                           |
| liveUntilCancelled  | The order will not be deleted at the end of the bar, but will remain active until deleted using the CancelOrder command or until it reaches its expiry time. |

### Return Value
an order object of the type "IOrder"

### Example
```cs
// Enter if two SMAs cross each other
if (CrossAbove(SMA(10), SMA(20), 1))
EnterLong("SMA Cross Entry");

// Close position
if (CrossBelow(SMA(10), SMA(20), 1))
ExitLongLimit(GetCurrentBid());
```

## ExitLongStop()
### Description
Exit long stop creates a sell stop order for closing a long position (short).

If a signature not containing a set amount is used, the amount is set by the [*DefaultQuantity*](#defaultquantity) or taken from the strategy dialog window.
See [*ExitLong()*], [*ExitLongLimit()*], [*ExitLongStop()*], [*ExitLongStopLimit()*].

### Usage
```cs
ExitLongStop(int quantity, double stopPrice)
ExitLongStop(double stopPrice, string fromEntry signal)
ExitLongStop(double stopPrice, string strategyName, string fromEntry signal)
ExitLongStop(int quantity, double stopPrice, string strategyName, string fromEntry signal)
```

For multi-bar strategies
```cs
ExitLongStop(int multibarSeriesIndex, bool liveUntilCancelled, int quantity, double stopPrice, string strategyName, string fromEntry signal)ExitLongStop
```

### Parameter
|                     |                                                                                                                                                              |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| strategyName          | An unambiguous name                                                                                                                                          |
| fromEntry signal    | The name of the associated entry signal                                                                                                                      |
| quantity            | The quantity to be sold                                                                                                                                      |
| multibarSeriesIndex | For [*Multibar*](#multibar), [*MultiBars*](#multibars) strategies. Index of the data series for which the exit order is to be executed. See [*BarsInProgress*](#barsinprogress).  |
| stopPrice           | A double value for the stop price                                                                                                                            |
| liveUntilCancelled  | The order will not be deleted at the end of the bar, but will remain active until deleted using the CancelOrder command or until it reaches its expiry time. |

### Return Value
an order object of the type "IOrder"

### Example
```cs
// Enter if two SMAs cross each other
if (CrossAbove(SMA(10), SMA(20), 1))
EnterLong("SMA Cross Entry");

// Close position
if (CrossBelow(SMA(10), SMA(20), 1))
ExitLongStop(Low[0]);
```

## ExitLongStopLimit()
### Description
Exit long stop limit creates a sell stop limit order for closing a long position (i.e. selling).

If a signature not containing a set amount is used, the amount is set by the [*DefaultQuantity*](#defaultquantity) or taken from the strategy dialog window.

See [*ExitLong()*], [*ExitLongLimit()*], [*ExitLongStop()*], [*ExitLongStopLimit()*].

### Usage
```cs
ExitLongStopLimit(double limitPrice, double stopPrice)
ExitLongStopLimit(int quantity, double limitPrice, double stopPrice)
ExitLongStopLimit(double limitPrice, double stopPrice, string fromEntry signal)
ExitLongStopLimit(double limitPrice, double stopPrice, string strategyName, string fromEntry signal)
ExitLongStopLimit(int quantity, double limitPrice, double stopPrice, string strategyName, string fromEntry signal)
```

For Multibar-Strategies
```cs
ExitLongStopLimit(int multibarSeriesIndex, bool liveUntilCancelled, int quantity, double limitPrice, double stopPrice, string strategyName, string fromEntry signal)
```

### Parameter
|                     |                                                                                                                                                              |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| strategyName          | An unambiguous name                                                                                                                                          |
| fromEntry signal    | The name of the associated entry signal                                                                                                                      |
| quantity            | The quantity to be sold                                                                                                                                      |
| multibarSeriesIndex | For [*Multibar*](#multibar), [*MultiBars*](#multibars) strategies. Index of the data series for which the exit order is to be executed. See [*BarsInProgress*](#barsinprogress). |
| limitPrice          | A double value for the limit price                                                                                                                           |
| stopPrice           | A double value for the stop price                                                                                                                            |
| liveUntilCancelled  | The order will not be deleted at the end of the bar, but will remain active until deleted using the CancelOrder command or until it reaches its expiry time. |

### Return Value
An order object of the type "IOrder"

### Example
```cs
// Enter if two SMAs cross each other
if (CrossAbove(SMA(10), SMA(20), 1))
EnterLong("SMA Cross Entry");

// Close position
if (CrossBelow(SMA(10), SMA(20), 1))
ExitLongStopLimit(Low[0]-10*TickSize, Low[0]);
```

## ExitOnClose
## ExitOnCloseSeconds
## ExitShort()
### Description
Exit short creates a buy-to-cover market order for closing a short position (buy).

If a signature not containing a set amount is used, the amount is set by the [*DefaultQuantity*](#defaultquantity) or taken from the strategy dialog window.
See [*ExitShort()*], [*ExitShortLimit()*], [*ExitShortStop()*], [*ExitShortStopLimit()*].

### Usage
```cs
ExitShort()
ExitShort(int quantity)
ExitShort(string fromEntry signal)
ExitShort(string strategyName, string fromEntry signal)
ExitShort(int quantity, string strategyName, string fromEntry signal)
```

For multi-bar strategies
```cs
ExitShort(int multibarSeriesIndex, int quantity, string strategyName, string fromEntry signal)
```

### Parameter
|                     |                                                                      |
|---------------------|----------------------------------------------------------------------|
| strategyName          | An unambiguous name                                                  |
| Quantity            | Order quantity to be bought                                          |
| multibarSeriesIndex | For [*Multibar*](#multibar), [*MultiBars*](#multibars) strategies. Index of the data series for which the exit order is to be executed. See [*BarsInProgress*](#barsinprogress). |
| fromEntry signal    | The name of the associated entry signal                              |

### Return Value
An order object of the type "IOrder"

### Example
```cs
// Enter if two SMAs cross each other
if (CrossBelow(SMA(10), SMA(20), 1))
EnterShort("SMA cross entry");

// Close position
if (CrossAbove(SMA(10), SMA(20), 1))
ExitShort();
```

## ExitShortLimit()
### Description
Exit short limit creates a buy-to-cover limit order for closing a short position (buy).

If a signature not containing a set amount is used, the amount is set by the [*DefaultQuantity*](#defaultquantity) or taken from the strategy dialog window.

See [*ExitShort()*], [*ExitShortLimit()*], [*ExitShortStop()*], [*ExitShortStopLimit()*].

### Usage
```cs
ExitShortLimit(double limitPrice)
ExitShortLimit(int quantity, double limitPrice)
ExitShortLimit(double limitPrice, string fromEntry signal)
ExitShortLimit(double limitPrice, string strategyName, string fromEntry signal)
ExitShortLimit(int quantity, double limitPrice, string strategyName, string fromEntry signal)
```

For multi-bar strategies
```cs
ExitShortLimit(int multibarSeriesIndex, bool liveUntilCancelled, int quantity, double limitPrice, string strategyName, string fromEntry signal)
```

### Parameter
|                     |                                                                                                                                                              |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| strategyName          | An unambiguous name                                                                                                                                          |
| fromEntry signal    | The name of the associated entry signal                                                                                                                      |
| quantity            | Order quantity to be bought                                                                                                                                  |
| multibarSeriesIndex | For [*Multibar*](#multibar), [*MultiBars*](#multibars) strategies. Index of the data series for which the exit order is to be executed. See [*BarsInProgress*](#barsinprogress).  |
| limitPrice          | A double value for the limit price                                                                                                                           |
| liveUntilCancelled  | The order will not be deleted at the end of the bar, but will remain active until deleted using the CancelOrder command or until it reaches its expiry time. |

### Return Value
An order object of the type "IOrder"

### Example
```cs
// Enter if two SMAs cross
if (CrossBelow(SMA(10), SMA(20), 1))
EnterShort("SMA cross entry");
// Close position
if (CrossAbove(SMA(10), SMA(20), 1))
ExitShortLimit(GetCurrentAsk());
```

## ExitShortStop()
### Description
Exit short stop creates a buy-to-cover stop order for closing a short position.
If a signature not containing a set amount is used, the amount is set by the [*DefaultQuantity*](#defaultquantity) or taken from the strategy dialog window.

See [*ExitShort()*], [*ExitShortLimit()*], [*ExitShortStop()*], [*ExitShortStopLimit()*].

### Usage
```cs
ExitShortStop(int quantity, double stopPrice)
ExitShortStop(double stopPrice, string fromEntry signal)
ExitShortStop(double stopPrice, string strategyName, string fromEntry signal)
ExitShortStop(int quantity, double stopPrice, string strategyName, string fromEntry signal)
```

For multi-bar strategies
```cs
ExitShortStop(int multibarSeriesIndex, bool liveUntilCancelled, int quantity, double stopPrice, string strategyName, string fromEntry signal)ExitLongStop
```

### Parameter
|                     |                                                                                                                                                              |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| strategyName          | An unambiguous name                                                                                                                                          |
| fromEntry signal    | The name of the associated entry signal                                                                                                                      |
| quantity            | Order quantity to be bought                                                                                                                                  |
| multibarSeriesIndex | For [*Multibar*](#multibar), [*MultiBars*](#multibars) strategies. Index of the data series for which the exit order is to be executed. See [*BarsInProgress*](#barsinprogress). |
| stopPrice           | A double value for the stop price                                                                                                                            |
| liveUntilCancelled  | The order will not be deleted at the end of the bar, but will remain active until deleted using the CancelOrder command or until it reaches its expiry time. |

### Return Value
An order object of the type "IOrder"

### Example
```cs
// Enter if two SMAs have crossed
if (CrossBelow(SMA(10), SMA(20), 1))
EnterShort("SMA cross entry");

// Close position
if (CrossAbove (SMA(10), SMA(20), 1))
ExitShortStop(High[0]);
```

## ExitShortStopLimit()
### Description
Exit short stop limit creates a buy-to-cover stop limit order for closing a short position.
If a signature not containing a set amount is used, the amount is set by the [*DefaultQuantity*](#defaultquantity) or taken from the strategy dialog window.

See [*ExitLong()*], [*ExitLongLimit()*], [*ExitLongStop()*], [*ExitLongStopLimit()*].

### Usage
```cs
ExitShortStopLimit(double limitPrice, double stopPrice)
ExitShortStopLimit(int quantity, double limitPrice, double stopPrice)
ExitShortStopLimit(double limitPrice, double stopPrice, string fromEntry signal)
ExitShortStopLimit(double limitPrice, double stopPrice, string strategyName, string fromEntry signal)
ExitShortStopLimit(int quantity, double limitPrice, double stopPrice, string strategyName, string fromEntry signal)
```

For multi-bar strategies
```cs
ExitShortStopLimit(int multibarSeriesIndex, bool liveUntilCancelled, int quantity, double limitPrice, double stopPrice, string strategyName, string fromEntry signal)
```

### Parameter
|                     |                                                                                                                                                              |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| strategyName          | An unambiguous name                                                                                                                                          |
| fromEntry signal    | The name of the associated entry signal                                                                                                                      |
| quantity            | Order quantity to be bought                                                                                                                                  |
| multibarSeriesIndex | For [*Multibar*](#multibar), [*MultiBars*](#multibars) strategies. Index of the data series for which the exit order is to be executed. See [*BarsInProgress*](#barsinprogress). |
| limitPrice          | A double value for the limit price                                                                                                                           |
| stopPrice           | A double value for the stop price                                                                                                                            |
| liveUntilCancelled  | The order will not be deleted at the end of the bar, but will remain active until deleted using the CancelOrder command or until it reaches its expiry time. |

### Return Value
An order object of the type "IOrder"

### Example
```cs
// Enter if two SMAs cross each other
if (CrossBelow(SMA(10), SMA(20), 1))
EnterShort("SMA cross entry");

// Close position
if (CrossAbove(SMA(10), SMA(20), 1))
ExitShortStopLimit(High[0]+10*TickSize, High[0]);
```

## GetAccountValue()
### Description
Get account value outputs information regarding the account for which the current strategy is being carried out.

See [*GetProfitLoss()*].

### Usage
```cs
GetAccountValue(AccountItem accountItem)
```

### Parameter
Possible values for account item are:

AccountItem.BuyingPower

AccountItem.CashValue

AccountItem.RealizedProfitLoss

### Return Value
A double value for the account item for historical bars, a zero (0) is returned

### Example
```cs
Print("The current account cash value is " + GetAccountValue(AccountItem.CashValue));
Print("The current account cash value with the leverage provided by the broker is " + GetAccountValue(AccountItem.BuyingPower));
Print("The current P/L already realized is " + GetAccountValue(AccountItem.RealizedProfitLoss));
```

## GetProfitLoss()
### Description
Get profit loss outputs the currently unrealized profit or loss for a running position.

See [*GetAccountValue()*].

### Usage
```cs
GetProfitLoss(int pLType);
```

### Parameter
Potential values for the P/L type are:

0 – Amount: P/L as a currency amount

1 – Percent: P/L in percent

2 – Risk: P/L in Van Tharp R-multiples ([*http://www.vantharp.com/tharp-concepts/risk-and-r-multiples.asp*])

3 – P/L in ticks

### Return Value
A double value for the unrealized profit or loss

### Example
```cs
Print("The current risk for the strategy " + this.Name + " is " + GetProfitLoss(1) + " " + Instrument.Currency);
Print("This equals "+ string.Format( "{0:F1} R.", GetProfitLoss(3)));
```
## GetScriptedCondition()
### Description
This method allows user to communicate between scripts.



## GetProfitLossAmount()
### Description
GetProfitLossAmount () provides the current unrealized gain or loss of a current position as the currency amount.

See [*GetAccountValue()*].

### Usage
```cs
GetProfitLossAmount(double profitLoss);
```

### Parameter
Double

### Return Value
A double value for the unrealized profit or loss

### Example
```cs
Print("Das momentane Risiko der Strategie " + this.Name + " beträgt " +GetProfitLossAmount(Position.OpenProfitLoss) + " " + Instrument.Currency);
```

## GetProfitLossRisk()
### Description
GetProfitLossRisk () returns the current unrealized gain or loss of a current position in R-multiples.

See [*GetAccountValue()*].

### Usage
```cs
GetProfitLossRisk();
```

### Parameter
None
### Return Value
A double value for the R-Multiple

### Example
```cs
Print("Das momentane Risiko der Strategie " + this.Name + " beträgt " + string.Format( "{0:F1} R.", GetProfitLossRisk()));
```

## IsAutomated
### Description
IsAutomated determines whether orders are activated automatically. IsAutomated is specified in the [*Initialize()*](#initialize) method.

If IsAutomated = true, then orders are automatically activated (default). If IsAutomated is assigned the value false, the corresponding order must be activated with order.[*ConfirmOrder()*].

### Parameter
Bool value

### Example
```cs
protected override void Initialize()
{
   IsAutomated = false;
}
```

## Order
### Description
IOrder is an object that contains information about an order that is currently managed by a strategy.


The individual properties are:

-   ** Action
    **One of four possible positions in the market:**
    - OrderAction.Buy
    - OrderAction.BuyToCover
    - OrderAction.Sell
    - OrderAction.SellShort

-   **AvgFillPrice**
    **The average purchase or selling price of a position.For positions without partial executions, this corresponds to the entry price.**

-   **Filled**
    For partial versions, Filled is less than Quantity

-   **FromEntrySignal**
    The trading instrument in which the position exists... See *Instruments*.

-   **LimitPrice**

-   **Name**
    The unique SignalName  (maybe mistake SignalName)

-   **OrderId**
    The unique OrderId

-   **OrderMode**
    One of three possible positions in the market:
    - OrderMode.Direct
    - OrderMode.Dynamic
    - OrderMode.Synthetic

-   **OrderState**
    The current status of the order can be queried (see *OnExecution* and *OnOrderUpdate*)
    - OrderState.Accepted
    - OrderState.Cancelled
    - OrderState.CancelRejected
    - OrderState.Filled
    - OrderState.PartFilled
    - OrderState.PendingCancel
    - OrderState.PendingReplace
    - OrderState.PendingSubmit
    - OrderState.Rejected
    - OrderState.ReplaceRejected
    - OrderState.Unknown
    - OrderState.Working

-   **OrderType**
    Possible order types:
    - OrderType.Limit
    - OrderType.Market
    - OrderType.Stop
    - OrderType.StopLimit

-   **Quantity**
    The quantity to be ordered

-   **StopPrice**

-   **Time**
    Time stamp

-   **TimeFrame**
    The TimeFrame, which is valid for the order.

-   **TimeFrame**

Possible Methods:

-   **order CancelOrder()**
    Delete the Order

-   **order.ConfirmOrder()**
    Confirm the order. This method have to be executed if IsAutomated is set to false and you want to run the order automatically. This is, for example, the case when an OCO or IfDone fabrication is to be produced.


## MarketPosition
See [*Position.MarketPosition*].

## Performance
### Description
Performance is an object containing information regarding all trades that have been generated by a strategy.

The trades are sorted into multiple lists. With the help of these lists it is easier to create a performance evaluation.

See Performance Characteristics.

The individual lists are:

-   **Performance.AllTrades**
    A [*Trade*] collection object containing all trades generated by a strategy.

-   **Performance.LongTrades**
    A [*Trade*] collection object containing all long trades generated by a strategy.

-   **Performance.ShortTrades**
    A [*Trade*] collection object containing all short trades generated by a strategy.

-   **Performance.WinningTrades**
    A [*Trade*] collection object containing all profitable trades generated by a strategy.

-   **Performance.LosingTrades**
    A [*Trade*] collection object containing all loss trades generated by a strategy.

### Example
```cs
// When exiting a strategy, create a performance evaluation
protected override void OnTermination()
{
Print("Performance evaluation of the strategy : " + this.Name);
Print("----------------------------------------------------");
Print("Amount of all trades: " + Performance.AllTrades.Count);
Print("Amount of winning trades: " + Performance.WinningTrades.Count);
Print("Amount of all loss trades: " + Performance.LosingTrades.Count);
Print("Amount of all long trades: " + Performance.LongTrades.Count);
Print("Amount of short trades: " + Performance.ShortTrades.Count);
Print("Result: " + Account.RealizedProfitLoss + " " + Account.Currency);
}
```

## Position
### Description
Position is an object containing information regarding the position currently being managed by a strategy.

The individual properties are:

-   **Position.AvgPrice**
    The average buy or sell price of a position.
    For positions without partial executions, this is equal to the entry price.

-   **Position.CreatedDateTime**
    Date and time at which the position was opened.

-   **Position.Instrument**
    The trading instrument in which the position exists.
    See *Instruments*.

-   **Position.MarketPosition**
    One of three possible positions in the market:
    - PositionType.Flat
    - PositionType.Long
    - PositionType.Short

-   **Position.OpenProfitLoss**
    The currently not yet realized profit or loss.
    See [*GetProfitLoss()*].

-   **Position.ProfitCurrency**
    Profit (or loss) displayed as a currency amount.

-   **Position.ProfitPercent**
    Profit (or loss) displayed in percent.

-   **Position.ProfitPoints**
    Profit (or loss) displayed in points or pips.

-   **Position.Quantity**
    Amount of stocks, contracts, CFDs etc. within a position.

### Example
```cs
if (Position.MarketPosition != PositionType.Flat)
{
Print("Average price " + Position.AvgPrice);
Print("Opening time " + Position.CreatedDateTime);
Print("Instrument " + Position.Instrument);
Print("Current positioning " + Position.MarketPosition);
Print("Unrealized P/L " + Position.OpenProfitLoss);
Print("P/L (currency) " + Position.ProfitCurrency);
Print("P/L (in percent) " + Position.ProfitPercent);
Print("P/L (in points) " + Position.ProfitPoints);
Print("Pieces " + Position.Quantity);
}
```

## Quantity
See [*Position.Quantity*][*Position.MarketPosition*].

## SetProfitTarget()
### Description
Set profit target immediately creates a "take profit" order after an entry order is generated. The order is sent directly to the broker and becomes active immediately.
If the profit target is static, you can also define SetProfitTarget() with the Initialize() method.

See [*SetStopLoss()*], [*SetTrailStop()*].

### Usage
```cs
SetProfitTarget(double currency)
SetProfitTarget(CalculationMode mode, double value)
SetProfitTarget(string fromEntry signal, CalculationMode mode, double value)
```

### Parameter
|    |           |
|------------------|------------------------------------|
| currency         | Sets the profit target in a currency, for example 500€.  |
| mode             | Possible values are:                                                                                                                                              
                    - CalculationMode.Percent (display in percent)                                                                                                                     
                    - CalculationMode.Price (display as price value)                                                                                                                   
                    - CalculationMode.Ticks (display in ticks or pips)      |
| value  | The distance between entry price and profit target. This is dependent upon the „mode" but generally refers to a monetary value, a percentage or a value in ticks. |
| fromEntry signal | The name of the entry signal for which the profit target is to be generated. The amount is taken from the entry order referenced.   |

### Example
```cs
protected override void Initialize()
{
// Creates a profit target order 10 ticks above break-even
SetProfitTarget(CalculationMode.Ticks, 10);
}
```

## SetStopLoss()
### Description
Set stop loss creates a stop loss order after an entry order is placed. The order is sent directly to the broker and becomes effective immediately.

If the stop loss is static, then SetStopLoss() can be defined with the Initialize() method.

See [*SetProfitTarget()*], [*SetTrailStop()*].

### Usage
```cs
SetStopLoss(double currency)
SetStopLoss(double currency, bool simulated)
SetStopLoss(CalculationMode mode, double value)
SetStopLoss(string fromEntry signal, CalculationMode mode, double value, bool simulated)
```

### Parameter
|      |      |
|------------------|---------------------------------------|
| currency         | The difference between the stop loss and the entry price (=risk) in a currency, such as 500€     |
| mode             | Potential values can be:  
                    - CalculationMode.Percent (display in percent)   
                    - CalculationMode.Price (display as price value)                                                                                                                                                             
                    - CalculationMode.Ticks (display in ticks or pips)    |
| simulated        | When set to "true," the stop order does not go live (as a market order) until the price has „touched" it for the first time (meaning that it is executed just as it would be under real market conditions). |
| value            | The distance between stop price and profit target. This is dependent upon the „mode" but generally refers to a monetary value, a percentage or a value in ticks.                                            |
| fromEntry signal | The name of the entry signal for which the stop order is to be generated. The amount is taken from the entry order referenced.                                                                              |

### Example
```cs
protected override void Initialize()
{
// Sets a stop of 500€
SetStopLoss(500);
}
```

## SetTrailStop()
### Description
Set trail stop creates a trail stop order after an entry order is generated. Its purpose is to protect you from losses, and after reaching break-even, to protect your gains.

The order is sent directly to the broker and becomes effective immediately.

If the stop loss price and the offset value are static, you can define SetTrailStop() with the Initialize() method.

If you use SetTrailStop() within the [*OnBarUpdate()*] method, you must make sure that the parameters are readjusted to the initial value, otherwise the most recently used settings will be used for the new position.

**Functionality:**

Assuming that you have SetTrailStop(CalculationMode.Ticks, 30) selected:

In a long position, the stop will be 30 ticks from the previously reached high. If the market makes a new high, the stop will be adjusted. However, the stop will no longer be moved downwards.

In a short position, this behavior starts with the most recent low.

**Tips:**

It is not possible to use SetStopLoss and SetTrailStop for the same position at the same time within one strategy. The SetStopLoss() method will always have precedence over the other methods.

However, it is possible to use both variants parallel to each other in the same strategy if they are referencing different entry signals.

Partial executions of a single order will cause a separate trading stop for each partial position.

If a SetProfitTarget() is used in addition to a SetTrailStop(), then both orders will be automatically linked to form an OCO order.

It is always a stop market order that is generated, and not a stop limit order.

If a position is closed by a different exit order within the strategy, then the TrailingStopOrder is automatically deleted.

See [*SetStopLoss()*], [*SetProfitTarget()*].

### Usage
```cs
SetTrailStop(double currency)
SetTrailStop(double currency, bool simulated)
SetTrailStop(CalculationMode mode, double value)
SetTrailStop(string fromEntry signal, CalculationMode mode, double value, bool simulated)
```

### Parameter
|           |      |
|------------------|---------------------------------------------------|
| currency         | The distance between the stop loss and the entry price      |
| mode             | Possible values are:  
  - CalculationMode.Percent
  - CalculationMode.Ticks   |
| simulated        | When set to "true," the stop order does not go live (as a market order) until the price has „touched" it for the first time (meaning that it is executed just as it would be under real market conditions). |
| value            | The distance between stop price and profit target. This is dependent upon the „mode" but generally refers to a monetary value, a percentage or a value in ticks.                                            |
| fromEntry signal | The name of the entry signal for which the stop order is to be generated. The amount is taken from the entry order referenced.                                                                              |

### Example
```cs
protected override void Initialize()
{
// Sets a trailing stop of 30 ticks
SetTrailStop(CalculationMode.Ticks, 30);
}
```

## SubmitOrder()
### Description
Submit order creates a user-defined order. For this order, no stop or limit order is placed in the market. All AgenaTrader control mechanisms are switched off for this order type. The user is responsible for managing the various stop and target orders, including partial executions.

See [*OnOrderUpdate()*], [*OnExecution()*].

### Usage
```cs
SubmitOrder(int multibarSeriesIndex, OrderAction orderAction, OrderType orderType, int quantity, double limitPrice, double stopPrice, string ocoId, string strategyName)
```

### Parameter
|                     |                                                                    |
|---------------------|--------------------------------------------------------------------|
| multibarSeriesIndex | For multi-bar strategies.                                          
                       Index of the data series for which the order is to be executed.     
                       See BarsInProgress.                                                 |
| orderAction         | Possible values are:                                               

                       OrderAction.Buy                                                     
                       Buy order for a long entry                                          

                       OrderAction.Sell                                                    
                       Sell order for closing a long position                              

                       OrderAction.SellShort                                               
                       Sell order for a short entry                                        

                       OrderAction.BuyToCover                                              
                       Buy order for closing a short position                              |
| orderType           | Possible values:                                                   
                       OrderType.Limit                                                     
                       OrderType.Market                                                    
                       OrderType.Stop                                                      
                       OrderType.StopLimit                                                 |
| quantity            | Amount                                                             |
| limitPrice          | Limit value. Inputting a 0 makes this parameter irrelevant         |
| stopPrice           | Stop value. Inputting a 0 makes this parameter irrelevant          |
| ocoId               | A unique ID (string) for linking multiple orders into an OCO group |
| strategyName          | An unambiguous signal name (string)                                |

### Return Value
an order object of the type "IOrder"

### Example
```cs
private IOrder entryOrder = null;
protected override void OnBarUpdate()
{
// Entry conditions
if (Close[0] > SMA(20)[0] && entryOrder == null)
entryOrder = SubmitOrder(0, OrderAction.Buy, OrderType.Market, 1, 0, 0, "", "Enter long");
}
```

## TimeInForce
### Description
The time in force property determines how long an order is valid for. The validity period is dependent upon which values are accepted by a broker.

TimeInForce is specified with the [*Initialize()*](#initialize) method.

Permitted values are:
TimeInForce.day
TimeInForce.loc
TimeInForce.gtc (GTC = good till canceled)
TimeInForce.gtd

**Default:** TimeInForce.GTC

### Usage
**TimeInForce**

### Example
```cs
protected override void Initialize()
{
TimeInForce = TimeInForce.Day;
}
```

## TraceOrders
### Description
The trace orders property is especially useful for keeping track of orders generated by strategies. It also provides an overview of which orders were generated by which strategies.
Trace orders can be specified with the [*Initialize()*](#initialize) method.

When TraceOrders is activated, each order will display the following values in the output window:

-   Instrument
-   Time frame
-   Action
-   Type
-   Limit price
-   Stop price
-   Quantity
-   Name

This information is useful when creating and debugging strategies.

### Usage
TraceOrders

### Parameter
none

### Return Value
**true** Tracing is currently switched on
**false** Tracing is switched off

### Example
```cs
protected override void Initialize()
{
ClearOutputWindow();
TraceOrders = true;
}
```

## Trade
### Description
Trade is an object containing information about trades that have been executed by a strategy or are currently running.

The individual properties are:

-   **Trade.AvgPrice**
    Average entry price

-   **Trade.ClosedProfitLoss**
    Profit or loss already realized

-   **Trade.Commission**
    Commissions

-   **Trade.CreatedDateTime**
    Time at which the trade was created

-   **Trade.EntryReason**
    Description of the entry signal
    For strategies: signal entry name

-   **Trade.ExitDateTime**
    Time at which the trade was closed

-   **Trade.ExitPrice**
    Exit price

-   **Trade.ExitReason**
    Description of the exit signal
    For strategies: name of the strategy

-   **Trade.Instrument**
    Description of the trading instrument

-   **Trade.MarketPosition**
    Positioning within the market
    - PositionType.Flat
    - PositionType.Long
    - PositionType.Short

-   **Trade.OpenProfitLoss**
    Unrealized profit/loss of a running position

-   **Trade.ProfitCurrency**
    Profit or loss in the currency that the account is held in

-   **Trade.ProfitLoss**
    Profit or loss

-   **Trade.ProfitPercent**
    Profit or loss in percent

-   **Trade.ProfitPercentWithCommission**
    Profit or loss in percent with commissions

-   **Trade.ProfitPoints**
    Profit or loss in points/pips

-   **Trade.Quantity**
    Quantity of stocks/contracts/ETFs/etc.

-   **Trade.TimeFrame**
    Timeframe in which the trade was opened

-   **Trade.Url**
    URL for the snapshot of the chart at the moment of creation

### Example
```cs
protected override void OnTermination()
{
  if (Performance.AllTrades.Count < 1) return;
  foreach (ITrade trade in Performance.AllTrades)
  {
    Print("Trade #"+trade.Id);
    Print("--------------------------------------------");
    Print("Average price " + trade.AvgPrice);
    Print("Realized P/L " + trade.ClosedProfitLoss);
    Print("Commissions " + trade.Commission);
    Print("Time of entry " + trade.CreatedDateTime);
    Print("Entry reason " + trade.EntryReason);
    Print("Time of exit " + trade.ExitDateTime);
    Print("Exit price " + trade.ExitPrice);
    Print("Exit reason " + trade.ExitReason);
    Print("Instrument " + trade.Instrument);
    Print("Positioning " + trade.MarketPosition);
    Print("Unrealized P/L " + trade.OpenProfitLoss);
    Print("P/L (currency) " + trade.ProfitCurrency);
    Print("P/L " + trade.ProfitLoss);
    Print("P/L (in percent) " + trade.ProfitPercent);
    Print("P/L (% with commission)" + trade.ProfitPercentWithCommission);
    Print("PL (in points) " + trade.ProfitPoints);
    Print("Quantity " + trade.Quantity);
    Print("Timeframe " + trade.TimeFrame);
    Print("URL for the snapshot " + trade.Url);
  }
}
```

## Unmanaged
# Backtesting and Optimization

## Performance Characteristics
Performance characteristics are the various factors that can be calculated for a list of trades. The trades can be generated by a strategy in real-time or based on a backtest.

The following are available:

-   all trades
-   all long trades
-   all short trades
-   all winning trades
-   all losing trades

See [*Performance*].

The individual factors are:

-   **AvgEtd**
    The average drawdown at the end of a trade
    &lt;TradeCollection&gt;.TradesPerformance.&lt;TradesPerformanceValues&gt;.AvgEtd
    ```cs
    Print("Average ETD of all trades is: " + Performance.AllTrades.TradesPerformance.Currency.AvgEtd);
    ```
-   **AvgMae**
    Average maximum adverse excursion
    &lt;TradeCollection&gt;.TradesPerformance.&lt;TradesPerformanceValues&gt;.AvgMae
    ```cs
    Print("Average MAE of all trades is: " + Performance.AllTrades.TradesPerformance.Currency.AvgMae);
    ```
-   **AvgMfe**
    Average maximum favorable excursion
    &lt;TradeCollection&gt;.TradesPerformance.&lt;TradesPerformanceValues&gt;.AvgMfe
    ```cs
    Print("Average MFE of all trades is: " + Performance.AllTrades.TradesPerformance.Currency.AvgMfe);
    ```
-   **AvgProfit**
    Average profit for all trades
    &lt;TradeCollection&gt;.TradesPerformance.&lt;TradesPerformanceValues&gt;.AvgProfit
    ```cs
    Print("Average profit of all trades is: " + Performance.AllTrades.TradesPerformance.Currency.AvgProfit);
    ```
-   **CumProfit**
    The cumulative winnings over all trades
    &lt;TradeCollection&gt;.TradesPerformance.&lt;TradesPerformanceValues&gt;.CumProfit
    ```cs
    Print("Average cumulative profit of all trades is: " + Performance.AllTrades.TradesPerformance.Currency.CumProfit);
    ```
-   **DrawDown**
    The drawdown for all trades
    &lt;TradeCollection&gt;.TradesPerformance.&lt;TradesPerformanceValues&gt;.DrawDow
    ```cs
    Print("Drawdown of all trades is: " + Performance.AllTrades.TradesPerformance.Currency.DrawDown);
    ```
-   **LargestLoser**
    The largest losing trade
    &lt;TradeCollection&gt;.TradesPerformance.&lt;TradesPerformanceValues&gt;.LargestLoser
    ```cs
    Print("Largest loss of all trades is: " + Performance.AllTrades.TradesPerformance.Currency.LargestLoser);
    ```
-   **LargestWinner**
    The largest winning trade
    &lt;TradeCollection&gt;.TradesPerformance.&lt;TradesPerformanceValues&gt;.LargestWinner
    ```cs
    Print("Largest win of all trades is: " + Performance.AllTrades.TradesPerformance.Currency.LargestWinner);
    ```
-   **ProfitPerMonth**
    The total performance (wins/losses) for the month (also in percent)
    &lt;TradeCollection&gt;.TradesPerformance.&lt;TradesPerformanceValues&gt;.ProfitPerMonth
    ```cs
    Print("Profit per month of all trades is: " + Performance.AllTrades.TradesPerformance.Currency.ProfitPerMonth);
    ```
-   **StdDev**
    The standard deviation for the wins/losses. With this, you are able to identify outliers. The smaller the standard deviation, the higher the expectation of winnings.

**All factors are double values.**

![Performance Characteristics](./media/image10.png)
