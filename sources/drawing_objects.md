
#DrawingObjects

## DrawAndrewsPitchfork()
### Description
This drawing object draws an Andrew’s Pitchfork.

Information concerning its usage:
-   [*vtad.de*](http://vtadwiki.vtad.de/index.php/Andrews\_Pitchfork)
-   [*hvolumen-analyse.de*](http://www.volumen-analyse.de/blog/?p=917)
-   [*Godmode-Trader.de*](http://www.godmode-trader.de/wissen/index.php/Chartlehrgang:Andrews\_Pitchfork)

### Usage
```cs
DrawAndrewsPitchfork(string name, bool autoScale, int start1BarsAgo, double start1Y, int start2BarsBack, double start2Y, int start3BarsAgo, double start3Y, Color color, DashStyle dashStyle, int width)
DrawAndrewsPitchfork(string name, bool autoScale, DateTime start1Time, double start1Y, DateTime start2Time, double start2Y, DateTime start3Time, double start3Y, Color color, DashStyle dashStyle, int width)
```

### Return Value
A drawing object of the type IAndrewsPitchfork (interface)

### Parameter
|                |                                                                                         |
|----------------|-----------------------------------------------------------------------------------------|
| name           | A clearly identifiable name for the drawing object                                      |
| autoScale      | Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety |
| start1BarsAgo  | Number of bars ago for start point 1 (x-axis)                                           |
| start1Time     | Date/time for start point 1 (x-axis)                                                    |
| start1Y        | y-value for start point 1                                                               |
| start2BarsAgo  | Number of bars ago for start point 2 (x-axis)                                           |
| start2Time     | Date/time for start point 2 (x-axis)                                                    |
| start2Y        | y-value for start point 2                                                               |
| start3BarsAgo  | Number of bars ago for start point 3 (x-axis)                                           |
| start3Time     | Date/time for start point 3 (x-axis)                                                    |
| start3Y        | y-value for start point 3                                                               |
| color          | Color of the object                                                                     |
| dashStyle      | Line styles:                                                                            |
|                |  DashStyle.Dash, DashStyle.DashDot, DashStyle.DashDotDot, DashStyle.Dot DashStyle.Solid |
|                | You need to integrate:                                                                  |
|                | using System.Drawing.Drawing2D;.                                                        |
| width          | Line strength in points                                                                 |

### Example
```cs
// Draw the Andrew’s Pitchfork (“MyAPF”)
DrawAndrewsPitchfork("MyAPF", true, 4, Low[4], 3, High[3], 1, Low[1], Color.Black, DashStyle.Solid, 2);
```

## DrawArc()
### Description
DrawArc() draws a circular arc.

### Usage
```cs
DrawArc(string name, int barsBackStart, double startY, int barsBackEnd, double endY, Color color)
DrawArc(string name, bool autoScale, int barsBackStart, double startY, int barsBackEnd, double endY, Color color, DashStyle dashStyle, int width)
DrawArc(string name, bool autoScale, DateTime startTime, double startY, DateTime endTime, double endY, Color color, DashStyle dashStyle, int width)
```

### Return Value
A drawing object of the type IArc (interface)

### Parameter
|              |                                                                                         |
|--------------|-----------------------------------------------------------------------------------------|
| name         | A clearly identifiable name for the drawing object                                      |
| autoScale    | Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety |
| barsBackStart| Number of bars ago for the starting point                                               |
| startTime    | Date/time for the starting point                                                        |
| startY       | y-value for the starting point                                                          |
| barsBackEnd  | Number of bars ago for the end point                                                    |
| endTime      | Date/time for the end point                                                             |
| endY         | y-value for the end point                                                               |
| color        | Color of the drawing object                                                             |
| dashStyle    | Line style                                                                              |                                                                                           
|              |  DashStyle.Dash                                                                         |  
|              |  DashStyle.DashDot                                                                      |  
|              |  DashStyle.DashDotDot                                                                   |  
|              |  DashStyle.Dot                                                                          |  
|              |  DashStyle.Solid                                                                        |  
|              |                                                                                         |  
|              |  You may have to integrate:                                                             |  
|              |  using System.Drawing.Drawing2D;                                                        |
| width        | Line strength in points                                                                 |

### Example
```cs
// Draws a blue arc
DrawArc("MyArc", true, 10, 10, 0, 20, Color.Blue, DashStyle.Solid, 3);
```

## DrawArrowDown()
### Description
DrawArrowDown() draws an arrow pointing downwards:

![DrawArrowDown()](./media/image21.png)

See [*DrawArrowUp()*](#drawarrowup), [*DrawArrowDown()*](#drawarrowdown), [*DrawDiamond()*](#drawdiamond), [*DrawDot()*](#drawdot), [*DrawSquare()*](#drawsquare), [*DrawTriangleUp()*](#drawtriangleup), [*DrawTriangleDown()*](#drawtriangledown).

### Usage
```cs   
DrawArrowDown(string name, bool autoScale, int barsAgo, double y, Color color)
DrawArrowDown(string name, bool autoScale, DateTime time, double y, Color color)
```

### Return Value
A drawing object of the type IArrowDown (interface)

### Parameter
|           |                                                                                         |
|-----------|-----------------------------------------------------------------------------------------|
| name      | A clearly identifiable name for the drawing object                                      |
| autoScale | Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety |
| barsAgo   | Sets the preceding bar on which the arrow should be drawn (0 = current bar)             |
| time      | Date/time of the bar on which the arrow should be drawn                                 |
| y         | y-value for the arrow                                                                   |
| color     | Color of the drawing object                                                             |

### Example
```cs
// Draws a red arrow 3 ticks above the high for the current bar
DrawArrowDown("MyArrow", true, 0, High[0] + 3*TickSize, Color.Red);
// Draws a red arrow on a three-bar reversal pattern
if(High[2] > High[3] && High[1] > High[2] && Close[0] < Open[0])
DrawArrowDown(CurrentBar.ToString(), true, 0, High[0] + 3*TickSize, Color.Red);
```

## DrawArrowLine()
### Description
DrawArrowLine() draws an arrow:

![DrawArrowLine()](./media/image22.png)

### Usage
```cs
 DrawArrowLine (string name,  int  barsBackStart,  double  startY,  int  barsBackEnd,  double  endY, Color color)
 DrawArrowLine (string name,  bool  autoScale,  int  barsBackStart,  double  startY,  int  barsBackEnd,  double  endY, Color color, DashStyle dashStyle,  int  width)
 DrawArrowLine (string name,  bool  autoScale, DateTime startTime,  double  startY, DateTime endTime,  double  endY, Color color, DashStyle dashStyle,  int  width)
```

### Return Value
A drawing object of the type IArrowLine (interface)

### Parameter
|              |                                                                                         |
|--------------|-----------------------------------------------------------------------------------------|
| name         | A clearly identifiable name for the drawing object                                      |
| autoScale    | Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety |
| barsBackStart| Sets the preceding bar at which the arrow should start (0 = current bar)                |
| startTime    | Date/time of the bar at which the arrow should start                                    |
| startY       | y-value for the starting point of the arrow                                             |
| barsBackEnd  | Sets the preceding bar at which the arrow should end (0 = current bar)                  |
| endTime      | Date/time at which the arrow should end                                                 |
| endY         | y-value at which the arrow should end                                                   |
| color        | Color of the drawing object                                                             |
| dashStyle    | Line style                                                                              |                                                                                           
|              |  DashStyle.Dash                                                                         |  
|              |  DashStyle.DashDot                                                                      |  
|              |  DashStyle.DashDotDot                                                                   |  
|              |  DashStyle.Dot                                                                          |  
|              |  DashStyle.Solid                                                                        |  
|              |                                                                                         |  
|              |  You may have to integrate:                                                             |  
|              |  using System.Drawing.Drawing2D;                                                        |
| width        | Line strength in points                                                                 |

### Example
```cs
// Draws a black arrow
DrawArrowLine("MyArrow", false, 10, 10, 0, 5, Color.Black, DashStyle.Solid, 4);
```

## DrawArrowUp()
### Description
DrawArowUp() draws an arrow pointing upwards:

![DrawArrowUp()](./media/image23.png)

See [*DrawArrowUp()*](#drawarrowup), [*DrawArrowDown()*](#drawarrowdown), [*DrawDiamond()*](#drawdiamond), [*DrawDot()*](#drawdot), [*DrawSquare()*](#drawsquare), [*DrawTriangleUp()*](#drawtriangleup), [*DrawTriangleDown()*](#drawtriangledown).

### Usage
```cs
DrawArrowUp(string name, bool autoScale, int barsAgo, double y, Color color)
DrawArrowUp(string name, bool autoScale, DateTime time, double y, Color color)
```

### Return Value
A drawing object of the type IArrowUp (interface)

### Parameter
|           |                                                                                         |
|-----------|-----------------------------------------------------------------------------------------|
| name      | A clearly identifiable name for the drawing object                                      |
| autoScale | Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety |
| barsAgo   | Sets the preceding bar on which the arrow should be drawn (0 = current bar)             |
| time      | Date/time at which the arrow should be drawn                                            |
| y         | y-value for the arrow                                                                   |
| color     | Color of the drawing object                                                             |

### Example
```cs
// Draws a green arrow for the current bar 3 ticks below the low
DrawArrowUp("MyArrow", true, 0, Low[0] - 3*TickSize, Color.Green);
```

## DrawDiamond()
### Description
DrawDiamond() draws a diamond:

![DrawDiamond()](./media/image24.png)

See [*DrawArrowUp()*](#drawarrowup), [*DrawArrowDown()*](#drawarrowdown), [*DrawDiamond()*](#drawdiamond), [*DrawDot()*](#drawdot), [*DrawSquare()*](#drawsquare), [*DrawTriangleUp()*](#drawtriangleup), [*DrawTriangleDown()*](#drawtriangledown).

### Usage
```cs
DrawDiamond(string name, bool autoScale, int barsAgo, double y, Color color)
DrawDiamond(string name, bool autoScale, DateTime time, double y, Color color)
```

### Return Value
A drawing object of the type IDiamond (interface)

### Parameter
|           |                                                                                         |
|-----------|-----------------------------------------------------------------------------------------|
| name      | A clearly identifiable name for the drawing object                                      |
| autoScale | Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety |
| barsAgo   | Defines the preceding bar on which the diamond should be drawn                          |
| time      | Date/time of the bar on which the diamond should be drawn                               |
| y         | y-value on which the diamond should be drawn                                            |
| color     | Color of the drawing object                                                             |

### Example
```cs
// Draws a light blue diamond for the current bar 5 ticks below the low
DrawDiamond("MyDiamond", true, 0, Low[0] - 5*TickSize, Color.SteelBlue);
```

## DrawDot()
### Description
DrawDot() draws a dot:

![DrawDot()](./media/image25.png)

See [*DrawArrowUp()*](#drawarrowup), [*DrawArrowDown()*](#drawarrowdown), [*DrawDiamond()*](#drawdiamond), [*DrawDot()*](#drawdot), [*DrawSquare()*](#drawsquare), [*DrawTriangleUp()*](#drawtriangleup), [*DrawTriangleDown()*](#drawtriangledown).

### Usage
```cs
DrawDot(string name, bool autoScale, int barsAgo, double y, Color color)
DrawDot(string name, bool autoScale, DateTime time, double y, Color color)
```

### Return Value
A drawing object of the type IDot (interface)

### Parameter
|           |                                                                                         |
|-----------|-----------------------------------------------------------------------------------------|
| name      | A clearly identifiable name for the drawing object                                      |
| autoScale | Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety |
| barsAgo   | Defines the preceding bar on which the dot should be drawn (0 = current bar)            |
| time      | The date/time at which the dot should be drawn                                          |
| y         | y-value at which the dot should be drawn                                                |
| color     | Color of the drawing object                                                             |

### Example
```cs
// Draws an orange dot for the current bar 5 ticks above the high
DrawDot("MyDot", true, 0, High[0] + 5*TickSize, Color.Orange);
```

## DrawEllipse()
### Description
DrawEllipse() draws an ellipse.

### Usage
```cs
DrawEllipse(string name, int barsBackStart, double startY, int barsBackEnd, double endY, Color color)
DrawEllipse(string name, bool autoScale, int barsBackStart, double startY, int barsBackEnd, double endY, Color color, Color areaColor, int areaOpacity)
DrawEllipse(string name, bool autoScale, DateTime startTime, double startY, DateTime endTime, double endY, Color color, Color areaColor, int areaOpacity)
```
### Return Value
A drawing object of the type IEllipse (interface)

### Parameter
|              |                                                                                         |
|--------------|-----------------------------------------------------------------------------------------|
| name         | A clearly identifiable name for the drawing object                                      |
| autoScale    | Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety |
| barsBackStart| Sets the preceding bar at which the ellipse should start                                |
| startTime    | Date/time at which the ellipse should start                                             |
| startY       | y-value for the start of the ellipse                                                    |
| barsBackEnd  | Sets the preceding bar at which the ellipse should end (0 = current bar)                |
| endTime      | Date/time at which the ellipse should end                                               |
| endY         | y-value for the end of the ellipse                                                      |
| color        | Border color of the drawing object                                                      |
| areaColor    | Fill color of the drawing object                                                        |
| areaOpacity  | Transparency of the fill color value between 0 and 10 (0 = completely transparent , 10 = completely opaque) |

### Example
```cs
// Draws a yellow ellipse from the current bar to 5 bars ago
DrawEllipse("MyEllipse", true, 5, High[5], 0, Close[0], Color.Yellow, Color.Yellow, 1);
```

## DrawExtendedLine()
### Description
DrawExtendedLine() draws a line with an infinite end point.

See [*DrawLine()*](#drawline), [*DrawHorizontalLine()*](#drawhorizontalline), [*DrawVerticalLine()*](#drawverticalline), [*DrawRay()*](#drawray).

### Usage
```cs
DrawExtendedLine(string name, int barsBackStart, double startY, int barsBackEnd, double endY, Color color)
DrawExtendedLine(string name, bool autoScale, int barsBackStart, double startY, int barsBackEnd, double endY, Color color, DashStyle dashStyle, int width)
DrawExtendedLine(string name, bool autoScale, DateTime startTime, double startY, DateTime endTime, double endY, Color color, DashStyle dashStyle, int width)
```

### Return Value
A drawing object of the type IExtendedLine (interface)

### Parameter
|              |                                                                                                         |
|--------------|---------------------------------------------------------------------------------------------------------|
| name         | A clearly identifiable name for the drawing object                                                      |
| autoScale    | Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety                 |
| barsBackStart| Number of bars ago for the start point                                                                  |
| startTime    | Date/time for the start point                                                                           |
| startY       | y-value for the start point                                                                             |
| barsBackEnd  | Number of bars ago for the second point (a true end point does not exist; the line extends to infinity) |
| endTime      | Date/time for the end point                                                                             |
| endY         | y-value for the end point                                                                               |
| color        | Color of the drawing object                                                                             |
| dashStyle    | Line style                                                                               |                                                                                           
|              |  DashStyle.Dash                                                                                         |  
|              |  DashStyle.DashDot                                                                                      |  
|              |  DashStyle.DashDotDot                                                                                   |  
|              |  DashStyle.Dot                                                                                          |  
|              |  DashStyle.Solid                                                                                        |  
|              |                                                                                                         |  
|              |  You may have to integrate:                                                                             |  
|              |  using System.Drawing.Drawing2D;                                                                        |
| width        | Line strength in points                                                                                 |

### Example
```cs
// Draws a line without an end point
DrawExtendedLine("MyExt.Line", false, 10, Close[10], 0, Close[0], Color.Black, DashStyle.Solid, 1);
```

## DrawFibonacciCircle()
### Description
DrawFibonacciCircle() draws a Fibonacci circle.

### Usage
```cs
DrawFibonacciCircle(string name, bool autoScale, int barsBackStart, double startY, int barsBackEnd, double endY)
DrawFibonacciCircle(string name, bool autoScale, DateTime startTime, double startY, DateTime endTime, double endY)
```

### Return Value
A drawing object of the type IFibonacciCircle (interface)

### Parameter
|              |                                                                                         |
|--------------|-----------------------------------------------------------------------------------------|
| name         | A clearly identifiable name for the drawing object                                      |
| autoScale    | Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety |
| barsBackStart| Defines the starting point in terms of bars ago                                         |
| startTime    | Date/time of the bar for the starting point                                             |
| startY       | y-value for the start of the Fibonacci circle                                           |
| barsBackEnd  | Defines the end point in terms of bars ago                                              |
| endTime      | Date/time for the end of the Fibonacci circle                                           |
| endY         | y-value for the end point of the Fibonacci circle                                       |

### Example
```cs
//Draws a Fibonacci circle
DrawFibonacciCircle("MyFibCircle", true, 5, Low[5], 0, High[0]);
```

## DrawFibonacciExtensions()
### Description
DrawFibonacciExtensions() draws Fibonacci extensions.

### Usage
```cs
DrawFibonacciExtensions(string name, bool autoScale, int start1BarsAgo, double start1Y, int start2BarsAgo, double start2Y, int start3BarsAgo, double start3Y)
DrawFibonacciExtensions(string name, bool autoScale, DateTime start1Time, double start1Y, DateTime start2Time, double start2Y, DateTime start3Time, double start3Y)
```

### Return Value
A drawing object of the type IFibonacciExtensions (interface)

### Parameter
|                |                                                                                         |
|----------------|-----------------------------------------------------------------------------------------|
| name           | A clearly identifiable name for the drawing object                                      |
| autoScale      | Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety |
| start1BarsAgo  | Number of bars ago for start point 1                                                    |
| start1Time     | Date/time for start point 1                                                             |
| start1Y        | y-value for start point 1                                                               |
| start2BarsAgo  | Number of bars ago for start point 2                                                    |
| start2Time     | Date/time for start point 2                                                             |
| start2Y        | y-value for the start point 2                                                           |
| start3BarsAgo  | Number of bars ago for start point 3                                                    |
| start3Time     | Date/time for start point 3                                                             |
| start3Y        | y-value for start point 3                                                               |

### Example
```cs
// Draws Fibonacci extensions
DrawFibonacciExtensions("MyFibExt", true, 4, Low[4], 3, High[3], 1, Low[1]);
```
## DrawFibonacciProjections()
### Description
Draw Fibonacci Projections () sketches Fibonacci Projections.

### Usage
```cs
DrawFibonacciProjections(string name, bool autoScale, DateTime start1Time, double start1Y,DateTime start2Time, double start2Y, DateTime start3Time, double start3Y)
```

### Return Value
A drawing object of the type IFibonacciProjections (Interface)

### Parameter
|                |                                                                                         |
|----------------|-----------------------------------------------------------------------------------------|
| name           | A clearly identifiable name for the drawing object                                      |
| autoScale      | Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety |
| start1Time     | Date/time for start point 1                                                             |
| start1Y        | y-value for start point 1                                                               |
| start2Time     | Date/time for start point 2                                                             |
| start2Y        | y-value for the start point 2                                                           |
| start3Time     | Date/time for start point 3                                                             |
| start3Y        | y-value for start point 3                                                               |

### Example
```cs
// zeichnet FibonacciProjections
DrawFibonacciProjections("MyFibPro", true, Low[4], 3, High[3], 1, Low[1], 2);

```

## DrawFibonacciRetracements()
### Description
DrawFibonacciRetracements() draws Fibonacci retracements.

### Usage
```cs
DrawFibonacciRetracements(string name, bool autoScale, int barsBackStart, double startY, int barsBackEnd, double endY)
DrawFibonacciRetracements(string name, bool autoScale, DateTime startTime, double startY, DateTime endTime, double endY)
```

### Return Value
A drawing object of the type IFibonacciRetracements (interface)

### Parameter
|              |                                                                                         |
|--------------|-----------------------------------------------------------------------------------------|
| name         | A clearly identifiable name for the drawing object                                      |
| autoScale    | Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety |
| barsBackStart| Defines how many bars ago the starting point of the Fibonacci retracement is located    |
| startTime    | Date/time of the bar at which the Fibonacci retracement should begin                    |
| startY       | y-value at which the Fibonacci retracement will begin                                   |
| barsBackEnd  | Defines how many bars ago the end point of the Fibonacci retracement is located         |
| endTime      | Date/time at which the Fibonacci retracement should end                                 |
| endY         | y-value at which the Fibonacci retracement should end                                   |

### Example
```cs
// Draws Fibonnaci retracements
DrawFibonacciRetracements("MyFibRet", true, 10, Low[10], 0, High[0]);
```

## DrawFibonacciTimeExtensions()
### Description
DrawFibonacciTimeExtensions() draws Fibonacci time extensions.

### Usage
```cs
DrawFibonacciTimeExtensions(string name, int barsBackStart, double startY, int barsBackEnd, double endY)
DrawFibonacciTimeExtensions(string name, DateTime startTime, double startY, DateTime endTime, double endY)
```

### Return Value
A drawing object of the type IFibonacciTimeExtensions (interface)

### Parameter
|              |                                                       |
|--------------|-------------------------------------------------------|
| name         | A clearly identifiable name for the drawing object    |
| barsBackStart| Defines how many bars ago the extensions should start |
| startTime    | Date/time at which the extensions should start        |
| startY       | y-value at which the extensions should start          |
| barsBackEnd  | Defines how many bars ago the extensions should end   |
| endTime      | Date/time at which the extensions should end          |
| endY         | y-value at which the extensions should end            |

### Example
```cs
// Draws Fibonacci time extensions
DrawFibonacciTimeExtensions("MyFibTimeExt", 10, Low[10], 0, High[0]);
```

## DrawGannFan()
### Description
DrawGannFan() draws a Gann fan.

### Usage
```cs
DrawGannFan(string name, bool autoScale, int barsAgo, double y)
DrawGannFan(string name, bool autoScale, DateTime time, double y)
```

### Return Value
A drawing object of the type IGannFan (interface)

### Parameter
|           |                                                                                         |
|-----------|-----------------------------------------------------------------------------------------|
| name      | A clearly identifiable name for the drawing object                                      |
| autoScale | Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety |
| barsAgo   | Sets the preceding bar on which the Gann fan should be drawn                            |
| time      | Date/time at which the Gann fan should start                                            |
| y         | y-value for the Gann fan                                                                |

### Example
```cs
// Shows a Gann fan at the low of the bar from 10 periods ago
DrawGannFan("MyGannFan", true, 10, Low[10]);
```

## DrawLine()
### Description
DrawLine() draws a (trend) line.

See [*DrawHorizontalLine()*](#drawhorizontalline), [*DrawVerticalLine()*](#drawverticalline), [*DrawExtendedLine()*](#drawextendedline), [*DrawRay()*](#drawray).

### Usage
```cs
DrawLine(string name, int barsBackStart, double startY, int barsBackEnd, double endY, Color color)
DrawLine(string name, bool autoScale, int barsBackStart, double startY, int barsBackEnd, double endY, Color color, DashStyle dashStyle, int width)
DrawLine(string name, bool autoScale, DateTime startTime, double startY, DateTime endTime, double endY, Color color, DashStyle dashStyle, int width)
```

### Return Value
A drawing object of the type ITrendLine (interface).

### Parameter
|              |                                                                                         |
|--------------|-----------------------------------------------------------------------------------------|
| name         | A clearly identifiable name for the drawing object                                      |
| autoScale    | Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety |
| barsBackStart| Number of bars ago for the starting point                                               |
| startTime    | Date/time for the starting point                                                        |
| startY       | y-value for the starting point                                                          |
| barsBackEnd  | Number of bars ago for the end point                                                    |
| endTime      | Date/time for the end point                                                             |
| endY         | y-value for the end point                                                               |
| color        | Color of the drawing object                                                             |
| dashStyle    | Line style                                                                              |                                                                                           
|              |  DashStyle.Dash                                                                         |  
|              |  DashStyle.DashDot                                                                      |  
|              |  DashStyle.DashDotDot                                                                   |  
|              |  DashStyle.Dot                                                                          |  
|              |  DashStyle.Solid                                                                        |  
|              |                                                                                         |  
|              |  You may have to integrate:                                                             |  
|              |  using System.Drawing.Drawing2D;                                                        |
| width        | Line strength in points                                                                 |

### Example
```cs
// Draws a line
DrawLine("MyLine", false, 10, Close[10], 0, Close[0], Color.Black, DashStyle.Solid, 1);
```

## DrawHorizontalLine()
### Description
DrawHorizontalLine() draws a horizontal line in the chart.

See [*DrawLine()*](#drawline), [*DrawVerticalLine()*](#drawverticalline), [*DrawExtendedLine()*](#drawextendedline), [*DrawRay()*](#drawray).

### Usage
```cs
DrawHorizontalLine(string name, double y, Color color)
DrawHorizontalLine(string name, bool autoScale, double y, Color color, DashStyle dashStyle, int width)
```

### Return Value
A drawing object of the type IHorizontalLine (interface)

### Parameter
|           |                                                                                         |
|-----------|-----------------------------------------------------------------------------------------|
| name      | A clearly identifiable name for the drawing object                                      |
| autoScale | Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety |
| y         | Any double value of your choice                                                         |
| color     | Line color                                                                              |
| dashStyle | Line style                                                                              |                                                                                           
|           |  DashStyle.Dash                                                                         |  
|           |  DashStyle.DashDot                                                                      |  
|           |  DashStyle.DashDotDot                                                                   |  
|           |  DashStyle.Dot                                                                          |  
|           |  DashStyle.Solid                                                                        |  
|           |                                                                                         |  
|           |  You may have to integrate:                                                             |  
|           |  using System.Drawing.Drawing2D;                                                        |
| width     | Line strength                                                                           |

### Example
```cs
// Draws a horizontal line at y=10
DrawHorizontalLine("MyHorizontalLine", 10, Color.Black);
```

## DrawRay()
### Description
DrawRay() draws a (trend) line and extends it to infinity.

See [*DrawLine()*](#drawline), [*DrawHorizontalLine()*](#drawhorizontalline), [*DrawVerticalLine()*](#drawverticalline), [*DrawExtendedLine()*](#drawextendedline).

### Usage
```cs
DrawRay(string name, int start1BarsAgo, double start1Y, int start2BarsAgo, double start2Y, Color color)
DrawRay(string name, bool autoScale, int start1BarsAgo, double start1Y, int start2BarsAgo, double start2Y, Color color, DashStyle dashStyle, int width)
DrawRay(string name, bool autoScale, DateTime start1Time, double start1Y, DateTime start2Time, double start2Y, Color color, DashStyle dashStyle, int width)
```

### Return Value
A drawing object of the type IRay (interface)

### Parameter
|                |                                                                                         |
|----------------|-----------------------------------------------------------------------------------------|
| name           | A clearly identifiable name for the drawing object                                      |
| autoScale      | Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety |
| start1BarsAgo  | Number of bars ago for start point 1                                                    |
| start1Time     | Date/time for anstartchor point 1                                                       |
| start1Y        | y-value for start point 1                                                               |
| start2BarsAgo  | Number of bars ago for start point 2                                                    |
| start2Time     | Date/time for start point 2                                                             |
| start2Y        | y-value for start point 2                                                               |
| color          | Color of the drawing object                                                             |
| dashStyle      | Line style                                                                              |                                                                                           
|                |  DashStyle.Dash                                                                         |  
|                |  DashStyle.DashDot                                                                      |  
|                |  DashStyle.DashDotDot                                                                   |  
|                |  DashStyle.Dot                                                                          |  
|                |  DashStyle.Solid                                                                        |  
|                |                                                                                         |  
|                |  You may have to integrate:                                                             |  
|                |  using System.Drawing.Drawing2D;                                                        |
| width          | Line strength                                                                           |

### Example
```cs
// Draws a line from the bar from 10 periods ago to the current bar (x-axis)
// --> line is extended to the right
// from y=3 to y=7
DrawRay("MyRay", 10, 3, 0, 7, Color.Green);
// Draws a line from the current bar to the bar from 10 periods ago
// --> line is extended to the left
// from y=3 to y=7
DrawRay("MyRay", 0, 3, 10, 7, Color.Green);
```

## DrawRectangle()
### Description
DrawRectangle() draws a rectangle.

### Usage
```cs
DrawRectangle(string name, int barsBackStart, double startY, int barsBackEnd, double endY, Color color)
DrawRectangle(string name, bool autoScale, int barsBackStart, double startY, int barsBackEnd, double endY, Color color, Color areaColor, int areaOpacity)
DrawRectangle(string name, bool autoScale, DateTime startTime, double startY, DateTime endTime, double endY, Color color, Color areaColor, int areaOpacity)
```
### Return Value
A drawing object of the type IRectangle (interface)

### Parameter
|              |                                                                                                        |
|--------------|--------------------------------------------------------------------------------------------------------|
| name         | A clearly identifiable name for the drawing object                                                     |
| autoScale    | Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety                |
| barsBackStart| Sets the preceding bar at which the one corner of the rectangle should be located (0 = current bar)    |
| startTime    | Date/time at which the start of the one rectangle corner should be located                             |
| startY       | y-value at which the one corner of the rectangle should be located                                     |
| barsBackEnd  | Sets the preceding bar at which the second corner of the rectangle should be located (0 = current bar) |
| endTime      | Date/time of the second rectangle corner                                                               |
| endY         | y-value of the second rectangle corner                                                                 |
| color        | Color of the drawing object                                                                            |
| areaColor    | Fill color of the drawing object                                                                       |
| areaOpacity  | Transparency of the fill color. Value between 0 and 10 (0 = completely transparent, 10 = completely opaque) |

### Example
```cs
// Draws a green rectangle from the low of 10 periods ago to the high of 5 periods ago
// with a fill color of pale green and a transparency of 2
DrawRectangle("MyRect", true, 10, Low[10], 5, High[5], Color.PaleGreen, Color.PaleGreen, 2);
```

## DrawRegion()
### Description
DrawRegion() fills a specific area on a chart.

### Usage
```cs
DrawRegion(string name, int barsBackStart, int barsBackEnd, IDataSeries series, double y, Color outlineColor, Color areaColor, int areaOpacity)
DrawRegion(string name, int barsBackStart, int barsBackEnd, IDataSeries series1, IDataSeries series2, Color outlineColor, Color areaColor, int areaOpacity)
DrawRegion(string name, DateTime startTime, DateTime endTime, IDataSeries series, double y, Color outlineColor, Color areaColor, int areaOpacity)
DrawRegion(string name, DateTime startTime, DateTime endTime, IDataSeries series1, IDataSeries series2, Color outlineColor, Color areaColor, int areaOpacity)
```

### Return Value
A drawing object of the type IRegion (interface)

### Parameter
|                  |                                                                                   |
|------------------|-----------------------------------------------------------------------------------|
| name             | A clearly identifiable name for the drawing object                                |
| barsBackStart    | Sets the preceding bar at which the drawing should begin (0 = current bar)        |
| startTime        | Start time for the drawing                                                        |
| barsBackEnd      | Sets the preceding bar at which the drawing should end (0 = current bar)          |
| endTime          | End time for the drawing                                                          |
| series1, series2 | Every data series, for example an indicator, close, high, low and so on. The respective value of the data series for the current bar is used as a y-value.  |
| y                | Any double value                                                                  |
| outlineColor     | Color for the border                                                              |
| areaColor        | Fill color for the area                                                           |
| areaOpacity      | Transparency of the fill color. Value between 0 and 10 (0 = completely transparent, 10 = completely opaque) |

### Example
```cs
// Fills the area between the upper and lower Bollinger Bands
DrawRegion("MyRegion", CurrentBar, 0, Bollinger(2, 14).Upper, Bollinger(2, 14).Lower, Color.Empty, Color.Lime, 100);
```

## DrawRegressionChannel()
### Description
DrawRegressionChannel() draws a regression channel.

### Usage
```cs
DrawRegressionChannel(string name, int barsBackStart, int barsBackEnd, Color color)
DrawRegressionChannel(string name, bool autoScale, int barsBackStart, int barsBackEnd, Color upperColor, DashStyle upperDashStyle, int upperWidth, Color middleColor, DashStyle middleDashStyle, int middleWidth, Color lowerColor, DashStyle lowerDashStyle, int lowerWidth)
DrawRegressionChannel(string name, bool autoScale, DateTime startTime, DateTime endTime, Color upperColor, DashStyle upperDashStyle, int upperWidth, Color middleColor, DashStyle middleDashStyle, int middleWidth, Color lowerColor, DashStyle lowerDashStyle, int lowerWidth)
```

### Return Value
A drawing object of the type IRegressionChannel (interface)

### Parameter
|                  |                                                                                         |
|------------------|-----------------------------------------------------------------------------------------|
| name             | A clearly identifiable name for the drawing object                                      |
| autoScale        | Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety |
| barsBackStart    | Sets the preceding bar at which the regression channel should start (0 = current bar)   |
| startTime        | Start time for the regression channel                                                   |
| barsBackEnd      | Sets the preceding bar at which the regression channel should end (0 = current bar)     |
| endTime          | End time for the regression channel                                                     |
| color            | Color of the drawing object                                                             |
| upperDashStyle, middleDashStyle, lowerDashStyle    |                                                                              
| dashStyle        | Line style                                                                              |                                                                                           
|                  |  DashStyle.Dash                                                                         |  
|                  |  DashStyle.DashDot                                                                      |  
|                  |  DashStyle.DashDotDot                                                                   |  
|                  |  DashStyle.Dot                                                                          |  
|                  |  DashStyle.Solid                                                                        |  
|                  |                                                                                         |  
|                  |  You may have to integrate:                                                             |  
|                  |  using System.Drawing.Drawing2D;                                                        |
| upperColor,  middleColor,    lowerColor        | Line color                                                                              |
| upperWidth,   middleWidth,  lowerWidth        | Line strength                                                                           |

### Example
```cs
// Draws a regression channel from the low of the bar from 10 days ago to the high of the bar from 5 days ago.
DrawRegressionChannel("MyRegChannel", 10, 0, Color.Black);
```

## DrawSquare()
### Description
DrawSquare() draws a square:

![DrawSquare()](./media/image26.png)

See [*DrawArrowUp()*](#drawarrowup), [*DrawArrowDown()*], [*DrawDiamond()*], [*DrawDot()*], [*DrawTriangleUp()*], [*DrawTriangleDown()*].

### Usage
```cs
DrawSqare(string name, bool autoScale, int barsAgo, double y, Color color)
DrawSqare(string name, bool autoScale, DateTime time, double y, Color color)
```

### Return Value
A drawing object of the type ISquare (interface)

### Parameter
|           |                                                                                         |
|-----------|-----------------------------------------------------------------------------------------|
| name      | A clearly identifiable name for the drawing object                                      |
| autoScale | Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety |
| barsAgo   | Sets the preceding bar at which the square should be drawn (0 = current bar)            |
| Time      | Date/time of the bar at which the square should be drawn                                |
| Y         | y-value for the square                                                                  |
| color     | Color of the drawing object                                                             |

### Example
```cs
// Draws a dark red square at the current bar 10 ticks above the high
DrawSquare("MySquare", true, 0, High[0] + 10*TickSize, Color.DarkRed);
```

## DrawText()
### Description
DrawText() writes whatever text you want onto the chart.

See [*DrawTextFixed()*], *PlotMethod*.

### Usage
```cs
DrawText(string name, string text, int barsAgo, double y, Color color)
DrawText(string name, bool autoScale, string text, int barsAgo, double y, int yPixelOffset, Color textColor, Font font, StringAlignment alignment, Color outlineColor, Color areaColor, int areaOpacity)
DrawText(string name, bool autoScale, string text, DateTime x, double y, int yPixelOffset, Color textColor, Font font, StringAlignment alignment, Color outlineColor, Color areaColor, int areaOpacity)
DrawText(string name, bool autoScale, string text, int barsAgo, double y, int yPixelOffset, Color textColor, Font font, StringAlignment alignment, HorizontalAlignment HAlign, VerticalAlignment VAlign, Color outlineColor, Color areaColor, int areaOpacity)
DrawText(string name, bool autoScale, string text, DateTime x, double y, int yPixelOffset, Color textColor, Font font, StringAlignment alignment, HorizontalAlignment HAlign, VerticalAlignment VAlign, Color outlineColor, Color areaColor, int areaOpacity)
```

**Important note:**
When using signatures that contain horizontal alignment and vertical alignment, you need to add the following lines:
```cs
using System.Windows.Forms;
using System.Windows.Forms.VisualStyles;
```

### Return Value
A drawing object of the type IText (interface)

### Parameter
|              |                                                                                             |
|--------------|---------------------------------------------------------------------------------------------|
| name         | A clearly identifiable name for the drawing object                                          |
| autoScale    | Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety     |
| Text         | Text to be displayed (may contain escape sequences)                                         |
| barsAgo      | Sets how many bars ago the text should be displayed                                         |
| Time         | Date/time of the bar at which the text should begin                                         |
| Y            | y-value at which the text should be written                                                 |
| yPixelOffset | Vertical offset of the text; positive numbers move it up, and negative numbers move it down |
| textColor    | Text color                                                                                  |
| Font         | Font                                                                                        |
| Alignment    | Possible values are:                                                                        
                - StringAlignment.Center                                                                     
                - StringAlignment.Far                                                                        
                - StringAlignment.Near                                                                       |
| HAlign       | Possible values are:                                                                        
                - HorizontalAlign.Left                                                                       
                - HorizontalAlign.Center                                                                     
                - HorizontalAlign.Right                                                                      |
| VAlign       | Possible values are:                                                                        
                - VerticalAlign.Top                                                                          
                - VerticalAlign.Center                                                                       
                - VerticalAlign.Bottom                                                                       |
| outlineColor | Border color around the text                                                                
                For no border, select Color.Empty                                                            |
| areaColor    | Fill color for the text box                                                                 |
| areaOpacity  | Transparency of the fill color                                                              
                Value between 0 and 10                                                                       
                0 = completely transparent                                                                   
                10 = completely opaque                                                                       |

### Example
```cs
// writes text at y=3.0
DrawText("MyText", "This is sample text.", 10, 3, Color.Black);
// writes red text in the font Arial 7
DrawText("MyText", false, "This is sample text.", Time[0], Close[0]+50*TickSize, 0,
Color.Red, new Font("Arial",7), StringAlignment.Center, Color.Blue, Color.DarkOliveGreen, 10);
```

This leads to the following result:

![DrawText()](./media/image27.png)

```cs
DrawText("MyTag",true,"Text",1,
// barsAgo
High[1], // y
10, // yPixelOffset
Color.Blue, // Text color
new Font("Arial", 10, FontStyle.Bold),
StringAlignment.Center,
HorizontalAlignment.Center,
VerticalAlignment.Bottom,
Color.Red, // Outline color
Color.Yellow, // Fill color
100); // Opacity
```

## DrawTextFixed()
### Description
DrawTextFixed() writes text into one of 5 predetermined locations on the chart.

See [*DrawText()*].

### Usage
```cs
DrawTextFixed(string name, string text, TextPosition textPosition)
DrawTextFixed(string name, string text, TextPosition textPosition, Color textColor, Font font, Color outlineColor, Color areaColor, int areaOpacity)
```

### Return Value
A drawing object of the type ITextFixed (interface)

### Parameter
|              |                                                                            |
|--------------|----------------------------------------------------------------------------|
| name         | A clearly identifiable name for the drawing object                         |
| text         | The text to be displayed                                                   |
| TextPosition | TextPosition.BottomLeft                                                    
                TextPosition.BottomRight                                                    
                TextPosition.Center                                                         
                TextPosition.TopLeft                                                        
                TextPosition.TopRight                                                       |
| textColor    | Text color                                                                 |
| font         | Font                                                                       |
| outlineColor | Color for the border around the text. For no border color, use Color.Empty |
| areaColor    | Fill color of the text box                                                 
                For no fill color, use Color.Empty                                          |
| areaOpacity  | Transparency of the fill color                                             
                Value between 0 and 10                                                      
                0 = completely transparent                                                  
                10 = completely opaque                                                      |

### Example
```cs
// Writes text into the middle of the chart
DrawTextFixed("MyText", "This is sample text.", TextPosition.Center);
// Writes red text with a blue border into the middle of the chart
DrawTextFixed("MyText", "This is sample text.", TextPosition.Center,
Color.Red, new Font("Arial",35), Color.Blue, Color.Empty, 10);
```

## DrawTrendChannel()
### Description
DrawTrendChannel() draws a trend channel.

### Usage
```cs
DrawTrendChannel(string name, bool autoScale, int start1BarsAgo, double start1Y, int start2BarsAgo, double start2Y, int start3BarsAgo, double start3Y)
DrawTrendChannel(string name, bool autoScale, DateTime start1Time, double start1Y, DateTime start2Time, double start2Y, DateTime start3Time, double start3Y)
```

### Return Value
A drawing object of the type ITrendChannel (interface)

### Parameter
|                |                                                                                         |
|----------------|-----------------------------------------------------------------------------------------|
| name           | A clearly identifiable name for the drawing object                                      |
| autoScale      | Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety |
| start1BarsAgo  | Number of bars ago for start point 1 (x-axis)                                           |
| start1Time     | Date/time for start point 1 (x-axis)                                                    |
| start1Y        | y-value for start point 1                                                               |
| start2BarsAgo  | Number of bars ago for start point 2 (x-axis)                                           |
| start2Time     | Date/time for start point 2 (x-axis)                                                    |
| start2Y        | y-value for start point 2                                                               |
| start3BarsAgo  | Number of bars ago for start point 3 (x-axis)                                           |
| start3Time     | Date/time for start point 3 (x-axis)                                                    |
| start3Y        | y-value for start point 3                                                               |

### Example
```cs
// Draws a trend channel
DrawTrendChannel("MyTrendChannel", true, 10, Low[10], 0, High[0], 10, High[10] + 5 * TickSize);
```

## DrawTriangle()
### Description
DrawTriangle() draws a triangle.

### Usage
```cs
DrawTriangle(string name, int start1BarsAgo, double start1Y, int start2BarsAgo, double start2Y, int start3BarsAgo, double start3Y, Color color)
DrawTriangle(string name, bool autoScale, int start1BarsAgo, double start1Y, int start2BarsAgo, double start2Y, int start3BarsAgo, double start3Y, Color color, Color areaColor, int areaOpacity)
DrawTriangle(string name, bool autoScale, DateTime start1Time, double start1Y, DateTime start2Time, double start2Y, DateTime start3Time, double start3Y, Color color, Color areaColor, int areaOpacity)
```

### Return Value
A drawing object of the type ITriangle (interface)

### Parameter
|                |                                                                                         |
|----------------|-----------------------------------------------------------------------------------------|
| name           | A clearly identifiable name for the drawing object                                      |
| autoScale      | Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety |
| start1BarsAgo  | Number of bars ago for start point 1 (x-axis)                                           |
| start1Time     | Date/time for start point 1 (x-axis)                                                    |
| start1Y        | y-value for start point 1                                                               |
| start2BarsAgo  | Number of bars ago for start point 2 (x-axis)                                           |
| start2Time     | Date/time for start point 2 (x-axis)                                                    |
| start2Y        | y-value for start point 2                                                               |
| start3BarsAgo  | Number of bars ago for start point 3 (x-axis)                                           |
| start3Time     | Date/time for start point 3 (x-axis)                                                    |
| start3Y        | y-value for start point 3                                                               |
| color          | Color of the drawing object                                                             |
| areaColor      | Fill color of the drawing object                                                        |
| areaOpacity    | Transparency of the fill color                                                          
                  Value between 0 and 10                                                                   
                  0 = completely transparent                                                               
                  10 = completely opaque                                                                   |

### Example
```cs
// Draws a green triangle
DrawTriangle("tag1", 4, Low[4], 3, High[3], 1, Low[1], Color.Green);
```

## DrawTriangleUp()
### Description
DrawTriangleUp() draws a small upwards-pointing triangle:

![DrawTriangleUp()](./media/image28.png)

See [*DrawArrowUp()*](#drawarrowup), [*DrawArrowDown()*], [*DrawDiamond()*], [*DrawDot()*], [*DrawSquare()*], [*DrawTriangleDown()*].

### Usage
```cs
DrawTriangleUp(string name, bool autoScale, int barsAgo, double y, Color color)
DrawTriangleUp(string name, bool autoScale, DateTime time, double y, Color color)
```

### Return Value
A drawing object of the type ITriangleUp (interface)

### Parameter
|           |                                                                                         |
|-----------|-----------------------------------------------------------------------------------------|
| name      | A clearly identifiable name for the drawing object                                      |
| autoScale | Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety |
| barsAgo   | Sets how many bars ago the triangle should be drawn                                     |
| time      | Date/time for the bar at which the triangle should be drawn                             |
| y         | y-value at which the triangle should be drawn                                           |
| color     | Color of the drawing object                                                             |

### Example
```cs
// Draws a small light green triangle at the current bar 10 ticks below the low
DrawTriangleUp("MyTriangleUp", true, 0, Low[0] - 10*TickSize, Color.LightGreen);
```

## DrawTriangleDown()
### Description
DrawTriangleDown() draws a small downwards-pointing triangle:

![DrawTriangleDown()](./media/image29.png)

See [*DrawArrowUp()*](#drawarrowup), [*DrawArrowDown()*], [*DrawDiamond()*], [*DrawDot()*], [*DrawSquare()*], [*DrawTriangleUp()*].

### Usage
```cs
DrawTriangleDown(string name, bool autoScale, int barsAgo, double y, Color color)
DrawTriangleDown(string name, bool autoScale, DateTime time, double y, Color color)
```

### Return Value
A drawing object of the type ITriangleDown (interface)

### Parameter
|           |                                                                                         |
|-----------|-----------------------------------------------------------------------------------------|
| name      | A clearly identifiable name for the drawing object                                      |
| autoScale | Adjusts the scale of the y-axis so that drawing objects can be viewed in their entirety |
| barsAgo   | Sets how many bars ago the triangle should be drawn                                     |
| time      | Date/time for the bar at which the triangle should be drawn                             |
| Y         | y-value at which the triangle should be drawn                                           |
| color     | Color of the drawing object                                                             |

### Example
```cs
// Draws a small red triangle at the current bar 10 ticks above the high
DrawTriangleDown("MyTriangleDown", true, 0, High[0] + 10*TickSize, Color.Red);
```

## DrawVerticalLine()
### Description
DrawVerticalLine() draws a vertical line in the chart.

See [*DrawLine()*](#drawline), [*DrawHorizontalLine()*](#drawhorizontalline), [*DrawExtendedLine()*](#drawextendedline), [*DrawRay()*](#drawray).

### Usage
```cs
DrawVerticalLine(string name, int barsAgo, Color color)
DrawVerticalLine(string name, int barsAgo, Color color, DashStyle dashStyle, int width)
DrawVerticalLine(string name, DateTime time, Color color, DashStyle dashStyle, int width)
```

### Return Value
A drawing object of the type IVerticalLine (interface)

### Parameter
|           |                                                                            |
|-----------|----------------------------------------------------------------------------|
| name      | A clearly identifiable name for the drawing object                         |
| barsAgo   | Sets how many bars ago the vertical line should be drawn (0 = current bar) |
| time      | Date/time of the bar at which the vertical line should be drawn            |
| color     | Line color                                                                 |
| dashStyle | Line style                                                                 

             DashStyle.Dash                                                              
             DashStyle.DashDot                                                           
             DashStyle.DashDotDot                                                        
             DashStyle.Dot                                                               
             DashStyle.Solid                                                             

             You may have to integrate:                                                  
             using System.Drawing.Drawing2D;                                             |
| width     | Line strength                                                              |

### Example
```cs
// Draws a vertical line at the bar from 10 periods ago
DrawVerticalLine("MyVerticalLine", 10, Color.Black);
```
