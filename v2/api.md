AWTRIX 2.0 has the possibility to be controlled by external systems. MQTT or REST API can be used for this.

For MQTT you can connect AWTRIX to an existing MQTT broker (You can do this in your systemsettings).
The base topic is **awtrix**.

For the REST API the base endpoint is  
**http://[AWTRIX-SERVER_IP]:7000/api/v3**


## Basic Controls

### Endpoint

?>  **/api/v3/basics**

**Method**

?> **POST**
___

### MQTT Topic
 
?> **awtrix/basics** 
___

### power
Turn AWTRIX On (true) or Off (false)

``` JSON
{"power": true}
```
___

### switchTo

Switch to the defined App

**Params**  
- switchTo: Appname

``` JSON
{"switchTo":"facebook"}
```
___

### Appstate

disables/enables an app

**Params**  
- enable: Appname
- disable: Appname

``` JSON
{"disable":"facebook"}
```
___

### app

Controls the Apploop

**Params**  
- app
  - "next" : next App
  - "back" : previous App
  - "pause" : pause the Apploop (toggle)

``` JSON
{"app":"next"}
```
___
### msgShort

scrolls the given text once

**Params**  
- msgShort: Textstring

``` JSON
{"msgShort":"Hello World"}
```
___
### msgEndless

scrolls the given text endless

**Params**  
- msgEndless: Textstring

``` JSON
{"msgEndless":"Hello World"}
```
___
### msgColor

Changes the color of the scrolling Text

**Params**  
- msgColor: Array of Integer [R,G,B]

``` JSON
{"msgColor":[255,0,0]}
```

___
### msgStop

stop message scrolling

``` JSON
{"msgStop":true}
```
___

___
### timer

starts a timer for the given timespan and shows an alert when the time has expired.

**Params** 
- timer: timespan in ```"hours:minutes:seconds"``` 
- color: Array of Integer [R,G,B] (optional)
- count: how many times the timer should blink (optional)
- text: text wich will shown (static, up to 8 characters) (optional)

``` JSON
{"timer":"00:00:10","color":[255,0,0],"count":10,"text":"AWTRIX"}

{"timer":"stop"} removes the timer
```
___

### Get basic informations

MQTT publish the information to the topic "[prefix]/response"

#### Values
- installedApps:  
  - returns all installed Apps
- activeApps:  
  - returns all active Apps
- settings:       
  - returns all Settings
- version:        
  - returns AWTRIX version
- uptime:         
  - returns AWTRIXs uptime
- matrixInfo:     
  - returns all informations from the connected Matrix

``` JSON
{"get":"activeApps"}
``` 


___


## Change Settings

All settings can be changed here.
As key the same keys are used as they can be found under system settings.
all values are Strings

### Endpoint
 
?> **/api/v3/settings**

**Method**

?> **POST**
___

### MQTT Topic
 
?> **awtrix/settings** 

___


Set one or more settings

``` JSON
{"brightness":100}
```

___


## Display informations in Appstyle

### Endpoint
 
?> **/api/v3/app**

 **Method**

?> **POST**
___
### MQTT Topic
 
?> **awtrix/app** 
___
 

Displays a temporary App with the given informations  
You can set predefined Icons uploaded with [AWTRIXER](https://blueforcer.de/download) as well with pure BMP data [Array of RGB565 Integer]. You can get this data also from AWTRIXER.
  

**Params**  
- force
  - Whether the given informations should be displayed immediately or after the current app (true/false).
  if set to false the given AppInformations are sorted into a Appqueue. After the current App, AWTRIX will show and delete all apps of the Appqueue one by one. So you are able to send many temporary apps at once. If there are no more apps in the queue, AWTRIX will continue to run its own apps.
- text
  - Text to be displayed (string)
- icon (optional)
  - icon ID of the Icon uploaded with AWTRIXER **OR** [Array of RGB565 Integer]
- color (optional)
  - custom textcolor (Array of Integer [R,G,B])
- moveIcon (optional)
  - Moves the Icon out of the screen to free space for text (true/false)
- count (optional)
  - how many times the text should scroll before switching to the next app. If the text doesnt need to scroll (because of the textlength) it will use the global appduration to switch. (Integer)
  
  
```Example
{"force":false,"icon":6,"text":"Awtrix","color":[255,0,0]}
```

___

## Drawing

### Endpoint
 
?>  /api/v3/draw

**Method**

?> **POST**
___
### Topic
 
```Topic
awtrix/draw
``` 
___

You can send various drawing commands to AWTRIX to create your individual screen. All methods are static, automatic scrolling of text is not possible. But you have the possibility to create your own drawingflow. For this you can define several commands in one JSON string which AWTRIX then processes one after the other. This makes even smaller animations possible.
The drawingmode starts with the first command.

Each command only fills the framebuffer. You need to run show to finally display the data on the matrix 


The following example is structured as follows:
 - Fill the entire Screen with grey
 - Write "Hello"
 - Show the last two commands on the matrix
 - Wait 3 Seconds before the next command
 - Draw a circle next to the Text
 - And show it
 - Wait another 3 seconds
 - Clear the whole frame
 - Draw a line from left-top corner to bottom right corner
 - Show the line on the matrix
 - Wait 3 Seconds
 - Exit the drawingmode
 - reapeat all twice

!> **Please note:** You need to call exit to leave the drawingmode and go back to normal state. If you set repeat, the exit command will be ignored and automatically exits the drawingmode when all repeats are done.  

```Example
{
  "repeat":2,
  "draw": [
    {
      "type": "fill",
      "color": [100,100,100]
    },
    {
      "type": "text",
      "string": "Hello",
      "position": [0,0],
      "color": [255,0,0]
    },
    {
      "type": "show"
    },
    {
      "type": "wait",
      "ms": 3000
    },
    {
      "type": "circle",
      "radius": 3,
      "position": [24,3],
      "color": [255,0,255]
    },
    {
      "type": "show"
    },
    {
      "type": "wait",
      "ms": 3000
    },
    {
      "type": "clear"
    },
    {
      "type": "line",
      "start": [0,0],
      "end": [31,7],
      "color": [255,255,255]
    },
    {
      "type": "show"
    },
    {
      "type": "wait",
      "ms": 3000
    },
    {
      "type": "exit"
    }
  ]
}
```


**Possible commands**

?> The first pixel (upper left corner) has the coordinate [0,0] while the last one (lower right corner) has [31,7]  

**<span style="color:blue">loop</span>** How often the drawing routines should be repeated (optional)

- **<span style="color:blue">text</span>** Displays a Text
  - string 
  - position 
    - Array of Integer [X,Y]
  - color
    - Array of Integer [R,G,B]
- **<span style="color:blue">rect</span>** Displays a Rectangle
  - position
    - Array of Integer [X,Y]
  - size
    - Array of Integer [Width,Height]
  - color
    - Array of Integer [R,G,B]
- **<span style="color:blue">line</span>** Displays a Line
  - start
    - Array of Integer [X0,Y0]
  - end
    - Array of Integer [X1,Y1]
  - color´
    - Array of Integer [R,G,B]
- **<span style="color:blue">circle</span>** Displays a Circle
  - position
    - Array of Integer [X,Y]
  - radius
    - Integer r
  - color
    - Array of Integer [R,G,B]
- **<span style="color:blue">pixel</span>**  Displays a single Pixel
  - position
    - Array of Integer [X,Y]
  - color
    - Array of Integer [R,G,B]
- **<span style="color:blue">bmp</span>**  Displays a Bitmap
  - position
    - Array of Integer [X,Y]
  - size
    - Array of Integer [Width,Height]
  - data
    - Array of RGB565 Integer [p0,p1,p2,p3,...]
- **<span style="color:blue">fill</span>**  Fills the entire Matrix with one Color
  - color
    - Array of Integer [R,G,B]
- **<span style="color:blue">wait</span>** Wait X milliseconds before the next Command
  - ms
    - Integer ms
- **<span style="color:blue">show</span>**  Shows all previours commands
- **<span style="color:blue">clear</span>** Clear the Matrix
- **<span style="color:blue">exit</span>**  Exit drawing mode and go back to normal state
