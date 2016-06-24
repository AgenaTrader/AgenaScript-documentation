<span id="_topic_EinfhrendeWorte" class="anchor"></span>**Introductory Words**

AgenaScript is AgenaTrader’s integrated programming language. The syntax is derived from C\# and thus closely resembles it.

AgenaScript allows you to execute any ideas/methods that are too complex for the ConditionEscort. From simple indicators to entire applications where AgenaTrader is only required to run in the background, anything that can be written in .NET can be implemented.

Information contained in this help document:

1\. Handling bars and instruments

You will find a detailed explanation of how AgenaScript reacts and interacts with individual bars or candles as well as various trading instruments.

2\. Events

AgenaScript is event-based and -driven. When (for example) a candle closes or a new candle opens, then an event has occurred. When a new price value is delivered by your data provider or a new order is executed by your broker, then these too are considered events. AgenaScript allows you to react to these events. You can read about the exact methodology in this and the following chapters.

3\. Indicators und oscillators

This section explains the indicators already available for AgenaTrader users in detail. The help document presents the indicator itself as well as its usage, the interpretation of the signals and the source of the information.\
Each indicator and oscillator can be used on its own with AgenaScripts; the exact syntax and meaning of its parameters are also described here. You will also find several code snippets here.

4\. Strategy programming

AgenaScript allows you to create your own trading strategies and execute them live within the market. Information pertaining to prerequisites and how orders are sent to the broker and managed internally can be found here.

5\. Keywords

Like every other programming language, AgenaTrader has a set of commands that can be converted and used via Scripts. You should be relatively well versed in these if you wish to create your own indicators or trading systems.

6\. Drawing objects

All drawing objects that can be used within the chart can also be accessed using AgenaScript. In this way, you can turn on/off certain lines, arrows, rectangles and other objects with specified conditions.

7\. Tips and tricks

This section provides solutions to problems of an unusual nature. To solve such problems, you need to be able to trace and understand source code and programming. More advanced programmers and users may find solutions and suggestions that could help them in their own programming.

<span id="_topic_Daten" class="anchor"></span>**Data**

Data is understood as information that is retrieved externally and uploaded to AgenaTrader, or as data series that are created by AgenaScripts.

Further detailed information can be found using the appropriate shortcuts:

-   [*Bars*]

-   [*Data series*]

-   [*Instrument*]

<span id="_topic_BarsKerzen" class="anchor"></span>**Bars (Candles)**

**Functionality**

A classical indicator calculates one or multiple values using an existing data series.

Data series can be anything from closing prices to daily lows or values of an hourly period etc.

Every period (meaning all candles of one day, one hour etc.) is assigned one or more indicator values.\
The following example is based on an indicator value, such as with a moving average, for example.\
To calculate a smoothed moving average, AgenaTrader needs a data series. In this example we will use the closing prices. All closing prices of a bar (candle) that are represented in the chart will be saved in a list and indexed.\
\
The current closing price, meaning the closing price of the bar that is on the right-hand side of the chart, will be assigned an index of 0. The bar to the left of that will have an index of 1 and so on. The oldest bar displayed will have an index value of 500.\
\
Whenever a new bar is added within a session it will become the new index 0; the bar to the left of it, which previously had an index of 0, will become index 1 and so on. The oldest bar will become index 501.\
Within a script (a self-created program/algorithm) the [***Close***] will be representative for the array (list) of all closing prices.\
The last closing price is thus **Close \[0\]**; the closing price previous to this will become **Close \[1\]**, the value before that will become **Close \[2\]** and the oldest bar will be **Close \[501\]**. The number within the squared brackets represents the index. AgenaTrader allows you to use the „bars ago“ expression for this in general cases.\
\
Obviously, every bar will not only have a closing value but also a [*High*], [*Low*], [*Open*], [*Median*], [*Typical*], [*Weighted*], [*Time*] and *Volume*. Thus, the high of the candle that occurred 10 days ago will be **High \[10\]**, yesterday’s low **Low \[1\]**...

**Important tip:**

The previous examples all assume that the calculations will occur at the end of a period. The value of the currently running index is not being taken into consideration.\
\
If you wish to use the values of the currently forming candle then you will need to set the value of

[*CalculateOnBarClose*] to „false“.

In this case the currently running bar will have the value 0, the bar next to the current bar will have the value 1 and so on. The oldest bar (as in the example above) would now have the value 502.\
\
With close \[0\] you would receive the most recent value of the last price that your data provider transmitted to AgenaTrader. All values of the bar (high \[0\], low \[0\]…) may still change as long as the bar is not yet finished/closed and a new bar has not yet started. Only the open \[0\] value will not change.

<span id="_topic_EigenschaftenvonBars" class="anchor"></span>**Properties of Bars**

**Properties of Bars**

"Bars" represents a list of all bars (candles) within a chart (see [*Functionality*][*Bars*]).

Bars (**public** IBars Bars) can be used directly in a script and equates to BarsArray \[0\] (see Bars.GetNextBeginEnd for more information).

The list of bars itself has many properties that can be used in AgenaScript. Properties are always indicated by a dot before the objects (in this case bars, list of candles).

-   [*Bars.BarsSinceSession*]

-   [*Bars.Count*]

-   [*Bars.FirstBarOfSession*]

-   [*Bars.GetBar*]

-   [*Bars.GetBarsAgo*]

-   [*Bars.GetByIndex*]

-   [*Bars.GetIndex*]

-   [*Bars.GetNextBeginEnd*]

-   [*Bars.GetOpen*] (+ GetHigh, GetLow, GetClose, GetTime and GetVolume)

-   [*Bars.Instrument*]

-   [*Bars.IsIntraDay*]

-   [*Bars.PercentComplete*]

-   [*Bars.SessionBegin*]

-   [*Bars.SessionEnd*]

-   [*Bars.SessionNextBegin*]

-   [*Bars.SessionNextEnd*]

-   [*Bars.TickCount*]

-   [*Bars.TimeFrame*]

-   [*Bars.TotalTicks*]

With the **OnBarUpdate** () method you can use any properties you want without having to test for a null reference.\
As soon as the function **OnBarUpdate** () is called up by AgenaScript, it is assumed that an object is also available. If you wish to use these properties outside of **OnBarUpdate** () then you should first perform a test for null references using **if** (Bars != **null**).

<span id="_topic_BarsBarsSinceSession" class="anchor"></span>Bars.BarsSinceSession

**Description**

Bars.BarsSinceSession outputs the amount of bars that have occurred since the beginning of the current trading session.

See further [*Properties*] of bars.

**Return Value**

Type int **Amount of Bars**

A value of -1 indicates a problem with referencing the correct session beginning.

**Usage**

Bars.BarsSinceSession

**Further Information**

Within [*OnBarUpdate ()*] this property can be used without having to test for a null reference. As soon as the OnBarUpdate () method is called up by AgenaScript, the object will become available.

If this property is used outside of OnBarUpdate () then you should test for a null reference before executing it. You can test using **if** (Bars!= **null**) .

**Example**

**Print** ("Since the start of the last trading session there have been” + Bars.BarsSinceSession + “bars.");

<span id="_topic_BarsCount" class="anchor"></span>Bars.Count

**Description**

Bars.Count gives you the amount of bars in a data series.

See [*Properties*] for additional information.

**Return Value**

Type int Amount of Bars

**Usage**

Bars.Count

**More Information**

The value of *CurrentBar* can only be lesser than or equal to Bars.Count - 1

When you specify how many bars are to be loaded within AgenaTrader, then the value of Bars.Count is equal to this setting. In the following example, Bars.Count would give back a value of 500.

![]

**Example**

**Print** ("There are a total of” + Bars.Count + “bars available.");

<span id="_topic_BarsFirstBarOfSession" class="anchor"></span>Bars.FirstBarOfSession

**Description**

With Bars.FirstBarOfSession you can determine whether the current bar is the first bar of the trading session.

See [*Properties*] of bars for more information.

**Return Value**

Type bool

**true**: The bar is the first bar of the current trading session\
**false**: The bar is not the first bar of the current trading session

**Usage**

Bars.FirstBarOfSession

**More Information**

With [*OnBarUpdate ()*] this property can be used without having to test for a null reference. As soon as the OnBarUpdate () method is called up, an object will become available.\
If this property is called up outside of OnBarUpdate () you should test for a null reference using if (Bars != null).

**Example**

**if** (Bars.FirstBarOfSession)

**Print** ("The current trading session started at” + Time \[0\]);

<span id="_topic_BarsGetBar" class="anchor"></span>Bars.GetBar

**Description**

Bars.GetBar outputs the first bars (from oldest to newest) that correspond to the specified date/time.

See [*Bars.GetBarsAgo*], [*Bars.GetByIndex*], [*Bars.GetIndex*].

**Parameter**

Type DateTime

**Return Value**

Type IBar Bar Object, for the bars corresponding to the timestamp

For a timestamp older than the oldest bar: 0 (null)\
For a timestamp younger than the newest bar: index of the last bar

**Usage**

Bars.**GetBar**(DateTime time)

**More Information**

For the indexing of bars please see [*Functionality*][*Bars*]

For more information about using DateTime see [*http://msdn.microsoft.com/de-de/library/system.datetime.aspx*]

**Example**

**Print** ("The closing price for 01.03.2012 at 18:00:00 was " + Bars.**GetBar**(**new DateTime**(2012, 01, 03, 18, 0, 0)).Close);

<span id="_topic_BarsGetBarsAgo" class="anchor"></span>Bars.GetBarsAgo

**Description**

Bars.GetBarsAgo outputs the index of the first bars (from oldest to newest) that correspond to the specified date/time.

See: [*Bars.GetBar*], [*Bars.GetByIndex*], [*Bars.GetIndex*].

**Parameter**

Type DateTime

**Return Value**

Type int Index of the bar that corresponds to the timestamp

With a timestamp older than the oldest bar: 0 (null)\
With a timestamp newer than the youngest bar: index of the last bar

**Usage**

Bars.**GetBarsAgo**(DateTime time)

**More Information**

For more information about indexing please see [*Functionality*][*Bars*]

For more information about using DateTime see [*http://msdn.microsoft.com/de-de/library/system.datetime.aspx*]

**Example**

**Print**("The bar for 01.03.2012 at 18:00:00 O’clock has an index of " + Bars.**GetBarsAgo**(**new DateTime**(2012, 01, 03, 18, 0, 0)));

<span id="_topic_BarsGetClose" class="anchor"></span>Bars.GetClose

Bars.GetClose(int index) – see [*Bars.GetOpen*].

<span id="_topic_BarsGetHigh" class="anchor"></span>Bars.GetHigh

Bars.GetHigh(int index) – see [*Bars.GetOpen*].

<span id="_topic_BarsGetByIndex" class="anchor"></span>Bars.GetByIndex

**Description**

Bars.GetByIndex outputs the index for the specified bar object

See [*Bars.GetBar*], [*Bars.GetBarsAgo*], [*Bars.GetIndex*].

**Parameter**

Type int Index

**Return Value**

Type IBar Bar object for the specified index

**Usage**

Bars.**GetByIndex** (int Index)

**More Information**

For indexing of bars see [*Functionality*][*Bars*]

**Example**

**Print**(Close\[0\] + " and " + Bars.**GetByIndex**(CurrentBar).Close + " are equal in this example.");

<span id="_topic_BarsGetIndex" class="anchor"></span>Bars.GetIndex

**Description**

Bars.GetIndex outputs the index of a bar – you can input either a bar object or a date-time object using this method.

See [*Bars.GetBar*], [*Bars.GetBarsAgo*], [*Bars.GetByIndex*].

**Parameter**

Type IBar bar\
or\
Type DateTime

**Return Value**

Type int The bar index of the specified bar object or DateTime object

**Usage**

Bars.**GetIndex** (IBar bar)

Bars.**GetIndex** (DateTime dt)

**More Information**

For more information about indexing see [*Functionality*][*Bars*]

**Example**

**int** barsAgo = 5;

IBar bar = Bars.**GetBar**(Time\[barsAgo\]);

**Print**(barsAgo + " and " + Bars.**GetIndex**(bar) + " are equal in this example.");

<span id="_topic_BarsGetLow" class="anchor"></span>Bars.GetLow

Bars.GetLow(int index) – see [*Bars.GetOpen*].

<span id="_topic_BarsGetNextBeginEnd" class="anchor"></span>Bars.GetNextBeginEnd

**Description**

Bars.GetNextBeginEnd outputs the date and time for the beginning and end of a trading session.

See [*Bars.SessionBegin*], [*Bars.SessionEnd*], [*Bars.SessionNextBegin*], [*Bars.SessionNextEnd*].

**Parameter**

  ---------- --------- --------------------------------------------------------------------------------------------
  DateTime   time      Date or time for which the data of the following trading session will be scanned/searched.
  iBars      bars      Bar object for which the data will be scanned/searched.
  int        barsago   Number of days in the past for which the data will be searched/scanned.
  ---------- --------- --------------------------------------------------------------------------------------------

**Return Value**

DateTime session begin\
DateTime session end

**Note:\
**The date for the beginning and the end of a trading session are connected components. If the specified date corresponds to the end date of the current trading session then the returned value for the beginning of a trading session may already be in the past. In this case the date for the following trading session cannot be returned.

**Usage**

Bars.**GetNextBeginEnd**(Bars bars, **int** barsAgo, **out** DateTime sessionBegin, **out** DateTime sessionEnd)

Bars.**GetNextBeginEnd**(DateTime time, **out** DateTime sessionBegin, **out** DateTime sessionEnd)

**More Information**

The two signatures will not necessarily output the same result.\
When using the bar signature, the supplied bar will be inspected for its session template association. The beginning and end of the next session will be taken from this template.\
\
When using the time signature, the date and time of the supplied bar will be used to calculate the data for the current and the following sessions.\
\
When using the time signature, a timestamp is transmitted that corresponds exactly to the beginning or the end time of a session.

More information can be found here [*http://msdn.microsoft.com/de-de/library/system.datetime.aspx*]

**Example**

DateTime sessionBegin;

DateTime sessionEnd;

**protected** override void **OnBarUpdate**()

{

Bars.**GetNextBeginEnd**(Bars, 0, **out** sessionBegin, **out** sessionEnd);

**Print**("Session Start: " + sessionBegin + " Session End: " + sessionEnd);

}

Bars.GetSessionDate

**Description**

Bars.GetSessionDate provides the date and the time of the start of a particular trading session.

The date and time for the start of the current trading session are also displayed correctly even if the function is used on a bar in the past.

<span id="_topic_BarsGetOpen" class="anchor"></span>Bars.GetOpen

**Description**

For reasons of compatibility, the following methods are available.

- Bars.GetOpen(int index) outputs the open for the bars referenced with &lt;index&gt;.

- Bars.GetHigh(int index) outputs the high for the bars referenced with &lt;index&gt;.

- Bars.GetLow(int index) outputs the low for the bars referenced with &lt;index&gt;.

- Bars.GetClose(int index) outputs the close for the bars referenced with &lt;index&gt;.

- Bars.GetTime(int index) outputs the timestamp for the bars referenced with &lt;index&gt;.

- Bars.GetVolume(int index) outputs the volume for the bars referenced with &lt;index&gt;.

**Caution**: The indexing will deviate from the [*Indexing*][*Bars*] normally used.\
Here, the indexing will begin with 0 for the oldest bar (on the left of the chart) and end with the newest bar on the right of the chart (=Bars.Count-1).

The indexing can easily be recalculated:

private int Convert(int idx)

{

return Math.Max(0,Bars.Count-idx-1-(CalculateOnBarClose?1:0));

}

**Parameter**

int index (0 .. Bars.Count-1)

**Return Value**

Type double for GetOpen, GetHigh, GetLow, GetClose and GetVolume

Type DateTime for GetTime

<span id="_topic_BarsGetTime" class="anchor"></span>Bars.GetTime

Bars.GetTime(int index) – see [*Bars.GetOpen*].

<span id="_topic_BarsGetVolume" class="anchor"></span>Bars.GetVolume

Bars.GetVolume(int index) – see [*Bars.GetOpen*].

<span id="_topic_BarsInstrument" class="anchor"></span>Bars.Instrument

**Description**

Bars.Instrument outputs an instrument object for the trading instrument displayed within the chart.

See [*Properties*] for more information.

**Parameter**

None

**Return Value**

Type Instrument

**Usage**

Bars.Instrument

**More Information**

For more information regarding the trading instruments please see [*Instrument*].

**Example**

// both outputs will provide the same result

**Print**("The currently displayed trading instrument has the symbol: " + Bars.Instrument);

Instrument i = Bars.Instrument;

**Print**("The currently displayed trading instrument has the symbol " + i.Symbol);

<span id="_topic_BarsIsIntraDay" class="anchor"></span>Bars.IsIntraDay

This function is not yet implemented.

We are working on this – please bear with us. Thank you for your patience.

![][1]

<span id="_topic_BarsPercentComplete" class="anchor"></span>Bars.PercentComplete

**Description**

Bars.PercentComplete outputs the value that displays what percentage a bar has already completed. A bar with a period of 10 minutes has completed 50% after 5 minutes.

For non-time-based charts (Kagi, LineBreak, Renko, Range, P&F etc.) this will output 0 during backtesting.

**Return Value**

**double**

A percentage value; 30% will be outputted as 0.3

**Usage**

Bars.PercentComplete

**More Information**

With [*OnBarUpdate()*][*OnBarUpdate ()*] this property can be used without having to test for a null reference. As soon as the OnBarUpdate() method is called up by AgenaScript, the object will become available.

If this property is used outside of OnBarUpdate() you should test for a null reference before executing it. You can test using **if** (Bars != **null**)

**Example**

// A 60 minute chart is looked at from an intraday perspective

// every 5 minutes before the current bar closes

// an acoustic signal shall be played

// 55 min. equals 92%

**Bool** remind = **false**;

**protected** override void **OnBarUpdate**()

{

**if** (FirstTickOfBar) remind = **true**;

**if** (remind && Bars.PercentComplete &gt;= 0.92)

{

remind = **false**;

**PlaySound**("Alert1");

}

}

<span id="_topic_BarsSessionBegin" class="anchor"></span>Bars.SessionBegin

**Description**

Bars.SessionBegin outputs the date and time for the beginning of the current trading session.

Date and time for the beginning of the current trading session will be displayed correctly when the function is used on a bar that has occurred in the past.

**Parameter**

None

**Return Value**

Type DateTime

**Usage**

Bars.GetSessionBegin

**More Information**

The time for the returned value will equal the starting time defined in the Market Escort for the specified exchange. The value itself is set within the Instrument Escort and can be called up in AgenaScript using the function [*Instrument.Exchange*] .

![][2]

**Example**

**Print**("The currently running trading session started at " + Bars.SessionBegin );

<span id="_topic_BarsSessionEnd" class="anchor"></span>Bars.SessionEnd

**Description**

Bars.SessionEnd outputs the time for the end of the currently running trading session.\
\
Date and time for the end of the current trading session will, in this case, also be outputted correctly when the function is used on a previous bar.

**Parameter**

None

**Return Value**

Type DateTime

**Usage**

Bars.GetSessionEnd

**More Information**

The time for the returned value will correlate with the end time of the trading session defined in the Market Escort for the exchange. The value itself can be set within the Instrument Escort and can be called up with AgenaScript using the [*Instrument.Exchange*] function.

![][3]

**Example**

**Print**("The current trading session will end at " + Bars.SessionEnd);

<span id="_topic_BarsSessionNextBegin" class="anchor"></span>Bars.SessionNextBegin

**Description**

Bars.SessionNextBegin outputs the date and time for the start of the next trading session.\
\
Date and time for the next session will be correctly outputted when the function is used on a bar in the past.

**Parameter**

None

**Return Value**

Type DateTime

**Usage**

Bars.GetSessionNextBegin

**More Information**

The time for the returned value will correlate to the value displayed in the MarketEscort. The value can be set within the Instrument Escort and can be called up using the [*Instrument.Exchange*] function.

![][2]

**Example**

**Print**("The next trading session starts at " + Bars.SessionNextBegin);

<span id="_topic_BarsSessionNextEnd" class="anchor"></span>Bars.SessionNextEnd

**Description**

Bars.SessionNextEnd outputs the date and time for the end of the next session.\
\
See [*Properties*] for more information.

**Parameter**

None

**Return Value**

Type DateTime

**Usage**

Bars.GetSessionNextEnd

**More Information**

The time for the returned value will correlate with the value specified within the MarketEscort. The value itself can be set within the Instrument Escort and can be called up with AgenaScript using the [*Instrument.Exchange*] function.

![][3]

**Example**

**Print**("The next trading session ends at " + Bars.SessionNextEnd);

<span id="_topic_BarsTickCount" class="anchor"></span>Bars.TickCount

**Description**

Bars.TickCount outputs the total numbers of ticks contained within a bar.

More information can be found in [*Properties*] of bars.

**Parameter**

None

**Return Value**

Type int

**Usage**

Bars.TickCount

**More Information**

With [*OnBarUpdate()*][*OnBarUpdate ()*] this property can be used without having to test for a null reference. As soon as the OnBarUpdate() method is called up by AgenaScript, the object will become available.

If this property is used outside of OnBarUpdate(), you should test for a null reference before executing it. You can test using **if** (Bars != **null**)

**Example**

**Print**("The current bar consists of " + Bars.TickCount + " Ticks.");

<span id="_topic_BarsTimeFrame" class="anchor"></span>Bars.TimeFrame

**Description**

Bars.TimeFrame outputs the timeframe object containing information regarding the currently used timeframe.

More information can be found here: [*Properties*]

**Parameter**

None

**Return Value**

Type ITimeFrame

**Usage**

Bars.TimeFrame

**More Information**

For more information about timeframe objects please see [*TimeFrame*].

With [*OnBarUpdate()*][*OnBarUpdate ()*] this property can be used without having to test for a null reference. As soon as the OnBarUpdate() method is called up by AgenaScript, the object will become available.

If this property is used outside of OnBarUpdate(),you should test for a null reference before executing it. You can test using **if** (Bars != **null**)

**Example**

//Usage within a 30 minute chart

TimeFrame tf = (TimeFrame) Bars.TimeFrame;

**Print**(Bars.TimeFrame); // outputs "30 Min"

**Print**(tf.Periodicity); // outputs "Minute"

**Print**(tf.PeriodicityValue); // outputs "30"

<span id="_topic_BarsTotalTicks" class="anchor"></span>Bars.TotalTicks

**Description**

Bars.TotalTicks outputs the total number of ticks from the moment the function is called up.

More information can be found here: [*Properties*].

**Parameter**

None

**Return Value**

Type int

**Usage**

Bars.TotalTicks

**More Information**

The data type int has a positive value range of 2147483647. When you assume 10 ticks per second, there will be no overlaps within 2 trading months with a daily runtime of 24 hours.

With [*OnBarUpdate()*][*OnBarUpdate ()*] this property can be used without having to test for a null reference. As soon as the OnBarUpdate() method is called up by AgenaScript, the object will become available.

If this property is used outside of OnBarUpdate(), you should test for a null reference before executing it. You can test using **if** (Bars != **null**)

**Example**

**Print**("The total amount of ticks is " + Bars.TotalTicks);

*Created with the Personal Edition of HelpNDoc: [*Free PDF documentation generator*]*

<span id="_topic_Datenserien1" class="anchor"></span>**Data Series**

**Description**

Data series are interpreted as freely usable data storage containers for your programs. Additionally, they an integrated component of AgenaTrader that saves the price changes for individual bars. We will be focusing on the latter function here.\
In the following section, the concept of data series will be explained in detail and understandably. All price data for the individual bars are organized and saved within data series.\
The following are available:

-   [*Open*][] [*Opens*]

-   [*High*][] [*Highs*]

-   [*Low*][] [*Lows*]

-   [*Close*][***Close***] [*Closes*]

-   [*Median*][] [*Medians*]

-   [*Typical*][] [*Typicals*]

-   [*Weighted*][] [*Weighteds*]

-   [*Time*][] [*Times*]

-   [*TimeFrame*][] [*TimeFrames*]

-   [*Volume*][] [*Volumes*]

*Created with the Personal Edition of HelpNDoc: [*Full featured EBook editor*][*Free PDF documentation generator*]*

<span id="_topic_Open" class="anchor"></span>Open

**Description**

Open is a [*DataSeries*][*Data series*] of the type [*DataSeries*], in which the historical opening prices are saved.

**Parameter**

BarsAgo Index Value (see [*Bars*])

**Usage**

Open

Open\[**int** barsAgo\]

**More Information**

The returned value is dependent upon the property of [*CalculateOnBarClose*].

**Example**

// Opening price for the current period

**Print**(Time\[0\] + " " + Open\[0\]);

// Opening price for the bars of 5 periods ago

**Print**(Time\[5\] + " " + Open\[5\]);

// Current value for the SMA 14 that is based on the opening prices (rounded)

**Print**("SMA(14) calculated using the opening prices: " + Instrument.**Round2TickSize**(**SMA**(Open, 14)\[0\]));

*Created with the Personal Edition of HelpNDoc: [*Free HTML Help documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_Opens" class="anchor"></span>Opens

**Description**

Opens is an array of data series that contains all open data series.

This array is only useful or meaningful for indicators or strategies that use multiple data from multiple timeframes.\
\
A new entry is entered into the array whenever a new timeframe is added to an indicator or strategy.

With **\[TimeFrameRequirements(("1 Day"), ("1 Week"))\]** the array will contain 3 entries:

Opens\[0\] The open data series of the chart timeframe\
Opens\[1\] The open data series of all bars in a daily timeframe\
Opens\[2\] The open data series of all bars in a weekly timeframe

Opens\[0\]\[0\] is equivalent to Open\[0\].

In addition, please see [*MultiBars*] for more information.

**Parameter**

barsAgo Index value for the individual bars within the data series (see [*Bars*])\
barSeriesIndex Index value for the various timeframes

**Usage**

Opens\[**int** barSeriesIndex\]

Opens\[**int** barSeriesIndex\]\[**int** barsAgo\]

**More Information**

The returned value is dependent upon the property of [*CalculateOnBarClose*].

**Example**

See example: [*Multibars*][*MultiBars*].

*Created with the Personal Edition of HelpNDoc: [*iPhone web sites made easy*]*

<span id="_topic_High" class="anchor"></span>High

**Description**

High is a *[DataSerie][*Data series*]s* of the type [*DataSeries*], in which the historical high prices are saved.

**Parameter**

barsAgo IndexValue (see [*Bars*])

**Usage**

High

High\[**int** barsAgo\]

**More Information**

The returned value is dependent upon the property of [*CalculateOnBarClose*].

**Example**

// High values of the current period

**Print**(Time\[0\] + " " + High\[0\]);

// High values of the bar from 5 periods ago

**Print**(Time\[5\] + " " + High\[5\]);

// the current value for the SMA 14 calculated on the basis of the high prices

**Print**("SMA(14) Calculated using the high prices: " + Instrument.**Round2TickSize**(**SMA**(High, 14)\[0\]));

*Created with the Personal Edition of HelpNDoc: [*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_Highs" class="anchor"></span>Highs

**Description**

Highs is an array of [*DataSeries*][4] that contains all high data series.

This array is only of value for indicators or strategies that use data from multiple timeframes.

A new entry is added to the array whenever a new time unit is added to an indicator or strategy.

With **\[TimeFrameRequirements(("1 Day"), ("1 Week"))\]** the array will contain 3 entries:

Highs\[0\] the high data series of the chart timeframe\
Highs\[1\] the high data series of all bars in a daily timeframe\
Highs\[2\] the high data series of all bars in a weekly timeframe

Highs\[0\]\[0\] is equivalent to High\[0\].

See [*MultiBars*].

**Parameter**

barsAgo Index value for the individual bars within the data series (see [*Bars*])\
barSeriesIndex Index value for the various timeframes

**Usage**

Highs\[**int** barSeriesIndex\]

Highs\[**int** barSeriesIndex\]\[**int** barsAgo\]

**More Information**

The returned value is dependent upon the property of [*CalculateOnBarClose*].

**Example**

Please see examples under [*Multibars*][*MultiBars*].

*Created with the Personal Edition of HelpNDoc: [*Full featured Help generator*]*

<span id="_topic_Low" class="anchor"></span>Low

**Description**

Low is a [*DataSeries*][*Data series*] of the type [*DataSeries*], in which the historical low prices are saved.

**Parameter**

barsAgo IndexValue (see [*Bars*])

**Usage**

Low

Low\[**int** barsAgo\]

**More Information**

The returned value is dependent upon the property of [*CalculateOnBarClose*].

**Example**

// Lowest value of the current period

**Print**(Time\[0\] + " " + Low\[0\]);

// Lowest value of the bar from 5 periods ago

**Print**(Time\[5\] + " " + Low\[5\]);

// The current value for the SMA 14 calculated on the basis of the low prices (smoothed)

**Print**("SMA(14) calculated using the high prices: " + Instrument.**Round2TickSize**(**SMA**(Low, 14)\[0\]));

*Created with the Personal Edition of HelpNDoc: [*Single source CHM, PDF, DOC and HTML Help creation*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_Lows" class="anchor"></span>Lows

**Description**

Lows is an array of [*DataSeries*][4] that contains all [*Low*] data series.

This array is only of value to indicators or strategies that use data from multiple time units.

A new entry is added whenever a new time unit is added to an indicator or strategy.

With **\[TimeFrameRequirements(("1 Day"), ("1 Week"))\]** the array will contain 3 entries:

Lows\[0\] the low data series for the chart timeframe\
Lows\[1\] the low data series for all bars in a daily timeframe\
Lows\[2\] the low data series for all bars in a weekly timeframe

Lows\[0\]\[0\] is equivalent to Low\[0\].

See [*MultiBars*].

**Parameter**

barsAgo Index value for the individual bars within the data series\
barSeriesIndex Index value for the various timeframes

**Usage**

Lows\[**int** barSeriesIndex\]

Lows\[**int** barSeriesIndex\]\[**int** barsAgo\]

**More Information**

The returned value is dependent upon the property [*CalculateOnBarClose*].

**Example**

See example [*Multibars*][*MultiBars*].

*Created with the Personal Edition of HelpNDoc: [*Free EPub and documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_Close" class="anchor"></span>Close

**Description**

Close is a [*DataSeries*][*Data series*] of the type [*DataSeries*], in which the historical closing prices are saved.

**Parameter**

barsAgo Index value (see [*Bars*])

**Usage**

Close

Close\[**int** barsAgo\]

**More Information**

The returned value is dependent upon the property [*CalculateOnBarClose*].

Indicators are usually calculated using the closing prices.

**Example**

// Closing price of the current period

**Print**(Time\[0\] + " " + Close\[0\]);

// Closing price of the bar from 5 periods ago

**Print**(Time\[5\] + " " + Close\[5\]);

// Current value for the SMA 14 based on the closing prices

**Print**("SMA(14) calculated using the closing prices: " + Instrument.**Round2TickSize**(**SMA**(Close, 14)\[0\]));

// Close does not need to be mentioned since it is used by default

**Print**("SMA(14) calculated using the closing prices: " + Instrument.**Round2TickSize**(**SMA**(14)\[0\]));

*Created with the Personal Edition of HelpNDoc: [*iPhone web sites made easy*]*

<span id="_topic_Closes" class="anchor"></span>Closes

**Description**

Closes is an array of [*DataSeries*][4] that contains all [*Low*] data series.

This array is only of importance to indicators or strategies that use data from multiple time units.

A new entry is added to the array whenever a timeframe is added to an indicator or strategy.

With **\[TimeFrameRequirements(("1 Day"), ("1 Week"))\]** the array will contain 3 entries:

Closes\[0\] the close data series of the chart timeframe\
Closes\[1\] the close data series of all bars in a daily timeframe\
Closes\[2\] the close data series of all bars in a weekly timeframe

Closes\[0\]\[0\] is equivalent to Close\[0\].

See [*MultiBars*].

**Parameter**

barsAgo Index value of the individual bars within the data series\
barSeriesIndex Index value for the various timeframes

**Usage**

Closes\[**int** barSeriesIndex\]

Closes\[**int** barSeriesIndex\]\[**int** barsAgo\]

**More Information**

The returned value is dependent upon the property [*CalculateOnBarClose*].

**Example**

See example [*Multibars*][*MultiBars*].

*Created with the Personal Edition of HelpNDoc: [*Create iPhone web-based documentation*][*iPhone web sites made easy*]*

<span id="_topic_Median" class="anchor"></span>Median

**Description**

Median is a [*DataSeries*][*Data series*] of the type [*DataSeries*], in which the historical median values are saved.

The median price of a bar is calculated using (high + low) / 2

See [*Typical*] & [*Weighted*].

**Parameter**

barsAgo Index value (see [*Bars*])

**Usage**

Median

Median\[**int** barsAgo\]

**More Information**

The returned value is dependent upon the property [*CalculateOnBarClose*].

Further information about median, typical und weighted:\
[*http://blog.nobletrading.com/2009/12/median-price-typical-price-weighted.html*]

**Example**

// Median price for the current period

**Print**(Time\[0\] + " " + Median\[0\]);

// Median price of the bar from 5 periods ago

**Print**(Time\[5\] + " " + Median\[5\]);

// Current value for the SMA 14 calculated using the median prices

**Print**("SMA(14) calculated using the median prices: " + Instrument.**Round2TickSize**(**SMA**(Median, 14)\[0\]));

*Created with the Personal Edition of HelpNDoc: [*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_Medians" class="anchor"></span>Medians

**Description**

Medians is an array of [*DataSeries*][4] that contains all [*Median*] data series.

This array is only of value to indicators or strategies that use data from multiple timeframes.

A new entry is added to the array whenever a new time frame is added to an indicator or strategy.

With **\[TimeFrameRequirements(("1 Day"), ("1 Week"))\]** the array will contain 3 entries:

Medians\[0\] the median data series of the chart timeframe\
Medians\[1\] the median data series of all bars in a daily timeframe\
Medians\[2\] the median data series of all bars in a weekly timeframe

Medians\[0\]\[0\] is equivalent to Medians\[0\].

See [*MultiBars*].

**Parameter**

barsAgo Index value for the individual bars within a data series\
barSeriesIndex Index value for the various timeframes

**Usage**

Medians\[**int** barSeriesIndex\]

Medians\[**int** barSeriesIndex\]\[**int** barsAgo\]

**More Information**

The returned value is dependent upon the property [*CalculateOnBarClose*].

**Example**

See example in [*Multibars*][*MultiBars*].

*Created with the Personal Edition of HelpNDoc: [*Free Web Help generator*][*Free PDF documentation generator*]*

<span id="_topic_Typical" class="anchor"></span>Typical

**Description**

Typical is a *DataSeries* of the type [*DataSeries*], in which the historical typical values are saved.

The typical price of a bar is calculated using (high + low + close) / 3.

See [*Median*] and [*Weighted*].

**Parameter**

barsAgo Index value (see [*Bars*])

**Usage**

Typical

Typical\[**int** barsAgo\]

**More Information**

The returned value is dependent upon the property [*CalculateOnBarClose*].

Further information on median, typical and weighted:\
[*http://blog.nobletrading.com/2009/12/median-price-typical-price-weighted.html*]

**Example**

// Typical price for the current period

**Print**(Time\[0\] + " " + Typical\[0\]);

// Typical price of the bar from 5 periods ago

**Print**(Time\[5\] + " " + Typical\[5\]);

// Current value for the SMA 14 calculated using the typical price

**Print**("SMA(14) calculated using the typical price: " + Instrument.**Round2TickSize**(**SMA**(Typical, 14)\[0\]));

*Created with the Personal Edition of HelpNDoc: [*Easy to use tool to create HTML Help files and Help web sites*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_Typicals" class="anchor"></span>Typicals

**Description**

Typicals is an array of *DataSeries* that contains all [*Typical*] data series.

This array is only of value to indicators and strategies that make use of multiple timeframes.

A new entry is added to the array whenever a new timeframe is added to an indicator or strategy.

With **\[TimeFrameRequirements(("1 Day"), ("1 Week"))\]** the array will contain 3 entries:

Typicals\[0\] the typical data series of the chart timeframe\
Typicals\[1\] the typical data series of all bars in a daily timeframe\
Typicals\[2\] the typical data series of all bars in a weekly timeframe

Typicals\[0\]\[0\] is equivalent to Typicals\[0\].

See [*MultiBars*].

**Parameter**

barsAgo Index value of the individual bars within a data series\
barSeriesIndex Index value of the various timeframes

**Usage**

Typicals\[**int** barSeriesIndex\]

Typicals\[**int** barSeriesIndex\]\[**int** barsAgo\]

**More Information**

The returned value is dependent upon the property [*CalculateOnBarClose*].

**Example**

See example [*Multibars*][*MultiBars*].

*Created with the Personal Edition of HelpNDoc: [*Full featured Help generator*]*

<span id="_topic_Weighted" class="anchor"></span>Weighted

**Description**

Weighted is a [*DataSeries*][*Data series*] of the type [*DataSeries*], in which the historical weighted values are saved.

The weighted price of a bar is calculated using the formula (high + low + 2\*close) / 4 and then weighted on the closing price.

See also [*Median*] and [*Typical*].

**Parameter**

barsAgo Index value (see [*Bars*])

**Usage**

Weighted

Weighted\[**int** barsAgo\]

**More Information**

The returned value is dependent upon the property [*CalculateOnBarClose*].

Information regarding median, typical und weighted:\
[*http://blog.nobletrading.com/2009/12/median-price-typical-price-weighted.html*]

**Example**

// Weighted price for the current period

**Print**(Time\[0\] + " " + Weighted\[0\]);

// Weighted price of the bar from 5 periods ago

**Print**(Time\[5\] + " " + Weighted\[5\]);

// Current value for the SMA 14 using the weighted price

**Print**("SMA(14) calculated using the weighted price: " + Instrument.**Round2TickSize**(**SMA**(Weighted, 14)\[0\]));

*Created with the Personal Edition of HelpNDoc: [*Free iPhone documentation generator*][*iPhone web sites made easy*]*

<span id="_topic_Weighteds" class="anchor"></span>Weighteds

**Description**

Weighteds is an array of [*DataSeries*][4] that contains all [*Weighted*] data series.

The array is only of value for indicators and strategies that use data from multiple timeframes.

A new entry is added to the array whenever a new timeframe is added to an indicator or strategy.

With **\[TimeFrameRequirements(("1 Day"), ("1 Week"))\]** the array will contain 3 entries:

Weighteds\[0\] the weighted data series of the chart timeframe\
Weighteds\[1\] the weighted data series of all bars in a daily timeframe\
Weighteds\[2\] the weighted data series of all bars in a weekly timeframe

Weighteds\[0\]\[0\] is equivalent to Weighteds\[0\].

See [*MultiBars*].

**Parameter**

barsAgo Index value of the individual bars within a data series\
barSeriesIndex Index value for the various timeframes

**Usage**

Weighteds\[**int** barSeriesIndex\]

Weighteds\[**int** barSeriesIndex\]\[**int** barsAgo\]

**More Information**

The returned value is dependent upon the property [*CalculateOnBarClose*].

**Example**

See example under [*Multibars*][*MultiBars*].

*Created with the Personal Edition of HelpNDoc: [*Free EPub and documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_Time" class="anchor"></span>Time

**Description**

Time is a [*DataSeries*][*Data series*] of the type [*DateTimeSeries*], in which the timestamps of the individual bars are saved.

**Parameter**

barsAgo Index value (see [*Bars*])

**Usage**

Time

Time\[**int** barsAgo\]

**More Information**

The returned value is dependent upon the property [*CalculateOnBarClose*].

**Example**

// Timestamp of the current period

**Print**(Time\[0\]);

// Timestamp of the bar from 5 periods ago

**Print**(Time\[5\]);

*Created with the Personal Edition of HelpNDoc: [*Full featured Documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_Times" class="anchor"></span>Times

**Description**

Times is an array of *DataSeries* that contains all [*Time*] data series.

This array is only of value to indicators and strategies that make use of multiple timeframes.\
A new entry is added to the array whenever a new timeframe is added to an indicator or strategy.

With **\[TimeFrameRequirements(("1 Day"), ("1 Week"))\]** the array will contain 3 entries:

Times\[0\] the time data series of the chart timeframe\
Times\[1\] the time data series of all bars in a daily timeframe\
Times\[2\] the time data series of all bars in a weekly timeframe

Times\[0\]\[0\] is equivalent to Times\[0\].

See [*MultiBars*].

**Parameter**

barsAgo Index value for the individual bars within a data series\
barSeriesIndex Index value for the various timeframes

**Usage**

Times\[**int** barSeriesIndex\]

Times\[**int** barSeriesIndex\]\[**int** barsAgo\]

**More Information**

The returned value is dependent upon the property [*CalculateOnBarClose*].

**Example**

See example [*Multibars*][*MultiBars*].

*Created with the Personal Edition of HelpNDoc: [*Easily create iPhone documentation*][*iPhone web sites made easy*]*

<span id="_topic_Volume" class="anchor"></span>Volume

**Description**

Volume is a [*DataSeries*][*Data series*] of the type [*DataSeries*], in which the historical volume information is saved.

**Parameter**

barsAgo Index value (see [*Bars*])

**Usage**

Volume

Volume\[**int** barsAgo\]

**More Information**

The returned value is dependent upon the property [*CalculateOnBarClose*].

The value returned by the [*VOL()*] indicator is identical with the volume described here;\
for example, Vol()\[3\] will have the same value as Volume\[3\].

**Example**

// Volume for the current period

**Print**(Time\[0\] + " " + Volume\[0\]);

// Volume of the bar from 5 periods ago

**Print**(Time\[5\] + " " + Volume\[5\]);

// Current value for the SMA 14 calculated using the volume

**Print**("SMA(14) calculated using the volume: " + Instrument.**Round2TickSize**(**SMA**(Volume, 14)\[0\]));

*Created with the Personal Edition of HelpNDoc: [*Single source CHM, PDF, DOC and HTML Help creation*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_Volumes" class="anchor"></span>Volumes

**Description**

Volumes is an array of *DataSeries* that contains all [*Volume*] data series.

This array is only of value for indicators or strategies that use data from multiple timeframes.

A new entry is added to the array whenever a new timeframe is added to an indicator or strategy.

With **\[TimeFrameRequirements(("1 Day"), ("1 Week"))\]** the array will contain 3 entries:

Volumes\[0\] the volume data series of the chart timeframe\
Volumes\[1\] the volume data series of all bars in the daily timeframe\
Volumes\[2\] the volume data series of all bars in the weekly timeframe

Volumes\[0\]\[0\] is equivalent to Volumes\[0\].

See [*MultiBars*].

**Parameter**

barsAgo Index value of the individual bars within a data series

barSeriesIndex Index value of the various timeframes

**Usage**

Volumes\[**int** barSeriesIndex\]

Volumes\[**int** barSeriesIndex\]\[**int** barsAgo\]

**More Information**

The returned value is dependent upon the property [*CalculateOnBarClose*].

**Example**

See example [*Multibars*][*MultiBars*].

*Created with the Personal Edition of HelpNDoc: [*Single source CHM, PDF, DOC and HTML Help creation*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_Instrumente" class="anchor"></span>**Instruments**

The term "instrument" denotes a tradable value such as a stock, ETF, future etc.

An instrument has various properties that can be used in AgenaScripts created by the user:

-   [*Instrument.Compare*]

-   [*Instrument.Currency*]

-   [*Instrument.Digits*]

-   [*Instrument.ETF*]

-   [*Instrument.Exchange*]

-   [*Instrument.Expiry*]

-   [*Instrument.InstrumentType*]

-   [*Instrument.Name*]

-   [*Instrument.PointValue*]

-   [*Instrument.Round2TickSize*]

-   [*Instrument.Symbol*]

-   [*Instrument.TickSize*]

With the **OnBarUpdate**() method you can use any properties you wish without having to test for a null reference.\
As soon as the **OnBarUpdate**() function is called up by AgenaScript, an object will become available. If you wish to use these properties outside of **OnBarUpdate**(), you should first perform a test for null references using **if** (Bars != **null**)

*Created with the Personal Edition of HelpNDoc: [*Full featured EBook editor*][*Free PDF documentation generator*]*

<span id="_topic_InstrumentCompare" class="anchor"></span>**Instrument.Compare**

**Description**

The Instrument.Compare function compares two market prices whilst taking into account the correct number of decimal points. The smallest possible price change is displayed by the value TickSize. This function simplifies the otherwise time-consuming comparison using floating-point operations.

**Parameter**

double value1\
double value2

**Return value**

Type int

1 - Value1 is bigger than value2\
-1 - Value1 is smaller than value2\
0 - Value1 and value2 are equal

**Usage**

Instrument.**Compare**(**double** Value1, **double** Value2)

**More Information**

If the tick size is 0,00001 – as it usually is with FX values – then the following will be displayed:

Compare(2, 1.99999) a 1, meaning 2 is bigger than 1.99999\
Compare(2, 2.000001) a 0, meaning the values are equal\
Compare(2, 1.999999) a 0, meaning the values are equal\
Compare(2, 2.00001) a -1, meaning 2 is smaller than 2.00001

**Example**

**Print**(Instrument.**Compare**(2, 1.999999));

*Created with the Personal Edition of HelpNDoc: [*Easily create Help documents*][*Full featured Help generator*]*

<span id="_topic_InstrumentCurrency" class="anchor"></span>**Instrument.Currency**

**Description**

Instrument.Currency outputs a currency object that contains the corresponding currency in which the instrument is traded.

**Parameter**

None

**Return Value**

A constant of the type “public enum currencies”

**Usage**

Instrument.Currency

**More Information**

The common currencies are: AUD, CAD, EUR, GBP, JPY or USD.

**Example**

**Print**(Instrument.Name + " is traded in " + Instrument.Currency);

*Created with the Personal Edition of HelpNDoc: [*Free CHM Help documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_InstrumentDigits" class="anchor"></span>**Instrument.Digits**

**Description**

Instrument.Digits outputs the number of decimal points in which the market price of the instrument is traded.

**Parameter**

none

**Return Value**

int Digits

**Usage**

Instrument.Digits

**More Information**

Stocks are usually traded to two decimal points. Forex can be traded (depending on the data provider) with 4 or 5 decimal places.

This function is especially useful when formatting the output of various instruments that need rounding. Also see [*TickSize*] and [*Instrument.Round2Ticks*][*Instrument.Round2TickSize*].

More information can be found here: [*Formatting of Numbers*].

**Example**

**Print**("The value of " +Instrument.Name + " is noted with a precision of " + Instrument.Digits +" Decimal points.");

*Created with the Personal Edition of HelpNDoc: [*Free PDF documentation generator*]*

<span id="_topic_InstrumentETF" class="anchor"></span>**Instrument.ETF**

**Description**

Instrument.ETF is used to differentiate between a stock and an ETF. This is necessary since ETFs are considered to be „stocks“ by some exchanges.

**Parameter**

none

**Return Value**

Type bool

**Usage**

Instrument.ETF

**More Information**

What is an ETF?

Wikipedia: [*http://de.wikipedia.org/wiki/Exchange-traded\_fund*]

**Example**

**if** (Instrument.InstrumentType == InstrumentType.Stock)

**if** (Instrument.ETF)

**Print**("The value is an ETF.");

**else**

**Print**("The value is a stock.");

*Created with the Personal Edition of HelpNDoc: [*Create iPhone web-based documentation*][*iPhone web sites made easy*]*

<span id="_topic_InstrumentExchange" class="anchor"></span>**Instrument.Exchange**

**Description**

Instrument.Exchange outputs the description/definition of the current exchange for the current instrument.

**Parameter**

none

**Return Value**

An exchange object of the type “public enum exchanges”

**Usage**

Instrument.Exchange

**More Information**

An overview of various exchanges: [*http://www.boersen-links.de/boersen.htm*]

**Example**

**Print**("The instrument " + Instrument.Name +" is traded on the " + Instrument.Exchange + " exchange.");

*Created with the Personal Edition of HelpNDoc: [*Full featured multi-format Help generator*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_InstrumentExpiry" class="anchor"></span>**Instrument.Expiry**

**Description**

Instrument.Expiry outputs the date (month and year) of the expiry of a financial instrument. Only derivative instruments such as options or futures will have an expiry date.

**Parameter**

None

**Return Value**

Type DateTime

For instruments without an expiry date the returned value is set to DateTime.MaxValue(= 31.12.9999 23.59:59)

**Usage**

Instrument.Expiry

**More Information**

The expiry date (expiry) can also be seen within the Instrument Escort:

![][5]

**Example**

**Print**("The instrument " + Instrument.Name +" will expire on " + Instrument.Expiry);

*Created with the Personal Edition of HelpNDoc: [*Create iPhone web-based documentation*][*iPhone web sites made easy*]*

<span id="_topic_InstrumentInstrumentType" class="anchor"></span>**Instrument.InstrumentType**

**Description**

Instrument.InstrumentType outputs a type object of the trading instrument.

**Parameter**

none

**Return Value**

Object of the type “public enum instrument”

**Usage**

Instrument.InstrumentType

**More Information**

Potential values are: future, stock, index, currency, option, CFD and unknown.

There is no ETF type. ETFs are considered to be of the type “stock” – see [*Instrument.ETF*].

The instrument type can also be viewed within the Instrument Escort:

![][6]

**Example**

**Print**("The instrument " + Instrument.Name + " is of the type " + Instrument.InstrumentType);

*Created with the Personal Edition of HelpNDoc: [*Write EPub books for the iPad*][*Free PDF documentation generator*]*

<span id="_topic_InstrumentName" class="anchor"></span>**Instrument.Name**

**Description**

Instrument.Name outputs the name/description of the trading instrument.

**Parameter**

none

**Return Value**

Type string

**Usage**

Instrument.Name

**More Information**

The instrument name can also be seen within the Instrument Escort:

![][7]

**Example**

**Print**("The currently loaded instrument inside the chart is named " + Instrument.Name);

*Created with the Personal Edition of HelpNDoc: [*Free iPhone documentation generator*][*iPhone web sites made easy*]*

<span id="_topic_InstrumentPointValue" class="anchor"></span>**Instrument.PointValue**

**Description**

Instrument.PointValue outputs the monetary value for a full point movement of the instrument.

**Parameter**

none

**Return Value**

double – point value

**Usage**

Instrument.PointValue

**More Information**

**Example for various point values** (per amount, CFD, futures contract, lot etc.)

Stock: generally 1.00 Euro or 1.00 USD.\
EUR/USD: 100,000 USD\
DAX future: 25.00 Euro

**Tick Value**

The tick value can be calculated by multiplying the point value with the tick size.

For example, the E-mini S&P 500 has a point value of \$50. The tick size equals 0.25. This means that there are 4 ticks in one full point for the E-mini S&P 500.\
Since 50 \* 0.25 = 50/4 this means that the tick value is \$12.50.

The point value can also be viewed within the Instrument Escort:

![][8]

**Example**

**Print**("When " + Instrument.Name + " rises for one full point then this is equal to " + Instrument.PointValue + " " + Instrument.Currency);

*Created with the Personal Edition of HelpNDoc: [*Easily create HTML Help documents*][*Full featured Help generator*]*

<span id="_topic_InstrumentRound2TickSize" class="anchor"></span>**Instrument.Round2TickSize**

**Description**

The function Instrument.Round2TickSize rounds the supplied market price to the smallest value divisible by the tick size of the instrument.

**Parameter**

double – market value

**Return value**

double

**Usage**

Instrument.**Round2TickSize**(**double** MarketPrice)

**More Information**

The number of decimal places to which the price is rounded depends on the instrument.\
If, for example, an instrument is a stock, then the rounding will be performed to 2 decimal places. For a Forex instrument, it may be carried out to 4 or 5 decimal places.

See [*TickSize*] and [*Instrument.Digits*].

Example of professional [*Formatting*][*Formatting of Numbers*].

**Example**

**double** Price = 12.3456789;

**Print**(Price + " rounded for a " + Instrument.Name + " valid value is " + Instrument.**Round2TickSize**(Price));

*Created with the Personal Edition of HelpNDoc: [*Free PDF documentation generator*]*

<span id="_topic_InstrumentSymbol" class="anchor"></span>**Instrument.Symbol**

**Description**

Instrument.Symbol outputs the symbol that identifies the trading instrument within AgenaTrader. Depending on the symbol, the mappings for the various data feed providers and brokers will be managed in different ways.

**Parameter**

none

**Return value**

Type string

**Usage**

Instrument.Symbol

**More Information**

By using symbols, identical stocks being traded on different exchanges can be identified and separated from each other. The symbol BMW.DE is the BMW stock on the XETRA exchange. BMW.CFG is the CFD for the BMW stock.

The instrument symbol can also be viewed within the Instrument Escort:

![][9]

**Example**

**Print**("The instrument currently loaded within the chart has the symbol: " + Instrument.Symbol);

*Created with the Personal Edition of HelpNDoc: [*Full featured Documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_InstrumentTickSize" class="anchor"></span>**Instrument.TickSize**

**Description**

The tick size is the smallest measurable unit that a financial instrument can move. This is usually called 1 tick.

**Parameter**

none

**Return Value**

double

**Usage**

Instrument.TickSize or simply TickSize

**More Information**

The keyword [*TickSize*] is equivalent to Instrument.TickSize. Both information requests will produce the same value and are thus interchangeable.

**Example**

Stock: 0.01\
ES future: 0.25\
EUR/USD: 0.00001

See [*Instrument.PointValue*] and [*Instrument.Digits*].

Examples of professional [*Formatting*][*Formatting of Numbers*].

**Example**

**Print**("The value of " + Instrument.Name + " can change for a minimum of " + Instrument.TickSize + " Tick(s).");

*Created with the Personal Edition of HelpNDoc: [*Easily create Help documents*][*Full featured Help generator*]*

<span id="_topic_Collections" class="anchor"></span>**Collections**

*Created with the Personal Edition of HelpNDoc: [*Free CHM Help documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_DrawObjects" class="anchor"></span>**DrawObjects**

**Description**

DrawObjects is a collection containing all drawing objects within the chart. All manually added drawings as well as those generated by scripts will be added within DrawObjects.\
\
The index for DrawObjects is the explicit name for the drawing object (string tag).

**Usage**

DrawObjects \[string tag\]

**Example\
\
Note:** To be able to use the interface definitions you must use the using method.

**using** AgenaTrader.Plugins;

// Output number of drawing objects within the chart and their tags

**Print**("The chart contains " + DrawObjects.Count + " drawing objects.");

**for each** (IDrawObject draw **in** DrawObjects) **Print**(draw.Tag);

//Draw a black trend line...

**DrawLine**("MyLine", **true**, 10, Close\[10\], 0, Close\[0\], Color.Black, DashStyle.Solid, 3);

// ... and change the color to red

ITrendLine line = (ITrendLine) DrawObjects\["MyLine"\];

**if** (line != **null**) line.Pen.Color = Color.Red;

// Set all lines within the chart to a line strength of 3,

// and lock it so that it cannot be edited or moved

**foreach** (IDrawObject draw **in** DrawObjects)

**if** (draw **is** IVerticalLine)

{

IVerticalLine vline = (IVerticalLine) draw;

vline.Locked = **true**;

vline.Editable = **false**;

vline.Pen.Width = 3;

}

*Created with the Personal Edition of HelpNDoc: [*Easily create iPhone documentation*][*iPhone web sites made easy*]*

<span id="_topic_Input" class="anchor"></span>**Input**

**Description**

Input is a [*DataSeries*] object in which the input data for an indicator or strategy is stored.

If the indicator is used without any explicit instructions for the input data, then the closing price for the current market prices will be used.

When calling up the SMA(20) the smoothing average is calculated on the basis of the closing prices for the current chart price data (this is equivalent to SMA(close,20).

Input\[0\] = Close\[0\].

When calling up the SMA(high, 20) the high price values are loaded and used for the calculation of the smoothing average.

Input\[0\] = High\[0\].

This way you can select which data series should be used for the calculation of the indicator.

**double** d = **RSI**(**SMA**(20), 14, 3)\[0\]; calculates the 14 period RSI using the SMA(20) as the input data series.\
Input\[0\] = SMA(20)\[0\].

**Usage**

Input

Input\[**int** barsAgo\]

**Example**

**Print**("The input data for the indicators are " + Input\[0\]);

*Created with the Personal Edition of HelpNDoc: [*iPhone web sites made easy*]*

<span id="_topic_Lines" class="anchor"></span>**Lines**

**Description**

Lines is a collection that contains all [*Line*] objects of an indicator.

When a line object is added to the indicator using the [*Add()*] method, this line is automatically added to the “lines” collection.

The order of the add commands determines how these lines are sorted. The first information request of Add() will create Lines\[0\], the next information request will be Lines\[1\] etc.

See [*Plots*].

**Usage**

Lines\[**int** index\]

**Example**

// Add "using System.Drawing.Drawing2D;" for DashStyle

**protected** override void **Initialize**()

{

**Add**(**new Line**(Color.Blue, 70, "Upper")); // saves into Lines\[0\]

**Add**(**new Line**(Color.Blue, 30, "Lower")); // saves into Lines\[1\]

}

**protected** override void **OnBarUpdate**()

{

// When the RSI is above 70, properties of the lines will be changed

**if** (**RSI**(14 ,3) &gt;= 70)

{

> Lines\[0\].Width = 3;
>
> Lines\[0\].Color = Color.Red;
>
> Lines\[0\].DashStyle = DashStyle.Dot;

}

**else**

{

> Lines\[0\].Width = 1;
>
> Lines\[0\].Color = Color.Blue;
>
> Lines\[0\].DashStyle = DashStyle.Solid;

}

}

*Created with the Personal Edition of HelpNDoc: [*Free CHM Help documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_PlotColors" class="anchor"></span>**PlotColors**

**Description**

PlotColors is a collection that contains all color series of all plot objects.

When a plot is added using the [*Add()*] method it automatically creates a color series object and is added to the PlotColors collection.

The order of the add commands determines how the plot colors are sorted. The first information request of Add() will create PlotColors\[0\], the following information request will create PlotColors\[1\] etc.

**Usage**

PlotColors\[**int** PlotIndex\]\[**int** barsAgo\]

**More Information**

More information regarding the collection class:\
[*http://msdn.microsoft.com/en-us/library/ybcx56wz%28v=vs.80%29.aspx*]

**Example**

**using** System;

**using** System.Collections.Generic;

**using** System.ComponentModel;

**using** System.Drawing;

**using** AgenaTrader.API;

**namespace** AgenaTrader.UserCode

{

\[**Description**("PlotColor Demo")\]

**public** class PlotColorsDemo : UserIndicator

{

**public** DataSeries SMA20 { get {return Values\[0\];} }

**public** DataSeries SMA50 { get {return Values\[1\];} }

**public** DataSeries SMA100 { get {return Values\[2\];} }

**private** Pen pen;

**protected** override void **Initialize**()

{

// Set line strength (width) to 4

pen = **new Pen**(Color.Empty, 4);

// Add three plots with the defined line strength to the chart

**Add**(**new Plot**(pen, PlotStyle.Line, "SMA20" )); //attached to PlotColors\[0\]

**Add**(**new Plot**(pen, PlotStyle.Line, "SMA50" )); //attached to PlotColors\[1\]

**Add**(**new Plot**(pen, PlotStyle.Line, "SMA100")); //attached to PlotColors\[2\]

Overlay = **true**;

}

**protected** override void **OnBarUpdate**()

{

// Add values to the three plots

SMA20.**Set** (**SMA**(20) \[0\]);

SMA50.**Set** (**SMA**(50) \[0\]);

SMA100.**Set**(**SMA**(100)\[0\]);

// Change colors depending on the trend

**if** (**Rising**(Close))

{

PlotColors\[0\]\[0\] = Color.LightGreen;

PlotColors\[1\]\[0\] = Color.Green;

PlotColors\[2\]\[0\] = Color.DarkGreen;

}

**else if** (**Falling**(Close))

{

PlotColors\[0\]\[0\] = Color.LightSalmon;

PlotColors\[1\]\[0\] = Color.Red;

PlotColors\[2\]\[0\] = Color.DarkRed;

}

**else**

{

PlotColors\[0\]\[0\] = Color.LightGray;

PlotColors\[1\]\[0\] = Color.Gray;

PlotColors\[2\]\[0\] = Color.DarkGray;

}

}

}

}

*Created with the Personal Edition of HelpNDoc: [*Easy EPub and documentation editor*][*Free PDF documentation generator*]*

<span id="_topic_Plots" class="anchor"></span>**Plots**

**Description**

Plots is a collection that contains the plot objects of an indicator.

When a plot object is added to an indicator using the Add() method, it is also automatically added to the “plots” collection.

The order of the add commands determines how the plots are sorted. The first Add() information request will create Plots\[0\], the following information request will create Plots\[1\] etc.

See [*Lines*].

**Usage**

Plots\[**int** index\]

**Example**

**protected** override void **Initialize**()

{

**Add**(**new Plot**(Color.Blue, "MySMA 20")); // saved to Plots\[0\]

}

**protected** override void **OnBarUpdate**()

{

Value.**Set**(**SMA**(20)\[0\]);

// If the market price is above the SMA colorize it green, otherwise red

**if** (Close\[0\] &gt; **SMA**(20)\[0\])

> Plots\[0\].PlotColor = Color.Green;

**else **

> Plots\[0\].PlotColor = Color.Red;

}

*Created with the Personal Edition of HelpNDoc: [*Easily create CHM Help documents*][*Full featured Help generator*]*

<span id="_topic_Values" class="anchor"></span>**Values**

**Description**

Values is a collection that contains the data series objects of an indicator.

When a plot is added to an indicator using the Add() method, a value object is automatically created and added to the “values” collection.

The order of the add commands determines how the values are sorted. The first information request will create Values\[0\], the next information request will create Values\[1\] etc.

**Value** is always identical to Values\[0\].

**Usage**

Values\[**int** index\]

Values\[**int** index\]\[**int** barsAgo\]

**More Information**

The methods known for a collection, Set() Reset() and Count(), are applicable for values.

Information on the class collection:\
[*http://msdn.microsoft.com/en-us/library/ybcx56wz%28v=vs.80%29.aspx*]

**Example**

// Check the second indicator value of one bar ago and set the value of the current indicator value based on it.

**if** (Values\[1\]\[1\] &lt; High\[0\] - Low\[0\])

Value.**Set**(High\[0\] - Low\[0\]);

**else**

Value.**Set**(High\[0\] - Close\[0\]);

*Created with the Personal Edition of HelpNDoc: [*Write EPub books for the iPad*][*Free PDF documentation generator*]*

<span id="_topic_Multibars" class="anchor"></span>**Multibars**

**Description**

An indicator or a strategy will always have the same underlying timeframe-units as those units being displayed within the chart. The values of an SMA(14) indicator displayed in a 5 minute chart will be calculated based on the last fourteen 5 minute bars. A daily chart, on the other hand, would use the closing prices of the past 14 days in order to calculate this value.\
\
The same method applies for your self-programmed indicators. A 5 minute chart will call up the [*OnBarUpdate()*][*OnBarUpdate ()*] for each 5 minute bar.\
\
If you want your self-created indicator to use a different timeframe, this is possible using multibars.

**Example**

**using** System;

**using** System.Collections.Generic;

**using** System.ComponentModel;

**using** System.Drawing;

**using** System.Linq;

**using** System.Xml;

**using** System.Xml.Serialization;

**using** AgenaTrader.API;

**using** AgenaTrader.Custom;

**using** AgenaTrader.Plugins;

**using** AgenaTrader.Helper;

**namespace** AgenaTrader.UserCode

{

\[**Description**("Multibar Demo")\]

// The indicator requires daily and weekly data

\[**TimeFrameRequirements**("1 Day", "1 Week")\]

**public** class MultiBarDemo : UserIndicator

{

**protected** override void **InitRequirements**()

> {
>
> **Add**(DatafeedHistoryPeriodicity.Day, 1);
>
> **Add**(DatafeedHistoryPeriodicity.Week, 1);
>
> }
>
> **protected** override void **Initialize**()

{

CalculateOnBarClose = **true**;

}

**protected** override void **OnBarUpdate**()

{

// The current value for the SMA 14 in a daily timeframe

**Print**(**SMA**(Closes\[1\], 14)\[0\]);

// Current value for the SMA 14 in a weekly timeframe

**Print**(**SMA**(Closes\[2\], 14)\[0\]);

}

}

}

**Additional Notes**

When using additional timeframes, a further entry with the respective data series for the bars of the new timeframe will be added to the arrays [*Opens*], [*Highs*], [*Lows*], [*Closes*], [*Medians*], [*Typicals*], [*Weighteds*], [*Times*] and [*Volumes*]. The indexing will occur in the order of the addition of the new timeframes.\
\
Closes\[0\]\[0\] is equivalent to Close\[0\].\
Closes\[1\]\[0\] equals the current closing price for the daily data series\
Closes\[2\]\[0\] equals the current closing price for the weekly data series

"Closes" is, of course, interchangeable with Opens, Highs, Lows etc.

See [*CurrentBars*], [*BarsInProgress*], [*TimeFrames*], [*TimeFrameRequirements*].

Additional syntax methods are available for multibars:

// Declare the variable TF\_DAY and define it

**private** static readonly TimeFrame TF\_Day = **new TimeFrame**(DatafeedHistoryPeriodicity.Day, 1);

**private** static readonly TimeFrame TF\_Week = **new TimeFrame**(DatafeedHistoryPeriodicity.Week, 1);

// The following instruction is identical to double d = Closes\[1\]\[0\];

**double** d = MultiBars.**GetBarsItem**(TF\_Day).Close\[0\];

// The following instruction is identical to double w = Closes\[2\]\[0\];

**double** w = MultiBars.**GetBarsItem**(TF\_Week).Close\[0\];

*Created with the Personal Edition of HelpNDoc: [*Full featured multi-format Help generator*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_CurrentBars1" class="anchor"></span>**CurrentBars**

**Description**

CurrentBars is an array of int values that contains the number of *[CurrentBar]s* for each bar.

This array is only of value for indicators or strategies that use data from multiple timeframes.

A new entry is added to the array whenever a new timeframe is added to an indicator or strategy.

With **\[TimeFrameRequirements(("1 Day"), ("1 Week"))\]** the array will contain 3 entries:

CurrentBars\[0\] Current bar for the primary data series (chart timeframe)\
CurrentBars\[1\] Current bar for the daily bars\
CurrentBars\[2\] Current bar for the weekly bars

CurrentBars\[0\] is equivalent to [*CurrentBar*][CurrentBar].

Also see [*MultiBars*].

**Parameter**

barSeriesIndex Index value for the various timeframes

**Usage**

CurrentBars\[**int** barSeriesIndex\]

**Example**

//Ensure that a minimum of 20 bars is loaded

**for** (**int** i=0; i&lt;CurrentBars.Count; i++)

**if** (CurrentBars\[i\] &lt; 20) return;

*Created with the Personal Edition of HelpNDoc: [*Easy to use tool to create HTML Help files and Help web sites*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_BarsInProgress" class="anchor"></span>**BarsInProgress**

**Description**

Within a multibars script, multiple bars objects are available. The OnBarUpdate() method\
will therefore also be called up for every bar within your script. In order to include/exclude events of specific data series, you can use the BarsInProgress method.

BarsInProgress is only of value for indicators or strategies that use data from multiple timeframes.\
\
With **\[TimeFrameRequirements("1 Day", "1 Week")\]** two timeframes will be added to the primary chart timeframe.

If OnBarUpdate() is called up by the primary data series, then BarsInProgress will equal zero. If OnBarUpdate() is called up by the daily bars, then BarsInProgress will equal 1. Weekly bars will have a value of 2.

See [*Multibars*][*MultiBars*] and [*CurrentBars*].

**Parameter**

none

**Usage**

BarsInProgress

**More Information**

Within a script that only works with primary timeframes, the value will always equal zero.

**Example**

// To demonstrate the methodology

// set CalculateOnBarClose=false

**Print**(Time\[0\] + " " + BarsInProgress);

// Calculate only for the chart timeframe

**protected** override void **OnBarUpdate**()

{

**if** (BarsInProgress &gt; 0) return;

// Logic for the primary data series

}

*Created with the Personal Edition of HelpNDoc: [*iPhone web sites made easy*]*

<span id="_topic_TimeFrames" class="anchor"></span>**TimeFrames**

**Description**

TimeFrames is an array of timeframe objects that contains a timeframe object for each individual bar object.

This array is only of value for indicators or strategies that use data from multiple timeframes.

A new entry is added to the array whenever a new timeframe is added to an indicator or strategy.

With **\[TimeFrameRequirements(("1 Day"), ("1 Week"))\]** the array will contain 3 entries:

TimeFrames \[0\] Timeframe of the primary data series (chart timeframe)\
TimeFrames \[1\] **Print**(TimeFrames\[1\]); // returns "1 Day"\
TimeFrames \[2\] **Print**(TimeFrames\[2\]); // returns "1 Week"

TimeFrames \[0\] is equivalent to [*TimeFrame*].

See [*MultiBars*].

**Parameter**

barSeriesIndex Index value for the various timeframes

**Usage**

TimeFrames \[**int** barSeriesIndex\]

**Example**

**if** (BarsInProgress == 0 && CurrentBar == 0)

**for** (**int** i = BarsArray.Count-1; i &gt;= 0; i--)

> **Print**("The Indicator " + **this**.Name + " uses Bars of the Timeframe " + TimeFrames\[i\]);

*Created with the Personal Edition of HelpNDoc: [*Easy CHM and documentation editor*][*Free PDF documentation generator*]*

<span id="_topic_EreignisseEvents" class="anchor"></span> **Events**

AgenaTrader is an [*event-oriented*] application by definition.

Programming in AgenaTrader using the various application programming interface ([*API*]) methods is based initially on the [*Overwriting*] of routines predefined for event handling.

The following methods can be used and therefore overwritten:

-   [*OnBarUpdate()*][*OnBarUpdate ()*]

-   [*OnExecution()*]

-   [*OnMarketData()*]

-   [*OnMarketDepth()*]

-   [*OnOrderUpdate()*]

-   [*OnStartUp()*]

-   [*OnTermination()*]

*Created with the Personal Edition of HelpNDoc: [*Easily create EBooks*][*Full featured Help generator*]*

<span id="_topic_OnBarUpdate" class="anchor"></span>**OnBarUpdate()**

**Description**

The OnBarUpdate() method is called up whenever a bar changes; depending on the variables of [*CalculateOnBarClose*], this will happen upon every incoming tick or when the bar has completed/closed.\
\
OnBarUpdate is the most important method and also, in most cases, contains the largest chunk of code for your self-created indicators or strategies.\
\
The editing begins with the oldest bar and goes up to the newest bar within the chart. The oldest bar has the number 0. The indexing and numbering will continue to happen; in order to obtain the numbering of the bars you can use the current bar variable. You can see an example illustrating this below.

Caution: the numbering/indexing is different from the bar index – see [*Bars*].

More information can be found here: [*Events*].

**Parameter**

none

**Return Value**

none

**Usage**

**protected** override void **OnBarUpdate**()

**Example**

**protected** override void **OnBarUpdate**()

{

**Print**("Calling of OnBarUpdate for the bar number " + CurrentBar + " from " +Time\[0\]);

}

*Created with the Personal Edition of HelpNDoc: [*Free EPub and documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_OnExecution" class="anchor"></span>**OnExecution()**

**Description**

The OnExecution() method is called up when an order is executed (filled).\
\
The status of a strategy can be changed by a strategy-managed order. This status change can be initiated by the changing of a volume, price or the status of the exchange (from “working” to “filled”). It is guaranteed that this method will be called up in the correct order for all events.

OnExecution() will always be executed AFTER [*OnOrderUpdate()*].

More information can be found here: [*Events*]

**Parameter**

An execution object of the type IExecution

**Return Value**

none

**Usage**

**protected** override void **OnExecution**(IExecution execution)

**Example**

**private** IOrder entryOrder = **null**;

**protected** override void **OnBarUpdate**()

{

**if** (entryOrder == **null** && Close\[0\] &gt; Open\[0\])

entryOrder = **EnterLong**();

}

**protected** override void **OnExecution**(IExecution execution)

{

// Example 1

**if** (entryOrder != **null** && execution.Order == entryOrder)

**Print**(execution.**ToString**());

// Example 2

**if** (execution.Order != **null** && execution.Order.OrderState == OrderState.Filled)

**Print**(execution.**ToString**());

}

*Created with the Personal Edition of HelpNDoc: [*Full featured Help generator*]*

<span id="_topic_OnMarketData" class="anchor"></span>**OnMarketData()**

**Description**

The OnMarketData() method is called up when a change in level 1 data has occurred, meaning whenever there is a change in the bid price, ask price, bid volume, or ask volume, and of course in the last price after a real turnover has occurred.\
\
In a multibar indicator, the BarsInProgress method identifies the data series that was used for an information request for OnMarketData().\
\
OnMarketData() will not be called up for historical data.\
More information can be found here: [*Events*].

**Notes regarding data from Yahoo (YFeed)**

The field "LastPrice" equals – as usual – either the bid price or the ask price, depending on the last revenue turnover.

The „MarketDataType“ field always equals the „last" value

The fields "Volume", "BidSize" and "AskSize" are always 0.

**Usage**

protected **override void** OnMarketData**(MarketDataEventArgs e)**

**Return Value**

none

**Parameter**

[*MarketDataEventArgs*] e

**Example**

**protected** override void **OnMarketData**(MarketDataEventArgs e)

{

**Print**("AskPrice "+e.AskPrice);

**Print**("AskSize "+e.AskSize);

**Print**("BidPrice "+e.BidPrice);

**Print**("BidSize "+e.BidSize);

**Print**("Instrument "+e.Instrument);

**Print**("LastPrice "+e.LastPrice);

**Print**("MarketDataType "+e.MarketDataType);

**Print**("Price "+e.Price);

**Print**("Time "+e.Time);

**Print**("Volume "+e.Volume);

}

*Created with the Personal Edition of HelpNDoc: [*Easy EBook and documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_OnMarketDepth" class="anchor"></span>**OnMarketDepth()**

**Description**

The OnMarketDepth() method is called up whenever there is a change in the level 2 data (market depth).\
\
In a multibar indicator, the BarsInProgress method identifies the data series for which the OnMarketDepth() method is called up.\
\
OnMarketDepth is not called up for historical data.

More information can be found here: [*Events*].

**Usage**

protected **override void** OnMarketDepth**(MarketDepthEventArgs e)**

**Return Value**

none

**Parameter**

[*MarketDepthEventArgs*] e

**Example**

**protected** override void **OnMarketDepth**(MarketDepthEventArgs e)

{

// Output for the current ask price

**if** (e.MarketDataType == MarketDataType.Ask && e.Operation == Operation.Update)

**Print**("The current ask is + e.Price + " " + e.Volume);

}

*Created with the Personal Edition of HelpNDoc: [*Easily create Web Help sites*][*Full featured Help generator*]*

<span id="_topic_OnOrderUpdate" class="anchor"></span>**OnOrderUpdate()**

**Description**

The OnOrderUpdate() method is called up whenever the status is changed by a strategy-managed order.\
A status change can therefore occur due to a change in the volume, price or status of the exchange (from “working” to “filled”). It is guaranteed that this method will be called up in the correct order for the relevant events.

**Important note:\
**If a strategy is to be controlled by order executions, we highly recommend that you use OnExecution() instead of OnOrderUpdate(). Otherwise there may be problems with partial executions.

More information can be found here: [*Events*].

**Parameter**

An order object of the type IOrder

**Return Value**

None

**Usage**

**protected** override void **OnOrderUpdate**(IOrder order)

**Example**

**private** IOrder entryOrder = **null**;

**protected** override void **OnBarUpdate**()

{

**if** (entryOrder == **null** && Close\[0\] &gt; Open\[0\])

entryOrder = **EnterLong**();

}

**protected** override void **OnOrderUpdate**(IOrder order)

{

**if** (entryOrder != **null** && entryOrder == order)

{

**Print**(order.**ToString**());

**if** (order.OrderState == OrderState.Cancelled)

{

**Print**("Order was canceled.");

entryOrder = **null**;

}

}

}

*Created with the Personal Edition of HelpNDoc: [*Single source CHM, PDF, DOC and HTML Help creation*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_OnStartUp" class="anchor"></span>**OnStartUp()**

**Description**

The OnStartUp() method can be overridden to initialize your own variables, perform license checks or call up user forms etc.\
OnStartUp() is only called up once at the beginning of the script, after [*Initialize()*] and before [*OnBarUpdate()*][*OnBarUpdate ()*] are called up.

See [*OnTermination()*].

More information can be found here: [*Events*] .

**Parameter**

none

**Return Value**

none

**Usage**

**protected** override void **OnStartUp**()

**Example**

**private** myForm Window;

**protected** override void **OnStartUp**()

{

**if** (ChartControl != **null**)

{

Window = **new myForm**();

Window.**Show**();

}

}

*Created with the Personal Edition of HelpNDoc: [*Free HTML Help documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_OnTermination" class="anchor"></span>**OnTermination()**

**Description**

The OnTermination() method can also be overridden in order to once again free up all the resources used in the script.

See [*Initialize()*] and [*OnStartUp()*].

More information can be found here: [*Events*].

**Parameter**

none

**Return Value**

none

**Usage**

**protected** override void **OnTermination**()

**More Information**

Caution:\
Please do not override the Dispose() method since this can only be used much later within the script. This would lead to resources being used and held for an extended period and thus potentially causing unexpected consequences for the entire application.

**Example**

**protected** override void **OnTermination**()

{

**if** (Window != **null**)

{

Window.**Dispose**();

Window = **null**;

}

}

*\
\
\
\
Created with the Personal Edition of HelpNDoc: [*Single source CHM, PDF, DOC and HTML Help creation*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

*\
\
***Strategy Programming**

<span id="_topic_Account" class="anchor"></span>**Account**

**Description**

Account is an object containing information about the account with which the current strategy is working.

The individual properties are:

-   **Account.AccountConnection\
    **Name for the broker connection used (the name assigned under the account connection submenu)

-   **Account.AccountType\
    **Type of account (live account, simulated account etc.)

-   **Account.Broker\
    **Name/definition for the broker

-   **Account.BuyingPower\
    **The current account equity in consideration of the leverage provided by the broker (IB leverages your account equity by a factor of 4, meaning that with 10000€ your buying power is equal to 40000€)

-   **Account.CashValue\
    **Amount (double)

-   **Account.Currency\
    **Currency in which the account is held

-   **Account.ExcessEquity\
    **Excess

-   **Account.InitialMargin\
    **Initial margin (depends on the broker, double)

-   **Account.InstrumentType\
    **Type of trading instrument (type AgenaTrader.Plugins.InstrumentTypes)

-   **Account.IsDemo\
    **True, if the account is a demo account

-   **Account.Name\
    **Name of the account (should be identical to Account.AccountConnection)

-   **Account.OverNightMargin\
    **Overnight margin (depends on the broker, double)

-   **Account.RealizedProfitLoss\
    **Realized profits and losses (double)

**Example**

**Print**("AccountConnection " + Account.AccountConnection);

**Print**("AccountType " + Account.AccountType);

**Print**("Broker " + Account.Broker);

**Print**("BuyingPower " + Account.BuyingPower);

**Print**("CashValue " + Account.CashValue);

**Print**("Currency " + Account.Currency);

**Print**("ExcessEquity " + Account.ExcessEquity);

**Print**("InitialMargin " + Account.InitialMargin);

**Print**("InstrumentTypes " + Account.InstrumentTypes);

**Print**("IsDemo " + Account.IsDemo);

**Print**("Name " + Account.Name);

**Print**("OverNightMargin " + Account.OverNightMargin);

**Print**("RealizedProfitLoss " + Account.RealizedProfitLoss);

*Created with the Personal Edition of HelpNDoc: [*Full featured Documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_BarsSinceEntry" class="anchor"></span>**BarsSinceEntry()**

**Description**

The property “BarsSinceEntry” returns the number of bars that have occurred since the last entry into the market.

**Usage**

**BarsSinceEntry**()

**BarsSinceEntry**(string signalName)

For multi-bar strategies

**BarsSinceEntry**(**int** barsInProgressIndex, string signalName, **int** entriesAgo)

**Parameter**

  --------------------- -----------------------------------------------------------------------------------------------------------
  signalName            The signal name (string) that has been used to clearly label the entry within an entry method.

  barsInProgressIndex   For [*Multibar*][*MultiBars*] strategies.
                        
                        Index for the data series for which the entry order was executed.\
                        See *[BarsInProgress][*BarsInProgress*].*

  entriesAgo            Number of entries in the past. A zero indicates the number of bars that have formed after the last entry.
  --------------------- -----------------------------------------------------------------------------------------------------------

**Example**

**Print**("The last entry was " + **BarsSinceEntry**() + " bars ago.");

*Created with the Personal Edition of HelpNDoc: [*Easily create CHM Help documents*][*Full featured Help generator*]*

<span id="_topic_BarsSinceExit" class="anchor"></span>**BarsSinceExit()**

**Description**

The property “BarsSinceExit” outputs the number of bars that have occurred since the last exit from the market.

**Usage**

**BarsSinceExit**()

**BarsSinceExit**(string signalName)

For multi-bar strategies

**BarsSinceExit**(**int** barsInProgressIndex, string signalName, **int** exitsAgo)

**Parameter**

  --------------------- ---------------------------------------------------------------------------------------------------------------------------
  signalName            The signal name (string) that has been used to clearly label the exit within the exit method.

  barsInProgressIndex   For [*Multibar*][*MultiBars*] strategies.
                        
                        Index of the data series for which the exit order has been executed.\
                        See [*BarsInProgress*].

  exitsAgo              Number of exits that have occurred in the past. A zero indicates the number of bars that have formed after the last exit.
  --------------------- ---------------------------------------------------------------------------------------------------------------------------

**Example**

**Print**("The last exit was " + **BarsSinceExit**() + " bars ago.");

*Created with the Personal Edition of HelpNDoc: [*Free HTML Help documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_CancelOrder" class="anchor"></span>**CancelOrder()**

**Description**

Cancel order deletes an order.

A cancel request is sent to the broker. There is no guarantee that the order will actually be deleted there. It may occur that the order receives a partial execution before it is deleted. Therefore we recommend that you check the status of the order with [*OnOrderUpdate()*].

**Usage**

**CancelOrder**(IOrder order)

**Parameter**

An order object of the type “IOrder”

**Example**

**private** IOrder myEntryOrder = **null**;

**private int** barNumberOfOrder = 0;

**protected** override void **OnBarUpdate**()

{

// Place an entry stop at the high of the current bar

**if** (myEntryOrder == **null**)

{

myEntryOrder = **EnterLongStop**(High\[0\], "stop long");

barNumberOfOrder = CurrentBar;

}

// Delete the order after 3 bars

**if** (Position.MarketPosition == PositionType.Flat &&

CurrentBar &gt; barNumberOfOrder + 3)

**CancelOrder**(myEntryOrder);

}

*Created with the Personal Edition of HelpNDoc: [*Easily create PDF Help documents*][*Full featured Help generator*]*

<span id="_topic_ChangeOrder" class="anchor"></span>**ChangeOrder()**

**Description**

Change order, as the name suggests, changes an order.

**Usage**

**ChangeOrder**(IOrder iOrder, **int** quantity, **double** limitPrice, **double** stopPrice)

**Parameter**

  ------------ ------------------------------------------
  iOrder       An order object of the type “IOrder”
  quantity     Number of units to be ordered
  limitPrice   Limit price. Set this to 0 if not needed
  stopPrice    Stop price. Set this to 0 if not needed
  ------------ ------------------------------------------

**Example**

**private** IOrder stopOrder = **null**;

**protected** override void **OnBarUpdate**()

{

// If the position is profiting by 4 ticks then set the stop to break-even

**if** (stopOrder != **null** && stopOrder.StopPrice &lt; Position.AvgPrice && Close\[0\] &gt;= Position.AvgPrice + 4 \* TickSize)

**ChangeOrder**(stopOrder, stopOrder.Quantity, stopOrder.LimitPrice, Position.AvgPrice);

}

*Created with the Personal Edition of HelpNDoc: [*Easily create CHM Help documents*][*Full featured Help generator*]*

<span id="_topic_DataSeriesConfigurable" class="anchor"></span>**DataSeriesConfigurable**

*Created with the Personal Edition of HelpNDoc: [*Full featured EBook editor*][*Free PDF documentation generator*]*

<span id="_topic_DefaultQuantity" class="anchor"></span>**DefaultQuantity**

**Description**

Change order changes an order.

Default quantity defines the amount to be used in a strategy. Default quantity is set within the [*Initialize()*] method.

**Usage**

**ChangeOrder**(IOrder iOrder, **int** quantity, **double** limitPrice, **double** stopPrice)

**Parameter**

an int value containing the amount (stocks, contracts etc.)

**Example**

**protected** override void **Initialize**()

{

DefaultQuantity = 100;

}

*Created with the Personal Edition of HelpNDoc: [*Easy EBook and documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_EnterLong" class="anchor"></span>**EnterLong()**

**Description**

Enter long creates a long position (buy).

If a signature not containing an amount is used, the amount is set by the [*DefaultQuantity*] or taken from the strategy dialog window.

See [*EnterLongLimit()*], [*EnterLongStop()*], [*EnterLongStopLimit()*].

**Usage**

**EnterLong**()

**EnterLong**(string signalName)

**EnterLong**(**int** quantity)

**EnterLong**(**int** quantity, string signalName)

For multi-bar strategies

**EnterLong**(**int** barsInProgressIndex, **int** quantity, string signalName)

**Parameter**

  --------------------- -----------------------------------------------------------------------------------------------
  signalName            An unambiguous name

  quantity              The amount of stocks/contracts

  barsInProgressIndex   For [*Multibar*][*MultiBars*] strategies.\
                        Index of the data series for which the entry order is to be executed. See [*BarsInProgress*].
  --------------------- -----------------------------------------------------------------------------------------------

**Return Value**

an order object of the type “IOrder”

**Example**

// Enter a long position if the last entry is 10 bars in the past

// and if two SMAs have crossed

**if** (**BarsSinceEntry**() &gt; 10 && **CrossAbove**(**SMA**(10), **SMA**(20), 1))

**EnterLong**("SMA cross entry");

*Created with the Personal Edition of HelpNDoc: [*Free EPub and documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_EnterLongLimit" class="anchor"></span>**EnterLongLimit()**

**Description**

Enter long limit creates a limit order for entering a long position (buy).

If a signature not containing a set amount is used, the amount is set by the [*DefaultQuantity*] or taken from the strategy dialog window.

See [*EnterLong()*], [*EnterLongStop()*], [*EnterLongStopLimit()*].

**Usage**

**EnterLongLimit**(**double** limitPrice)

**EnterLongLimit**(**double** limitPrice, string signalName)

**EnterLongLimit**(**int** quantity, **double** limitPrice)

**EnterLongLimit**(**int** quantity, **double** limitPrice, string signalName)

For Multibar-Strategies

**EnterLongLimit**(**int** barsInProgressIndex, **bool** liveUntilCancelled, **int** quantity, **double** limitPrice, string signalName)

**Parameter**

  --------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------
  signalName            An unambiguous name

  quantity              Amount of stocks/contracts/etc.

  barsInProgressIndex   For [*Multibar*][*MultiBars*] strategies.\
                        Index of the data series for which the entry order is to be executed.\
                        See [*BarsInProgress*].

  limitPrice            A double value for the limit price

  liveUntilCancelled    The order will not be deleted at the end of the bar, but will remain active until removed with [*CancelOrder*] or until it reaches its expiry (see [*TimeInForce*]).
  --------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------

**Return Value**

an order object of the type “IOrder”

**Example**

// A long position is placed if the last entry was 10 bars ago and the two SMAs have crossed each other

**if** (**BarsSinceEntry**() &gt; 10 && **CrossAbove**(**SMA**(10), **SMA**(20), 1))

**EnterLongLimit**("SMA cross entry");

*Created with the Personal Edition of HelpNDoc: [*Free help authoring tool*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_EnterLongStop" class="anchor"></span>**EnterLongStop()**

**Description**

Enter long stop creates a limit order for entering a long position (buy).

If a signature not containing a set amount is used, the amount is set by the [*DefaultQuantity*] or taken from the strategy dialog window.

See [*EnterLong()*], [*EnterLongLimit()*], [*EnterLongStopLimit()*].

**Usage**

**EnterLongStop**(**double** stopPrice)

**EnterLongStop**(**double** stopPrice, string signalName)

**EnterLongStop**(**int** quantity, **double** stopPrice)

**EnterLongStop**(**int** quantity, **double** stopPrice, string signalName)

For multi-bar strategies

**EnterLongStop**(**int** barsInProgressIndex, **bool** liveUntilCancelled, **int** quantity, **double** stopPrice, string signalName)

**Parameter**

  --------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  signalName            An unambiguous name

  quantity              Amount of stocks or contracts etc.

  barsInProgressIndex   For [*Multibar*][*MultiBars*] strategies\
                        Index of the data series for which an entry order is to be executed.\
                        See [*BarsInProgress*].

  stopPrice             A double value for the stop price

  liveUntilCancelled    The order will not be deleted at the end of the bar, but will remain active until deleted with the [*CancelOrder*] command or until it reaches its expiry time (see [*TimeInForce*]).
  --------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**Return Value**

an order object of the type “IOrder”

**Example**

**private** IOrder myEntryOrder = **null**;

// Place an entry order at the high of the current bar

**if** (myEntryOrder == **null**)

myEntryOrder = **EnterLongStop**(High\[0\], "Stop Long");

*Created with the Personal Edition of HelpNDoc: [*Easily create PDF Help documents*][*Full featured Help generator*]*

<span id="_topic_EnterLongStopLimit" class="anchor"></span>**EnterLongStopLimit()**

**Description**

Enter long stop limit creates a buy stop limit order for entering a long position.

If a signature not containing a set amount is used, the amount is set by the [*DefaultQuantity*] or taken from the strategy dialog window.

See [*EnterLong()*], [*EnterLongLimit()*], [*EnterLongStop()*].

**Usage**

**EnterLongStopLimit**(**double** limitPrice, **double** stopPrice)

**EnterLongStopLimit**(**double** limitPrice, **double** stopPrice, string signalName)

**EnterLongStopLimit**(**int** quantity, **double** limitPrice, **double** stopPrice)

**EnterLongStopLimit**(**int** quantity, **double** limitPrice, **double** stopPrice, string signalName)

For multi-bar strategies

**EnterLongStopLimit**(**int** barsInProgressIndex, **bool** liveUntilCancelled, **int** quantity, **double** limitPrice, **double** stopPrice, string signalName)

**Parameter**

  --------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  signalName            An unambiguous name

  quantity              Amount of stocks or contracts to be ordered

  barsInProgressIndex   For [*Multibar*][*MultiBars*] strategies.\
                        Index of the data series for which the entry order is to be executed.\
                        See [*BarsInProgress*].

  stopPrice             A double value for the stop price

  limitPrice            A double value for the limit price

  liveUntilCancelled    The order will not be deleted at the end of the bar, but will remain active until canceled with the CancelOrder command or until it reaches its expiry (see [*TimeInForce*]).
  --------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**Return Value**

an order object of the type “IOrder”

**Example**

**private** IOrder myEntryOrder = **null**;

// Place an entry stop at the high of the current bar\
// if the high is reached, a limit order will be placed 2 ticks above the high

**if** (myEntryOrder == **null**)

myEntryOrder = **EnterLongStopLimit**(High\[0\]+2\*TickSize, High\[0\], "Stop Long");

*Created with the Personal Edition of HelpNDoc: [*Single source CHM, PDF, DOC and HTML Help creation*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_EnterShort" class="anchor"></span>**EnterShort()**

**Description**

Enter short creates a market order for entering a short position (naked sell).

If a signature not containing a set amount is used, the amount is set by the [*DefaultQuantity*] or taken from the strategy dialog window.

See [*EnterShortLimit()*], [*EnterShortStop()*], [*EnterShortStopLimit()*].

**Usage**

**EnterShort**()

**EnterShort**(string signalName)

**EnterShort**(**int** quantity)

**EnterShort**(**int** quantity, string signalName)

For multi-bar strategies

**EnterShort**(**int** barsInProgressIndex, **int** quantity, string signalName)

**Parameter**

  --------------------- -----------------------------------------------------------------------
  signalName            An unambiguous name

  quantity              Amount of stocks/contracts etc.

  barsInProgressIndex   For [*Multibar*][*MultiBars*] strategies\
                        Index of the data series for which the entry order is to be executed\
                        See [*BarsInProgress*].
  --------------------- -----------------------------------------------------------------------

**Return Value**

an order object of the type “IOrder”

**Example**

// A short position will be placed if the last entry is 10 bars in the past and two SMAs have crossed each other

**if** (**BarsSinceEntry**() &gt; 10 && **CrossBelow**(**SMA**(10), **SMA**(20), 1))

**EnterShort**("SMA cross entry");

*Created with the Personal Edition of HelpNDoc: [*Full featured Help generator*]*

<span id="_topic_EnterShortLimit" class="anchor"></span>**EnterShortLimit()**

**Description**

Enter short limit creates a limit order for entering a short position (naked short).

If a signature not containing a set amount is used, the amount is set by the [*DefaultQuantity*] or taken from the strategy dialog window.

See [*EnterShort()*], [*EnterShortStop()*], [*EnterShortStopLimit()*].

**Usage**

**EnterShortLimit**(**double** limitPrice)

**EnterShortLimit**(**double** limitPrice, string signalName)

**EnterShortLimit**(**int** quantity, **double** limitPrice)

**EnterShortLimit**(**int** quantity, **double** limitPrice, string signalName)

For Multibar-Strategies

**EnterShortLimit**(**int** barsInProgressIndex, **bool** liveUntilCancelled, **int** quantity, **double** limitPrice, string signalName)

**Parameter**

  --------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  signalName            An unambiguous name

  quantity              Amount to be ordered

  barsInProgressIndex   For [*Multibar*][*MultiBars*] strategies.\
                        Index of the data series for which the entry order is to be executed.\
                        See [*BarsInProgress*].

  limitPrice            A double value for the limit price

  liveUntilCancelled    The order will not be deleted at the end of the bar, but will remain active until deleted with the CancelOrder command or until it reaches its expiry (see [*TimeInForce*]).
  --------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**Return Value**

an order object of the type “IOrder”

**Example**

// Enter a short position if the last entry is 10 bars in the past and two SMAs have crossed each other

**if** (**BarsSinceEntry**() &gt; 10 && **CrossBelow**(**SMA**(10), **SMA**(20), 1))

**EnterShortLimit**("SMA cross entry");

*Created with the Personal Edition of HelpNDoc: [*Free Web Help generator*][*Free PDF documentation generator*]*

<span id="_topic_EnterShortStop" class="anchor"></span>**EnterShortStop()**

**Description**

Enter short stop creates a limit order for entering a short position.\
If a signature not containing a set amount is used, the amount is set by the [*DefaultQuantity*] or taken from the strategy dialog window.\
\
See [*EnterShort()*], [*EnterShortLimit()*], [*EnterShortStopLimit()*].

**Usage**

**EnterShortStop**(**double** stopPrice)

**EnterShortStop**(**double** stopPrice, string signalName)

**EnterShortStop**(**int** quantity, **double** stopPrice)

**EnterShortStop**(**int** quantity, **double** stopPrice, string signalName)

For multi-bar strategies

**EnterShortStop**(**int** barsInProgressIndex, **bool** liveUntilCancelled, **int** quantity, **double** stopPrice, string signalName)

**Parameter**

  --------------------- ---------------------------------------------------------------------------------------------------------------
  signalName            An unambiguous name

  quantity              Amount to be ordered

  barsInProgressIndex   For [*Multibar*][*MultiBars*] strategies.\
                        Index of the data series for which the entry order is to be executed.\
                        See [*BarsInProgress*].

  stopPrice             A double value for the stop price

  liveUntilCancelled    The order will remain active until canceled using the CancelOrder command or until it reaches its expiry time
  --------------------- ---------------------------------------------------------------------------------------------------------------

**Return Value**

an order object of the type “IOrder”

**Example**

**private** IOrder myEntryOrder = **null**;

// Place an entry stop at the low of the current bar

**if** (myEntryOrder == **null**)

myEntryOrder = **EnterShortStop**(Low\[0\], "stop short");

*Created with the Personal Edition of HelpNDoc: [*Full featured Documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_EnterShortStopLimit" class="anchor"></span>**EnterShortStopLimit()**

**Description**

Enter short stop limit creates a sell stop limit order for entering a short position.

If a signature not containing a set amount is used, the amount is set by the [*DefaultQuantity*] or taken from the strategy dialog window.

See [*EnterShort()*], [*EnterShortLimit()*], [*EnterShortStop()*].

**Usage**

**EnterShortStopLimit**(**double** limitPrice, **double** stopPrice)

**EnterShortStopLimit**(**double** limitPrice, **double** stopPrice, string signalName)

**EnterShortStopLimit**(**int** quantity, **double** limitPrice, **double** stopPrice)

**EnterShortStopLimit**(**int** quantity, **double** limitPrice, **double** stopPrice, string signalName)

For multi-bar strategies

**EnterShortStopLimit**(**int** barsInProgressIndex, **bool** liveUntilCancelled, **int** quantity, **double** limitPrice, **double** stopPrice, string signalName)

**Parameter**

  --------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------
  signalName            An unambiguous name

  quantity              Amount to be ordered

  barsInProgressIndex   For [*Multibar*][*MultiBars*] strategies.\
                        Index of the data series for which an entry order is to be placed.\
                        See [*BarsInProgress*].

  stopPrice             A double value for the stop price

  limitPrice            A double value for the limit price

  liveUntilCancelled    The order will not be deleted at the end of the bar, but will remain active until deleted using the CancelOrder command or until it reaches its expiry time.
  --------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------

**Return Value**

An order object of the type “IOrder”

**Example**

**private** IOrder myEntryOrder = **null**;

// Place an entry stop at the low of the current bar; if the low is reached then place a limit order 2 ticks below the low

**if** (myEntryOrder == **null**)

myEntryOrder = **EnterShortStopLimit**(Low\[0\]-2\*TickSize, Low\[0\], "stop short");

*Created with the Personal Edition of HelpNDoc: [*Free iPhone documentation generator*][*iPhone web sites made easy*]*

<span id="_topic_EntriesPerDirection" class="anchor"></span>**EntriesPerDirection**

**Description**

Entries per direction defines the maximum number of entries permitted in one direction (long or short).

Whether the name of the entry signal is taken into consideration or not is defined within [*EntryHandling*].

Entries per direction is defined with the [*Initialize()*] method.

**Usage**

**EntriesPerDirection**

**Parameter**

An int value for the maximum entries permitted in one direction.

**Example**

// Example 1

// If one of the two entry conditions is true and a long position is opened, then the other entry signal will be ignored

**protected** override void **Initialize**()

{

EntriesPerDirection = 1;

EntryHandling = EntryHandling.AllEntries;

}

**protected** override void **OnBarUpdate**()

{

**if** (**CrossAbove**(**SMA**(10), **SMA**(20), 1)

**EnterLong**("SMA Cross Entry");

**if** (**CrossAbove**(**RSI**(14, 3), 30, 1)

**EnterLong**("RSI Cross Entry);

}

// Example 2

// For each differently named entry signal, a long position will be opened

**protected** override void **Initialize**()

{

EntriesPerDirection = 1;

EntryHandling = EntryHandling.UniqueEntries;

}

**protected** override void **OnBarUpdate**()

{

**if** (**CrossAbove**(**SMA**(10), **SMA**(20), 1)

**EnterLong**("SMA Cross Entry");

**if** (**CrossAbove**(**RSI**(14, 3), 30, 1)

**EnterLong**("RSI Cross Entry);

}

*Created with the Personal Edition of HelpNDoc: [*Easy EBook and documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_EntryHandling" class="anchor"></span>**EntryHandling**

**Description**

Entry handling decides how the maximum number of entries permitted in one direction is interpreted ([*EntriesPerDirection*]).

Entry handling is defined with the [*Initialize()*] method.

**EntryHandling.AllEntries**

AgenaTrader continues to create entry orders until the maximum number of entries permitted (defined in [*EntriesPerDirection*]) per direction (long or short) is reached, regardless of how the entry signals are named.

If entries per direction = 2, then enter long ("SMA crossover") and enter long ("range breakout") combined will reach the maximum number of long entries permitted.

**EntryHandling.UniqueEntries**

AgenaTrader continues to generate entry orders until the maximum number of entries (defined in entries per direction) in one direction (long or short) for the differently named entry signals has been reached.\
\
If entries per direction = 2, then it is possible for two signals for enter long ("SMA crossover") *and* 2 signals for enter long ("range breakout") to be traded.

**Usage**

**EntryHandling**

**Example**

See [*EntriesPerDirection*].

*Created with the Personal Edition of HelpNDoc: [*Easily create EBooks*][*Full featured Help generator*]*

<span id="_topic_ExcludeTradeHistoryInBacktest" class="anchor"></span>**ExcludeTradeHistoryInBacktest**

*Created with the Personal Edition of HelpNDoc: [*Easily create iPhone documentation*][*iPhone web sites made easy*]*

<span id="_topic_ExitLong" class="anchor"></span>**ExitLong()**

**Description**

Exit long creates a sell market order for closing a long position (sell).

If a signature not containing a set amount is used, the amount is set by [*DefaultQuantity*] or taken from the strategy dialog window.

See [*ExitLong()*], [*ExitLongLimit()*], [*ExitLongStop()*], [*ExitLongStopLimit()*].

**Usage**

**ExitLong**()

**ExitLong**(**int** quantity)

**ExitLong**(string fromEntry signal)

**ExitLong**(string signalName, string fromEntry signal)

**ExitLong**(**int** quantity, string signalName, string fromEntry signal)

For multi-bar strategies

**ExitLong**(**int** barsInProgressIndex, **int** quantity, string signalName, string fromEntry signal)

**Parameter**

  --------------------- -----------------------------------------------------------------------
  signalName            An unambiguous name

  quantity              The quantity to be sold

  barsInProgressIndex   For [*Multibar*][*MultiBars*] strategies.\
                        Index of the data series for which the exit order is to be executed.\
                        See [*BarsInProgress*].

  fromEntry signal      The name of the attached entry signal
  --------------------- -----------------------------------------------------------------------

**Return Value**

an order object of the type “IOrder”

**Example**

// Enter if two SMAs cross each other

**if** (**CrossAbove**(**SMA**(10), **SMA**(20), 1))

**EnterLong**("SMA Cross Entry");

// Close position

**if** (**CrossBelow**(**SMA**(10), **SMA**(20), 1))

**ExitLong**();

*Created with the Personal Edition of HelpNDoc: [*Full featured EPub generator*][*Free PDF documentation generator*]*

<span id="_topic_ExitLongLimit" class="anchor"></span>**ExitLongLimit()**

**Description**

Exit long limit creates a sell limit order for closing a long position (i.e. for selling).

If a signature not containing a set amount is used, the amount is set by the [*DefaultQuantity*] or taken from the strategy dialog window.\
\
See [*ExitLong()*], [*ExitLongLimit()*], [*ExitLongStop()*], [*ExitLongStopLimit()*].

**Usage**

**ExitLongLimit**(**double** limitPrice)

**ExitLongLimit**(**int** quantity, **double** limitPrice)

**ExitLongLimit**(**double** limitPrice, string fromEntry signal)

**ExitLongLimit**(**double** limitPrice, string signalName, string fromEntry signal)

**ExitLongLimit**(**int** quantity, **double** limitPrice, string signalName, string fromEntry signal)

For multi-bar strategies

**ExitLongLimit**(**int** barsInProgressIndex, **bool** liveUntilCancelled, **int** quantity, **double** limitPrice, string signalName, string fromEntry signal)

**Parameter**

  --------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------
  signalName            An unambiguous name

  fromEntry signal      The name of the attached entry signal

  quantity              Order quantity to be sold

  barsInProgressIndex   For [*Multibar*][*MultiBars*] strategies.\
                        Index of the data series for which the exit order is to be executed.\
                        See [*BarsInProgress*].

  limitPrice            A double value for the limit price

  liveUntilCancelled    The order will not be deleted at the end of the bar, but will remain active until deleted using the CancelOrder command or until it reaches its expiry time.
  --------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------

**Return Value**

an order object of the type “IOrder”

**Example**

// Enter if two SMAs cross each other

**if** (**CrossAbove**(**SMA**(10), **SMA**(20), 1))

**EnterLong**("SMA Cross Entry");

// Close position

**if** (**CrossBelow**(**SMA**(10), **SMA**(20), 1))

**ExitLongLimit**(**GetCurrentBid**());

*Created with the Personal Edition of HelpNDoc: [*Write EPub books for the iPad*][*Free PDF documentation generator*]*

<span id="_topic_ExitLongStop" class="anchor"></span>**ExitLongStop()**

**Description**

Exit long stop creates a sell stop order for closing a long position (short).

If a signature not containing a set amount is used, the amount is set by the [*DefaultQuantity*] or taken from the strategy dialog window.\
See [*ExitLong()*], [*ExitLongLimit()*], [*ExitLongStop()*], [*ExitLongStopLimit()*].

**Usage**

**ExitLongStop**(**int** quantity, **double** stopPrice)

**ExitLongStop**(**double** stopPrice, string fromEntry signal)

**ExitLongStop**(**double** stopPrice, string signalName, string fromEntry signal)

**ExitLongStop**(**int** quantity, **double** stopPrice, string signalName, string fromEntry signal)

For multi-bar strategies

**ExitLongStop**(**int** barsInProgressIndex, **bool** liveUntilCancelled, **int** quantity, **double** stopPrice, string signalName, string fromEntry signal)ExitLongStop

**Parameter**

  --------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------
  signalName            An unambiguous name

  fromEntry signal      The name of the associated entry signal

  quantity              The quantity to be sold

  barsInProgressIndex   For [*Multibar*][*MultiBars*] strategies.\
                        Index of the data series for which the exit order is to be executed.\
                        See [*BarsInProgress*].

  stopPrice             A double value for the stop price

  liveUntilCancelled    The order will not be deleted at the end of the bar, but will remain active until deleted using the CancelOrder command or until it reaches its expiry time.
  --------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------

**Return Value**

an order object of the type “IOrder”

**Example**

// Enter if two SMAs cross each other

**if** (**CrossAbove**(**SMA**(10), **SMA**(20), 1))

**EnterLong**("SMA Cross Entry");

// Close position

**if** (**CrossBelow**(**SMA**(10), **SMA**(20), 1))

**ExitLongStop**(Low\[0\]);

*Created with the Personal Edition of HelpNDoc: [*Write EPub books for the iPad*][*Free PDF documentation generator*]*

<span id="_topic_ExitLongStopLimit" class="anchor"></span>**ExitLongStopLimit()**

**Description**

Exit long stop limit creates a sell stop limit order for closing a long position (i.e. selling).

If a signature not containing a set amount is used, the amount is set by the [*DefaultQuantity*] or taken from the strategy dialog window.

See [*ExitLong()*], [*ExitLongLimit()*], [*ExitLongStop()*], [*ExitLongStopLimit()*].

**Usage**

**ExitLongStopLimit**(**double** limitPrice, **double** stopPrice)

**ExitLongStopLimit**(**int** quantity, **double** limitPrice, **double** stopPrice)

**ExitLongStopLimit**(**double** limitPrice, **double** stopPrice, string fromEntry signal)

**ExitLongStopLimit**(**double** limitPrice, **double** stopPrice, string signalName, string fromEntry signal)

**ExitLongStopLimit**(**int** quantity, **double** limitPrice, **double** stopPrice, string signalName, string fromEntry signal)

For Multibar-Strategies

**ExitLongStopLimit**(**int** barsInProgressIndex, **bool** liveUntilCancelled, **int** quantity, **double** limitPrice, **double** stopPrice, string signalName, string fromEntry signal)

**Parameter**

  --------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------
  signalName            An unambiguous name

  fromEntry signal      The name of the associated entry signal

  quantity              The quantity to be sold

  barsInProgressIndex   For [*Multibar*][*MultiBars*] strategies.\
                        Index of the data series for which the exit order is to be executed.\
                        See [*BarsInProgress*].

  limitPrice            A double value for the limit price

  stopPrice             A double value for the stop price

  liveUntilCancelled    The order will not be deleted at the end of the bar, but will remain active until deleted using the CancelOrder command or until it reaches its expiry time.
  --------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------

**Return Value**

an order object of the type “IOrder”

**Example**

// Enter if two SMAs cross each other

**if** (**CrossAbove**(**SMA**(10), **SMA**(20), 1))

**EnterLong**("SMA Cross Entry");

// Close position

**if** (**CrossBelow**(**SMA**(10), **SMA**(20), 1))

**ExitLongStopLimit**(Low\[0\]-10\*TickSize, Low\[0\]);

*Created with the Personal Edition of HelpNDoc: [*Full featured Help generator*]*

<span id="_topic_ExitOnClose" class="anchor"></span>**ExitOnClose**

*Created with the Personal Edition of HelpNDoc: [*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_ExitOnCloseSeconds" class="anchor"></span>**ExitOnCloseSeconds**

*Created with the Personal Edition of HelpNDoc: [*Free Web Help generator*][*Free PDF documentation generator*]*

<span id="_topic_ExitShort" class="anchor"></span>**ExitShort()**

**Description**

Exit short creates a buy-to-cover market order for closing a short position (buy).

If a signature not containing a set amount is used, the amount is set by the [*DefaultQuantity*] or taken from the strategy dialog window.\
See [*ExitShort()*], [*ExitShortLimit()*], [*ExitShortStop()*], [*ExitShortStopLimit()*].

**Usage**

**ExitShort**()

**ExitShort**(**int** quantity)

**ExitShort**(string fromEntry signal)

**ExitShort**(string signalName, string fromEntry signal)

**ExitShort**(**int** quantity, string signalName, string fromEntry signal)

For multi-bar strategies

**ExitShort**(**int** barsInProgressIndex, **int** quantity, string signalName, string fromEntry signal)

**Parameter**

  --------------------- -----------------------------------------------------------------------
  signalName            An unambiguous name

  Quantity              Order quantity to be bought

  barsInProgressIndex   For [*Multibar*][*MultiBars*] strategies.\
                        Index of the data series for which the exit order is to be executed.\
                        See [*BarsInProgress*].

  fromEntry signal      The name of the associated entry signal
  --------------------- -----------------------------------------------------------------------

**Return Value**

an order object of the type “IOrder”

**Example**

// Enter if two SMAs cross each other

**if** (**CrossBelow**(**SMA**(10), **SMA**(20), 1))

**EnterShort**("SMA cross entry");

// Close position

**if** (**CrossAbove**(**SMA**(10), **SMA**(20), 1))

**ExitShort**();

*Created with the Personal Edition of HelpNDoc: [*Easy CHM and documentation editor*][*Free PDF documentation generator*]*

<span id="_topic_ExitShortLimit" class="anchor"></span>**ExitShortLimit()**

**Description**

Exit short limit creates a buy-to-cover limit order for closing a short position (buy).

If a signature not containing a set amount is used, the amount is set by the [*DefaultQuantity*] or taken from the strategy dialog window.

See [*ExitShort()*], [*ExitShortLimit()*], [*ExitShortStop()*], [*ExitShortStopLimit()*].

**Usage**

**ExitShortLimit**(**double** limitPrice)

**ExitShortLimit**(**int** quantity, **double** limitPrice)

**ExitShortLimit**(**double** limitPrice, string fromEntry signal)

**ExitShortLimit**(**double** limitPrice, string signalName, string fromEntry signal)

**ExitShortLimit**(**int** quantity, **double** limitPrice, string signalName, string fromEntry signal)

For multi-bar strategies

**ExitShortLimit**(**int** barsInProgressIndex, **bool** liveUntilCancelled, **int** quantity, **double** limitPrice, string signalName, string fromEntry signal)

**Parameter**

  --------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------
  signalName            An unambiguous name

  fromEntry signal      The name of the associated entry signal

  quantity              Order quantity to be bought

  barsInProgressIndex   For [*Multibar*][*MultiBars*] strategies\
                        Index of the data series for which the exit order is to be executed.\
                        See [*BarsInProgress*].

  limitPrice            A double value for the limit price

  liveUntilCancelled    The order will not be deleted at the end of the bar, but will remain active until deleted using the CancelOrder command or until it reaches its expiry time.
  --------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------

**Return Value**

an order object of the type “IOrder”

**Example**

// Enter if two SMAs cross

**if** (**CrossBelow**(**SMA**(10), **SMA**(20), 1))

**EnterShort**("SMA cross entry");

// Close position

**if** (**CrossAbove**(**SMA**(10), **SMA**(20), 1))

**ExitShortLimit**(**GetCurrentAsk**());

*Created with the Personal Edition of HelpNDoc: [*Easily create PDF Help documents*][*Full featured Help generator*]*

<span id="_topic_ExitShortStop" class="anchor"></span>**ExitShortStop()**

**Description**

Exit short stop creates a buy-to-cover stop order for closing a short position.\
\
If a signature not containing a set amount is used, the amount is set by the [*DefaultQuantity*] or taken from the strategy dialog window.

See [*ExitShort()*], [*ExitShortLimit()*], [*ExitShortStop()*], [*ExitShortStopLimit()*].

**Usage**

**ExitShortStop**(**int** quantity, **double** stopPrice)

**ExitShortStop**(**double** stopPrice, string fromEntry signal)

**ExitShortStop**(**double** stopPrice, string signalName, string fromEntry signal)

**ExitShortStop**(**int** quantity, **double** stopPrice, string signalName, string fromEntry signal)

For multi-bar strategies

**ExitShortStop**(**int** barsInProgressIndex, **bool** liveUntilCancelled, **int** quantity, **double** stopPrice, string signalName, string fromEntry signal)ExitLongStop

**Parameter**

  --------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------
  signalName            An unambiguous name

  fromEntry signal      The name of the associated entry signal

  quantity              Order quantity to be bought

  barsInProgressIndex   For [*Multibar*][*MultiBars*] strategies.\
                        Index of the data series for which the exit order is to be executed.\
                        See [*BarsInProgress*].

  stopPrice             A double value for the stop price

  liveUntilCancelled    The order will not be deleted at the end of the bar, but will remain active until deleted using the CancelOrder command or until it reaches its expiry time.
  --------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------

**Return Value**

an order object of the type “IOrder”

**Example**

// Enter if two SMAs have crossed

**if** (**CrossBelow**(**SMA**(10), **SMA**(20), 1))

**EnterShort**("SMA cross entry");

// Close position

**if** (**CrossAbove** (**SMA**(10), **SMA**(20), 1))

**ExitShortStop**(High\[0\]);

*Created with the Personal Edition of HelpNDoc: [*Easily create EBooks*][*Full featured Help generator*]*

<span id="_topic_ExitShortStopLimit" class="anchor"></span>**ExitShortStopLimit()**

**Description**

Exit short stop limit creates a buy-to-cover stop limit order for closing a short position.\
If a signature not containing a set amount is used, the amount is set by the [*DefaultQuantity*] or taken from the strategy dialog window.

See [*ExitLong()*], [*ExitLongLimit()*], [*ExitLongStop()*], [*ExitLongStopLimit()*].

**Usage**

**ExitShortStopLimit**(**double** limitPrice, **double** stopPrice)

**ExitShortStopLimit**(**int** quantity, **double** limitPrice, **double** stopPrice)

**ExitShortStopLimit**(**double** limitPrice, **double** stopPrice, string fromEntry signal)

**ExitShortStopLimit**(**double** limitPrice, **double** stopPrice, string signalName, string fromEntry signal)

**ExitShortStopLimit**(**int** quantity, **double** limitPrice, **double** stopPrice, string signalName, string fromEntry signal)

For multi-bar strategies

**ExitShortStopLimit**(**int** barsInProgressIndex, **bool** liveUntilCancelled, **int** quantity, **double** limitPrice, **double** stopPrice, string signalName, string fromEntry signal)

**Parameter**

  --------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------
  signalName            An unambiguous name

  fromEntry signal      The name of the associated entry signal

  quantity              Order quantity to be bought

  barsInProgressIndex   For [*Multibar*][*MultiBars*] strategies.\
                        Index of the data series for which the exit order is to be executed.\
                        See [*BarsInProgress*].

  limitPrice            A double value for the limit price

  stopPrice             A double value for the stop price

  liveUntilCancelled    The order will not be deleted at the end of the bar, but will remain active until deleted using the CancelOrder command or until it reaches its expiry time.
  --------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------

**Return Value**

an order object of the type “IOrder”

**Example**

// Enter if two SMAs cross each other

**if** (**CrossBelow**(**SMA**(10), **SMA**(20), 1))

**EnterShort**("SMA cross entry");

// Close position

**if** (**CrossAbove**(**SMA**(10), **SMA**(20), 1))

**ExitShortStopLimit**(High\[0\]+10\*TickSize, High\[0\]);

*Created with the Personal Edition of HelpNDoc: [*Full featured Documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_GetAccountValue" class="anchor"></span>**GetAccountValue()**

**Description**

Get account value outputs information regarding the account for which the current strategy is being carried out.

See [*GetProfitLoss()*].

**Usage**

**GetAccountValue**(AccountItem accountItem)

**Parameter**

Possible values for account item are:

AccountItem.BuyingPower

AccountItem.CashValue

AccountItem.RealizedProfitLoss

**Return Value**

a double value for the account item

for historical bars, a zero (0) is returned

**Example**

**Print**("The current account cash value is " + **GetAccountValue**(AccountItem.CashValue));

**Print**("The current account cash value with the leverage provided by the broker is " + **GetAccountValue**(AccountItem.BuyingPower));

**Print**("The current P/L already realized is " + **GetAccountValue**(AccountItem.RealizedProfitLoss));

*Created with the Personal Edition of HelpNDoc: [*Free CHM Help documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_GetProfitLoss" class="anchor"></span>**GetProfitLoss()**

**Description**

Get profit loss outputs the currently unrealized profit or loss for a running position.

See [*GetAccountValue()*].

**Usage**

**GetProfitLoss**(**int** pLType);

**Parameter**

Potential values for the P/L type are:

0 – Amount: P/L as a currency amount

1 – Percent: P/L in percent

2 – Risk: P/L in Van Tharp R-multiples ([*http://www.vantharp.com/tharp-concepts/risk-and-r-multiples.asp*])

3 – P/L in ticks

**Return Value**

a double value for the unrealized profit or loss

**Example**

**Print**("The current risk for the strategy " + **this**.Name + " is " + **GetProfitLoss**(1) + " " + Instrument.Currency);

**Print**("This equals "+ string.**Format**( "{0:F1} R.", **GetProfitLoss**(3)));

*Created with the Personal Edition of HelpNDoc: [*Easy EPub and documentation editor*][*Free PDF documentation generator*]*

<span id="_topic_MarketPosition" class="anchor"></span>**MarketPosition**

See [*Position.MarketPosition*].

*Created with the Personal Edition of HelpNDoc: [*Full featured multi-format Help generator*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_Performance" class="anchor"></span>**Performance**

**Description**

Performance is an object containing information regarding all trades that have been generated by a strategy.

The trades are sorted into multiple lists. With the help of these lists it is easier to create a performance evaluation.

See *Performance Characteristics*.

The individual lists are:

-   **Performance.AllTrades\
    **A [*Trade*] collection object containing all trades generated by a strategy

-   **Performance.LongTrades\
    **A [*Trade*] collection object containing all long trades generated by a strategy

-   **Performance.ShortTrades\
    **A [*Trade*] collection object containing all short trades generated by a strategy

-   **Performance.WinningTrades\
    **A [*Trade*] collection object containing all profitable trades generated by a strategy

-   **Performance.LosingTrades\
    **A [*Trade*] collection object containing all loss trades generated by a strategy

**Example**

// When exiting a strategy, create a performance evaluation

**protected** override void **OnTermination**()

{

**Print**("Performance evaluation of the strategy : " + **this**.Name);

**Print**("----------------------------------------------------");

**Print**("Amount of all trades: " + Performance.AllTrades.Count);

**Print**("Amount of winning trades: " + Performance.WinningTrades.Count);

**Print**("Amount of all loss trades: " + Performance.LosingTrades.Count);

**Print**("Amount of all long trades: " + Performance.LongTrades.Count);

**Print**("Amount of short trades: " + Performance.ShortTrades.Count);

**Print**("Result: " + Account.RealizedProfitLoss + " " + Account.Currency);

}

*Created with the Personal Edition of HelpNDoc: [*Free help authoring tool*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_Position" class="anchor"></span>**Position**

**Description**

Position is an object containing information regarding the position currently being managed by a strategy.

The individual properties are:

-   **Position.AvgPrice\
    **The average buy or sell price of a position.\
    For positions without partial executions, this is equal to the entry price.

-   **Position.CreatedDateTime\
    **Date and time at which the position was opened.

-   **Position.Instrument\
    **The trading instrument in which the position exists.\
    See *Instruments*.

-   **Position.MarketPosition\
    **One of three possible positions in the market:\
    - PositionType.Flat\
    - PositionType.Long\
    - PositionType.Short

-   **Position.OpenProfitLoss\
    **The currently not yet realized profit or loss.\
    See [*GetProfitLoss()*].

-   **Position.ProfitCurrency\
    **Profit (or loss) displayed as a currency amount.

-   **Position.ProfitPercent\
    **Profit (or loss) displayed in percent.

-   **Position.ProfitPoints\
    **Profit (or loss) displayed in points or pips.

-   **Position.Quantity\
    **Amount of stocks, contracts, CFDs etc. within a position.

**Example**

**if** (Position.MarketPosition != PositionType.Flat)

{

**Print**("Average price " + Position.AvgPrice);

**Print**("Opening time " + Position.CreatedDateTime);

**Print**("Instrument " + Position.Instrument);

**Print**("Current positioning " + Position.MarketPosition);

**Print**("Unrealized P/L " + Position.OpenProfitLoss);

**Print**("P/L (currency) " + Position.ProfitCurrency);

**Print**("P/L (in percent) " + Position.ProfitPercent);

**Print**("P/L (in points) " + Position.ProfitPoints);

**Print**("Pieces " + Position.Quantity);

}

*Created with the Personal Edition of HelpNDoc: [*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_Quantity" class="anchor"></span>**Quantity**

See [*Position.Quantity*][*Position.MarketPosition*].

*Created with the Personal Edition of HelpNDoc: [*Full featured EBook editor*][*Free PDF documentation generator*]*

<span id="_topic_SetProfitTarget" class="anchor"></span>**SetProfitTarget()**

**Description**

Set profit target immediately creates a “take profit” order after an entry order is generated. The order is sent directly to the broker and becomes active immediately.\
\
If the profit target is static, you can also define SetProfitTarget() with the Initialize() method.

See [*SetStopLoss()*], [*SetTrailStop()*].

**Usage**

**SetProfitTarget**(**double** currency)

**SetProfitTarget**(CalculationMode mode, **double value**)

**SetProfitTarget**(string fromEntry signal, CalculationMode mode, **double value**)

**Parameter**

  ------------------ -------------------------------------------------------------------------------------------------------------------------------------------------------------------
  currency           Sets the profit target in a currency, for example 500€.

  mode               Possible values are:
                     
                     - CalculationMode.Percent (display in percent)\
                     - CalculationMode.Price (display as price value)\
                     - CalculationMode.Ticks (display in ticks or pips)

  value              The distance between entry price and profit target. This is dependent upon the „mode“ but generally refers to a monetary value, a percentage or a value in ticks.

  fromEntry signal   The name of the entry signal for which the profit target is to be generated. The amount is taken from the entry order referenced.
  ------------------ -------------------------------------------------------------------------------------------------------------------------------------------------------------------

**Example**

**protected** override void **Initialize**()

{

// Creates a profit target order 10 ticks above break-even

**SetProfitTarget**(CalculationMode.Ticks, 10);

}

*Created with the Personal Edition of HelpNDoc: [*Free Web Help generator*][*Free PDF documentation generator*]*

<span id="_topic_SetStopLoss" class="anchor"></span>**SetStopLoss()**

**Description**

Set stop loss creates a stop loss order after an entry order is placed. The order is sent directly to the broker and becomes effective immediately.

If the stop loss is static, then SetStopLoss() can be defined with the Initialize() method.

See [*SetProfitTarget()*], [*SetTrailStop()*].

**Usage**

**SetStopLoss**(**double** currency)

**SetStopLoss**(**double** currency, **bool** simulated)

**SetStopLoss**(CalculationMode mode, **double value**)

**SetStopLoss**(string fromEntry signal, CalculationMode mode, **double value**, **bool** simulated)

**Parameter**

  ------------------ -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  currency           The difference between the stop loss and the entry price (=risk) in a currency, such as 500€

  mode               Potential values can be:
                     
                     - CalculationMode.Percent (display in percent)\
                     - CalculationMode.Price (display as price value)\
                     - CalculationMode.Ticks (display in ticks or pips)

  simulated          When set to “true,” the stop order does not go live (as a market order) until the price has „touched“ it for the first time (meaning that it is executed just as it would be under real market conditions).

  value              The distance between stop price and profit target. This is dependent upon the „mode“ but generally refers to a monetary value, a percentage or a value in ticks.

  fromEntry signal   The name of the entry signal for which the stop order is to be generated. The amount is taken from the entry order referenced.
  ------------------ -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**Example**

**protected** override void **Initialize**()

{

// Sets a stop of 500€

**SetStopLoss**(500);

}

*Created with the Personal Edition of HelpNDoc: [*Free CHM Help documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_SetTrailStop" class="anchor"></span>**SetTrailStop()**

**Description**

Set trail stop creates a trail stop order after an entry order is generated. Its purpose is to protect you from losses, and after reaching break-even, to protect your gains.

The order is sent directly to the broker and becomes effective immediately.

If the stop loss price and the offset value are static, you can define SetTrailStop() with the Initialize() method.

If you use SetTrailStop() within the [*OnBarUpdate()*][*OnBarUpdate ()*] method, you must make sure that the parameters are readjusted to the initial value, otherwise the most recently used settings will be used for the new position.

**Functionality:**

Assuming that you have SetTrailStop(CalculationMode.Ticks, 30) selected:

In a long position, the stop will be 30 ticks from the previously reached high. If the market makes a new high, the stop will be adjusted. However, the stop will no longer be moved downwards.

In a short position, this behavior starts with the most recent low.

**Tips:**

-   It is not possible to use SetStopLoss and SetTrailStop for the same position at the same time within one strategy. The SetStopLoss() method will always have precedence over the other methods.

-   However, it is possible to use both variants parallel to each other in the same strategy if they are referencing different entry signals.

-   Partial executions of a single order will cause a separate trading stop for each partial position.

-   If a SetProfitTarget() is used in addition to a SetTrailStop(), then both orders will be automatically linked to form an OCO order.

-   It is always a stop market order that is generated, and not a stop limit order.

-   If a position is closed by a different exit order within the strategy, then the TrailingStopOrder is automatically deleted.

See [*SetStopLoss()*], [*SetProfitTarget()*].

**Usage**

**SetTrailStop**(**double** currency)

**SetTrailStop**(**double** currency, **bool** simulated)

**SetTrailStop**(CalculationMode mode, **double value**)

**SetTrailStop**(string fromEntry signal, CalculationMode mode, **double value**, **bool** simulated)

**Parameter**

  ------------------ -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  currency           The distance between the stop loss and the entry price

  mode               Possible values are:
                     
                     - CalculationMode.Percent\
                     - CalculationMode.Ticks

  simulated          When set to “true,” the stop order does not go live (as a market order) until the price has „touched“ it for the first time (meaning that it is executed just as it would be under real market conditions).

  value              The distance between stop price and profit target. This is dependent upon the „mode“ but generally refers to a monetary value, a percentage or a value in ticks.

  fromEntry signal   The name of the entry signal for which the stop order is to be generated. The amount is taken from the entry order referenced.
  ------------------ -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**Example**

**protected** override void **Initialize**()

{

// Sets a trailing stop of 30 ticks

**SetTrailStop**(CalculationMode.Ticks, 30);

}

*Created with the Personal Edition of HelpNDoc: [*Write EPub books for the iPad*][*Free PDF documentation generator*]*

<span id="_topic_SubmitOrder" class="anchor"></span>**SubmitOrder()**

**Description**

Submit order creates a user-defined order. For this order, no stop or limit order is placed in the market. All AgenaTrader control mechanisms are switched off for this order type. The user is responsible for managing the various stop and target orders, including partial executions.

See [*OnOrderUpdate()*], [*OnExecution()*].

**Usage**

**SubmitOrder**(**int** barsInProgressIndex, OrderAction orderAction, OrderType orderType, **int** quantity, **double** limitPrice, **double** stopPrice, string ocoId, string signalName)

**Parameter**

  --------------------- --------------------------------------------------------------------
  barsInProgressIndex   For multi-bar strategies.\
                        Index of the data series for which the order is to be executed.\
                        See BarsInProgress.

  orderAction           Possible values are:
                        
                        -   OrderAction.Buy\
                            Buy order for a long entry
                        
                        -   OrderAction.Sell\
                            Sell order for closing a long position
                        
                        -   OrderAction.SellShort\
                            Sell order for a short entry
                        
                        -   OrderAction.BuyToCover\
                            Buy order for closing a short position
                        
                        

  orderType             Possible values:
                        
                        -   OrderType.Limit
                        
                        -   OrderType.Market
                        
                        -   OrderType.Stop
                        
                        -   OrderType.StopLimit
                        
                        

  quantity              Amount

  limitPrice            Limit value. Inputting a 0 makes this parameter irrelevant

  stopPrice             Stop value. Inputting a 0 makes this parameter irrelevant

  ocoId                 A unique ID (string) for linking multiple orders into an OCO group

  signalName            An unambiguous signal name (string)
  --------------------- --------------------------------------------------------------------

**Return Value**

an order object of the type “IOrder”

**Example**

**private** IOrder entryOrder = **null**;

**protected** override void **OnBarUpdate**()

{

// Entry conditions

**if** (Close\[0\] &gt; **SMA**(20)\[0\] && entryOrder == **null**)

entryOrder = **SubmitOrder**(0, OrderAction.Buy, OrderType.Market, 1, 0, 0, "", "Enter long");

}

*Created with the Personal Edition of HelpNDoc: [*Easily create PDF Help documents*][*Full featured Help generator*]*

<span id="_topic_TimeInForce" class="anchor"></span>**TimeInForce**

**Description**

The time in force property determines how long an order is valid for. The validity period is dependent upon which values are accepted by a broker.

TimeInForce is specified with the [*Initialize()*] method.

Permitted values are:\
\
TimeInForce.Day\
TimeInForce.GTC (GTC = good till canceled)

**Default:** TimeInForce.GTC

**Usage**

**TimeInForce**

**Example**

**protected** override void **Initialize**()

{

TimeInForce = TimeInForce.Day;

}

*Created with the Personal Edition of HelpNDoc: [*iPhone web sites made easy*]*

<span id="_topic_TraceOrders" class="anchor"></span>**TraceOrders**

**Description**

The trace orders property is especially useful for keeping track of orders generated by strategies. It also provides an overview of which orders were generated by which strategies.\
\
Trace orders can be specified with the [*Initialize()*] method.

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

**Usage**

TraceOrders

**Parameter**

none

**Return Value**

**true** Tracing is currently switched on\
**false** Tracing is switched off

**Example**

**protected** override void **Initialize**()

{

**ClearOutputWindow**();

TraceOrders = **true**;

}

*Created with the Personal Edition of HelpNDoc: [*Full featured multi-format Help generator*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_Trade" class="anchor"></span>**Trade**

**Description**

Trade is an object containing information about trades that have been executed by a strategy or are currently running.

The individual properties are:

-   **Trade.AvGPrice\
    **Average entry price

-   **Trade.ClosedProfitLoss\
    **Profit or loss already realized

-   **Trade.Commission\
    **Commissions

-   **Trade.CreatedDateTime\
    **Time at which the trade was created

-   **Trade.EntryReason\
    **Description of the entry signal\
    For strategies: signal entry name

-   **Trade.ExitDateTime\
    **Time at which the trade was closed

-   **Trade.ExitPrice\
    **Exit price

-   **Trade.ExitReason\
    **Description of the exit signal\
    For strategies: name of the strategy

-   **Trade.Instrument\
    **Description of the trading instrument

-   **Trade.MarketPosition\
    **Positioning within the market\
    - PositionType.Flat\
    - PositionType.Long\
    - PositionType.Short

-   **Trade.OpenProfitLoss\
    **Unrealized profit/loss of a running position

-   **Trade.ProfitCurrency\
    **Profit or loss in the currency that the account is held in

-   **Trade.ProfitLoss\
    **Profit or loss

-   **Trade.ProfitPercent\
    **Profit or loss in percent

-   **Trade.ProfitPercentWithCommission\
    **Profit or loss in percent with commissions

-   **Trade.ProfitPoints\
    **Profit or loss in points/pips

-   **Trade.Quantity\
    **Quantity of stocks/contracts/ETFs/etc.

-   **Trade.TimeFrame\
    **Timeframe in which the trade was opened

-   **Trade.Url\
    **URL for the snapshot of the chart at the moment of creation

**Example**

**protected** override void **OnTermination**()

{

**if** (Performance.AllTrades.Count &lt; 1) return;

**foreach** (ITrade trade **in** Performance.AllTrades)

{

**Print**("Trade \#"+trade.Id);

**Print**("--------------------------------------------");

**Print**("Average price " + trade.AvgPrice);

**Print**("Realized P/L " + trade.ClosedProfitLoss);

**Print**("Commissions " + trade.Commission);

**Print**("Time of entry " + trade.CreatedDateTime);

**Print**("Entry reason " + trade.EntryReason);

**Print**("Time of exit " + trade.ExitDateTime);

**Print**("Exit price " + trade.ExitPrice);

**Print**("Exit reason " + trade.ExitReason);

**Print**("Instrument " + trade.Instrument);

**Print**("Positioning " + trade.MarketPosition);

**Print**("Unrealized P/L " + trade.OpenProfitLoss);

**Print**("P/L (currency) " + trade.ProfitCurrency);

**Print**("P/L " + trade.ProfitLoss);

**Print**("P/L (in percent) " + trade.ProfitPercent);

**Print**("P/L (% with commission)" + trade.ProfitPercentWithCommission);

**Print**("PL (in points) " + trade.ProfitPoints);

**Print**("Quantity " + trade.Quantity);

**Print**("Timeframe " + trade.TimeFrame);

**Print**("URL for the snapshot " + trade.Url);

**Print**("");

}

}

*Created with the Personal Edition of HelpNDoc: [*Easy to use tool to create HTML Help files and Help web sites*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_Unmanaged" class="anchor"></span>**Unmanaged**

*Created with the Personal Edition of HelpNDoc: [*Free CHM Help documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_BacktestundOptimierung" class="anchor"></span>**Backtesting and Optimization**

*Created with the Personal Edition of HelpNDoc: [*Free help authoring environment*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_PerformanceKennzahlen" class="anchor"></span>**Performance Characteristics**

Performance characteristics are the various factors that can be calculated for a list of trades. The trades can be generated by a strategy in real-time or based on a backtest.

The following are available:

-   all trades

-   all long trades

-   all short trades

-   all winning trades

-   all losing trades

See [*Performance*].

The individual factors are:

-   **AvgEtd\
    **The average drawdown at the end of a trade\
    &lt;TradeCollection&gt;.TradesPerformance.&lt;TradesPerformanceValues&gt;.AvgEtd\
    **Print**("Average ETD of all trades is: " + Performance.AllTrades.TradesPerformance.Currency.AvgEtd);

-   **AvgMae\
    **Average maximum adverse excursion\
    &lt;TradeCollection&gt;.TradesPerformance.&lt;TradesPerformanceValues&gt;.AvgMae\
    **Print**("Average MAE of all trades is: " + Performance.AllTrades.TradesPerformance.Currency.AvgMae);

-   **AvgMfe\
    **Average maximum favorable excursion\
    &lt;TradeCollection&gt;.TradesPerformance.&lt;TradesPerformanceValues&gt;.AvgMfe\
    **Print**("Average MFE of all trades is: " + Performance.AllTrades.TradesPerformance.Currency.AvgMfe);

-   **AvgProfit\
    **Average profit for all trades\
    &lt;TradeCollection&gt;.TradesPerformance.&lt;TradesPerformanceValues&gt;.AvgProfit\
    **Print**("Average profit of all trades is: " + Performance.AllTrades.TradesPerformance.Currency.AvgProfit);

-   **CumProfit\
    **The cumulative winnings over all trades\
    &lt;TradeCollection&gt;.TradesPerformance.&lt;TradesPerformanceValues&gt;.CumProfit\
    **Print**("Average cumulative profit of all trades is: " + Performance.AllTrades.TradesPerformance.Currency.CumProfit);

-   **DrawDown\
    **The drawdown for all trades\
    &lt;TradeCollection&gt;.TradesPerformance.&lt;TradesPerformanceValues&gt;.DrawDown\
    **Print**("Drawdown of all trades is: " + Performance.AllTrades.TradesPerformance.Currency.DrawDown);

-   **LargestLoser\
    **The largest losing trade\
    &lt;TradeCollection&gt;.TradesPerformance.&lt;TradesPerformanceValues&gt;.LargestLoser\
    **Print**("Largest loss of all trades is: " + Performance.AllTrades.TradesPerformance.Currency.LargestLoser);

-   **LargestWinner\
    **The largest winning trade\
    &lt;TradeCollection&gt;.TradesPerformance.&lt;TradesPerformanceValues&gt;.LargestWinner\
    **Print**("Largest win of all trades is: " + Performance.AllTrades.TradesPerformance.Currency.LargestWinner);

-   **ProfitPerMonth\
    **The total performance (wins/losses) for the month (also in percent)\
    &lt;TradeCollection&gt;.TradesPerformance.&lt;TradesPerformanceValues&gt;.ProfitPerMonth\
    **Print**("Profit per month of all trades is: " + Performance.AllTrades.TradesPerformance.Currency.ProfitPerMonth);

-   **StdDev**\
    The standard deviation for the wins/losses. With this, you are able to identify outliers. The smaller the standard deviation, the higher the expectation of winnings.

**All factors are double values.**

![][10]

*Created with the Personal Edition of HelpNDoc: [*Create iPhone web-based documentation*][*iPhone web sites made easy*]*

<span id="_topic_Schlsselworte" class="anchor"></span>**Keywords**

*Created with the Personal Edition of HelpNDoc: [*Full featured EPub generator*][*Free PDF documentation generator*]*

<span id="_topic_Add" class="anchor"></span>**Add()**

**Description**

The add method allows you to add plots or line objects to the chart. When a new plot object is added using Add(), this automatically creates a data series of the type DataSeries, which is attached to this object. The value collection allows you to reference and access this data series.\
\
Add() can be used with the Initialize() and the OnBarUpdate() methods.

**Parameter**

plot – a [*Plot*] object\
line – a [*Line*] object

**Usage**

**Add**(Plot plot)

**Add**(Line line)

**Example**

\#**region** Usings

**using** System;

**using** System.Collections.Generic;

**using** System.ComponentModel;

**using** System.Drawing;

**using** System.Linq;

**using** System.Xml;

**using** System.Xml.Serialization;

**using** AgenaTrader.API;

**using** AgenaTrader.Custom;

**using** AgenaTrader.Plugins;

**using** AgenaTrader.Helper;

\#**endregion**

**namespace** AgenaTrader.UserCode

{

\[**Description**("Enter the description for the new custom indicator here")\]

**public** class MyIndicator : UserIndicator

{

**protected** override void **Initialize**()

{

// Two blue lines will be placed into the chart, one at 70 and the other at 30

**Add**(**new Line**(Color.Blue, 70, "UpperLine"));

**Add**(**new Line**(Color.Blue, 30, "LowerLine"));

// Add 2 plots

**Add**(**new Plot**(Color.Red, "myFastSMA"));

**Add**(**new Plot**(Color.Blue, "mySlowSMA"));

}

**protected** override void **OnBarUpdate**()

{

//The set method is assigned to the value of the current bar

FastSMA.**Set**( **SMA**(8)\[0\] ); // is identical with Values\[0\].Set( SMA(8)\[0\] );

SlowSMA.**Set**( **SMA**(50)\[0\] ); // is identical with Values\[1\].Set( SMA(50)\[0\] );

}

// Two data series are made available here

// These are not necessary for the display of the indicator // With the help of these series, one indicator can access the other

// For example: double d = MyIndicator.FastSMA\[0\] - MyIndicator.SlowSMA\[0\];

\[**Browsable**(**false**)\]

\[**XmlIgnore**()\]

**public** DataSeries FastSMA

{

get { return Values\[0\]; }

}

\[**Browsable**(**false**)\]

\[**XmlIgnore**()\]

**public** DataSeries SlowSMA

{

get { return Values\[1\]; }

}

}

}

*Created with the Personal Edition of HelpNDoc: [*Free EPub and documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_Alert" class="anchor"></span>**Alert()**

**Description**

The alert method creates an acoustic and/or visual alarm.

**Usage**

**Alert**(string message, **bool** showMessageBox, string soundLocation);

Due to compatability reasons, an old signature is still used here. When using this method, the color settings and the “re-arm seconds” parameter are ignored.

**Alert**(string id, AlertPriority priority, string message, string soundLocation, **int** rearmSeconds, Color backColor, Color forColor);

**Return Value**

None

**Parameter**

  ---------------- -----------------------------------------------------------------------------------------------------------------------
  message          Alert text displayed within the messages tab
  soundLocation    Name of a sound file in the \*.wav format. If no path is specified, then “My Documents\\AgenaTrader\\Sounds\\ is used
  showMessageBox   If set to “true”, a message box will be displayed in addition to the sound
  ---------------- -----------------------------------------------------------------------------------------------------------------------

**Example**

// Message will be outputted if the SMA(20) crosses below the SMA(50)

**if** (**CrossBelow**(**SMA**(20), **SMA**(50), 1))

**Alert**("Check short signal!", **true**, "Alert4.wav");

To use music files in a different path, you need to specify the path:

string pathOfSoundfile = Environment.**GetFolderPath**(Environment.SpecialFolder.MyDocuments)+@"\\MyAlertSounds\\";

string nameOfSoundFile = "MyAlertSoundFile.wav";

**Alert**("Message text", **true**, pathOfSoundfile + nameOfSoundFile);

*Created with the Personal Edition of HelpNDoc: [*Full featured Help generator*]*

<span id="_topic_AllowRemovalOfDrawObjects" class="anchor"></span>**AllowRemovalOfDrawObjects**

**Description**

“AllowRemovalOfDrawObjects” is a property of indicators that can be set under [*Initialize()*].

**AllowRemovalOfDrawObjects = true**

Drawing objects that are drawn by an indicator or a strategy can be manually removed from the chart.

**AllowRemovalOfDrawObjects = false (default)**

Drawing objects that have been created by a strategy or indicator CANNOT be manually removed from the chart. They are removed once the indicator or strategy is removed.

This property can be queried and will return “true” or “false”.

**Usage**

AllowRemovalOfDrawObjects

**Example**

**protected** override void **Initialize**()

{

**Add**(**new Plot**(Color.Red, "MyPlot1"));

//Drawing objects can be manually removed from the chart

AllowRemovalOfDrawObjects = **true**;

}

*Created with the Personal Edition of HelpNDoc: [*Full featured multi-format Help generator*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_Attribute" class="anchor"></span>**Attribute**

Attribute is a component of the C\# language. Within AgenaScript, indicators, and strategies, you can use these attributes in the same manner as you would in C\#.\
\
Information regarding the usage of attributes can be found here:

[*http://msdn.microsoft.com/de-de/library/z0w1kczw%28v=vs.80%29.aspx*]

The most commonly used attributes in AgenaScript are:

-   [*Browsable*]

-   [*Category*]

-   [*ConditionalValue*]

-   [*Description*]

-   [*DisplayName*]

-   [*TimeFrameRequirements*]

-   [*XmlIgnore*]

*Created with the Personal Edition of HelpNDoc: [*Free help authoring tool*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_Browsable" class="anchor"></span>**Browsable**

Browsable is an *[Attribut]e* within AgenaScript.

AgenaScript uses public variables for entering parameters for indicators (such as periods for the SMA) and for outputting events and calculations within indicators (for example, data series).\
\
Variables used for entering parameters must be displayed in the properties dialog. Data series are exempt from this.\
\
Public variables with the browsable attribute set to false are not displayed within the properties dialog.

By default, browsable is set to true. Therefore, within a variable containing an entry parameter, the attribute does not need to be specified.

**Example for a parameter:**

The parameter should be displayed and queried in the properties window. Therefore browsable should be set to true.

\[Description("Numbers of bars used for calculations")\]

\[Category("Parameters")\]

public int Period

{

get { return period; }

set { period = Math.Max(1, value); }

}

**Example for a data series:**

\[Browsable(false)\]

\[DisplayName("Lower band")\]

\[XmlIgnore\]

public DataSeries Lower

{

get { return Values\[0\]; }

}

*Created with the Personal Edition of HelpNDoc: [*Full featured EBook editor*][*Free PDF documentation generator*]*

<span id="_topic_Category" class="anchor"></span>**Category**

Category is an *[Attribut]e* in AgenaScript.

The category attribute defines under which category in the properties dialog the parameter is shown.\
\
If this attribute is missing, the parameters category is accepted as the standard.

The following example shows how to create the new category “My Parameters” in the properties dialog:

\[Category("My Parameters")\]

\[DisplayName("Period number")\]

public double \_period

{

get { return \_period; }

set { \_period = value; }

}

![][11]

*Created with the Personal Edition of HelpNDoc: [*Full featured multi-format Help generator*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_ConditionalValue" class="anchor"></span>**ConditionalValue**

Conditional value is an *[Attribut]e* in AgenaScript.

Normally, when making comparisons within the ConditionEscort, the data series generated by indicators are used. One such example would be checking whether a moving average lies above or below a specific price value.\
An indicator can also yield values that are not contained within data series, such as values of the type int, double, char, Boolean, string, etc.\
\
To use these values within the scanner or ConditionEscort, they have to be labeled with the conditional value attribute.

\[**Browsable**(**false**)\]

\[XmlIgnore\]

\[ConditionalValue\]

**public int** PublicVariable

{

get

{

**Update**();

return \_internVariable;

}

}

*Created with the Personal Edition of HelpNDoc: [*Easy EBook and documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_Description" class="anchor"></span>**Description**

Description is an attribute in AgenaScript.

The description attribute is used in AgenaScript for classes and public variables.\
\
As an attribute of the class, the text is a description of the function of the entire indicator.

\[Description("Displays the tick count of a bar.")\]

public class TickCounter : UserIndicator

{

...

As an attribute of a public variable, the text is a description of the function of the parameter.

\[Description("Number of standard deviations")\]

\[DisplayName("\# of std. dev.")\]

public double NumStdDev

{

get { return numStdDev; }

set { numStdDev = Math.Max(0, value); }

}

The descriptions are displayed in the relevant properties dialog.

*Created with the Personal Edition of HelpNDoc: [*Easily create HTML Help documents*][*Full featured Help generator*]*

<span id="_topic_DisplayName" class="anchor"></span>**DisplayName**

Display name is an attribute in AgenaScript.

The display name attribute defines the text shown in the properties dialog for the parameter.

If this attribute is not specified, the name of the public variable is used.

\[Description("Number of standard deviations")\]

\[DisplayName("\# of std. dev.")\]

public double NumStdDev

{

get { return numStdDev; }

set { numStdDev = Math.Max(0, value); }

}

*Created with the Personal Edition of HelpNDoc: [*Easily create Web Help sites*][*Full featured Help generator*]*

<span id="_topic_TimeFrameRequirements" class="anchor"></span>**TimeFrameRequirements**

Timeframe requirements is an attribute in AgenaScripts.

If you want a script to use data from various timeframes, the class requires the attribute „TimeFrameRequirements“. You can specify multiple timeframes here:

\[**TimeFrameRequirements**("1 day")\]

\[**TimeFrameRequirements**("15 minutes", "1 day", "1 week")\]

The amount of data provided for the other timeframes will always be the same as the number of actual candles loaded into the chart. If there are 500 candles for a 5-minute chart, then 500 candles of another timeframe will also be loaded. In the first example above, 500 daily candles will be loaded. In the second example, 500 15-minute candles, 500 daily candles and 500 weekly candles will be loaded.\
\
The amount of data can become rather large very quickly, thus you should take precautions when using this attribute.

See [*MultiBars*].

**Important:**

If a class uses a different indicator that requires one or more secondary timeframes, then the “TimeFrameRequirements” attribute must be set for the class retrieving the data. An example for this can be seen here: [*GetDayBar*].

*Created with the Personal Edition of HelpNDoc: [*Easy EBook and documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_XmlIgnore" class="anchor"></span>**XMLIgnore**

XML ignore is an attribute in AgenaScript.

AgenaTrader saves all parameter settings for the indicators in a template. The template files are saved in an XML format. In order to avoid a parameter being saved as part of the template, the attribute XML ignore can be set.

To save parameters in an XML file, the values must be serialized. Under most circumstances, AgenaTrader performs this automatically. Self-defined data types cannot be serialized automatically, so in this case the programmer is responsible for the correct serialization.\
\
In the following example, the color and font are used as parameters of an indicator. AgenaTrader has two methods for serializing color and font information (TextColorSerialize and TextFontSerialize). Both parameters – TextColor and TextFont – thus need to be marked with the XML ignore parameter.

**private** Color \_textColor = Color.Blue;

**private** Font \_textFont = **new Font**("Arial", 12, FontStyle.Bold);

\[XmlIgnore\]

\[Description("Textcolor")\]

public Color TextColor

{

get { return \_textColor; }

set { \_textColor = value; }

}

\[Browsable(false)\]

public string TextColorSerialize

{

get { return SerializableColor.ToString(\_textColor); }

set { \_textColor = SerializableColor.FromString(value); }

}

\[XmlIgnore()\]

\[Description("TextFont")\]

public Font TextFont

{

get { return \_textFont; }

set { \_textFont = value; }

}

\[Browsable(false)\]

public string TextFontSerialize

{

get { return SerializableFont.ToString(\_textFont); }

set { \_textFont = SerializableFont.FromString(value); }

}

*Created with the Personal Edition of HelpNDoc: [*Full featured EBook editor*][*Free PDF documentation generator*]*

<span id="_topic_AutoScale" class="anchor"></span>**AutoScale**

**Description**

Auto scale is a property of indicators that can be set within the Initialize() method.

**AutoScale = true (default)**

The price axis (y-axis) of the chart is set so that all plots and lines of an indicator are visible.

**AutoScale = false**

Plots and lines of an indicator or strategy are not accounted for in the scaling of the y-axis. Therefore they may lie outside of the visible chart area.

This property can be queried and will return either “true” or “false”.

**Usage**

AutoScale

**Example**

**protected** override void **Initialize**()

{

**Add**(**new Plot**(Color.Red, "MyPlot1"));

//Scale the chart so that all drawing objects are visible

AutoScale = **true**;

}

*Created with the Personal Edition of HelpNDoc: [*Easy EBook and documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_BarsRequired" class="anchor"></span>**BarsRequired**

**Description**

The property “BarsRequired” determines how many historical bars are required for an indicator or a strategy to call up the OnBarUpdate() method for the first time and thus begin the calculations. Bars required should be set within the Initialize() method.\
\
The setting should be chosen carefully. If you require 100 days for the calculation of a moving average, then you should ensure that at least 100 days of historical data are loaded.\
\
The property can be queried in the script and will return an int value.

When OnBarUpdate is called up for the first time, the CurrentBar property is 0 regardless of the value of BarsRequired.

**Usage**

BarsRequired

**Example**

**protected** override void **Initialize**()

{

**Add**(**new Plot**(Color.Red, "MyPlot1"));

//The indicator requires a minimum of 50 bars loaded into the history

BarsRequired = 50;

}

*Created with the Personal Edition of HelpNDoc: [*Free help authoring environment*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_CalculateOnBarClose" class="anchor"></span>**CalculateOnBarClose**

**Description**

The property “CalculateOnBarClose” determines the events for which AgenaTrader can call up the OnBarUpdate() method.

**CalculateOnBarClose = true\
**OnBarUpdate() is called up when a bar is closed and the next incoming tick creates a new bar.

**CalculateOnBarClose = false\
**OnBarUpdate() is called up for each new incoming tick.\
If you are running AgenaTrader on older hardware, this may cause performance issues with instruments that are highly liquid.\
\
The property can be queried in the script and will return a value of the type Boolean (true or false).\
\
CalculateOnBarClose can be used within Initialize() and also within OnBarUpdate().\
\
OnBarUpdate is only called up for the closing price of each bar with historical data, even if CalculateOnBarClose is set to false.\
\
When an indicator is called up by another indicator, the CalculateOnBarClose property of the retrieved indicator overwrites the indicator performing the retrieving.

**Usage**

CalculateOnBarClose

**More Information**

See [*Bars*].

**Example**

**protected** override void **Initialize**()

{

//Indicator calculation should only occur when a bar has closed/finished

CalculateOnBarClose = **true**;

}

*Created with the Personal Edition of HelpNDoc: [*Free help authoring environment*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_ChartControl" class="anchor"></span>**ChartControl**

Chart control is an object that provides reading access of various properties for the chart.

The important properties are:

-   ChartFontColor, BackColor

-   UpColor, DownColor

-   Font

-   BarMarginLeft, BarMarginRight

-   BarSpace, BarWidth

-   BarsPainted

-   FirstBarPainted, LastBarPainted

-   BarsVisible

-   FirstBarVisible, LastBarVisible

-   GetXByBarIdx, GetYByValue

An example can be seen here: [*PlotSample*].

**BarsPainted und BarsVisible:**

BarsPainted contains the number of bars that a chart *could* display from the left to right border with the current width and distance of the candles.\
BarsVisible contains the number of bars actually visible.

**FirstBarPainted und FirstBarVisible:**

FirstBarPainted contains the number of the bar that *would* be displayed on the left border of the chart.

FirstBarVisible contains the number of the bar that is actually shown as the first bar on the left side of the chart area.

Example: the chart has been moved so that the first bar of the chart is now in the middle of the chart.

FirstBarPainted would be negative.

FirstBarVisible would be 0.

**LastBarPainted und LastBarVisible:**

LastBarPainted contains the number of the bar that *would* be displayed on the right border of the chart.

LastBarVisible contains the number of the bar that is actually displayed on the right side of the chart.

Example: the chart has been moved so that the last bar of the chart is displayed in the middle section.

LastBarPainted would be larger than Bars.Count.

LastBarVisible would be Bars.Count -1.

*Created with the Personal Edition of HelpNDoc: [*Free EPub and documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_ClearOutputWindow" class="anchor"></span>**ClearOutputWindow()**

**Description**

The ClearOutputWindow() method empties the output window. The method can be used within Initialize() as well as within OnBarUpdate().\
\
The output window contains all outputs that have been created with the [*Print()*] command.\
Using the output window is a great method for code debugging.

**Usage**

ClearOutputWindow()

**Parameter**

none

**Return Value**

none

**Example**

**protected** override void **Initialize**()

{

**Add**(**new Plot**(Color.Red, "MyPlot1"));

// Delete the content of the output window

ClearOutputWindow();

}

*Created with the Personal Edition of HelpNDoc: [*iPhone web sites made easy*]*

<span id="_topic_CrossAbove" class="anchor"></span>**CrossAbove()**

**Description**

The CrossAbove() method allows you to check whether a crossing of two values has occurred (from bottom to top) within a predefined number of periods. The values can be a market price, an indicator, a data series or a constant value.

See [*CrossBelow()*], [*Rising()*], [*Falling()*].

**Usage**

**CrossAbove**(IDataSeries series1, **double value**, **int** lookBackPeriod)

**CrossAbove**(IDataSeries series1, IDataSeries series2, **int** lookBackPeriod)

**Return Value**

**true** a cross has occurred\
**false** a cross has not occurred

**Parameter**

  --------------------- ----------------------------------------------------------
  lookBackPeriod        Number of bars within which a cross will be searched for
  series1 und series2   A data series such as an indicator, close, high, etc.
  value                 A constant value of the type double
  --------------------- ----------------------------------------------------------

**Example**

// Puts out a notice if the SMA(20) crosses above the SMA(50)

**if** (**CrossAbove**(**SMA**(20), **SMA**(50), 1))

**Print**("SMA(20) has risen above SMA(50)!");

// Puts out a notice if the SMA(20) crosses above the value of 40

**if** (**CrossAbove**(**SMA**(20), 40, 1))

**Print**("SMA(20) has risen above 40!");

// Put out a notice for a long entry if the SMA(20) has crossed above the SMA(50) within the last 5 bars.

**if** (**CrossAbove**(**SMA**(20), **SMA**(50), 1) && Close\[0\] &gt; Close\[1\])

**Print**("Long entry !!!");

*Created with the Personal Edition of HelpNDoc: [*Full featured Help generator*]*

<span id="_topic_CrossBelow" class="anchor"></span>**CrossBelow()**

**Description**

Using the CrossBelow() method, you can test whether or not a cross below has occurred within a predefined number of periods. The values can be the market price, an indicator, any data series, or a constant value.

See [*CrossAbove()*], [*Rising()*], [*Falling()*].

**Usage**

**CrossBelow**(IDataSeries series1, **double value**, **int** lookBackPeriod)

**CrossBelow**(IDataSeries series1, IDataSeries series2, **int** lookBackPeriod)

**Return Value**

**true** a cross has occurred\
**false** a cross has not occurred

**Parameter**

  --------------------- ----------------------------------------------------------
  lookBackPeriod        Number of Bars within which a cross will be searched for
  series1 und series2   A data series such as an indicator, close, high etc.
  value                 A constant value of the type double
  --------------------- ----------------------------------------------------------

**Example**

// Puts out a notice if the SMA(20) crosses below the SMA(50)

**if** (**CrossBelow**(**SMA**(20), **SMA**(50), 1))

**Print**("SMA(20) has fallen below SMA(50)!");

// Puts out a notice if the SMA(20) falls below the value of 40

**if** (**CrossBelow**(**SMA**(20), 40, 1))

**Print**("SMA(20) has fallen below 40!");

// Puts out a notice for a short entry if a crossing of the SMA(20) below the SMA(50) has occurred within the last 5 bars.

.

**if** (**CrossBelow**(**SMA**(20), **SMA**(50), 1) && Close\[1\] &gt; Close\[0\])

**Print**("Short entry !!!");

*Created with the Personal Edition of HelpNDoc: [*Free help authoring tool*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_CurrentBar1" class="anchor"></span>**CurrentBar**

**Description**

Current bar is a method of indexing bars used in the OnBarUpdate() method. If a chart contains 500 bars and an indicator is to be calculated on the basis of these, then AgenaTrader will begin calculating from the oldest bar. The oldest bar receives the number 0. Once the calculation for this bar has been completed, the OnBarUpdate() method is called up for the next bar, which in turn receives the number 1. This continues until the last bar, which receives a value of 500.

**Parameter**

none

**Return Value**

Current bar is a variable of the type int, which always contains the number of the bar currently being used.

**Usage**

CurrentBar

**More Information**

The OnBarUpdate() method uses numbering different from that of CurrentBar in terms of the [*Barindex*][*Bars*]. Understanding this difference is of great importance, which is why we ask you to please read the following paragraph carefully:

CurrentBar numbers continuously from the oldest to youngest bar starting with 0. The BarIndex for the youngest bar is always 0. In the example referenced below this paragraph, Time\[0\] stands for the timestamp of the current bar. The index of the oldest bar always has 1 added to it. Thus a logical numbering of barsAgo is possible. The timestamp for the bar of 5 periods ago is Time\[5\].\
\
For using multiple timeframes (multi-bars) in an indicator, see CurrentBars.

**Example**

**protected** override void **OnBarUpdate**()

{

**Print**("Call of OnBarUpdate for bar nr. " + CurrentBar + " of " + Time\[0\]);

}

*Created with the Personal Edition of HelpNDoc: [*Easily create CHM Help documents*][*Full featured Help generator*]*

<span id="_topic_DatafeedHistoryPeriodicity" class="anchor"></span>**DatafeedHistoryPeriodicity**

**Description**

Datafeed history periodicity is a data type.

**Definition**

public enum DatafeedHistoryPeriodicity

-   DatafeedHistoryPeriodicity.Tick

-   DatafeedHistoryPeriodicity.Second

-   DatafeedHistoryPeriodicity.Minute

-   DatafeedHistoryPeriodicity.Day

-   DatafeedHistoryPeriodicity.Week

-   DatafeedHistoryPeriodicity.Month

-   DatafeedHistoryPeriodicity.Range

-   DatafeedHistoryPeriodicity.Volume

See [*TimeFrame*], [*TimeFrames*].

*Created with the Personal Edition of HelpNDoc: [*Easily create Help documents*][*Full featured Help generator*]*

<span id="_topic_Datenserien" class="anchor"></span>**DataSeries**

**Description**

Data series (data rows) are an easy yet powerful method of saving additional values for individual bars. For example, when calculating the smoothing average, each bar is assigned the value calculated for this bar.\
A data series is an array that contains as many elements as there are bars displayed in a chart. AgenaTrader ensures that data series are correctly synchronized with the bars.\
\
Data series are used in exactly the same way as the close or time series. They can therefore also be used for the input data for various indicators.\
\
In the table below you will find 4 newly created data series (highlighted). Each data series has exactly one value of a special data type (int, bool, string) attached to it per bar. The indexing with barsAgo is thus identical to the data series provided by the system.

![][12]

**Usable Data Series in AgenaTrader**

-   [*BoolSeries*]

-   [*DataSeries*]

-   [*DateTimeSeries*]

-   [*FloatSeries*]

-   [*IntSeries*]

-   [*LongSeries*]

-   [*StringSeries*]

In addition, there are also data series such as ColorSeries, although these are only used for internal purposes and should not be used directly.\
To change the color of plots, please use [*PlotColors*].

**Set(), Reset() und ContainsValue()**

Each data series contains a **Set()**, **Reset()** and **ContainsValue()** method.\
\
With Set(value) or Set(int barsAgo, value) you can place values into the data series for the current position, or in this case into the barsAgo position.\
\
With Reset() or Reset(int barsAgo) you can delete a value from the data series for the current position or for the barsAgo position. This has the result that no valid value exists at this position any more.\
\
Programming with the help of the reset method can simplify otherwise complex logic. This is especially true for Boolean series, where only “true” or “false” values can be included.\
\
The ContainsValue() checks whether a data series has a value for a specific position.

**Information about Data Types**

[*http://msdn.microsoft.com/de-de/library/s1ax56ch%28v=vs.80%29.aspx*]

*Created with the Personal Edition of HelpNDoc: [*Easy EPub and documentation editor*][*Free PDF documentation generator*]*

<span id="_topic_BoolSeries" class="anchor"></span>**BoolSeries**

**Description**

Bool series is a data series that contains a Boolean value for each bar. The number of elements in this series correlates with the exact number of bars within the chart.

**Create New Bool Series**

In the area for the declaration of variables, simply declare a new variable:

//Variable declaration

**private** BoolSeries myBoolSeries;

With the Initialize() method, this variable assigns a new instance of the Bool series:

**protected** override void **Initialize**()

{

myBoolSeries = **new BoolSeries**(**this**);

CalculateOnBarClose = **true**;

}

**Assign Values**

Assigning a value to the data series for the current position:

myBoolSeries.**Set**(**true**);

Writing a value in the past into the data series:

myBoolSeries.**Set**(**int** barsAgo, **bool** Value);

**Delete Values**

Removing the current value for the data series:

myBoolSeries.**Reset**();

Removing a value in the past from the data series:

myBoolSeries.**Reset**(**int** barsAgo);

**Check Values for their Validity**

myBoolSeries.**ContainsValue**(**int** barsAgo);

**Read Value**

**Print** ("For the bar of " + Time\[0\] + " ago the value of the data series is: " + myBoolSeries\[0\]);

**Example**

**protected** override void **OnBarUpdate**()

{

**if** (Close\[0\] &gt; Open\[0\])

myBoolSeries.**Set**(**true**);

**else**

myBoolSeries.**Set**(**false**);

}

*Created with the Personal Edition of HelpNDoc: [*Full featured multi-format Help generator*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_DataSeries" class="anchor"></span>**DataSeries**

**Description**

Data series is a [*DataSeries*][*Data series*] that can contain a double value for each bar. The number of elements in this series corresponds to the exact number of bars within the charts.

Data series for double values are the data series most commonly used for indicators.

**Create a New Data Series**

In the declaration area for variables:

//Variable declaration

**private** DataSeries myDataSeries;

With the Initialize() method, this variable is assigned a new instance:

**protected** override void **Initialize**()

{

myDataSeries = **new DataSeries**(**this**);

CalculateOnBarClose = **true**;

}

**Assign Values**

Assigning a value to the data series for the current position:

myDataSeries.**Set**(**true**);

Writing a value in the past into the data series:

myDataSeries.**Set**(**int** barsAgo, **duble** Value);

**Delete Values**

Removing the current value from the data series:

myDataSeries.**Reset**();

Removing a value in the past from the data series:

myDataSeries.**Reset**(**int** barsAgo);

**Check Values for their Validity**

myDataSeries.**ContainsValue**(**int** barsAgo);

**Read Value**

**Print** ("For the bar from " + Time\[0\] + " ago the value for the data series is: " + myDataSeries\[0\]);

**Example**

//Saves the span between the high and low of a bar

myDataSeries.**Set**(Math.**Abs**(High\[0\]-Low\[0\]));

*Created with the Personal Edition of HelpNDoc: [*Free CHM Help documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_DateTimeSeries" class="anchor"></span>**DateTimeSeries**

**Description**

Date time series is a [*DataSeries*][*Data series*] that can record a date time value for each bar. The number of elements in this series corresponds to the number of bars in the chart.

**Create a New Data Series**

Create a new variable in the declaration area:

//Variable declaration

**private** DateTimeSeries myDataSeries;

Assign a new instance of DateTimeSeries for the variable with the Initialize() method:

**protected** override void **Initialize**()

{

myDataSeries = **new DateTimeSeries**(**this**);

CalculateOnBarClose = **true**;

}

**Assign Values**

Assigning a value to the current position of the data series:

myDataSeries.**Set**(DateTime Value);

Writing a value from the past into the data series:

myDataSeries.**Set**(**int** barsAgo, DateTime Value);

**Delete Values**

Removing the current value from the data series:

myDataSeries.**Reset**();

Remove a past value from the data series:

myDataSeries.**Reset**(**int** barsAgo);

**Check Values for their Validity**

myDataSeries.**ContainsValue**(**int** barsAgo);

**Read Value**

**Print** ("For the bar from " + Time\[0\] + " ago the value of the data series is: " + myDataSeries\[0\]);

**Example**

//Saves the difference of -6 hours (eastern time, New York) for a time zone conversion

myDataSeries.**Set**(Time\[0\].**AddHours**(-6);

*Created with the Personal Edition of HelpNDoc: [*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_FloatSeries" class="anchor"></span>**FloatSeries**

**Description**

Float series is a DataSeries that contains a float value for each bar in the chart. The number of elements in this series corresponds to the number of bars within the chart.

**Create a New Data Series**

Create a new variable in the declaration area:

//Variable declaration

**private** FloatSeries myDataSeries;

Assign a new instance of the FloatSeries to the variable with the Initialize() method:

**protected** override void **Initialize**()

{

myDatatSeries = **new FloatSeries**(**this**);

CalculateOnBarClose = **true**;

}

**Assign Values**

Assigning a value to the current position of the data series

myDataSeries.**Set**(**float** Value);

Writing a value from the past into the data series:

myDataSeries.**Set**(**int** barsAgo, **float** Value);

**Delete Values**

Removing the current value from the data series:

myDataSeries.**Reset**();

Removing a value located in the past from the data series:

myDataSeries.**Reset**(**int** barsAgo);

**Check Values for their Validity**

myDataSeries.**ContainsValue**(**int** barsAgo);

**Read Value**

**Print** ("For the bar from " + Time\[0\] + " ago the value for the data series is: " + myDataSeries\[0\]);

**Example**

//Saves the span between the high and the low of a bar

myDataSeries.**Set**(Math.**Abs**((**float**) High\[0\] - (**float**) Low\[0\]));

*Created with the Personal Edition of HelpNDoc: [*Free HTML Help documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_IntSeries" class="anchor"></span>**IntSeries**

**Description**

Int series is a data series that can assign an integer value for each bar. The number of elements in this series corresponds to the number of bars within the chart.

**Create a New Data Series**

Create a new variable in the declaration area:

//Variable declaration

**private** IntSeries myDataSeries;

Assign an instance of the int series to the variable with the Initialize() method:

**protected** override void **Initialize**()

{

myDataSeries = **new IntSeries**(**this**);

CalculateOnBarClose = **true**;

}

**Assign Values**

Assigning a value to the current position of the data series

myDataSeries.**Set**(**int** Value);

Writing a value from the past into the data series:

myDataSeries.**Set**(**int** barsAgo, **int** Value);

**Delete Values**

Removing the current value from the data series:

myDataSeries.**Reset**();

Removing a value located in the past from the data series:

myDataSeries.**Reset**(**int** barsAgo);

**Check Values for their Validity**

myDataSeries.**ContainsValue**(**int** barsAgo);

**Read Value**

**Print** (For the bar from + Time\[0\] + the value of the data series is:+ myDataSeries\[0\]);

**Example**

//Saves the span in ticks between high and low for each bar

myDataSeries.**Set**((**int**) ((High\[0\] - Low\[0\]) / TickSize));

*Created with the Personal Edition of HelpNDoc: [*Full featured multi-format Help generator*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_LongSeries" class="anchor"></span>**LongSeries**

**Description**

Long series is a data series that can include an integer value for each bar. The number of elements in this series corresponds to the number of bars within the chart.

**Create a New Data Series**

Create a new variable in the declaration area:

//Variable declaration

**private** LongSeries myDataSeries;

Assign a new instance of the long series to the variable with the Initialize() method:

**protected** override void **Initialize**()

{

myDataSeries = **new LongSeries**(**this**);

CalculateOnBarClose = **true**;

}

**Assign Values**

Assigning a value to the current position of the data series:

myDataSeries.**Set**(**long** Value);

Writing a value from the past into the data deries:

myDataSeries.**Set**(**int** barsAgo, **long** Value);

**Delete Values**

Removing the current value from the data series:

myDataSeries.**Reset**();

Removing a value located in the past from the data series:

myDataSeries.**Reset**(**int** barsAgo);

**Check Values for their Validity**

myDataSeries.**ContainsValue**(**int** barsAgo);

**Read Value**

**Print** (For the bar from + Time\[0\] + the value of the data series is:+ myDataSeries\[0\]);

**Example**

//Saves the span of ticks between high and low for each bar

myDataSeries.**Set**((**long**) ((High\[0\] - Low\[0\]) / TickSize));

*Created with the Personal Edition of HelpNDoc: [*Write EPub books for the iPad*][*Free PDF documentation generator*]*

<span id="_topic_StringSeries" class="anchor"></span>**StringSeries**

**Description**

String series is a data series for string values that are saved for each bar. The number of elements in this series corresponds to the number of bars within the chart.

**Create a New Data Series**

Create a new variable in the declaration area:

//Variable declaration

**private** StringSeries myDataSeries;

Assign an instance of string series to the variable with the Initialize() method:

**protected** override void **Initialize**()

{

myDataSeries = **new StringSeries**(**this**);

CalculateOnBarClose = **true**;

}

**Assign Values**

Assigning a value to the current position of the data series:

myDataSeries.**Set**(string Value);

Writing a value from the past into the data series:

myDataSeries.**Set**(**int** barsAgo, string Value);

**Delete Values**

Remove the current value from the data series:

myDataSeries.**Reset**();

Remove a value located in the past from the data series:

myDataSeries.**Reset**(**int** barsAgo);

**Check Values for their Validity**

myDataSeries.**ContainsValue**(**int** barsAgo);

**Read Value**

**Print** (For the bar from + Time\[0\] + the value of the data series is:+ myDataSeries\[0\]);

**Example**

//Save the current calendar day for each bar (Monday… Tuesday etc.)

myDataSeries.**Set**(string.**Format**("{0:dddd}", Time\[0\]));

*Created with the Personal Edition of HelpNDoc: [*Easy to use tool to create HTML Help files and Help web sites*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_DayOfWeek" class="anchor"></span>**DayOfWeek**

**Description**

“DayOfWeek” outputs the date-time value (such as a timestamp) for each bar.

Of course, all other methods defined within the C\# language for usage of date-time objects are also available, such as day, month, year, hour, minute, second, day of week etc.

See [*http://msdn.microsoft.com/de-de/library/03ybds8y.aspx*]

**Definition**

Property DayOfWeek

public enum DayOfWeek

-   DayOfWeek.Monday

-   DayOfWeek.Tuesday

-   DayOfWeek.Wednesday

-   DayOfWeek.Thursday

-   DayOfWeek.Friday

-   DayOfWeek.Saturday

-   DayOfWeek.Sunday

**Example**

//Outputs the weekday for each bar

**Print**(Time\[0\].DayOfWeek);

//Do not execute trades on a Friday

**if** (Time\[0\].DayOfWeek == DayOfWeek.Friday)

return;

*Created with the Personal Edition of HelpNDoc: [*Free help authoring environment*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_Displacement" class="anchor"></span>**Displacement**

**Description**

By implementing “Displacement”, you can shift a drawn indicator line right or left along the x-axis.\
\
This property can be queried within the script and will return an int value.

Blue line: Displacement = 0 (Original)\
Red line: Displacement = -5\
Green line: Displacement = +5

![][13]

**Usage**

Displacement

**Parameter**

int Offset Number of bars by which the indicator is to be moved.

**Example**

**protected** override void **Initialize**()

{

**Add**(**new Plot**(Color.Red, "MyPlot1"));

//Displacement of the plot by one bar to the right

Displacement = 1;

}

*Created with the Personal Edition of HelpNDoc: [*iPhone web sites made easy*]*

<span id="_topic_DisplayInDataBox" class="anchor"></span>**DisplayInDataBox**

**Description**

The property “DisplayInDataBox” states whether the value of an indicator is contained in the data box of the chart or not.

The property can be queried in the script and returns a value of the type Boolean (true or false).

**DisplayInDataBox = true (default)**

The indicator values are displayed in the data box.

**DisplayInDataBox = false**

The indicator values are not displayed in the data box.

The following image displays the values of 3 smoothed averages in the data box.

![][14]

**Usage**

DisplayInDataBox

**Example**

**protected** override void **Initialize**()

{

**Add**(**new Plot**(Color.Red, "MyPlot1"));

//Values will not be shown in the data box

DisplayInDataBox = **false**;

}

*Created with the Personal Edition of HelpNDoc: [*Easy EBook and documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_DrawOnPricePanel" class="anchor"></span>**DrawOnPricePanel**

**Description**

The property “DrawOnPricePanel” determines the panel in which the drawing objects are drawn.

**DrawOnPricePanel = true (default)**

Drawing objects are shown in the price chart

**DrawOnPricePanel = false**

Drawing objects are drawn in the panel (subchart) assigned to the indicator

If the indicator is already assigned to the price chart (overlay = true) then this property has no effect, meaning that no additional subchart is opened.\
\
The property can be queried within the script and returns a Boolean value.

**Usage**

DrawOnPricePanel

**Example**

**protected** override void **Initialize**()

{

// Indicator is drawn in a new subchart

Overlay = **false**;

**Add**(**new Plot**(Color.Red, "MyPlot1"));

// Drawing object is drawn in the price chart

DrawOnPricePanel = **true**;

}

**protected** override void **OnBarUpdate**()

{

// Draws a vertical line in the price chart for the bar from 5 minutes ago

**DrawVerticalLine**("MyVerticalLine", 5, Color.Black);

}

*Created with the Personal Edition of HelpNDoc: [*Easily create Web Help sites*][*Full featured Help generator*]*

<span id="_topic_Falling" class="anchor"></span>**Falling()**

**Description**

The Falling() method allows you to test whether an “is falling” condition exists, i.e. whether the current value is smaller than the value of the previous bar.

See [*CrossAbove()*], [*CrossBelow()*], [*Rising()*].

**Usage**

**Falling**(IDataSeries series)

**Return Value**

**true** If the data series is falling\
**false** If the data series is not falling

**Parameter**

series a data series such as an indicator, close, high etc.

**Example**

// Check whether SMA(20) is falling

**if** (**Falling**(**SMA**(20)))

**Print**("The SMA(20) is currently falling.");

*Created with the Personal Edition of HelpNDoc: [*Easily create EBooks*][*Full featured Help generator*]*

<span id="_topic_FarbenColors" class="anchor"></span>**Colors**

AgenaScript provides you with the following commands for defining colors and making color changes to the chart:

-   [*BarColor*] Color of a bar

-   [*BackColor*] Background color of the chart

-   [*BackColorAll*] Background color of the chart and all panels

ChartControl.UpColor Color of up ticks (up bars)\
ChartControl.DownColor Color of down ticks (down bars)

For each bar, its colors are saved in the following data series. If these data series are written in, the color of the referenced bar will change.

-   [*BarColorSeries*]

-   [*CandleOutlineColorSeries*]

-   [*BackColorSeries*]

-   [*BackColorAllSeries*]

*Created with the Personal Edition of HelpNDoc: [*Free iPhone documentation generator*][*iPhone web sites made easy*]*

<span id="_topic_BarColor" class="anchor"></span>**BarColor**

**Description**

Bar color changes the color of a bar.

See [*Colors*], [*BarColorSeries*], [*BackColor*], [*BackColorAll*], [*CandleOutlineColor*].

**Parameter**

a color object of the type “public struct color”

**Usage**

BarColor

**Example**

// If the closing price is above the SMA(14), color the bar orange

**if** (Close\[0\] &gt; **SMA**(14)\[0\]) BarColor = Color.Orange;

![][15]

*Created with the Personal Edition of HelpNDoc: [*Easily create EBooks*][*Full featured Help generator*]*

<span id="_topic_BackColor" class="anchor"></span>**BackColor**

**Description**

Back color changes the background color of a bar or gives the current background color of a bar when queried.

See [*Colors*], [*BarColor*], [*BackColorAll*], [*CandleOutlineColor*].

**Parameter**

a color object of the type “public struct color”

**Usage**

BackColor

**Example**

// Every Monday, change the bar background color to blue

**if** (Time\[0\].DayOfWeek == DayOfWeek.Monday)

BackColor = Color.Blue;

![][16]

// Changing the bar background color depending on a smoothing average

// Market price above the SMA(14) to green

// Market price below the SMA(14) to maroon

BackColor = **SMA**(14)\[0\] &gt;= Close\[0\] ? Color.Maroon : Color.LimeGreen;

![][17]

*Created with the Personal Edition of HelpNDoc: [*Full featured Help generator*]*

<span id="_topic_BackColorAll" class="anchor"></span>**BackColorAll**

**Description**

Back color all changes the background color of a bar within the chart window and in all subcharts.

See [*Colors*], [*BarColor*], [*BackColor*], [*CandleOutlineColor*].

**Parameter**

A color object of the type “public struct color”

**Usage**

BackColorAll

**Example**

// Every Monday, change the bar background color to blue

**if** (Time\[0\].DayOfWeek == DayOfWeek.Monday)

BackColorAll = Color.Blue;

![][18]

*Created with the Personal Edition of HelpNDoc: [*Free help authoring tool*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_BarColorSeries" class="anchor"></span>**BarColorSeries**

**Description**

Bar color series is a data series containing the color for each bar.

See [*Colors*], [*BackColorSeries*], [*BackColorAllSeries*], [*CandleOutlineColorSeries*].

**Parameter**

a color object of the type “public struct color”

int barsAgo

**Usage**

BarColorSeries

BarColorSeries\[**int** barsAgo\]

When using the method with an index \[**int** barsAgo\] the color for the referenced bar will be changed or returned.

**Caution: Only the color of a bar whose color has been explicitly changed beforehand will be returned. In all other cases, the “Color.Empty” value will be returned.**

**Example**

**protected** override void **OnBarUpdate**()

{

**if** (CurrentBar == Bars.Count-1-(CalculateOnBarClose?1:0))

{

> // Color the current bar blue
>
> // This is identical to BarColor = color.Blue

BarColorSeries\[0\] = Color.Blue;

// Color the previous bars green

BarColorSeries\[1\] = Color.Orange;

// Color the third bar yellow

BarColorSeries\[2\] = Color.Yellow;

}

}

![][19]

*Created with the Personal Edition of HelpNDoc: [*Full featured Documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_BackColorSeries" class="anchor"></span>**BackColorSeries**

**Description**

Back color series is a data series containing the background color for each bar. If the background color for the subcharts is to be included, please use “BackColorAllSeries” instead.

See [*Colors*], [*BarColorSeries*][*BarColor*], [*BackColorAllSeries*][] [*CandleOutlineColorSeries*].

**Parameter**

a color object of the type “public struct color”

int barsAgo

**Usage**

BackColorSeries

BackColorSeries\[**int** barsAgo\]

When using this method with an index \[**int** barsAgo\] the background color for the referenced bar will be outputted.

**Example**

// Which background color does the current bar have?

**Print** (BackColorSeries\[0\]);

// Set the current bar’s background color to blue

// This is identical to BackColor = Color.Blue

BackColorSeries\[3\] = Color.Blue;

// Set background color for the previous bar to green

BackColorSeries\[1\] = Color.Green;

*Created with the Personal Edition of HelpNDoc: [*Easily create iPhone documentation*][*iPhone web sites made easy*]*

<span id="_topic_BackColorAllSeries" class="anchor"></span>**BackColorAllSeries**

**Description**

Back color all series is a data series containing the background color for each bar. The difference to BackColorSeries is that the background color of the subchart is included.

See [*Colors*], [*BarColorSeries*], [*BackColorSeries*], [*CandleOutlineColorSeries*].

**Parameter**

a color object of the type “public struct color”

int barsAgo

**Usage**

BackColorAllSeries

BackColorAllSeries\[**int** barsAgo\]

When using the method with an index \[**int** barsAgo\] the background color for the referenced bar will be changed or returned.

**Example**

See [*BackColorSeries*].

*Created with the Personal Edition of HelpNDoc: [*iPhone web sites made easy*]*

<span id="_topic_CandleOutlineColor" class="anchor"></span>**CandleOutlineColor**

**Description**

Candle outline color changes the border/outline color (including the wick) of a bar.

If the color of the bar is changed using BarColor and the outline is not changed using CandleOutlineColor, the outline color is adjusted to match the color of the bar.

See [*Colors*], [*BarColor*], [*BackColor*], [*BackColorAll*].

**Parameter**

a color object of the type “public struct color”

**Usage**

CandleOutlineColor

**Example**

**if** (**SMA**(14)\[0\] &gt; **SMA**(200)\[0\])

CandleOutlineColor = Color.LimeGreen;

**else**

CandleOutlineColor = Color.Red;

![][20]

*Created with the Personal Edition of HelpNDoc: [*Free Web Help generator*][*Free PDF documentation generator*]*

<span id="_topic_CandleOutlineColorSeries" class="anchor"></span>**CandleOutlineColorSeries**

**Description**

Candle outline color series is a data series that saves the outline color for each bar.

See [*Colors*], [*BarColorSeries*], [*BackColorSeries*], [*BackColorAllSeries*].

**Parameter**

a color object of the type “public struct color”

int barsAgo

**Usage**

CandleOutlineColorSeries\
CandleOutlineColorSeries\[**int** barsAgo\]

When using this method with an index \[**int** barsAgo\] the border color for the referenced bar will be outputted.

**Caution: Color.Empty will be outputted for a bar unless it has been previously changed.**

**Example**

// Set the outline color of the current bar to blue

CandleOutlineColorSeries\[0\] = Color.Blue;

// Change the outline color to the chart default value

CandleOutlineColorSeries\[0\] = Color.Empty;

*Created with the Personal Edition of HelpNDoc: [*Create iPhone web-based documentation*][*iPhone web sites made easy*]*

<span id="_topic_FirstTickOfBar" class="anchor"></span>**FirstTickOfBar**

**Description**

FirstTickOfBar is a property of the type “bool” that returns “true” if the currently incoming tick is associated with a new bar. This means that this tick is the first tick of a new bar.\
\
This property can only be meaningfully applied when the indicator or strategy is running in the tick-by-tick mode, meaning that CalculateOnBarClose = false and the data feed is able to output real-time values.\
\
When using end-of-day data in a daily chart, the “FirstTickOfBar” is always true for the last bar.\
FirstTickOfBar should not be used outside of the OnBarUpdate() method.\
See [*Bars.TickCount*].

**Usage**

FirstTickOfBar

**Example**

// Within a tick-by-tick strategy, execute one part bar-by-bar only

**if** (FirstTickOfBar)

{

**if** (**CCI**(20)\[1\] &lt; -250)

**EnterLong**();

return;

}

*Created with the Personal Edition of HelpNDoc: [*Free HTML Help documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_FirstTickOfBarMtf" class="anchor"></span>**FirstTickOfBarMtf**

**Description**

FirstTickOfBarMtf is the **m**ulti-**t**ime **f**rame variant of the [*FirstTickOfBar*] property.

The setting of CalculateOnBarClose only affects the primary timeframe (chart timeframe). When working with multi-bars, the ticks of the secondary timeframes are provided on a tick-by-tick basis independently of the CalculateOnBarClose setting.\
\
With the help of FirstTickOfBarMtf, it is possible to determine when a new bar has begun in a secondary timeframe.

**Usage**

FirstTickOfBarMtf(BarsInProgress)

**Parameter**

FirstTickOfBarMtf(BarsInProgress).

See [*BarsInProgress*].

**Example**

**if** (**FirstTickOfBarMtf**(BarsInProgress))

**Print**("A new bar has begun.");

*Created with the Personal Edition of HelpNDoc: [*Full featured Documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_GetCurrentAsk" class="anchor"></span>**GetCurrentAsk()**

**Description**

The GetCurrentAsk() method returns the current value of the ask side of the order book. If no level 1 data is available to AgenaTrader, then this function simply outputs the last trade value.

See [*GetCurrentBid()*] and [*OnMarketData()*].

**Usage**

**GetCurrentAsk**()

**Return Value**

double value

**Parameter**

none

**Example**

If an entry condition is fulfilled, then 1 contract should be sold at the current ask price:

**private** IOrder entryOrder = **null**;

**protected** override void **OnBarUpdate**()

{

// Entry condition

**if** (Close\[0\] &lt; **SMA**(20)\[0\] && entryOrder == **null**)

// Sell 1 contract at the current ask price

entryOrder = **SubmitOrder**(0, OrderAction.SellShort, OrderType.Limit, 1, **GetCurrentAsk**(), 0, "", "Enter short");

}

*Created with the Personal Edition of HelpNDoc: [*Easily create EBooks*][*Full featured Help generator*]*

<span id="_topic_GetCurrentBid" class="anchor"></span>**GetCurrentBid()**

**Description**

The GetCurrentBid() method returns the current value of the bid side of the order book. If no level 1 data is available to AgenaTrader, then the function outputs the last traded price.

See [*GetCurrentAsk()*] and [*OnMarketData()*].

**Usage**

**GetCurrentBid**()

**Return Value**

double value

**Parameter**

none

**Example**

If an entry condition is fulfilled, then 1 contract should be sold at the current bid price:

**private** IOrder entryOrder = **null**;

**protected** override void **OnBarUpdate**()

{

// Entry condition

**if** (Close\[0\] &gt; **SMA**(20)\[0\] && entryOrder == **null**)

// Sell 1 contract at the current bid price

entryOrder = **SubmitOrder**(0, OrderAction.Buy, OrderType.Limit, 1, **GetCurrentBid**(), 0, "", "Enter long");

}

*Created with the Personal Edition of HelpNDoc: [*Free Web Help generator*][*Free PDF documentation generator*]*

<span id="_topic_HighestBar" class="anchor"></span>**HighestBar**

**Description**

The HighestBar() method searches within a predetermined number of periods for the highest bar and outputs how many bars ago it can be found.

See [*LowestBar()*].

**Parameter**

period Number of bars within which the bar is searched for

series Every data series, such as close, high, low, etc.

**Return Value**

**int** barsAgo How many bars ago the high occurred

**Usage**

**HighestBar**(IDataSeries series, **int** period)

**Example**

// How many bars ago was the highest high for the current session?

**Print**(**HighestBar**(High, Bars.BarsSinceSession - 1));

// What value did the market price have at the highest high of the session?

**Print**("The highest price for the session was: " + Open\[**HighestBar**(High, Bars.BarsSinceSession - 1)\]);

*Created with the Personal Edition of HelpNDoc: [*Easily create Help documents*][*Full featured Help generator*]*

<span id="_topic_Historical" class="anchor"></span>**Historical**

**Description**

Historical allows you to check whether AgenaScript is working with historical or real-time data.\
\
As long as OnBarUpdate() is called up for historical data, then historical = true. As soon as live data is being used, then historical = false.\
\
During a backtest, historical is always true.

**Usage**

Historical

**Return Value**

**true** when using historical data\
**false** when using real-time data

**Example**

**protected** override void **OnBarUpdate**()

{

// only execute for real-time data

**if** (Historical) return;

// Trading technique

}

*Created with the Personal Edition of HelpNDoc: [*Easily create iPhone documentation*][*iPhone web sites made easy*]*

<span id="_topic_Initialize" class="anchor"></span>**Initialize()**

**Description**

The Initialize() method is called up once at the beginning of an indicator or strategy calculation. This method can be used to set indicator properties, initialize your own variables, or add plots.

**Parameter**

none

**Return Value**

none

**Usage**

**protected** override void **Initialize**()

**Important Keywords**

-   [*Add()*]

-   [*AllowRemovalOfDrawObjects*]

-   [*AutoScale*]

-   [*BarsRequired*]

-   [*CalculateOnBarClose*]

-   [*ClearOutputWindow()*]

-   [*Displacement*]

-   [*DisplayInDataBox*]

-   [*DrawOnPricePanel*]

-   [*InputPriceType*]

-   [*Overlay*]

-   [*PaintPriceMarkers*]

-   [*SessionBreakLines*]

-   [*VerticalGridLines*]

> **Additional Keywords for Strategies**

-   [*DefaultQuantity*]

-   [*EntriesPerDirection*]

-   [*EntryHandling*]

-   [*SetStopLoss()*]

-   [*SetProfitTarget()*]

-   [*SetTrailStop()*]

-   [*TimeInForce*]

-   [*TraceOrders*]

**More Information**

**Caution:**

The Initialize() method is not only called up at the beginning of an indicator or strategy calculation, but also if the chart is reloaded unexpectedly or if the properties dialog of indicators is opened and so on.\
\
Developers of custom AgenaScripts should NOT use this method for running their own routines, opening forms, performing license checks, etc. The OnStartUp() method should be used for these kind of tasks.

**Example**

**protected** override void **Initialize**()

{

**Add**(**new Plot**(Color.Blue, "myPlot"));

**ClearOutputWindow**();

AutoScale = **false**;

Overlay = **true**;

PaintPriceMarkers = **false**;

DisplayInDataBox = **false**;

CalculateOnBarClose = **true**;

}

*Created with the Personal Edition of HelpNDoc: [*Easily create Web Help sites*][*Full featured Help generator*]*

<span id="_topic_InitRequirements" class="anchor"></span>**InitRequirements()**

**Description**

The InitRequirements() method is called up once at the beginning of an indicator and/or strategy calculation. This method is only necessary when using multi-bars.\
\
Within InitRequirements, no other programming commands are executed. For initializing, the Initialize() or OnStartUp() method should be used.

**Parameter**

none

**Return Value**

none

**Example**

**protected** override void **InitRequirements**()

{

**Add**(DatafeedHistoryPeriodicity.Day, 1);

**Add**(DatafeedHistoryPeriodicity.Week, 1);

}

*Created with the Personal Edition of HelpNDoc: [*Full featured multi-format Help generator*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_InputPriceType" class="anchor"></span>**InputPriceType**

**Description**

The input price type property determines which price series is used by default when calculating an indicator, if no other data series is explicitly stated.\
\
InputPriceType can be set with the Initialize() method; this specification is then valid for all further calculations.\
\
If InputPriceType is in OnBarUpdate(), these changes are only valid starting with the next instruction.\
Every further appearance of InputPriceType will be ignored!

See [*PriceType*]

**Usage**

InputPriceType

**Example1**

**protected** override void **Initialize**()

{

**ClearOutputWindow**();

InputPriceType = PriceType.Low;

}

**protected** override void **OnBarUpdate**()

{

// The input data series for the indicator (input) is low

**Print**(Low\[0\] + " " + Input\[0\] + " " + InputPriceType);

}

**Example2**

**protected** override void **OnBarUpdate**()

{

// These values are identical

// since close is used as the input data series by default

**Print**(**SMA**(20)\[0\] + " " + **SMA**(Close, 20)\[0\]);

InputPriceType = PriceType.Low;

// From here on out, low is used instead of close

// Both values are identical

**Print**(**SMA**(20)\[0\] + " " + **SMA**(Low, 20)\[0\]);

InputPriceType = PriceType.High;

// The instructions will be ignored

// Input = low is still in effect

}

*Created with the Personal Edition of HelpNDoc: [*Full featured multi-format Help generator*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_Instrument" class="anchor"></span>**Instrument**

**Description**

With “instrument”, information concerning the trading instrument (stock, future etc.) is made available.

Detailed information can be found here: *Instruments*.

*Created with the Personal Edition of HelpNDoc: [*Easy to use tool to create HTML Help files and Help web sites*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_Line" class="anchor"></span>**Line()**

**Description**

A line object is used for drawing a horizontal line in the chart. Usually, these are upper and lower trigger lines for indicators such as the RSI (70 and 30).\
\
The lines described here are not to be confused with lines from the drawing objects (see “DrawHorizontalLine”).\
\
Line objects can be added to an indicator with the help of the Add() method, and with this, added to the lines collection.

See [*Plot*].

**Parameter**

  ------- --------------------------------------------------------------
  Color   Line color
  Name    Description
  Pen     A pen object
  Value   Defines which value on the y-axis the line will be drawn for
  ------- --------------------------------------------------------------

**Usage**

**Line**(Color color, **double value**, string name)

**Line**(Pen pen, **double value**, string name)

**More Information**

Information on the pen class: [*http://msdn.microsoft.com/de-de/library/system.drawing.pen.aspx*]

**Example**

// Example 1

// A new line with standard values drawn at the value of 70

**Add**(**new Line**(Color.Black, 70, "Upper"));

// Example 2

// A new line with self-defined values

**private** Line line;

**private** Pen pen;

**protected** override void **Initialize**()

{

// Define a red pen with the line strength 1

pen = **new Pen**(Color.Red, 1);

// Define a horizontal line at 10

line = **new Line**(pen, 10, "MyLine");

// add the defined line to the indicator

**Add**(line);

}

// Example 3

// Short form for the line in example 2

**Add**(**new Line**(**new Pen**(Color.Red, 1), 10, "MyLine"));

*Created with the Personal Edition of HelpNDoc: [*iPhone web sites made easy*]*

<span id="_topic_Log" class="anchor"></span>**Log()**

**Description**

Log() allows you to write outputs in the AgenaTrader log file (log tab). 5 different log levels are supported.

Note:

If the log tab is not viewable, it can be displayed using the tools log.

**Usage**

**Log**(string message, LogLevel logLevel)

**Parameter**

  ---------- --------------------------
  message    Text (message)

  logLevel   Possible values are
             
             -   InfoLogLevel.Info
             
             -   InfoLogLevel.Message
             
             -   InfoLogLevel.Warning
             
             -   InfoLogLevel.Alert
             
             -   InfoLogLevel.Error
             
             
  ---------- --------------------------

**Example**

**Log**("This is information.", InfoLogLevel.Info);

**Log**("This is a message.", InfoLogLevel.Message);

**Log**("This is a warning.", InfoLogLevel.Warning); // yellow

**Log**("This is an alarm.", InfoLogLevel.Alert);

**Log**("This is a mistake.", InfoLogLevel.Error); // red

*Created with the Personal Edition of HelpNDoc: [*Easily create CHM Help documents*][*Full featured Help generator*]*

<span id="_topic_LowestBar" class="anchor"></span>**LowestBar**

**Description**

The LowestBar() method attempts to find the lowest bar within a predefined number of periods.

See [*HighestBar()*].

**Parameter**

period Number of bars that will be searched for the lowest bar

series Every data series, such as close, high, low etc.

**Return Value**

**int** barsAgo How many bars ago the low occurred

**Usage**

**LowestBar**(IDataSeries series, **int** period)

**Example**

// How many bars ago was the lowest low of the session?

**Print**(**LowestBar**(Low, Bars.BarsSinceSession - 1));

// Which price did the lowest open of the current session have?

**Print**("The lowest open price of the current session was: " + Open\[**LowestBar**(Low, Bars.BarsSinceSession - 1)\]);

*Created with the Personal Edition of HelpNDoc: [*Easily create EBooks*][*Full featured Help generator*]*

<span id="_topic_MarketDataEventArgs" class="anchor"></span>**MarketDataEventArgs**

**Description**

The data type MarketDataEventArgs represents a change in the level 1 data and is used as a parameter of the OnMarketData() function.

  ---------------- ----------------------------------------------------------------------------------------------------------------------------------
  AskSize          Current order volume on the ask side

  AskPrice         Current ask price

  BidSize          Current order volume on the bid side

  BidPrice         Current bid price.

  Instrument       An object of the type instrument that contains the trading instrument for which the level 1 data is outputted. See *Instruments*

  LastPrice        Last traded price

  MarketDataType   Potential values are:
                   
                   MarketDataType.Ask
                   
                   MarketDataType.AskSize
                   
                   MarketDataType.Bid
                   
                   MarketDataType.BidSize
                   
                   MarketDataType.Last
                   
                   MarketDataType.Volume

  Price            This is equal to last price. This field only exists for compatability reasons

  Time             A date-time value containing the timestamp of the change

  Volume           A long value that shows the volume
  ---------------- ----------------------------------------------------------------------------------------------------------------------------------

**Example**

See [*OnMarketData()*].

*Created with the Personal Edition of HelpNDoc: [*Easy CHM and documentation editor*][*Free PDF documentation generator*]*

<span id="_topic_MarketDepthEventArgs" class="anchor"></span>**MarketDepthEventArgs**

**Description**

The data type MarketDepthEventArgs represents a change in the level 2 data (market depth) and is used as a parameter within OnMarketDepth().

  ---------------- ----------------------------------------------------------------
  MarketDataType   Potential values are:
                   
                   MarketDataType.Ask\
                   MarketDataType.Bid

  MarketMaker      A string value containing the market maker ID

  Position         An int value that defines the position within the market depth

  Operation        Represents the action caused by a change in the order book.
                   
                   Values can be:
                   
                   Operation.Insert\
                   Operation.Remove\
                   Operation.Update

  Price            A double value that displays the bid/ask price

  Time             A date-time value containing the timestamp of the change

  Volume           A long value that shows the volume
  ---------------- ----------------------------------------------------------------

**Example**

See [*OnMarketDepth()*].

*Created with the Personal Edition of HelpNDoc: [*Easily create EBooks*][*Full featured Help generator*]*

<span id="_topic_Overlay" class="anchor"></span>**Overlay**

**Description**

The overlay property defines whether the indicator outputs are displayed in the price chart above the bars or whether a separate chart window is opened below the charting area.

**Overlay = true**

The indicator is drawn above the price (for example an [*SMA*])

**Overlay = false (default)**

A separate chart window is opened (RSI)

This property can be queried within the script and outputs a value of the type Boolean (true or false).

**Usage**

Overlay

**Example**

**protected** override void **Initialize**()

{

**Add**(**new Plot**(Color.Red, "MyPlot1"));

//The indicator should be displayed within a separate window

Overlay = **false**;

}

*Created with the Personal Edition of HelpNDoc: [*Full featured Documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_PaintPriceMarkers" class="anchor"></span>**PaintPriceMarkers**

**Description**

The paint price markers property defines whether the so-called price markers for the indicator outputs are displayed on the right-hand chart border (in the price axis) or not. In some cases it makes sense to switch these off for a better overview in the chart.\
\
**PaintPriceMarkers = true (default)**

Price markers are shown in the price axis

**PaintPriceMarkers = false**

Price markers are not shown in the price axis

This property can be queried within the script and returns a value of the type Boolean (true or false).

**Usage**

PaintPriceMarkers

**Example**

**protected** override void **Initialize**()

{

**Add**(**new Plot**(Color.Red, "MyPlot1"));

//Do not show price markers in the price axis

PaintPriceMarkers = **false**;

}

*Created with the Personal Edition of HelpNDoc: [*Easily create EBooks*][*Full featured Help generator*]*

<span id="_topic_PlaySound" class="anchor"></span>**PlaySound()**

**Description**

This method allows you to play a wav file.

**Usage**

**PlaySound**(wavFile)

**Return Value**

none

**Parameter**

wavFile File name of the wav file to be played

**Example**

**using** System.IO;

string path = Environment.**GetFolderPath**(Environment.SpecialFolder.MyDocuments);

string file = "\\\\AgenaTrader\\\\Sounds\\\\Alert1.wav";

**PlaySound**(path + file);

*Created with the Personal Edition of HelpNDoc: [*Free help authoring environment*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_Plot" class="anchor"></span>**Plot()**

**Description**

A plot (drawing) is used to visually display indicators in a chart. Plot objects are assigned to an indicator with the help of the Add() method and attached to the plots collection.\
\
See [*Line*].

**Parameter**

  ----------- ----------------------------
  Color       Drawing color

  Pen         Pen object

  PlotStyle   Line type
              
              -   PlotStyle.Bar
              
              -   PlotStyle.Block
              
              -   PlotStyle.Cross
              
              -   PlotStyle.Dot
              
              -   PlotStyle.Hash
              
              -   PlotStyle.Line
              
              -   PlotStyle.Square
              
              -   PlotStyle.TriangleDown
              
              -   PlotStyle.TriangleUp
              
              

  Name        Description
  ----------- ----------------------------

**Usage**

**Plot**(Color color, string name)

**Plot**(Pen pen, string name)

**Plot**(Color color, PlotStyle plotStyle, string name)

**Plot**(Pen pen, PlotStyle plotStyle, string name)

**More Information**

Information on the pen class: [*http://msdn.microsoft.com/de-de/library/system.drawing.pen.aspx*]

**Example**

// Example 1

// Plot with standard values (line with line strength 1)

**Add**(**new Plot**(Color.Green, "MyPlot"));

// Example 2

// user-defined values for pen and plot style

**private** Plot plot;

**private** Pen pen;

**protected** override void **Initialize**()

{

// a red pen with the line strength of 6 is defined

pen = **new Pen**(Color.Blue, 6);

// a point line with a thick red pen from above is defined

plot = **new Plot**(pen, PlotStyle.Dot, "MyPlot");

// The defined plot is to be used as a representation for an indicator

**Add**(plot);

}

// Example 3

// Abbreviation of example 2

**protected** override void **Initialize**()

{

**Add**(**new Plot**(**new Pen**(Color.Blue, 6), PlotStyle.Dot, "MyPlot"));

}

*Created with the Personal Edition of HelpNDoc: [*Easy to use tool to create HTML Help files and Help web sites*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_PlotMethode" class="anchor"></span>**PlotMethod**

**Description**

In each indicator, the plot method can be overridden in order to add your own graphics (GDI+) to the price chart with the help of the graphics class (System.Drawing).

See [*http://msdn.microsoft.com/de-de/library/system.drawing.graphics.aspx*].

The [*ChartControl*] object offers several parameters.

More examples: [*Bar Numbering*][*PlotSample*], *Chart Background Image*.

**Parameter**

graphics The graphics object of the price chart (context)

rectangle The size of the drawing area (type “public struct rectangle”)

double min The smallest price in the y-axis

double max The biggest price in the y-axis

**Return Value**

none

**Usage**

**public** override void **Plot**(Graphics graphics, Rectangle r, **double** min, **double** max)

**Example**

**using** System;

**using** System.Collections.Generic;

**using** System.ComponentModel;

**using** System.Drawing;

**using** System.Drawing.Drawing2D;

**using** AgenaTrader.API;

**using** AgenaTrader.Custom;

**using** AgenaTrader.Plugins;

**namespace** AgenaTrader.UserCode

{

\[**Description**("Example for the usage of the plot method.")\]

**public** class PlotSample : UserIndicator

{

**private** StringFormat stringFormat = **new StringFormat**();

**private** SolidBrush brush = **new SolidBrush**(Color.Black);

**private** Font font = **new Font**("Arial", 10);

**protected** override void **Initialize**()

{

ChartOnly = **true**;

Overlay = **true**;

}

**protected** override void **OnBarUpdate**()

{}

**protected** override void **OnTermination**()

{

brush.**Dispose**();

stringFormat.**Dispose**();

}

**public** override void **Plot**(Graphics graphics, Rectangle r, **double** min, **double** max)

{

// Fill a rectangle

SolidBrush tmpBrush = **new SolidBrush**(Color.LightGray);

graphics.**FillRectangle**(tmpBrush, **new Rectangle** (0, 0, 300, 300));

tmpBrush.**Dispose**();

// Draw a red line from top left to bottom right

Pen pen = **new Pen**(Color.Red);

graphics.**DrawLine**(pen, r.X, r.Y, r.X + r.Width, r.Y + r.Height);

// Draw a red line from bottom left to top right

// Use anti-alias (the line appears smoother)

// The current settings for the smoothing are saved

// Restore after drawing

SmoothingMode oldSmoothingMode = graphics.SmoothingMode; //Save settings

graphics.SmoothingMode = SmoothingMode.AntiAlias; // Use higher smoothing settings

graphics.**DrawLine**(pen, r.X, r.Y + r.Height, r.X + r.Width, r.Y);

graphics.SmoothingMode = oldSmoothingMode; // Settings restored

pen.**Dispose**();

// Text in the upper left corner (position 10,35)

stringFormat.Alignment = StringAlignment.Near; // Align text to the left

brush.Color = Color.Blue;

graphics.**DrawString**("Hello world!", font, brush, r.X + 10, r.Y + 35, stringFormat);

// Text in the left lower corner and draw a line around it

brush.Color = Color.Aquamarine;

graphics.**FillRectangle**(brush, r.X + 10, r.Y + r.Height - 20, 140, 19);

// Draw outside line

pen = **new Pen**(Color.Black);

graphics.**DrawRectangle**(pen, r.X + 10, r.Y + r.Height - 20, 140, 19);

pen.**Dispose**();

// Write text

brush.Color = Color.Red;

graphics.**DrawString**("Here is bottom left!", font, brush, r.X + 10, r.Y + r.Height - 20, stringFormat);

}

}

}

*Created with the Personal Edition of HelpNDoc: [*Easy to use tool to create HTML Help files and Help web sites*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_PriceType" class="anchor"></span>**PriceType**

**Description**

Price type describes a form of price data.

See [*InputPriceType*]

Following variables are available:

-   PriceType.Close

-   PriceType.High

-   PriceType.Low

-   PriceType.Median

-   PriceType.Open

-   PriceType.Typical

-   PriceType.Volume

-   PriceType.Weighted

**Usage**

PriceType

**Example**

See [*InputPriceType*]

*Created with the Personal Edition of HelpNDoc: [*Free PDF documentation generator*]*

<span id="_topic_Print" class="anchor"></span>**Print()**

**Description**

The Print() method writes outputs in the AgenaTrader output window.\
\
See [*ClearOutputWindow()*].

**Usage**

**Print**(string message)

**Print**(**bool value**)

**Print**(**double value**)

**Print**(**int value**)

**Print**(DateTime **value**)

**Print**(string format, string message)

**Parameter**

string Text an individual message text

**Return Value**

none

**More Information**

Information regarding output formatting: *Formatting numbers*.

Hints about the String.Format() method: [*http://msdn.microsoft.com/de-de/library/fht0f5be%28v=vs.80%29.aspx*]

**Example**

// "Quick&Dirty" formatting of a number with 2 decimal points

**Print**(Close\[0\].**ToString**("0.00"));

// Output day of the week from the timestamp for the bar

**Print**(string.**Format**("{0:dddd}", Time\[0\]));

// An additional empty row with an escape sequence

**Print**("One empty row afterwards \\n");

*Created with the Personal Edition of HelpNDoc: [*Full featured Documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_RemoveDrawObject" class="anchor"></span>**RemoveDrawObject()**

**Description**

The RemoveDrawObject() method removes a specific drawing object from the chart based on a unique identifier (tag).\
See [*RemoveDrawObjects()*].

**Usage**

**RemoveDrawObject**(string tag)

**Return Value**

none

**Parameter**

string tag The clearly identifiable name for the drawing object

**Example**

**RemoveDrawObjects**("My line");

*Created with the Personal Edition of HelpNDoc: [*Free HTML Help documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_RemoveDrawObjects" class="anchor"></span>**RemoveDrawObjects()**

**Description**

This method removes all drawings from the chart\
\
See [*RemoveDrawObject()*].

**Usage**

**RemoveDrawObjects**()

**Return Value**

none

**Example**

//Delete all drawings from the chart

**RemoveDrawObjects**();

*Created with the Personal Edition of HelpNDoc: [*Free iPhone documentation generator*][*iPhone web sites made easy*]*

<span id="_topic_Rising" class="anchor"></span>**Rising()**

**Description**

With this method you can check if an uptrend exists, i.e. if the current value is bigger than the previous bar’s value.

See [*CrossAbove()*], [*CrossBelow()*], [*Falling()*].

**Usage**

**Rising**(IDataSeries series)

**Return Value**

**true** If the data series is rising\
**false** If the data series is not rising

**Parameter**

series A data series such as an indicator, close, high etc.

**Example**

// Check if SMA(20) is rising

**if** (**Rising**(**SMA**(20)))

**Print**("The SMA(20) is currently rising.");

*Created with the Personal Edition of HelpNDoc: [*Easy EBook and documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_SessionBreakLines" class="anchor"></span>**SessionBreakLines**

**Description**

The property SessionBreakLines defines whether the vertical lines that represent a trading session stop are displayed within the chart.\
\
This may be useful for stocks and other instruments if you wish to display session breaks/trading stops.\
\
**SessionBreakLines = true (default)**

Session break lines are shown

**SessionBreakLines = false**

Session break lines are not shown

This property can be queried within the script and outputs a value of the type Boolean (true or false).

**Usage**

SessionBreakLines

**Example**

**protected** override void **Initialize**()

{

**Add**(**new Plot**(Color.Red, "MyPlot1"));

//Session break lines should not be shown

SessionBreakLines = **false**;

}

*Created with the Personal Edition of HelpNDoc: [*Full featured EBook editor*][*Free PDF documentation generator*]*

<span id="_topic_TickSize" class="anchor"></span>**TickSize**

A tick is the smallest possible price change of a financial instrument within an exchange. If, for example, the trading prices are specified to 2 decimal places, then a tick equals 0.01. You can expect Forex instruments to be specified to within 4 or 5 decimal places. A tick is called a pip in Forex trading and usually equals 0.0001 or 0.00001.\
\
The tick value is usually predefined by the exchange and does not (usually) change.\
\
See [*Instrument.TickSize*].

Usually, a tick is displayed as a decimal number. Historically speaking (especially in American exchanges) stocks have been noted with tick sizes of 1/16 of a dollar.\
This notation is still widespread within commodities. Corn futures (ZC) are noted in ¼ US cents/bushel (usually equals 12.50 US\$ per contract).\
US treasury bonds are noted in a tick size of 1/32 points, which equals 31.25\$.\
\
Notations are usually made with apostrophes, for example:

149'00 equals exactly 149,\
149'01 equals 149 1/32 (meaning 149.03125),\
149'31 equals 149 31/32 (149.96875),\
and the next value after this is 150’00

In the so-called T-Bond intermonth spreads, notations are specified in quarters of 1/32, resulting in point values of 7.8125 per contract.

Notations have a dash:

17-24 equals 17 24/32 points,\
17-242 equals 17 24.25/32 points,\
17-245 equals 17 24.5/32 points and\
17-247 equals 17 24.75/32 points.\
The next notation after 17-247 is 17-25 and then 17-252, 17-255 etc.\
After 17-317 comes 18.

The individual contract specifications can be found on the websites of the respective exchanges.

CME: [*www.cmegroup.com*] under Products & Trading\
Eurex (FDAX): [*www.eurexchange.com/exchange-en/products/idx/dax/17206/*]

See [*Instrument.TickSize*].

*Created with the Personal Edition of HelpNDoc: [*Full featured Documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_TimeFrame" class="anchor"></span>**TimeFrame**

See [*Bars.TimeFrame*].

When using multiple timeframes ([*Multibars*][*MultiBars*]) in an indicator, please see [*TimeFrames*].

*Created with the Personal Edition of HelpNDoc: [*iPhone web sites made easy*]*

<span id="_topic_ToDay" class="anchor"></span>**ToDay()**

**Description**

To day is a method specifically suited for inexperienced programmers who have problems with the potentially complex .net date-time structure of C\#.\
\
Experienced programmers can continue using the date-time function directly.

To day outputs an int representation in the format of yyyymmdd.\
(yyyy = year, mm = month, dd = day)

13.08.2012 would thus be 20120813.

See [*ToTime*].

Help with date-time: [*http://msdn.microsoft.com/de-de/library/system.datetime.aspx*]

**Usage**

ToDay(DateTime time)

**Example**

// Do not trade on the 11^th^ of September

**if** (**ToDay**(Time\[0\]) = 20130911)

return;

*Created with the Personal Edition of HelpNDoc: [*Easily create Help documents*][*Full featured Help generator*]*

<span id="_topic_ToTime" class="anchor"></span>**ToTime()**

**Description**

To time is a method specifically suited for inexperienced programmers who have problems with the potentially complex .net date-time structure of C\#.

To time outputs an int representation in the format hhmmss.\
(hh = hour, mm = minute, ss = seconds)

The time 07:30 will be displayed as 73000 and 14:15:12 will become 141512.

See [*ToDay*].

Help with date-time: [*http://msdn.microsoft.com/de-de/library/system.datetime.aspx*]

**Usage**

ToTime(DateTime time)

**Example**

// Only enter trades between 08:15 and 16:35

**if** (**ToTime**(Time\[0\]) &gt;= 81500 && **ToTime**(Time\[0\]) &lt;= 163500)

{

// Any trading technique

}

*Created with the Personal Edition of HelpNDoc: [*Free help authoring tool*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_Update" class="anchor"></span>**Update()**

**Description**

The Update() method calls up the OnBarUpdate method in order to recalculate the indicator values.

Update() is to be used with caution and is intended for use by experienced programmers.

**Usage**

**Update**()

**Return Value**

none

**Parameter**

none

**Example**

The effect of update can be illustrated with the help of 2 indicators.\
\
The first indicator, Ind1, uses a public variable from the indicator Ind2.

**Code from Ind1:**

**public** class Ind1 : UserIndicator

{

**protected** override void **OnBarUpdate**()

{

> **Print**( **Ind2**().MyPublicVariable );

}

}

**Code from Ind2:**

**private double** myPublicVariable = 0;

**protected** override void **OnBarUpdate**()

{

myPublicVariable = 1;

}

**public double** MyPublicVariable

{

get

{

**Update**();

return myPublicVariable;

}

}

**Without Update() - Wrong\
**If Ind2 is called up by Ind1, the get-method of MyPublicVariable is called up in Ind2. Without Update(), the value of MyPublicVariable would be returned. In this case it would be 0.

**With Update() - Correct\
**By calling up Update(), OnBarUpdate() is initially executed by Ind2. This sets MyPublicVariable to 1. Lastly, the value 1 is passed on to the requesting indicator.

*Created with the Personal Edition of HelpNDoc: [*Free iPhone documentation generator*][*iPhone web sites made easy*]*

<span id="_topic_Value" class="anchor"></span>**Value**

**Description**

Value is a data series object containing the first data series of an indicator.

When the Add() method is called up, a value object is automatically created and added to the values collection.

Value is identical to Values\[0\].

**Usage**

Value

Value\[**int** barsAgo\]

**More Information**

The methods known for a collection, Set(), Reset(), and Count(), can be used for values.

**Example**

See [*Values*].

*Created with the Personal Edition of HelpNDoc: [*Easily create EPub books*][*Full featured Help generator*]*

<span id="_topic_VerticalGridLines" class="anchor"></span>**VerticalGridLines**

**Description**

The property VerticalGridLines defines whether or not the regularly spaced vertical lines (the so-called grid) are shown within the charting area.

**VerticalGridLines = true (default)**

Vertical grid lines are shown

**VerticalGridLines = false**

Vertical grid lines are not shown

This property can be queried within the script and returns a value of the type Boolean (true or false).

**Usage**

VerticalGridLines

**Example**

**protected** override void **Initialize**()

{

**Add**(**new Plot**(Color.Red, "MyPlot1"));

// Vertical grid lines shall not be shown within the chart

VerticalGridLines = **false**;

}

*Created with the Personal Edition of HelpNDoc: [*Free iPhone documentation generator*][*iPhone web sites made easy*]*

<span id="_topic_ZeichenobjekteDrawings" class="anchor"></span>**DrawingObjects**

*Created with the Personal Edition of HelpNDoc: [*Easily create Web Help sites*][*Full featured Help generator*]*

<span id="_topic_DrawAndrewsPitchfork" class="anchor"></span>**DrawAndrewsPitchfork()**

**Description**

This drawing object draws an Andrew’s Pitchfork.

Information concerning its usage:

[*http://vtadwiki.vtad.de/index.php/Andrews\_Pitchfork*]\
[*http://www.volumen-analyse.de/blog/?p=917*]\
[*http://www.godmode-trader.de/wissen/index.php/Chartlehrgang:Andrews\_Pitchfork*]

**Usage**

**DrawAndrewsPitchfork**(string tag, **bool** autoScale, **int** anchor1BarsAgo, **double** anchor1Y, **int** anchor2BarsAgo, **double** anchor2Y, **int** anchor3BarsAgo, **double** anchor3Y, Color color, DashStyle dashStyle, **int** width)

**DrawAndrewsPitchfork**(string tag, **bool** autoScale, DateTime anchor1Time, **double** anchor1Y, DateTime anchor2Time, **double** anchor2Y, DateTime anchor3Time, **double** anchor3Y, Color color, DashStyle dashStyle, **int** width)

**Return Value**

A drawing object of the type IAndrewsPitchfork (interface)

**Parameter**

  ---------------- -----------------------------------------------------------------------------------------
  tag              A clearly identifiable name for the drawing object

  autoScale        Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety

  anchor1BarsAgo   Number of bars ago for anchor point 1 (x-axis)

  anchor1Time      Date/time for anchor point 1 (x-axis)

  anchor1Y         y-value for anchor point 1

  anchor2BarsAgo   Number of bars ago for anchor point 2 (x-axis)

  anchor2Time      Date/time for anchor point 2 (x-axis)

  anchor2Y         y-value for anchor point 2

  anchor3BarsAgo   Number of bars ago for anchor point 3 (x-axis)

  anchor3Time      Date/time for anchor point 3 (x-axis)

  anchor3Y         y-value for anchor point 3

  color            Color of the object

  dashStyle        Line styles:
                   
                   DashStyle.Dash\
                   DashStyle.DashDot\
                   DashStyle.DashDotDot\
                   DashStyle.Dot\
                   DashStyle.Solid
                   
                   You need to integrate:\
                   using System.Drawing.Drawing2D;.

  width            Line strength in points
  ---------------- -----------------------------------------------------------------------------------------

**Example**

// Draw the Andrew’s Pitchfork (“MyAPF”)

**DrawAndrewsPitchfork**("MyAPF", **true**, 4, Low\[4\], 3, High\[3\], 1, Low\[1\], Color.Black, DashStyle.Solid, 2);

*Created with the Personal Edition of HelpNDoc: [*Easy EBook and documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_DrawArc" class="anchor"></span>**DrawArc()**

**Description**

DrawArc() draws a circular arc.

**Usage**

**DrawArc**(string tag, **int** startBarsAgo, **double** startY, **int** endBarsAgo, **double** endY, Color color)

**DrawArc**(string tag, **bool** autoScale, **int** startBarsAgo, **double** startY, **int** endBarsAgo, **double** endY, Color color, DashStyle dashStyle, **int** width)

**DrawArc**(string tag, **bool** autoScale, DateTime startTime, **double** startY, DateTime endTime, **double** endY, Color color, DashStyle dashStyle, **int** width)

**Return Value**

A drawing object of the type IArc (interface)

**Parameter**

  -------------- -----------------------------------------------------------------------------------------
  tag            A clearly identifiable name for the drawing object

  autoScale      Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety

  startBarsAgo   Number of bars ago for the starting point

  startTime      Date/time for the starting point

  startY         y-value for the starting point

  endBarsAgo     Number of bars ago for the end point

  endTime        Date/time for the end point

  endY           y-value for the end point

  color          Color of the drawing object

  dashStyle      Line style
                 
                 DashStyle.Dash\
                 DashStyle.DashDot\
                 DashStyle.DashDotDot\
                 DashStyle.Dot\
                 DashStyle.Solid
                 
                 You may have to integrate:\
                 using System.Drawing.Drawing2D;

  width          Line strength in points
  -------------- -----------------------------------------------------------------------------------------

**Example**

// Draws a blue arc\
**DrawArc**("MyArc", **true**, 10, 10, 0, 20, Color.Blue, DashStyle.Solid, 3);

*Created with the Personal Edition of HelpNDoc: [*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_DrawArrowDown" class="anchor"></span>**DrawArrowDown()**

**Description**

DrawArrowDown() draws an arrow pointing downwards:

![][21]

See [*DrawArrowUp()*], [*DrawDiamond()*], [*DrawDot()*], [*DrawSquare()*], [*DrawTriangleUp()*], [*DrawTriangleDown()*].

**Usage**

**DrawArrowDown**(string tag, **bool** autoScale, **int** barsAgo, **double** y, Color color)

**DrawArrowDown**(string tag, **bool** autoScale, DateTime time, **double** y, Color color)

**Return Value**

A drawing object of the type IArrowDown (interface)

**Parameter**

  ----------- -----------------------------------------------------------------------------------------
  tag         A clearly identifiable name for the drawing object
  autoScale   Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety
  barsAgo     Sets the preceding bar on which the arrow should be drawn (0 = current bar)
  time        Date/time of the bar on which the arrow should be drawn
  y           y-value for the arrow
  color       Color of the drawing object
  ----------- -----------------------------------------------------------------------------------------

**Example**

// Draws a red arrow 3 ticks above the high for the current bar

**DrawArrowDown**("MyArrow", **true**, 0, High\[0\] + 3\*TickSize, Color.Red);

// Draws a red arrow on a three-bar reversal pattern

**if** (High\[2\] &gt; High\[3\] && High\[1\] &gt; High\[2\] && Close\[0\] &lt; Open\[0\])

**DrawArrowDown**(CurrentBar.**ToString**(), **true**, 0, High\[0\] + 3\*TickSize, Color.Red);

*Created with the Personal Edition of HelpNDoc: [*Free iPhone documentation generator*][*iPhone web sites made easy*]*

<span id="_topic_DrawArrowLine" class="anchor"></span>**DrawArrowLine()**

**Description**

DrawArrowLine() draws an arrow:

![][22]

**Usage**

**DrawArrowLine**(string tag, **int** startBarsAgo, **double** startY, **int** endBarsAgo, **double** endY, Color color)

**DrawArrowLine**(string tag, **bool** autoScale, **int** startBarsAgo, **double** startY, **int** endBarsAgo, **double** endY, Color color, DashStyle dashStyle, **int** width)

**DrawArrowLine**(string tag, **bool** autoScale, DateTime startTime, **double** startY, DateTime endTime, **double** endY, Color color, DashStyle dashStyle, **int** width)

**Return Value**

A drawing object of the type IArrowLine (interface)

**Parameter**

  -------------- -----------------------------------------------------------------------------------------
  tag            A clearly identifiable name for the drawing object

  autoScale      Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety

  startBarsAgo   Sets the preceding bar at which the arrow should start (0 = current bar)

  startTime      Date/time of the bar at which the arrow should start

  startY         y-value for the starting point of the arrow

  endBarsAgo     Sets the preceding bar at which the arrow should end (0 = current bar)

  endTime        Date/time at which the arrow should end

  endY           y-value at which the arrow should end

  color          Color of the drawing object

  dashStyle      Line style
                 
                 DashStyle.Dash\
                 DashStyle.DashDot\
                 DashStyle.DashDotDot\
                 DashStyle.Dot\
                 DashStyle.Solid
                 
                 You may have to integrate:\
                 using System.Drawing.Drawing2D;

  width          Line strength in points
  -------------- -----------------------------------------------------------------------------------------

**Example**

// Draws a black arrow

**DrawArrowLine**("MyArrow", **false**, 10, 10, 0, 5, Color.Black, DashStyle.Solid, 4);

*Created with the Personal Edition of HelpNDoc: [*Easy EPub and documentation editor*][*Free PDF documentation generator*]*

<span id="_topic_DrawArrowUp" class="anchor"></span>**DrawArrowUp()**

**Description**

DrawArowUp() draws an arrow pointing upwards:

![][23]

See [*DrawArrowDown()*], [*DrawDiamond()*], [*DrawDot()*], [*DrawSquare()*], [*DrawTriangleUp()*], [*DrawTriangleDown()*].

**Usage**

**DrawArrowUp**(string tag, **bool** autoScale, **int** barsAgo, **double** y, Color color)

**DrawArrowUp**(string tag, **bool** autoScale, DateTime time, **double** y, Color color)

**Return Value**

A drawing object of the type IArrowUp (interface)

**Parameter**

  ----------- -----------------------------------------------------------------------------------------
  tag         A clearly identifiable name for the drawing object
  autoScale   Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety
  barsAgo     Sets the preceding bar on which the arrow should be drawn (0 = current bar)
  time        Date/time at which the arrow should be drawn
  y           y-value for the arrow
  color       Color of the drawing object
  ----------- -----------------------------------------------------------------------------------------

**Example**

// Draws a green arrow for the current bar 3 ticks below the low

**DrawArrowUp**("MyArrow", **true**, 0, Low\[0\] - 3\*TickSize, Color.Green);

*Created with the Personal Edition of HelpNDoc: [*Easily create HTML Help documents*][*Full featured Help generator*]*

<span id="_topic_DrawDiamond" class="anchor"></span>**DrawDiamond()**

**Description**

DrawDiamond() draws a diamond:

![][24]

See [*DrawArrowUp()*], [*DrawArrowDown()*], [*DrawDot()*], [*DrawSquare()*], [*DrawTriangleUp()*], [*DrawTriangleDown()*].

**Usage**

**DrawDiamond**(string tag, **bool** autoScale, **int** barsAgo, **double** y, Color color)

**DrawDiamond**(string tag, **bool** autoScale, DateTime time, **double** y, Color color)

**Return Value**

A drawing object of the type IDiamond (interface)

**Parameter**

  ----------- -----------------------------------------------------------------------------------------
  tag         A clearly identifiable name for the drawing object
  autoScale   Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety
  barsAgo     Defines the preceding bar on which the diamond should be drawn
  time        Date/time of the bar on which the diamond should be drawn
  y           y-value on which the diamond should be drawn
  color       Color of the drawing object
  ----------- -----------------------------------------------------------------------------------------

**Example**

// Draws a light blue diamond for the current bar 5 ticks below the low

**DrawDiamond**("MyDiamond", **true**, 0, Low\[0\] - 5\*TickSize, Color.SteelBlue);

*Created with the Personal Edition of HelpNDoc: [*Easy EBook and documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_DrawDot" class="anchor"></span>**DrawDot()**

**Description**

DrawDot() draws a dot:

![][25]

See [*DrawArrowUp()*], [*DrawArrowDown()*], [*DrawDiamond()*], [*DrawSquare()*], [*DrawTriangleUp()*], [*DrawTriangleDown()*].

**Usage**

**DrawDot**(string tag, **bool** autoScale, **int** barsAgo, **double** y, Color color)

**DrawDot**(string tag, **bool** autoScale, DateTime time, **double** y, Color color)

**Return Value**

A drawing object of the type IDot (interface)

**Parameter**

  ----------- -----------------------------------------------------------------------------------------
  tag         A clearly identifiable name for the drawing object
  autoScale   Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety
  barsAgo     Defines the preceding bar on which the dot should be drawn (0 = current bar)
  time        The date/time at which the dot should be drawn
  y           y-value at which the dot should be drawn
  color       Color of the drawing object
  ----------- -----------------------------------------------------------------------------------------

**Example**

// Draws an orange dot for the current bar 5 ticks above the high

**DrawDot**("MyDot", **true**, 0, High\[0\] + 5\*TickSize, Color.Orange);

*Created with the Personal Edition of HelpNDoc: [*Easily create Web Help sites*][*Full featured Help generator*]*

<span id="_topic_DrawEllipse" class="anchor"></span>**DrawEllipse()**

**Description**

DrawEllipse() draws an ellipse.

**Usage**

**DrawEllipse**(string tag, **int** startBarsAgo, **double** startY, **int** endBarsAgo, **double** endY, Color color)

**DrawEllipse**(string tag, **bool** autoScale, **int** startBarsAgo, **double** startY, **int** endBarsAgo, **double** endY, Color color, Color areaColor, **int** areaOpacity)

**DrawEllipse**(string tag, **bool** autoScale, DateTime startTime, **double** startY, DateTime endTime, **double** endY, Color color, Color areaColor, **int** areaOpacity)

**Return Value**

A drawing object of the type IEllipse (interface)

**Parameter**

  -------------- -----------------------------------------------------------------------------------------
  tag            A clearly identifiable name for the drawing object

  autoScale      Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety

  startBarsAgo   Sets the preceding bar at which the ellipse should start

  startTime      Date/time at which the ellipse should start

  startY         y-value for the start of the ellipse

  endBarsAgo     Sets the preceding bar at which the ellipse should end (0 = current bar)

  endTime        Date/time at which the ellipse should end

  endY           y-value for the end of the ellipse

  color          Border color of the drawing object

  areaColor      Fill color of the drawing object

  areaOpacity    Transparency of the fill color\
                 Value between 0 and 10\
                 0 = completely transparent\
                 10 = completely opaque
  -------------- -----------------------------------------------------------------------------------------

**Example**

// Draws a yellow ellipse from the current bar to 5 bars ago

**DrawEllipse**("MyEllipse", **true**, 5, High\[5\], 0, Close\[0\], Color.Yellow, Color.Yellow, 1);

*Created with the Personal Edition of HelpNDoc: [*Easy to use tool to create HTML Help files and Help web sites*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_DrawExtendedLine" class="anchor"></span>**DrawExtendedLine()**

**Description**

DrawExtendedLine() draws a line with an infinite end point.

See [*DrawLine()*], [*DrawHorizontalLine()*], [*DrawVerticalLine()*], [*DrawRay()*].

**Usage**

**DrawExtendedLine**(string tag, **int** startBarsAgo, **double** startY, **int** endBarsAgo, **double** endY, Color color)

**DrawExtendedLine**(string tag, **bool** autoScale, **int** startBarsAgo, **double** startY, **int** endBarsAgo, **double** endY, Color color, DashStyle dashStyle, **int** width)

**DrawExtendedLine**(string tag, **bool** autoScale, DateTime startTime, **double** startY, DateTime endTime, **double** endY, Color color, DashStyle dashStyle, **int** width)

**Return Value**

A drawing object of the type IExtendedLine (interface)

**Parameter**

  -------------- ---------------------------------------------------------------------------------------------------------
  tag            A clearly identifiable name for the drawing object

  autoScale      Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety

  startBarsAgo   Number of bars ago for the start point

  startTime      Date/time for the start point

  startY         y-value for the start point

  endBarsAgo     Number of bars ago for the second point (a true end point does not exist; the line extends to infinity)

  endTime        Date/time for the end point

  endY           y-value for the end point

  color          Color of the drawing object

  dashStyle      Line style
                 
                 DashStyle.Dash\
                 DashStyle.DashDot\
                 DashStyle.DashDotDot\
                 DashStyle.Dot\
                 DashStyle.Solid
                 
                 You may have to integrate:\
                 using System.Drawing.Drawing2D;

  width          Line strength in points
  -------------- ---------------------------------------------------------------------------------------------------------

**Example**

// Draws a line without an end point

**DrawExtendedLine**("MyExt.Line", **false**, 10, Close\[10\], 0, Close\[0\], Color.Black, DashStyle.Solid, 1);

*Created with the Personal Edition of HelpNDoc: [*Free EPub and documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_DrawFibonacciCircle" class="anchor"></span>**DrawFibonacciCircle()**

**Description**

DrawFibonacciCircle() draws a Fibonacci circle.

**Usage**

**DrawFibonacciCircle**(string tag, **bool** autoScale, **int** startBarsAgo, **double** startY, **int** endBarsAgo, **double** endY)

**DrawFibonacciCircle**(string tag, **bool** autoScale, DateTime startTime, **double** startY, DateTime endTime, **double** endY)

**Return Value**

A drawing object of the type IFibonacciCircle (interface)

**Parameter**

  -------------- -----------------------------------------------------------------------------------------
  tag            A clearly identifiable name for the drawing object
  autoScale      Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety
  startBarsAgo   Defines the starting point in terms of bars ago
  startTime      Date/time of the bar for the starting point
  startY         y-value for the start of the Fibonacci circle
  endBarsAgo     Defines the end point in terms of bars ago
  endTime        Date/time for the end of the Fibonacci circle
  endY           y-value for the end point of the Fibonacci circle
  -------------- -----------------------------------------------------------------------------------------

**Example**

//Draws a Fibonacci circle

**DrawFibonacciCircle**("MyFibCircle", **true**, 5, Low\[5\], 0, High\[0\]);

*Created with the Personal Edition of HelpNDoc: [*Easy CHM and documentation editor*][*Free PDF documentation generator*]*

<span id="_topic_DrawFibonacciExtensions" class="anchor"></span>**DrawFibonacciExtensions()**

**Description**

DrawFibonacciExtensions() draws Fibonacci extensions.

**Usage**

**DrawFibonacciExtensions**(string tag, **bool** autoScale, **int** anchor1BarsAgo, **double** anchor1Y, **int** anchor2BarsAgo, **double** anchor2Y, **int** anchor3BarsAgo, **double** anchor3Y)

**DrawFibonacciExtensions**(string tag, **bool** autoScale, DateTime anchor1Time, **double** anchor1Y, DateTime anchor2Time, **double** anchor2Y, DateTime anchor3Time, **double** anchor3Y)

**Return Value**

A drawing object of the type IFibonacciExtensions (interface)

**Parameter**

  ---------------- -----------------------------------------------------------------------------------------
  tag              A clearly identifiable name for the drawing object
  autoScale        Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety
  anchor1BarsAgo   Number of bars ago for anchor point 1
  anchor1Time      Date/time for anchor point 1
  anchor1Y         y-value for anchor point 1
  anchor2BarsAgo   Number of bars ago for anchor point 2
  anchor2Time      Date/time for anchor point 2
  anchor2Y         y-value for the anchor point 2
  anchor3BarsAgo   Number of bars ago for anchor point 3
  anchor3Time      Date/time for anchor point 3
  anchor3Y         y-value for anchor point 3
  ---------------- -----------------------------------------------------------------------------------------

**Example**

// Draws Fibonacci extensions

**DrawFibonacciExtensions**("MyFibExt", **true**, 4, Low\[4\], 3, High\[3\], 1, Low\[1\]);

*Created with the Personal Edition of HelpNDoc: [*Full featured Help generator*]*

<span id="_topic_DrawFibonacciRetracements" class="anchor"></span>**DrawFibonacciRetracements()**

**Description**

DrawFibonacciRetracements() draws Fibonacci retracements.

**Usage**

**DrawFibonacciRetracements**(string tag, **bool** autoScale, **int** startBarsAgo, **double** startY, **int** endBarsAgo, **double** endY)

**DrawFibonacciRetracements**(string tag, **bool** autoScale, DateTime startTime, **double** startY, DateTime endTime, **double** endY)

**Return Value**

A drawing object of the type IFibonacciRetracements (interface)

**Parameter**

  -------------- -----------------------------------------------------------------------------------------
  tag            A clearly identifiable name for the drawing object
  autoScale      Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety
  startBarsAgo   Defines how many bars ago the starting point of the Fibonacci retracement is located
  startTime      Date/time of the bar at which the Fibonacci retracement should begin
  startY         y-value at which the Fibonacci retracement will begin
  endBarsAgo     Defines how many bars ago the end point of the Fibonacci retracement is located
  endTime        Date/time at which the Fibonacci retracement should end
  endY           y-value at which the Fibonacci retracement should end
  -------------- -----------------------------------------------------------------------------------------

**Example**

// Draws Fibonnaci retracements

**DrawFibonacciRetracements**("MyFibRet", **true**, 10, Low\[10\], 0, High\[0\]);

*Created with the Personal Edition of HelpNDoc: [*Free help authoring environment*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_DrawFibonacciTimeExtensions" class="anchor"></span>**DrawFibonacciTimeExtensions()**

**Description**

DrawFibonacciTimeExtensions() draws Fibonacci time extensions.

**Usage**

**DrawFibonacciTimeExtensions**(string tag, **int** startBarsAgo, **double** startY, **int** endBarsAgo, **double** endY)

**DrawFibonacciTimeExtensions**(string tag, DateTime startTime, **double** startY, DateTime endTime, **double** endY)

**Return Value**

A drawing object of the type IFibonacciTimeExtensions (interface)

**Parameter**

  -------------- -------------------------------------------------------
  tag            A clearly identifiable name for the drawing object
  startBarsAgo   Defines how many bars ago the extensions should start
  startTime      Date/time at which the extensions should start
  startY         y-value at which the extensions should start
  endBarsAgo     Defines how many bars ago the extensions should end
  endTime        Date/time at which the extensions should end
  endY           y-value at which the extensions should end
  -------------- -------------------------------------------------------

**Example**

// Draws Fibonacci time extensions

**DrawFibonacciTimeExtensions**("MyFibTimeExt", 10, Low\[10\], 0, High\[0\]);

*Created with the Personal Edition of HelpNDoc: [*Create iPhone web-based documentation*][*iPhone web sites made easy*]*

<span id="_topic_DrawGannFan" class="anchor"></span>**DrawGannFan()**

**Description**

DrawGannFan() draws a Gann fan.

**Usage**

**DrawGannFan**(string tag, **bool** autoScale, **int** barsAgo, **double** y)

**DrawGannFan**(string tag, **bool** autoScale, DateTime time, **double** y)

**Return Value**

A drawing object of the type IGannFan (interface)

**Parameter**

  ----------- -----------------------------------------------------------------------------------------
  tag         A clearly identifiable name for the drawing object
  autoScale   Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety
  barsAgo     Sets the preceding bar on which the Gann fan should be drawn
  time        Date/time at which the Gann fan should start
  y           y-value for the Gann fan
  ----------- -----------------------------------------------------------------------------------------

**Example**

// Shows a Gann fan at the low of the bar from 10 periods ago

**DrawGannFan**("MyGannFan", **true**, 10, Low\[10\]);

*Created with the Personal Edition of HelpNDoc: [*Full featured EBook editor*][*Free PDF documentation generator*]*

<span id="_topic_DrawLine" class="anchor"></span>**DrawLine()**

**Description**

DrawLine() draws a (trend) line.

See [*DrawHorizontalLine()*], [*DrawVerticalLine()*], [*DrawExtendedLine()*], [*DrawRay()*].

**Usage**

**DrawLine**(string tag, **int** startBarsAgo, **double** startY, **int** endBarsAgo, **double** endY, Color color)

**DrawLine**(string tag, **bool** autoScale, **int** startBarsAgo, **double** startY, **int** endBarsAgo, **double** endY, Color color, DashStyle dashStyle, **int** width)

**DrawLine**(string tag, **bool** autoScale, DateTime startTime, **double** startY, DateTime endTime, **double** endY, Color color, DashStyle dashStyle, **int** width)

**Return Value**

A drawing object of the type ITrendLine (interface).

**Parameter**

  -------------- -----------------------------------------------------------------------------------------
  tag            A clearly identifiable name for the drawing object

  autoScale      Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety

  startBarsAgo   Number of bars ago for the starting point

  startTime      Date/time for the starting point

  startY         y-value for the starting point

  endBarsAgo     Number of bars ago for the end point

  endTime        Date/time for the end point

  endY           y-value for the end point

  color          Color of the drawing object

  dashStyle      Line style
                 
                 DashStyle.Dash\
                 DashStyle.DashDot\
                 DashStyle.DashDotDot\
                 DashStyle.Dot\
                 DashStyle.Solid
                 
                 You may have to integrate:\
                 using System.Drawing.Drawing2D;

  width          Line strength in points
  -------------- -----------------------------------------------------------------------------------------

**Example**

// Draws a line

**DrawLine**("MyLine", **false**, 10, Close\[10\], 0, Close\[0\], Color.Black, DashStyle.Solid, 1);

*Created with the Personal Edition of HelpNDoc: [*Single source CHM, PDF, DOC and HTML Help creation*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_DrawHorizontalLine" class="anchor"></span>**DrawHorizontalLine()**

**Description**

DrawHorizontalLine() draws a horizontal line in the chart.

See [*DrawLine()*], [*DrawVerticalLine()*], [*DrawExtendedLine()*], [*DrawRay()*].

**Usage**

**DrawHorizontalLine**(string tag, **double** y, Color color)

**DrawHorizontalLine**(string tag, **bool** autoScale, **double** y, Color color, DashStyle dashStyle, **int** width)

**Return Value**

A drawing object of the type IHorizontalLine (interface)

**Parameter**

  ----------- -----------------------------------------------------------------------------------------
  tag         A clearly identifiable name for the drawing object

  autoScale   Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety

  y           Any double value of your choice

  color       Line color

  dashStyle   Line style
              
              DashStyle.Dash\
              DashStyle.DashDot\
              DashStyle.DashDotDot\
              DashStyle.Dot\
              DashStyle.Solid
              
              You may have to integrate:\
              using System.Drawing.Drawing2D;

  width       Line strength
  ----------- -----------------------------------------------------------------------------------------

**Example**

// Draws a horizontal line at y=10

**DrawHorizontalLine**("MyHorizontalLine", 10, Color.Black);

*Created with the Personal Edition of HelpNDoc: [*Full featured EPub generator*][*Free PDF documentation generator*]*

<span id="_topic_DrawRay" class="anchor"></span>**DrawRay()**

**Description**

DrawRay() draws a (trend) line and extends it to infinity.

See [*DrawLine()*], [*DrawHorizontalLine()*], [*DrawVerticalLine()*], [*DrawExtendedLine()*].

**Usage**

**DrawRay**(string tag, **int** anchor1BarsAgo, **double** anchor1Y, **int** anchor2BarsAgo, **double** anchor2Y, Color color)

**DrawRay**(string tag, **bool** autoScale, **int** anchor1BarsAgo, **double** anchor1Y, **int** anchor2BarsAgo, **double** anchor2Y, Color color, DashStyle dashStyle, **int** width)

**DrawRay**(string tag, **bool** autoScale, DateTime anchor1Time, **double** anchor1Y, DateTime anchor2Time, **double** anchor2Y, Color color, DashStyle dashStyle, **int** width)

**Return Value**

A drawing object of the type IRay (interface)

**Parameter**

  ---------------- -----------------------------------------------------------------------------------------
  tag              A clearly identifiable name for the drawing object

  autoScale        Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety

  anchor1BarsAgo   Number of bars ago for anchor point 1

  anchor1Time      Date/time for anchor point 1

  anchor1Y         y-value for anchor point 1

  anchor2BarsAgo   Number of bars ago for anchor point 2

  anchor2Time      Date/time for anchor point 2

  anchor2Y         y-value for anchor point 2

  color            Color of the drawing object

  dashStyle        Line style
                   
                   DashStyle.Dash\
                   DashStyle.DashDot\
                   DashStyle.DashDotDot\
                   DashStyle.Dot\
                   DashStyle.Solid
                   
                   You may have to integrate:\
                   using System.Drawing.Drawing2D;.

  width            Line strength
  ---------------- -----------------------------------------------------------------------------------------

**Example**

// Draws a line from the bar from 10 periods ago to the current bar (x-axis)

// --&gt; line is extended to the right

// from y=3 to y=7

**DrawRay**("MyRay", 10, 3, 0, 7, Color.Green);

// Draws a line from the current bar to the bar from 10 periods ago

// --&gt; line is extended to the left

// from y=3 to y=7

**DrawRay**("MyRay", 0, 3, 10, 7, Color.Green);

*Created with the Personal Edition of HelpNDoc: [*Easy CHM and documentation editor*][*Free PDF documentation generator*]*

<span id="_topic_DrawRectangle" class="anchor"></span>**DrawRectangle()**

**Description**

DrawRectangle() draws a rectangle.

**Usage**

**DrawRectangle**(string tag, **int** startBarsAgo, **double** startY, **int** endBarsAgo, **double** endY, Color color)

**DrawRectangle**(string tag, **bool** autoScale, **int** startBarsAgo, **double** startY, **int** endBarsAgo, **double** endY, Color color, Color areaColor, **int** areaOpacity)

**DrawRectangle**(string tag, **bool** autoScale, DateTime startTime, **double** startY, DateTime endTime, **double** endY, Color color, Color areaColor, **int** areaOpacity)

**Return Value**

A drawing object of the type IRectangle (interface)

**Parameter**

  -------------- --------------------------------------------------------------------------------------------------------
  tag            A clearly identifiable name for the drawing object

  autoScale      Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety

  startBarsAgo   Sets the preceding bar at which the one corner of the rectangle should be located (0 = current bar)

  startTime      Date/time at which the start of the one rectangle corner should be located

  startY         y-value at which the one corner of the rectangle should be located

  endBarsAgo     Sets the preceding bar at which the second corner of the rectangle should be located (0 = current bar)

  endTime        Date/time of the second rectangle corner

  endY           y-value of the second rectangle corner

  color          Color of the drawing object

  areaColor      Fill color of the drawing object

  areaOpacity    Transparency of the fill color\
                 Value between 0 and 10\
                 0 = completely transparent\
                 10 = completely opaque
  -------------- --------------------------------------------------------------------------------------------------------

**Example**

// Draws a green rectangle from the low of 10 periods ago to the high of 5 periods ago

// with a fill color of pale green and a transparency of 2

**DrawRectangle**("MyRect", **true**, 10, Low\[10\], 5, High\[5\], Color.PaleGreen, Color.PaleGreen, 2);

*Created with the Personal Edition of HelpNDoc: [*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_DrawRegion" class="anchor"></span>**DrawRegion()**

**Description**

DrawRegion() fills a specific area on a chart.

**Usage**

**DrawRegion**(string tag, **int** startBarsAgo, **int** endBarsAgo, IDataSeries series, **double** y, Color outlineColor, Color areaColor, **int** areaOpacity)

**DrawRegion**(string tag, **int** startBarsAgo, **int** endBarsAgo, IDataSeries series1, IDataSeries series2, Color outlineColor, Color areaColor, **int** areaOpacity)

**DrawRegion**(string tag, DateTime startTime, DateTime endTime, IDataSeries series, **double** y, Color outlineColor, Color areaColor, **int** areaOpacity)

**DrawRegion**(string tag, DateTime startTime, DateTime endTime, IDataSeries series1, IDataSeries series2, Color outlineColor, Color areaColor, **int** areaOpacity)

**Return Value**

A drawing object of the type IRegion (interface)

**Parameter**

  ------------------ -----------------------------------------------------------------------------------
  tag                A clearly identifiable name for the drawing object

  startBarsAgo       Sets the preceding bar at which the drawing should begin (0 = current bar)

  startTime          Start time for the drawing

  endBarsAgo         Sets the preceding bar at which the drawing should end (0 = current bar)

  endTime            End time for the drawing

  series1, series2   Every data series, for example an indicator, close, high, low and so on.
                     
                     The respective value of the data series for the current bar is used as a y-value.

  y                  Any double value

  outlineColor       Color for the border

  areaColor          Fill color for the area

  areaOpacity        Transparency of the fill color\
                     Value between 0 and 10\
                     0 = completely transparent\
                     10 = completely opaque
  ------------------ -----------------------------------------------------------------------------------

**Example**

// Fills the area between the upper and lower Bollinger Bands

**DrawRegion**("MyRegion", CurrentBar, 0, **Bollinger**(2, 14).Upper, **Bollinger**(2, 14).Lower, Color.Empty, Color.Lime, 100);

*Created with the Personal Edition of HelpNDoc: [*Easy EPub and documentation editor*][*Free PDF documentation generator*]*

<span id="_topic_DrawRegressionChannel" class="anchor"></span>**DrawRegressionChannel()**

**Description**

DrawRegressionChannel() draws a regression channel.

**Usage**

**DrawRegressionChannel**(string tag, **int** startBarsAgo, **int** endBarsAgo, Color color)

**DrawRegressionChannel**(string tag, **bool** autoScale, **int** startBarsAgo, **int** endBarsAgo, Color upperColor, DashStyle upperDashStyle, **int** upperWidth, Color middleColor, DashStyle middleDashStyle, **int** middleWidth, Color lowerColor, DashStyle lowerDashStyle, **int** lowerWidth)

**DrawRegressionChannel**(string tag, **bool** autoScale, DateTime startTime, DateTime endTime, Color upperColor, DashStyle upperDashStyle, **int** upperWidth, Color middleColor, DashStyle middleDashStyle, **int** middleWidth, Color lowerColor, DashStyle lowerDashStyle, **int** lowerWidth)

**Return Value**

A drawing object of the type IRegressionChannel (interface)

**Parameter**

  ------------------- -----------------------------------------------------------------------------------------
  tag                 A clearly identifiable name for the drawing object

  autoScale           Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety

  startBarsAgo        Sets the preceding bar at which the regression channel should start (0 = current bar)

  startTime           Start time for the regression channel

  endBarsAgo          Sets the preceding bar at which the regression channel should end (0 = current bar)

  endTime             End time for the regression channel

  color               Color of the drawing object

  upperDashStyle,\    Line style
  middleDashStyle,\   
  lowerDashStyle      DashStyle.Dash\
                      DashStyle.DashDot\
                      DashStyle.DashDotDot\
                      DashStyle.Dot\
                      DashStyle.Solid
                      
                      You may have to integrate:\
                      using System.Drawing.Drawing2D;

  upperColor,\        Line color
  middleColor,\       
  lowerColor          

  upperWidth,\        Line strength
  middleWidth,\       
  lowerWidth          
  ------------------- -----------------------------------------------------------------------------------------

**Example**

// Draws a regression channel from the low of the bar from 10 days ago to the high of the bar from 5 days ago.

**DrawRegressionChannel**("MyRegChannel", 10, 0, Color.Black);

*Created with the Personal Edition of HelpNDoc: [*Free EPub and documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_DrawSquare" class="anchor"></span>**DrawSquare()**

**Description**

DrawSquare() draws a square:

![][26]

See [*DrawArrowUp()*], [*DrawArrowDown()*], [*DrawDiamond()*], [*DrawDot()*], [*DrawTriangleUp()*], [*DrawTriangleDown()*].

**Usage**

**DrawSqare**(string tag, **bool** autoScale, **int** barsAgo, **double** y, Color color)

**DrawSqare**(string tag, **bool** autoScale, DateTime time, **double** y, Color color)

**Return Value**

A drawing object of the type ISquare (interface)

**Parameter**

  ----------- -----------------------------------------------------------------------------------------
  Tag         A clearly identifiable name for the drawing object
  autoScale   Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety
  barsAgo     Sets the preceding bar at which the square should be drawn (0 = current bar)
  Time        Date/time of the bar at which the square should be drawn
  Y           y-value for the square
  color       Color of the drawing object
  ----------- -----------------------------------------------------------------------------------------

**Example**

// Draws a dark red square at the current bar 10 ticks above the high\
**DrawSquare**("MySquare", **true**, 0, High\[0\] + 10\*TickSize, Color.DarkRed);

*Created with the Personal Edition of HelpNDoc: [*Full featured EBook editor*][*Free PDF documentation generator*]*

<span id="_topic_DrawText" class="anchor"></span>**DrawText()**

**Description**

DrawText() writes whatever text you want onto the chart.

See [*DrawTextFixed()*], *PlotMethod*.

**Usage**

**DrawText**(string tag, string text, **int** barsAgo, **double** y, Color color)

**DrawText**(string tag, **bool** autoScale, string text, **int** barsAgo, **double** y, **int** yPixelOffset, Color textColor, Font font, StringAlignment alignment, Color outlineColor, Color areaColor, **int** areaOpacity)

**DrawText**(string tag, **bool** autoScale, string text, DateTime x, **double** y, **int** yPixelOffset, Color textColor, Font font, StringAlignment alignment, Color outlineColor, Color areaColor, **int** areaOpacity)

**DrawText**(string tag, **bool** autoScale, string text, **int** barsAgo, **double** y, **int** yPixelOffset, Color textColor, Font font, StringAlignment alignment, HorizontalAlignment HAlign, VerticalAlignment VAlign, Color outlineColor, Color areaColor, **int** areaOpacity)

**DrawText**(string tag, **bool** autoScale, string text, DateTime x, **double** y, **int** yPixelOffset, Color textColor, Font font, StringAlignment alignment, HorizontalAlignment HAlign, VerticalAlignment VAlign, Color outlineColor, Color areaColor, **int** areaOpacity)

**Important note:\
**When using signatures that contain horizontal alignment and vertical alignment, you need to add the following lines:

**using** System.Windows.Forms;

**using** System.Windows.Forms.VisualStyles;

**Return Value**

A drawing object of the type IText (interface)

**Parameter**

  -------------- ---------------------------------------------------------------------------------------------
  Tag            A clearly identifiable name for the drawing object

  autoScale      Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety

  Text           Text to be displayed (may contain escape sequences)

  barsAgo        Sets how many bars ago the text should be displayed

  Time           Date/time of the bar at which the text should begin

  Y              y-value at which the text should be written

  yPixelOffset   Vertical offset of the text; positive numbers move it up, and negative numbers move it down

  textColor      Text color

  Font           Font

  Alignment      Possible values are:\
                 - StringAlignment.Center\
                 - StringAlignment.Far\
                 - StringAlignment.Near

  HAlign         Possible values are:\
                 - HorizontalAlign.Left\
                 - HorizontalAlign.Center\
                 - HorizontalAlign.Right

  VAlign         Possible values are:\
                 - VerticalAlign.Top\
                 - VerticalAlign.Center\
                 - VerticalAlign.Bottom

  outlineColor   Border color around the text\
                 For no border, select Color.Empty

  areaColor      Fill color for the text box

  areaOpacity    Transparency of the fill color\
                 Value between 0 and 10\
                 0 = completely transparent\
                 10 = completely opaque
  -------------- ---------------------------------------------------------------------------------------------

**Example**

// writes text at y=3.0

**DrawText**("MyText", "This is sample text.", 10, 3, Color.Black);

// writes red text in the font Arial 7

**DrawText**("MyText", **false**, "This is sample text.", Time\[0\], Close\[0\]+50\*TickSize, 0,

Color.Red, **new Font**("Arial",7), StringAlignment.Center, Color.Blue, Color.DarkOliveGreen, 10);

This leads to the following result:

![][27]

**DrawText** (

"MyTag",

**true**,

"Text",

1, // barsAgo

High\[1\], // y

10, // yPixelOffset

Color.Blue, // Text color

**new Font**("Arial", 10, FontStyle.Bold),

StringAlignment.Center,

HorizontalAlignment.Center,

VerticalAlignment.Bottom,

Color.Red, // Outline color

Color.Yellow, // Fill color

100); // Opacity

*Created with the Personal Edition of HelpNDoc: [*Easily create CHM Help documents*][*Full featured Help generator*]*

<span id="_topic_DrawTextFixed" class="anchor"></span>**DrawTextFixed()**

**Description**

DrawTextFixed() writes text into one of 5 predetermined locations on the chart.

See [*DrawText()*].

**Usage**

**DrawTextFixed**(string tag, string text, TextPosition textPosition)

**DrawTextFixed**(string tag, string text, TextPosition textPosition, Color textColor, Font font, Color outlineColor, Color areaColor, **int** areaOpacity)

**Return Value**

A drawing object of the type ITextFixed (interface)

**Parameter**

  -------------- ----------------------------------------------------------------------------
  tag            A clearly identifiable name for the drawing object

  text           The text to be displayed

  TextPosition   TextPosition.BottomLeft\
                 TextPosition.BottomRight\
                 TextPosition.Center\
                 TextPosition.TopLeft\
                 TextPosition.TopRight

  textColor      Text color

  font           Font

  outlineColor   Color for the border around the text. For no border color, use Color.Empty

  areaColor      Fill color of the text box\
                 For no fill color, use Color.Empty

  areaOpacity    Transparency of the fill color\
                 Value between 0 and 10\
                 0 = completely transparent\
                 10 = completely opaque
  -------------- ----------------------------------------------------------------------------

**Example**

// Writes text into the middle of the chart

**DrawTextFixed**("MyText", "This is sample text.", TextPosition.Center);

// Writes red text with a blue border into the middle of the chart

**DrawTextFixed**("MyText", "This is sample text.", TextPosition.Center,

Color.Red, **new Font**("Arial",35), Color.Blue, Color.Empty, 10);

*Created with the Personal Edition of HelpNDoc: [*Single source CHM, PDF, DOC and HTML Help creation*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_DrawTrendChannel" class="anchor"></span>**DrawTrendChannel()**

**Description**

DrawTrendChannel() draws a trend channel.

**Usage**

**DrawTrendChannel**(string tag, **bool** autoScale, **int** anchor1BarsAgo, **double** anchor1Y, **int** anchor2BarsAgo, **double** anchor2Y, **int** anchor3BarsAgo, **double** anchor3Y)

**DrawTrendChannel**(string tag, **bool** autoScale, DateTime anchor1Time, **double** anchor1Y, DateTime anchor2Time, **double** anchor2Y, DateTime anchor3Time, **double** anchor3Y)

**Return Value**

A drawing object of the type ITrendChannel (interface)

**Parameter**

  ---------------- -----------------------------------------------------------------------------------------
  tag              A clearly identifiable name for the drawing object
  autoScale        Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety
  anchor1BarsAgo   Number of bars ago for anchor point 1 (x-axis)
  anchor1Time      Date/time for anchor point 1 (x-axis)
  anchor1Y         y-value for anchor point 1
  anchor2BarsAgo   Number of bars ago for anchor point 2 (x-axis)
  anchor2Time      Date/time for anchor point 2 (x-axis)
  anchor2Y         y-value for anchor point 2
  anchor3BarsAgo   Number of bars ago for anchor point 3 (x-axis)
  anchor3Time      Date/time for anchor point 3 (x-axis)
  anchor3Y         y-value for anchor point 3
  ---------------- -----------------------------------------------------------------------------------------

**Example**

// Draws a trend channel

**DrawTrendChannel**("MyTrendChannel", **true**, 10, Low\[10\], 0, High\[0\], 10, High\[10\] + 5 \* TickSize);

*Created with the Personal Edition of HelpNDoc: [*Easy to use tool to create HTML Help files and Help web sites*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_DrawTriangle" class="anchor"></span>**DrawTriangle()**

**Description**

DrawTriangle() draws a triangle.

**Usage**

**DrawTriangle**(string tag, **int** anchor1BarsAgo, **double** anchor1Y, **int** anchor2BarsAgo, **double** anchor2Y, **int** anchor3BarsAgo, **double** anchor3Y, Color color)

**DrawTriangle**(string tag, **bool** autoScale, **int** anchor1BarsAgo, **double** anchor1Y, **int** anchor2BarsAgo, **double** anchor2Y, **int** anchor3BarsAgo, **double** anchor3Y, Color color, Color areaColor, **int** areaOpacity)

**DrawTriangle**(string tag, **bool** autoScale, DateTime anchor1Time, **double** anchor1Y, DateTime anchor2Time, **double** anchor2Y, DateTime anchor3Time, **double** anchor3Y, Color color, Color areaColor, **int** areaOpacity)

**Return Value**

A drawing object of the type ITriangle (interface)

**Parameter**

  ---------------- -----------------------------------------------------------------------------------------
  tag              A clearly identifiable name for the drawing object

  autoScale        Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety

  anchor1BarsAgo   Number of bars ago for anchor point 1 (x-axis)

  anchor1Time      Date/time for anchor point 1 (x-axis)

  anchor1Y         y-value for anchor point 1

  anchor2BarsAgo   Number of bars ago for anchor point 2 (x-axis)

  anchor2Time      Date/time for anchor point 2 (x-axis)

  anchor2Y         y-value for anchor point 2

  anchor3BarsAgo   Number of bars ago for anchor point 3 (x-axis)

  anchor3Time      Date/time for anchor point 3 (x-axis)

  anchor3Y         y-value for anchor point 3

  color            Color of the drawing object

  areaColor        Fill color of the drawing object

  areaOpacity      Transparency of the fill color\
                   Value between 0 and 10\
                   0 = completely transparent\
                   10 = completely opaque
  ---------------- -----------------------------------------------------------------------------------------

**Example**

// Draws a green triangle

**DrawTriangle**("tag1", 4, Low\[4\], 3, High\[3\], 1, Low\[1\], Color.Green);

*Created with the Personal Edition of HelpNDoc: [*Easily create CHM Help documents*][*Full featured Help generator*]*

<span id="_topic_DrawTriangleUp" class="anchor"></span>**DrawTriangleUp()**

**Description**

DrawTriangleUp() draws a small upwards-pointing triangle:

![][28]

See [*DrawArrowUp()*], [*DrawArrowDown()*], [*DrawDiamond()*], [*DrawDot()*], [*DrawSquare()*], [*DrawTriangleDown()*].

**Usage**

**DrawTriangleUp**(string tag, **bool** autoScale, **int** barsAgo, **double** y, Color color)

**DrawTriangleUp**(string tag, **bool** autoScale, DateTime time, **double** y, Color color)

**Return Value**

A drawing object of the type ITriangleUp (interface)

**Parameter**

  ----------- -----------------------------------------------------------------------------------------
  tag         A clearly identifiable name for the drawing object
  autoScale   Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety
  barsAgo     Sets how many bars ago the triangle should be drawn
  time        Date/time for the bar at which the triangle should be drawn
  y           y-value at which the triangle should be drawn
  color       Color of the drawing object
  ----------- -----------------------------------------------------------------------------------------

**Example**

// Draws a small light green triangle at the current bar 10 ticks below the low

**DrawTriangleUp**("MyTriangleUp", **true**, 0, Low\[0\] - 10\*TickSize, Color.LightGreen);

*Created with the Personal Edition of HelpNDoc: [*Full featured Documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_DrawTriangleDown" class="anchor"></span>**DrawTriangleDown()**

**Description**

DrawTriangleDown() draws a small downwards-pointing triangle:

![][29]

See [*DrawArrowUp()*], [*DrawArrowDown()*], [*DrawDiamond()*], [*DrawDot()*], [*DrawSquare()*], [*DrawTriangleUp()*].

**Usage**

**DrawTriangleDown**(string tag, **bool** autoScale, **int** barsAgo, **double** y, Color color)

**DrawTriangleDown**(string tag, **bool** autoScale, DateTime time, **double** y, Color color)

**Return Value**

A drawing object of the type ITriangleDown (interface)

**Parameter**

  ----------- -----------------------------------------------------------------------------------------
  tag         A clearly identifiable name for the drawing object
  autoScale   Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety
  barsAgo     Sets how many bars ago the triangle should be drawn
  time        Date/time for the bar at which the triangle should be drawn
  Y           y-value at which the triangle should be drawn
  color       Color of the drawing object
  ----------- -----------------------------------------------------------------------------------------

**Example**

// Draws a small red triangle at the current bar 10 ticks above the high

**DrawTriangleDown**("MyTriangleDown", **true**, 0, High\[0\] + 10\*TickSize, Color.LightGreen);

*Created with the Personal Edition of HelpNDoc: [*Free help authoring environment*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_DrawVerticalLine" class="anchor"></span>**DrawVerticalLine()**

**Description**

DrawVerticalLine() draws a vertical line in the chart.

See [*DrawLine()*], [*DrawHorizontalLine()*], [*DrawExtendedLine()*], [*DrawRay()*].

**Usage**

**DrawVerticalLine**(string tag, **int** barsAgo, Color color)

**DrawVerticalLine**(string tag, **int** barsAgo, Color color, DashStyle dashStyle, **int** width)

**DrawVerticalLine**(string tag, DateTime time, Color color, DashStyle dashStyle, **int** width)

**Return Value**

A drawing object of the type IVerticalLine (interface)

**Parameter**

  ----------- ----------------------------------------------------------------------------
  tag         A clearly identifiable name for the drawing object

  barsAgo     Sets how many bars ago the vertical line should be drawn (0 = current bar)

  time        Date/time of the bar at which the vertical line should be drawn

  color       Line color

  dashStyle   Line style
              
              DashStyle.Dash\
              DashStyle.DashDot\
              DashStyle.DashDotDot\
              DashStyle.Dot\
              DashStyle.Solid
              
              You may have to integrate:\
              using System.Drawing.Drawing2D;

  width       Line strength
  ----------- ----------------------------------------------------------------------------

**Example **

// Draws a vertical line at the bar from 10 periods ago

**DrawVerticalLine**("MyVerticalLine", 10, Color.Black);

*Created with the Personal Edition of HelpNDoc: [*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_TippsTricks" class="anchor"></span>**Hints & Advice**

*Created with the Personal Edition of HelpNDoc: [*Create iPhone web-based documentation*][*iPhone web sites made easy*]*

<span id="_topic_BarNummerierungimChart" class="anchor"></span>**Bar Numbering Within the Chart**

The following example demonstrates the usage of the plot method and the properties of the [*ChartControl*] object.

![][30]

Note:\
For demonstration purposes, each time Paint is called up within the “Bar Numbering” section, “New” and “Dispose” will also be called up multiple times.\
From a performance point of view, this solution can be better implemented by using constant variable declarations and calling up “Dispose” within the OnTermination statement.

**using** System;

**using** System.Collections.Generic;

**using** System.ComponentModel;

**using** System.Drawing;

**using** System.Drawing.Drawing2D;

**using** System.Linq;

**using** System.Xml;

**using** System.Xml.Serialization;

**using** AgenaTrader.API;

**using** AgenaTrader.Custom;

**using** AgenaTrader.Plugins;

**using** AgenaTrader.Helper;

**namespace** AgenaTrader.UserCode

{

\[**Description**("PlotSample")\]

**public** class PlotSample : UserIndicator

{

Pen pen = **new Pen**(Color.Blue);

StringFormat sf = **new StringFormat**();

SolidBrush brush = **new SolidBrush**(Color.Black);

Font font = **new Font**("Arial", 10, FontStyle.Bold);

**protected** override void **Initialize**()

{

Overlay = **true**;

}

**protected** override void **OnTermination**()

{

**if** (pen!=**null**) pen.**Dispose**();

**if** (sf!=**null**) sf.**Dispose**();

**if** (brush!=**null**) brush.**Dispose**();

**if** (font!=**null**) font.**Dispose**();

}

**protected** override void **OnBarUpdate**()

{}

**public** override void **Plot**(Graphics g, Rectangle r, **double** min, **double** max)

{

**if** (Bars == **null** || ChartControl == **null**) return;

// Properties of ChartControl

string s;

s = "bounds: "+r.X.**ToString**()+" "+r.Y.**ToString**()+" "+r.Height.**ToString**()+" "+r.Width.**ToString**();

g.**DrawString**(s, font, brush, 10, 50, sf);

s = "min: "+Instrument.**Round2TickSize**(min).**ToString**()+" max: "+Instrument.**Round2TickSize**(max).**ToString**();

g.**DrawString**(s, font, brush, 10, 70, sf);

s = "BarSpace: "+ChartControl.BarSpace.**ToString**()+" BarWidth: "+ChartControl.BarWidth.**ToString**();

g.**DrawString**(s, font, brush, 10, 90, sf);

s = "Bars.Count: "+Bars.Count.**ToString**();

g.**DrawString**(s, font, brush, 10, 110, sf);

s = "BarsPainted: "+ChartControl.BarsPainted.**ToString**() + " FirstBarPainted: "+ChartControl.FirstBarPainted.**ToString**() + " LastBarPainted: "+ChartControl.LastBarPainted.**ToString**();

g.**DrawString**(s, font, brush, 10, 130, sf);

s = "BarsVisible: "+ChartControl.BarsVisible.**ToString**() + " FirstBarVisible: "+ChartControl.FirstBarVisible.**ToString**() + " LastBarVisible: "+ChartControl.LastBarVisible.**ToString**();

g.**DrawString**(s, font, brush, 10, 150, sf);

// Bar numbering

StringFormat \_sf = **new StringFormat**();

SolidBrush \_brush = **new SolidBrush**(Color.Blue);

Font \_font = **new Font**("Arial", 8);

SizeF \_stringSize = **new SizeF**();

\_sf.Alignment = StringAlignment.Center;

**for** (**int** i=ChartControl.FirstBarVisible; i&lt;=ChartControl.LastBarVisible; i++)

{

string text = i.**ToString**();

\_stringSize = g.**MeasureString**(text, \_font);

**int** x = ChartControl.**GetXByBarIdx**(Bars, i);

**int** y = ChartControl.**GetYByValue**(**this**, High\[**Abs2Ago**(i)\] + 3\*TickSize) - (**int**) \_stringSize.Height;

g.**DrawString**(text, \_font, \_brush, x, y, \_sf);

}

\_sf.**Dispose**();

\_brush.**Dispose**();

\_font.**Dispose**();

}

**private int Abs2Ago**(**int** idx)

{

return Math.**Max**(0,Bars.Count-idx-1-(CalculateOnBarClose?1:0));

}

}

}

*Created with the Personal Edition of HelpNDoc: [*Free iPhone documentation generator*][*iPhone web sites made easy*]*

<span id="_topic_EigenesChartHintergrundbild" class="anchor"></span>**Custom Chart Background Image**

The plot method allows you to add a background image to the chart.\
\
The following example uses an image with the JPG format located in the main directory on the hard drive (C:).

**using** System;

**using** System.Drawing;

**using** AgenaTrader.Custom;

**using** AgenaTrader.Plugins;

**namespace** AgenaTrader.UserCode

{

**public** class BackgroundPicture : UserIndicator

{

Image img;

**protected** override void **OnStartUp**()

{

**try** { img = Image.**FromFile**("C:\\\\MyCar.jpg"); } **catch** {}

}

**public** override void **Plot**(Graphics g, Rectangle r, **double** min, **double** max)

{

**if** (ChartControl == **null** || img == **null**) return;

g.**DrawImage**(img,r);

}

}

}

![][31]

*Created with the Personal Edition of HelpNDoc: [*Full featured Help generator*]*

<span id="_topic_FileAuswahlindenProperties" class="anchor"></span>**File Selection in the Properties**

To enable file selection within the properties dialog of an indicator, you will need a type converter.\
\
The following example displays how a selection of WAV files can be programmed for an alert:

**using** System;

**using** System.IO;

**using** System.Collections;

**using** System.ComponentModel;

**using** AgenaTrader.Custom;

**using** AgenaTrader.Plugins;

**namespace** AgenaTrader.UserCode

{

\[**Description**("File Picker Example.")\]

**public** class FilePicker : UserIndicator

{

**private** string \_soundFile = "Alert4.wav";

**private** static string \_dir = Environment.**GetFolderPath**(Environment.SpecialFolder.MyDocuments) + @"\\AgenaTrader\\Sounds\\";

**internal** class MyConverter : TypeConverter

{

**public** override **bool GetStandardValuesSupported**(ITypeDescriptorContext context)

{

return **true**;

}

**public** override StandardValuesCollection **GetStandardValues**(ITypeDescriptorContext context)

{

**if** (context == **null**) return **null**;

ArrayList list = **new ArrayList**();

DirectoryInfo dir = **new DirectoryInfo**(\_dir);

FileInfo\[\] files = dir.**GetFiles**("\*.wav");

**foreach** (FileInfo file **in** files) list.**Add**(file.Name);

return **new** TypeConverter.**StandardValuesCollection**(list);

}

}

**protected** override void **OnStartUp**()

{

**PlaySound**(\_soundFile);

}

\[**Description**("Choose file to play.")\]

\[**Category** ("Sound")\]

\[**TypeConverter**(**typeof**(MyConverter))\]

**public** string SoundFile

{

get { return \_soundFile; }

set { \_soundFile = **value**; }

}

}

}

*Created with the Personal Edition of HelpNDoc: [*Easily create EPub books*][*Full featured Help generator*]*

<span id="_topic_FormatierenvonZahlen" class="anchor"></span>**Formatting of Numbers**

**Formatting of Numbers**

**General information on formatting in C\#**

**double** d = 123.4567890;

**Print**("Without formatting : " + d.**ToString**()); // 123.456789

**Print**("As a currency : " + d.**ToString**("C")); // 123.46 €

**Print**("Exponential : " + d.**ToString**("E")); // 1.234568E+002

**Print**("As a fixed point : " + d.**ToString**("F2")); // 123.46

**Print**("General : " + d.**ToString**("G")); // 123.456789

**Print**("As a percentage : " + d.**ToString**("P0")); // 12.346%

**Print**("To 2 decimal places : " + d.**ToString**("N2")); // 123.45

**Print**("To 3 decimal places : " + d.**ToString**("N3")); // 123.457

**Print**("To 4 decimal places : " + d.**ToString**("N4")); // 123.4568

**Useful Functions**

Returns the currency symbol for the current instrument:

**public** string **getWaehrungssymbol**() {

string s = "";

**switch** (Instrument.Currency) {

**case** Currencies.USD : s = "\$"; break;

**case** Currencies.EUR : s = "€"; break;

**case** Currencies.CHF : s = "CHF"; break;

**case** Currencies.GBP : s = ((**char**)163).**ToString**(); break;

**case** Currencies.JPY : s = ((**char**)165).**ToString**(); break;

}

return s;

}

Converts a number into a currency with a thousands separator and 2 decimal places.\
The block separation per 1000 units can be set in “Culture”.

**public** string **getWaehrungOhneSymbol**(**double** d) {

// Separate 1000s and two decimal points

return d.**ToString**("\#,\#\#0.00");

}

Converts a number into a currency with a thousands separator and 2 decimal places and a currency symbol:

**public** string **getWaehrungMitSymbol**(**double** d) {

// Dollar is prefixed, everything else is added afterwards

string s=**getWaehrungOhneSymbol**(d);

string w=**getWaehrungssymbol**();

**if** (w=="\$") s=w+" "+s; **else** s+=" "+w;

return s;

}

Converts a number into a currency with a thousands separator and 2 decimal places as well as a currency symbol, and fills up to a fixed length with empty spaces.\
The function is great for outputting values into a table.

**public** string **getWaehrungMitSymbol**(**double** d, **int** Laenge) {

// Leading spaces until a fixed length has been reached

string s=**getWaehrungMitSymbol**(d);

**for** (**int** i=s.Length; i&lt;Laenge; i++) s=" "+s;

return s;

}

Converts a number into a percentage. Nothing is calculated, only formatted.\
Leading plus sign, a decimal place and a percent sign.

**public** string **getPercent**(**double** d) {

d=Math.**Round**(d, 1);

string s=(d&gt;0)?"+":""; // Leading plus sign

return s+d.**ToString**("0.0")+"%";

}

Formats the market price depending on the number of decimal places to which the currency is notated.\
This includes a thousands separator and fixed length, meaning that zeros are filled on the right hand side.\
Because Culture Info is being used, you must integrate the NameSpace **System.Globalization**.

**public** string **format**(**double** d)

{

**int** tickLength = 0;

// ticksize.ToString() is for example 6J = "1E-06" and length is then 5

// and not 8 as it should be with "0.000001")

**if** (TickSize &lt; 1) tickLength = TickSize.**ToString**("0.\#\#\#\#\#\#\#\#\#\#\#").Length - 2;

string f = "{0:n"+tickLength.**ToString**()+"}";

return string.**Format**(CultureInfo.CurrentCulture, f, d);

}

**Example**

**double** profit = 1234.567890;

**Print**("getCurrencyWithoutSymbol ": + **getWaehrungOhneSymbol**(Gewinn)); // 1234.57

**Print**("getCurrencyWithSymbol :" + **getWaehrungMitSymbol**(Gewinn)); // \$ 1,234.57

**Print**("getCurrencyWithSymbol :" + **getWaehrungMitSymbol**(Gewinn)); // \$ 1,234.57

**double** percentage profit = 12.3456789;

**Print**("getPercent :" + **getPercent**(ProzGewinn)); // +12.3%

**double** price = 123.4567;

**Print**("getPrice :" + **getKurs**(Kurs)); // 123.46

*Created with the Personal Edition of HelpNDoc: [*Free EBook and documentation generator*][*Free PDF documentation generator*]*

<span id="_topic_IndexConvertierung" class="anchor"></span>**Index Conversion**

There are two types of indexing in AgenaTrader.

1.

The bars are numbered from youngest to oldest.\
This type is used in the OnBarUpdate() method.\
The last bar has an index of 0, while the oldest bar has the index Bars.Count-1.

2.

The bars are numbered from oldest to youngest.\
This type is most commonly used in the Plot() method in “for” loops.\
The oldest Bbar receives an index of 0, while the youngest bar has the index Bars.Count-1.\
\
The following function can be used to recalculate the index types:

private int Convert(int idx)

{

return Math.Max(0,Bars.Count-idx-1-(CalculateOnBarClose?1:0));

}

*Created with the Personal Edition of HelpNDoc: [*Easily create EBooks*][*Full featured Help generator*]*

<span id="_topic_Indikatornamenberschreiben" class="anchor"></span>**Overwriting Indicator Names**

The name of an indicator (or a strategy) is displayed within the properties dialog and at the top edge of the chart. Use the ToString() property to overwrite it.

public override string ToString()

{

return "My Name";

}

*Created with the Personal Edition of HelpNDoc: [*Free help authoring tool*][*Create HTML Help, DOC, PDF and print manuals from 1 single source*]*

<span id="_topic_RechteckmitabgerundetenEcken" class="anchor"></span>**Rectangle with Rounded Corners**

By using the graphics methods, you can create interesting forms and place them onto the chart.\
One example of this is the RoundedRectangle class, which is a rectangle with rounded corners.

![][32]

**Example Code:**

**using** System;

**using** System.Collections.Generic;

**using** System.ComponentModel;

**using** System.Drawing;

**using** System.Linq;

**using** System.Xml;

**using** System.Xml.Serialization;

**using** System.Drawing.Drawing2D;

**using** AgenaTrader.API;

**using** AgenaTrader.Custom;

**using** AgenaTrader.Plugins;

**namespace** AgenaTrader.UserCode

{

\[**Description**("Demo of RoundedRectangles")\]

**public** class DemoRoundedRectangle : UserIndicator

{

**protected** override void **Initialize**()

{

Overlay = **true**;

}

**protected** override void **OnBarUpdate**() {}

**public** override void **Plot**(Graphics g, Rectangle r, **double** min, **double** max)

{

GraphicsPath path;

// draws a rectangle with rounded corners

path = RoundedRectangle.**Create**(30, 50, 100, 100,8);

g.**DrawPath**(Pens.Black, path);

// draws a filled rectangle with a radius of 20

// only round the upper left and lower right corner

path = RoundedRectangle.**Create**(160, 50, 100, 100, 20,

RoundedRectangle.RectangleCorners.TopLeft|RoundedRectangle.RectangleCorners.BottomRight);

g.**FillPath**(Brushes.Orange, path);

}

}

**public** abstract class RoundedRectangle

{

**public enum** RectangleCorners

{

None = 0, TopLeft = 1, TopRight = 2, BottomLeft = 4, BottomRight = 8,

All = TopLeft | TopRight | BottomLeft | BottomRight

}

**public** static GraphicsPath **Create**(**int** x, **int** y, **int** width, **int** height, **int** radius, RectangleCorners corners)

{

Rectangle r = **new Rectangle**(x,y,width, height);

Rectangle tlc = **new Rectangle**(r.Left, r.Top,Math.**Min**(2 \* radius, r.Width),Math.**Min**(2 \* radius, r.Height));

Rectangle trc = tlc;

trc.X = r.Right - 2 \* radius;

Rectangle blc = tlc;

blc.Y = r.Bottom - 2 \* radius;

Rectangle brc = blc;

brc.X = r.Right - 2 \* radius;

Point\[\] n = **new** Point\[\]

{

**new Point**(tlc.Left, tlc.Bottom), tlc.Location,

**new Point**(tlc.Right, tlc.Top), trc.Location,

**new Point**(trc.Right, trc.Top),

**new Point**(trc.Right, trc.Bottom),

**new Point**(brc.Right, brc.Top),

**new Point**(brc.Right, brc.Bottom),

**new Point**(brc.Left, brc.Bottom),

**new Point**(blc.Right, blc.Bottom),

**new Point**(blc.Left, blc.Bottom), blc.Location

};

GraphicsPath p = **new GraphicsPath**();

p.**StartFigure**();

//Top left corner

**if** ((RectangleCorners.TopLeft & corners) == RectangleCorners.TopLeft)

p.**AddArc**(tlc, 180, 90);

**else**

p.**AddLines**(**new** Point\[\] { n\[0\], n\[1\], n\[2\] });

//Top edge

p.**AddLine**(n\[2\], n\[3\]);

//Top right corner

**if** ((RectangleCorners.TopRight & corners) == RectangleCorners.TopRight)

p.**AddArc**(trc, 270, 90);

**else**

p.**AddLines**(**new** Point\[\] { n\[3\], n\[4\], n\[5\] });

//Right edge

p.**AddLine**(n\[5\], n\[6\]);

//Bottom right corner

**if** ((RectangleCorners.BottomRight & corners) == RectangleCorners.BottomRight)

p.**AddArc**(brc, 0, 90);

**else**

p.**AddLines**(**new** Point\[\] { n\[6\], n\[7\], n\[8\] });

//Bottom edge

p.**AddLine**(n\[8\], n\[9\]);

//Bottom left corner

**if** ((RectangleCorners.BottomLeft & corners) == RectangleCorners.BottomLeft)

p.**AddArc**(blc, 90, 90);

**else**

p.**AddLines**(**new** Point\[\] { n\[9\], n\[10\], n\[11\] });

//Left edge

p.**AddLine**(n\[11\], n\[0\]);

p.**CloseFigure**();

return p;

}

**public** static GraphicsPath **Create**(Rectangle rect, **int** radius, RectangleCorners c)

{ return **Create**(rect.X, rect.Y, rect.Width, rect.Height, Math.**Max**(1,radius), c); }

**public** static GraphicsPath **Create**(**int** x, **int** y, **int** width, **int** height, **int** radius)

{ return **Create**(x, y, width, height, Math.**Max**(1,radius), RectangleCorners.All); }

**public** static GraphicsPath **Create**(Rectangle rect, **int** radius)

{ return **Create**(rect.X, rect.Y, rect.Width, rect.Height, Math.**Max**(1,radius)); }

**public** static GraphicsPath **Create**(**int** x, **int** y, **int** width, **int** height)

{ return **Create**(x, y, width, height, 8); }

**public** static GraphicsPath **Create**(Rectangle rect)

{ return **Create**(rect.X, rect.Y, rect.Width, rect.Height); }

}

}

*Created with the Personal Edition of HelpNDoc: [*Free CHM Help documentation generator*][*Free PDF documentation generator*]*

  [*Bars*]: #_topic_BarsKerzen
  [*Data series*]: #_topic_Datenserien
  [*Instrument*]: #_topic_Instrumente
  [***Close***]: #_topic_Close
  [*High*]: #_topic_High
  [*Low*]: #_topic_Low
  [*Open*]: #_topic_Open
  [*Median*]: #_topic_Median
  [*Typical*]: #_topic_Typical
  [*Weighted*]: #_topic_Weighted
  [*Time*]: #_topic_Time
  [*CalculateOnBarClose*]: #_topic_CalculateOnBarClose
  [*Bars.BarsSinceSession*]: #_topic_BarsBarsSinceSession
  [*Bars.Count*]: #_topic_BarsCount
  [*Bars.FirstBarOfSession*]: #_topic_BarsFirstBarOfSession
  [*Bars.GetBar*]: #_topic_BarsGetBar
  [*Bars.GetBarsAgo*]: #_topic_BarsGetBarsAgo
  [*Bars.GetByIndex*]: #_topic_BarsGetByIndex
  [*Bars.GetIndex*]: #_topic_BarsGetIndex
  [*Bars.GetNextBeginEnd*]: #_topic_BarsGetNextBeginEnd
  [*Bars.GetOpen*]: #_topic_BarsGetOpen
  [*Bars.Instrument*]: #_topic_BarsInstrument
  [*Bars.IsIntraDay*]: #_topic_BarsIsIntraDay
  [*Bars.PercentComplete*]: #_topic_BarsPercentComplete
  [*Bars.SessionBegin*]: #_topic_BarsSessionBegin
  [*Bars.SessionEnd*]: #_topic_BarsSessionEnd
  [*Bars.SessionNextBegin*]: #_topic_BarsSessionNextBegin
  [*Bars.SessionNextEnd*]: #_topic_BarsSessionNextEnd
  [*Bars.TickCount*]: #_topic_BarsTickCount
  [*Bars.TimeFrame*]: #_topic_BarsTimeFrame
  [*Bars.TotalTicks*]: #_topic_BarsTotalTicks
  [*Properties*]: #_topic_EigenschaftenvonBars
  [*OnBarUpdate ()*]: #_topic_OnBarUpdate
  []: media/image1.png{width="3.0833333333333335in" height="3.78125in"}
  [*http://msdn.microsoft.com/de-de/library/system.datetime.aspx*]: http://msdn.microsoft.com/de-de/library/system.datetime.aspx
  [1]: media/image2.png{width="2.5in" height="1.875in"}
  [*Instrument.Exchange*]: #_topic_InstrumentExchange
  [2]: media/image3.png{width="3.0416666666666665in" height="3.4375in"}
  [3]: media/image4.png{width="3.0416666666666665in" height="3.4375in"}
  [*TimeFrame*]: #_topic_TimeFrame
  [*Free PDF documentation generator*]: http://www.helpndoc.com
  [*Opens*]: #_topic_Opens
  [*Highs*]: #_topic_Highs
  [*Lows*]: #_topic_Lows
  [*Closes*]: #_topic_Closes
  [*Medians*]: #_topic_Medians
  [*Typicals*]: #_topic_Typicals
  [*Weighteds*]: #_topic_Weighteds
  [*Times*]: #_topic_Times
  [*TimeFrames*]: #_topic_TimeFrames
  [*Volume*]: #_topic_Volume
  [*Volumes*]: #_topic_Volumes
  [*DataSeries*]: #_topic_DataSeries
  [*MultiBars*]: #_topic_Multibars
  [*iPhone web sites made easy*]: http://www.helpndoc.com/feature-tour/iphone-website-generation
  [*Create HTML Help, DOC, PDF and print manuals from 1 single source*]: http://www.helpndoc.com/help-authoring-tool
  [4]: #_topic_Datenserien1
  [*Full featured Help generator*]: http://www.helpndoc.com/feature-tour
  [*http://blog.nobletrading.com/2009/12/median-price-typical-price-weighted.html*]: http://blog.nobletrading.com/2009/12/median-price-typical-price-weighted.html
  [*DateTimeSeries*]: #_topic_DateTimeSeries
  [*VOL()*]: #_topic_VolumenVOL
  [*Instrument.Compare*]: #_topic_InstrumentCompare
  [*Instrument.Currency*]: #_topic_InstrumentCurrency
  [*Instrument.Digits*]: #_topic_InstrumentDigits
  [*Instrument.ETF*]: #_topic_InstrumentETF
  [*Instrument.Expiry*]: #_topic_InstrumentExpiry
  [*Instrument.InstrumentType*]: #_topic_InstrumentInstrumentType
  [*Instrument.Name*]: #_topic_InstrumentName
  [*Instrument.PointValue*]: #_topic_InstrumentPointValue
  [*Instrument.Round2TickSize*]: #_topic_InstrumentRound2TickSize
  [*Instrument.Symbol*]: #_topic_InstrumentSymbol
  [*Instrument.TickSize*]: #_topic_InstrumentTickSize
  [*TickSize*]: #_topic_TickSize
  [*Formatting of Numbers*]: #_topic_FormatierenvonZahlen
  [*http://de.wikipedia.org/wiki/Exchange-traded\_fund*]: http://de.wikipedia.org/wiki/Exchange-traded_fund
  [*http://www.boersen-links.de/boersen.htm*]: http://www.boersen-links.de/boersen.htm
  [5]: media/image5.png{width="5.875in" height="4.260416666666667in"}
  [6]: media/image6.png{width="5.875in" height="4.260416666666667in"}
  [7]: media/image7.png{width="5.875in" height="4.260416666666667in"}
  [8]: media/image8.png{width="5.875in" height="4.260416666666667in"}
  [9]: media/image9.png{width="5.875in" height="4.260416666666667in"}
  [*Line*]: #_topic_Line
  [*Add()*]: #_topic_Add
  [*Plots*]: #_topic_Plots
  [*http://msdn.microsoft.com/en-us/library/ybcx56wz%28v=vs.80%29.aspx*]: http://msdn.microsoft.com/en-us/library/ybcx56wz%28v=vs.80%29.aspx
  [*Lines*]: #_topic_Lines
  [*CurrentBars*]: #_topic_CurrentBars1
  [*BarsInProgress*]: #_topic_BarsInProgress
  [*TimeFrameRequirements*]: #_topic_TimeFrameRequirements
  [CurrentBar]: #_topic_CurrentBar1
  [*event-oriented*]: http://de.wikipedia.org/wiki/Ereignis_%28Programmierung%29
  [*API*]: http://de.wikipedia.org/wiki/Programmierschnittstelle
  [*Overwriting*]: http://de.wikipedia.org/wiki/%C3%9Cberschreiben_%28OOP%29
  [*OnExecution()*]: #_topic_OnExecution
  [*OnMarketData()*]: #_topic_OnMarketData
  [*OnMarketDepth()*]: #_topic_OnMarketDepth
  [*OnOrderUpdate()*]: #_topic_OnOrderUpdate
  [*OnStartUp()*]: #_topic_OnStartUp
  [*OnTermination()*]: #_topic_OnTermination
  [*Events*]: #_topic_EreignisseEvents
  [*MarketDataEventArgs*]: #_topic_MarketDataEventArgs
  [*MarketDepthEventArgs*]: #_topic_MarketDepthEventArgs
  [*Initialize()*]: #_topic_Initialize
  [*DefaultQuantity*]: #_topic_DefaultQuantity
  [*EnterLongLimit()*]: #_topic_EnterLongLimit
  [*EnterLongStop()*]: #_topic_EnterLongStop
  [*EnterLongStopLimit()*]: #_topic_EnterLongStopLimit
  [*EnterLong()*]: #_topic_EnterLong
  [*CancelOrder*]: #_topic_CancelOrder
  [*TimeInForce*]: #_topic_TimeInForce
  [*EnterShortLimit()*]: #_topic_EnterShortLimit
  [*EnterShortStop()*]: #_topic_EnterShortStop
  [*EnterShortStopLimit()*]: #_topic_EnterShortStopLimit
  [*EnterShort()*]: #_topic_EnterShort
  [*EntryHandling*]: #_topic_EntryHandling
  [*EntriesPerDirection*]: #_topic_EntriesPerDirection
  [*ExitLong()*]: #_topic_ExitLong
  [*ExitLongLimit()*]: #_topic_ExitLongLimit
  [*ExitLongStop()*]: #_topic_ExitLongStop
  [*ExitLongStopLimit()*]: #_topic_ExitLongStopLimit
  [*ExitShort()*]: #_topic_ExitShort
  [*ExitShortLimit()*]: #_topic_ExitShortLimit
  [*ExitShortStop()*]: #_topic_ExitShortStop
  [*ExitShortStopLimit()*]: #_topic_ExitShortStopLimit
  [*GetProfitLoss()*]: #_topic_GetProfitLoss
  [*GetAccountValue()*]: #_topic_GetAccountValue
  [*http://www.vantharp.com/tharp-concepts/risk-and-r-multiples.asp*]: http://www.vantharp.com/tharp-concepts/risk-and-r-multiples.asp
  [*Position.MarketPosition*]: #_topic_Position
  [*Trade*]: #_topic_Trade
  [*SetStopLoss()*]: #_topic_SetStopLoss
  [*SetTrailStop()*]: #_topic_SetTrailStop
  [*SetProfitTarget()*]: #_topic_SetProfitTarget
  [*Performance*]: #_topic_Performance
  [10]: media/image10.png{width="6.552083333333333in" height="4.979166666666667in"}
  [*Plot*]: #_topic_Plot
  [*http://msdn.microsoft.com/de-de/library/z0w1kczw%28v=vs.80%29.aspx*]: http://msdn.microsoft.com/de-de/library/z0w1kczw%28v=vs.80%29.aspx
  [*Browsable*]: #_topic_Browsable
  [*Category*]: #_topic_Category
  [*ConditionalValue*]: #_topic_ConditionalValue
  [*Description*]: #_topic_Description
  [*DisplayName*]: #_topic_DisplayName
  [*XmlIgnore*]: #_topic_XmlIgnore
  [Attribut]: #_topic_Attribute
  [11]: media/image11.png{width="5.416666666666667in" height="5.885416666666667in"}
  [*GetDayBar*]: #_topic_GetDayBar
  [*PlotSample*]: #_topic_BarNummerierungimChart
  [*Print()*]: #_topic_Print
  [*CrossBelow()*]: #_topic_CrossBelow
  [*Rising()*]: #_topic_Rising
  [*Falling()*]: #_topic_Falling
  [*CrossAbove()*]: #_topic_CrossAbove
  [12]: media/image12.png{width="3.3125in" height="2.5833333333333335in"}
  [*BoolSeries*]: #_topic_BoolSeries
  [*FloatSeries*]: #_topic_FloatSeries
  [*IntSeries*]: #_topic_IntSeries
  [*LongSeries*]: #_topic_LongSeries
  [*StringSeries*]: #_topic_StringSeries
  [*PlotColors*]: #_topic_PlotColors
  [*http://msdn.microsoft.com/de-de/library/s1ax56ch%28v=vs.80%29.aspx*]: http://msdn.microsoft.com/de-de/library/s1ax56ch%28v=vs.80%29.aspx
  [*http://msdn.microsoft.com/de-de/library/03ybds8y.aspx*]: http://msdn.microsoft.com/de-de/library/03ybds8y.aspx
  [13]: media/image13.png{width="4.729166666666667in" height="4.0in"}
  [14]: media/image14.png{width="4.708333333333333in" height="4.03125in"}
  [*BarColor*]: #_topic_BarColor
  [*BackColor*]: #_topic_BackColor
  [*BackColorAll*]: #_topic_BackColorAll
  [*BarColorSeries*]: #_topic_BarColorSeries
  [*CandleOutlineColorSeries*]: #_topic_CandleOutlineColorSeries
  [*BackColorSeries*]: #_topic_BackColorSeries
  [*BackColorAllSeries*]: #_topic_BackColorAllSeries
  [*Colors*]: #_topic_FarbenColors
  [*CandleOutlineColor*]: #_topic_CandleOutlineColor
  [15]: media/image15.png{width="4.447916666666667in" height="4.020833333333333in"}
  [16]: media/image16.png{width="4.5in" height="4.020833333333333in"}
  [17]: media/image17.png{width="4.479166666666667in" height="4.0in"}
  [18]: media/image18.png{width="4.489583333333333in" height="4.020833333333333in"}
  [19]: media/image19.png{width="4.40625in" height="3.875in"}
  [20]: media/image20.png{width="5.114583333333333in" height="4.677083333333333in"}
  [*FirstTickOfBar*]: #_topic_FirstTickOfBar
  [*GetCurrentBid()*]: #_topic_GetCurrentBid
  [*GetCurrentAsk()*]: #_topic_GetCurrentAsk
  [*LowestBar()*]: #_topic_LowestBar
  [*AllowRemovalOfDrawObjects*]: #_topic_AllowRemovalOfDrawObjects
  [*AutoScale*]: #_topic_AutoScale
  [*BarsRequired*]: #_topic_BarsRequired
  [*ClearOutputWindow()*]: #_topic_ClearOutputWindow
  [*Displacement*]: #_topic_Displacement
  [*DisplayInDataBox*]: #_topic_DisplayInDataBox
  [*DrawOnPricePanel*]: #_topic_DrawOnPricePanel
  [*InputPriceType*]: #_topic_InputPriceType
  [*Overlay*]: #_topic_Overlay
  [*PaintPriceMarkers*]: #_topic_PaintPriceMarkers
  [*SessionBreakLines*]: #_topic_SessionBreakLines
  [*VerticalGridLines*]: #_topic_VerticalGridLines
  [*TraceOrders*]: #_topic_TraceOrders
  [*PriceType*]: #_topic_PriceType
  [*http://msdn.microsoft.com/de-de/library/system.drawing.pen.aspx*]: http://msdn.microsoft.com/de-de/library/system.drawing.pen.aspx
  [*HighestBar()*]: #_topic_HighestBar
  [*SMA*]: #_topic_SMASimpleMovingAverage
  [*http://msdn.microsoft.com/de-de/library/system.drawing.graphics.aspx*]: http://msdn.microsoft.com/de-de/library/system.drawing.graphics.aspx
  [*ChartControl*]: #_topic_ChartControl
  [*http://msdn.microsoft.com/de-de/library/fht0f5be%28v=vs.80%29.aspx*]: http://msdn.microsoft.com/de-de/library/fht0f5be%28v=vs.80%29.aspx
  [*RemoveDrawObjects()*]: #_topic_RemoveDrawObjects
  [*RemoveDrawObject()*]: #_topic_RemoveDrawObject
  [*www.cmegroup.com*]: http://www.cmegroup.com
  [*www.eurexchange.com/exchange-en/products/idx/dax/17206/*]: file:///C:\Users\goose\Documents\www.eurexchange.com\exchange-en\products\idx\dax\17206\
  [*ToTime*]: #_topic_ToTime
  [*ToDay*]: #_topic_ToDay
  [*Values*]: #_topic_Values
  [*http://vtadwiki.vtad.de/index.php/Andrews\_Pitchfork*]: http://vtadwiki.vtad.de/index.php/Andrews_Pitchfork
  [*http://www.volumen-analyse.de/blog/?p=917*]: http://www.volumen-analyse.de/blog/?p=917
  [*http://www.godmode-trader.de/wissen/index.php/Chartlehrgang:Andrews\_Pitchfork*]: http://www.godmode-trader.de/wissen/index.php/Chartlehrgang:Andrews_Pitchfork
  [21]: media/image21.png{width="0.21875in" height="0.2604166666666667in"}
  [*DrawArrowUp()*]: #_topic_DrawArrowUp
  [*DrawDiamond()*]: #_topic_DrawDiamond
  [*DrawDot()*]: #_topic_DrawDot
  [*DrawSquare()*]: #_topic_DrawSquare
  [*DrawTriangleUp()*]: #_topic_DrawTriangleUp
  [*DrawTriangleDown()*]: #_topic_DrawTriangleDown
  [22]: media/image22.png{width="1.3229166666666667in" height="0.3541666666666667in"}
  [23]: media/image23.png{width="0.22916666666666666in" height="0.20833333333333334in"}
  [*DrawArrowDown()*]: #_topic_DrawArrowDown
  [24]: media/image24.png{width="0.19791666666666666in" height="0.2708333333333333in"}
  [25]: media/image25.png{width="0.19791666666666666in" height="0.20833333333333334in"}
  [*DrawLine()*]: #_topic_DrawLine
  [*DrawHorizontalLine()*]: #_topic_DrawHorizontalLine
  [*DrawVerticalLine()*]: #_topic_DrawVerticalLine
  [*DrawRay()*]: #_topic_DrawRay
  [*DrawExtendedLine()*]: #_topic_DrawExtendedLine
  [26]: media/image26.png{width="0.20833333333333334in" height="0.19791666666666666in"}
  [*DrawTextFixed()*]: #_topic_DrawTextFixed
  [27]: media/image27.png{width="4.21875in" height="2.6145833333333335in"}
  [*DrawText()*]: #_topic_DrawText
  [28]: media/image28.png{width="0.2604166666666667in" height="0.19791666666666666in"}
  [29]: media/image29.png{width="0.23958333333333334in" height="0.20833333333333334in"}
  [30]: media/image30.png{width="6.364583333333333in" height="4.625in"}
  [31]: media/image31.png{width="6.260416666666667in" height="4.625in"}
  [32]: media/image32.png{width="4.583333333333333in" height="4.239583333333333in"}
