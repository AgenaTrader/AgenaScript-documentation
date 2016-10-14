
#Events

AgenaTrader is an *event-oriented* application by definition.

Programming in AgenaTrader using the various application programming interface ([*API*]) methods is based initially on the [*Overwriting*] of routines predefined for event handling.

The following methods can be used and therefore overwritten:

-   [*OnBarUpdate()*]
-   [*OnExecution()*]
-   [*OnMarketData()*]
-   [*OnMarketDepth()*]
-   [*OnOrderUpdate()*]
-   [*OnStartUp()*]
-   [*OnTermination()*]

## OnBarUpdate()
### Description
The OnBarUpdate() method is called up whenever a bar changes; depending on the variables of [*CalculateOnBarClose*](#CalculateOnBarClose), this will happen upon every incoming tick or when the bar has completed/closed.
OnBarUpdate is the most important method and also, in most cases, contains the largest chunk of code for your self-created indicators or strategies.
The editing begins with the oldest bar and goes up to the newest bar within the chart. The oldest bar has the number 0. The indexing and numbering will continue to happen; in order to obtain the numbering of the bars you can use the current bar variable. You can see an example illustrating this below.

Caution: the numbering/indexing is different from the bar index – see [*Bars*](#Bars).

More information can be found here: [*Events*](#Events).

### Parameter
none

### Return Value
none

### Usage
```cs
protected override void OnBarUpdate()
```

### Example
```cs
protected override void OnBarUpdate()
{
    Print("Calling of OnBarUpdate for the bar number " + CurrentBar + " from " +Time[0]);
}
```

## OnExecution()
### Description
The OnExecution() method is called up when an order is executed (filled).
The status of a strategy can be changed by a strategy-managed order. This status change can be initiated by the changing of a volume, price or the status of the exchange (from “working” to “filled”). It is guaranteed that this method will be called up in the correct order for all events.

OnExecution() will always be executed AFTER [*OnOrderUpdate()*].

More information can be found here: [*Events*]

### Parameter
An execution object of the type IExecution

### Return Value
none

### Usage
```cs
protected override void OnExecution(IExecution execution)
```

### Example
```cs
private IOrder entryOrder = null;
protected override void OnBarUpdate()
{
    if (entryOrder == null && Close[0] > Open[0])
    entryOrder = EnterLong();
}
protected override void OnExecution(IExecution execution)
{
    // Example 1
    if (entryOrder != null && execution.Order == entryOrder)
    Print(execution.ToString());
    // Example 2
    if (execution.Order != null && execution.Order.OrderState == OrderState.Filled)
    Print(execution.ToString());
}
```

## OnMarketData()
### Description
The OnMarketData() method is called up when a change in level 1 data has occurred, meaning whenever there is a change in the bid price, ask price, bid volume, or ask volume, and of course in the last price after a real turnover has occurred.
In a multibar indicator, the BarsInProgress method identifies the data series that was used for an information request for OnMarketData().
OnMarketData() will not be called up for historical data.
More information can be found here: [*Events*](#Events).

**Notes regarding data from Yahoo (YFeed)**

The field "LastPrice" equals – as usual – either the bid price or the ask price, depending on the last revenue turnover.

The „MarketDataType“ field always equals the „last" value

The fields "Volume", "BidSize" and "AskSize" are always 0.

### Usage
```cs
protected override void OnMarketData(MarketDataEventArgs e)
```

### Return Value
none

### Parameter
[*MarketDataEventArgs*] e

### Example
```cs
protected override void OnMarketData(MarketDataEventArgs e)
{
    Print("AskPrice "+e.AskPrice);
    Print("AskSize "+e.AskSize);
    Print("BidPrice "+e.BidPrice);
    Print("BidSize "+e.BidSize);
    Print("Instrument "+e.Instrument);
    Print("LastPrice "+e.LastPrice);
    Print("MarketDataType "+e.MarketDataType);
    Print("Price "+e.Price);
    Print("Time "+e.Time);
    Print("Volume "+e.Volume);
}
```

## OnMarketDepth()
### Description
The OnMarketDepth() method is called up whenever there is a change in the level 2 data (market depth).
In a multibar indicator, the BarsInProgress method identifies the data series for which the OnMarketDepth() method is called up.
OnMarketDepth is not called up for historical data.

More information can be found here: [*Events*](#Events).

### Usage
```cs
protected override void OnMarketDepth(MarketDepthEventArgs e)
```

### Return Value
none

### Parameter
[*MarketDepthEventArgs*] e

### Example
```cs
protected override void OnMarketDepth(MarketDepthEventArgs e)
{
    // Output for the current ask price
    if (e.MarketDataType == MarketDataType.Ask && e.Operation == Operation.Update)
    Print("The current ask is" + e.Price + " " + e.Volume);
}
```

## OnOrderUpdate()
### Description
The OnOrderUpdate() method is called up whenever the status is changed by a strategy-managed order.
A status change can therefore occur due to a change in the volume, price or status of the exchange (from “working” to “filled”). It is guaranteed that this method will be called up in the correct order for the relevant events.

**Important note:**
**If a strategy is to be controlled by order executions, we highly recommend that you use OnExecution() instead of OnOrderUpdate(). Otherwise there may be problems with partial executions.**

More information can be found here: [*Events*].

### Parameter
An order object of the type IOrder

### Return Value
None

### Usage
```cs
protected override void OnOrderUpdate(IOrder order)
```

### Example
```cs
private IOrder entryOrder = null;
protected override void OnBarUpdate()
{
    if (entryOrder == null && Close[0] > Open[0])
    entryOrder = EnterLong();
}
protected override void OnOrderUpdate(IOrder order)
{
    if (entryOrder != null && entryOrder == order)
    {
        Print(order.ToString());
        if (order.OrderState == OrderState.Cancelled)
        {
			Print("Order was canceled.");
			entryOrder = null;
        }
    }
}
```

## OnStartUp()
### Description
The OnStartUp() method can be overridden to initialize your own variables, perform license checks or call up user forms etc.
OnStartUp() is only called up once at the beginning of the script, after [*Initialize()*] and before [*OnBarUpdate()*] are called up.

See [*OnTermination()*].

More information can be found here: [*Events*] .

### Parameter
none

### Return Value
none

### Usage
```cs
protected override void OnStartUp()
```

### Example
```cs
private myForm Window;
protected override void OnStartUp()
{
    if (ChartControl != null)
    {
    Window = new myForm();
    Window.Show();
    }
}
```

## OnTermination()
### Description
The OnTermination() method can also be overridden in order to once again free up all the resources used in the script.

See [*Initialize()*](#Initialize) and [*OnStartUp()*](#OnStartUp).

More information can be found here: [*Events*](#Events).

### Parameter
none

### Return Value
none

### Usage
```cs
protected override void OnTermination()
```

### More Information
**Caution:**
Please do not override the Dispose() method since this can only be used much later within the script. This would lead to resources being used and held for an extended period and thus potentially causing unexpected consequences for the entire application.

### Example
```cs
protected override void OnTermination()
{
    if (Window != null)
    {
        Window.Dispose();
        Window = null;
    }
}
```
