# Creating New SwitchList Styles Using HTML Templates #

SwitchList now allows you to create your own custom, fancy switchlists so you can match handwritten or computer generated switch lists from your favorite railroad.  You describe your switchlist using an HTML "template" - a web page with placeholders at the location of details to fill in later - and the SwitchList program will use your template to present your switchlist on the web interface and normal SwitchList interface.

To get started, download the latest version of SwitchList.  The distribution now includes a folder called
Custom Switchlist Examples" that describes how to install a custom switchlist style, and provides the Thomas example shown here.

You can also download a separate zip file containing only the alternate switchlist styles, and use those as a starting point for your custom switchlist.  Download the "Custom Switchlist Templates.zip" file from http://code.google.com/p/switchlist/downloads/list

## Example Switchlist Template: Thomas ##

Let's start off with an example of a switchlist style you could create yourself.

The Thomas switchlist template uses large text written in the Comic Sans handwritten font and a cartoon picture of a train to display a switchlist more suitable for the four-year-old model railroaders on your layout.  It loads the pictures from the Internet, and positions the pictures using standard HTML formatting.

Here is an example of the Thomas switchlist style in action: http://www.vasonabranch.com/railroad/Thomas.html

Here is the HTML template that creates it: http://code.google.com/p/switchlist/source/browse/trunk/template_examples/Thomas/switchlist.html

If you look at the HTML template, you'll see exactly what you'd need to create for your own switchlist.  Now, let's back up and understand what goes into that template.

## Templates ##

To create a new switchlist, you create a web page (HTML file) but use special placeholders to note where the names of freight cars or the layout appears, and use some special text to instruct what should happen with optional text.  The placeholders and special text you add to the file can be referred to as a "template language" because it's actually a tiny programming language instructing SwitchList how to draw your form.

SwitchList's template language is borrowed from MGTemplateEngine open source project.  (It's similar to other template tools used for creating web pages, such as Django templates.)  Placeholders in your template are strings wrapped in a pair of curly brackets.  Writing the following in your switchlist template
```
{{freightCar.reportingMarks}} 
```

will cause the reporting marks of the current freight car to appear in the resulting switchlist where this appears.
```
Please move {{freightCar.reportingMarks}} from {{freightCar.currentLocation.name}} to {{freightCar.nextStop.name}}.
```
will cause the text "Please move SP 27310 from Ainsley Cannery to San Jose Yard" to appear when freightCar refers to an SP boxcar.

Each placeholder can access parts of an object.  freightCar is a freight car object, and it has various values attached to it - reporting marks, current location, next location, and car type.

Templates can also loop using {% %} tags to group commands in the template language.  Templates can loop over several objects ((% for %}).  The following template would produce a numbered list (the ol tag does an "ordered list") of instructions on where to move each car in the train:
```
<ul>
{% for myFreightCar in train.allFreightCarsInVisitOrder %}
    <li> Please move {{myFreightCar.reportingMarks}} from {{myFreightCar.currentLocation.name}} to {{myFreightCar.nextStop.name}}.
{% /for %}
</ul>
```
Templates can also use conditions.  This snippet either prints "LOADED" if the freight car has a cargo and the cargo is currently loaded in the car, and "EMPTY" otherwise:
```
{% if freightCar.isLoaded %} LOADED {% else %} EMPTY {% /if %} 
```
See the full [documentation for MGTemplateEngine](http://mattgemmell.com/2008/05/20/mgtemplateengine-templates-with-cocoa) to learn more about what you can put in templates, or look at some of the example switchlists.

Now, let's return to the Thomas example now that we've learned how templates work.  The Thomas template loops over all cars with some lines like this:
```
<ol>
{% for car in train.allFreightCarsInVisitOrder %}
  <p>
  <li>Move the {{car.carTypeRel.carTypeDescription}}
  <span class="reportingMarks">{{car.reportingMarks}}</span>
  from   <b>{{car.currentLocation.name}}</b> to
  …
{% /for %}
</ol>
```
The `<ol>` tag creates a numbered list of cars to move.  The train.allFreightCarsInVisitOrder provides a list of cars sorted by the order the train visits each car's town, and each freight car is assigned to "car" on a pass through the loop.  The text in the for loop is drawn once per car; notice how the numbered list item (`<li>`) just appears in the text, and how the reporting marks, long car description, and current location are represented with template expressions.


## Template Filters: ##

MGTemplateEngine lets you specify filters which change the form of a string value.  Filters are added to a value by appending "| filterName".  For example,
```
{{layout.layoutName | uppercase}}
```
would display the layout name in all capital letters.  MGTemplateEngine also defines a dateformat field that allows you to use a  [standard format](http://unicode.org/reports/tr35/tr35-4.html#Date_Format_Patterns) to let you customize how you display the date.  For example, {{ layout.currentDate | date\_format: MM/dd/YY }} displays dates in a form like "3/31/73", while {{ layout.currentDate | date\_format: yyyy-MMM-dd}} displays dates in a form like "2007-Mar-12".

SwitchList also defines its own handy filters.  The jitter filter adds some random spaces at the beginning, middle, or end of a string so that identical strings arranged vertically won't perfectly line up.  This is very helpful for making switchlists with handwritten fonts look handwritten:
```
{{freightCar.reportingMarks | jitter}}  
```
See the default switchlist for an example of jitter's use.

For more sophisticated reports and switchlist styles, you can include JavaScript in your web page in the usual web fashion.  If you embed strings of station or car names, you might have problems if the names have special characters not allowed in JavaScript strings such as single or double quotes.  The js\_escape\_string filter will correctly escape the name values so they may be included within a JavaScript string.

```
var layoutName = "{{layout.layoutName | js_escape_string}}";
```

This new filter only exists in versions of SwitchList 1.21 and greater.

## Creating your own SwitchList ##

To create your own special switchlist, create a folder in ~/Library/Application Support/SwitchList with the name of your switch list style, and place a switchlist.html and, if you want, switchlist.css file in there to get started.

As a starting point, borrow an existing stock switchlist style, either from the SwitchList sources here, or get a set of samples by downloading the "Custom Switchlist Templates.zip" compressed archive from http://code.google.com/p/switchlist/downloads/list

For example, if I wanted a new switchlist called "WP", I would make sure the SwitchList folder existed in Library/Application Support, then create a new directory called WP in that folder.  A file in that folder called switchlist.html would hold the template for the switchlist, and a Cascading Style Sheets (CSS) file called "switchlist.css could be placed in the same directory to hold formatting details for the switchlist.  (Look around on the internet for details about CSS files.)   For security, SwitchList only allows files in the template directory to be read from your machine, though a template could load images and other files out on the Internet.

## Making Switchlists Customizable by Others ##
Many of the details you'd like in a custom switchlist make it specific to your railroad - the name and address of the railroad in a title block, specific colors or logos on the page, or custom instructions for your crew.  SwitchList 1.2.9.2 and greater allows you to identify layout-specific details, and let others customize the template as they see fit.

To identify portions of the template to customize, you do the following:

```
The railroad president is {{ OPTIONAL_Railroad_Owner | default: Leland Stanford }}
```

What this chunk of template says is that there's an optional setting for the name of the railroad owner.  If a setting called "Railroad Owner" is provided, that name will be used.  Otherwise, the text "The railroad president is Leland Stanford" will be displayed.

The variable name in the double curly brackets must start with OPTIONAL_.
In SwitchList, the rest of the name (with under bars switched to spaces) will be displayed as the setting.  The "default:" word is followed with the text that's used if the optional setting is not provided._

Custom template settings can be used in the middle of the template and can include HTML.  A user might fill in an optional setting called "Railroad Address" with "123 N. First St.<br>San Jose CA 95101", where the "<br>" tag indicates a line break.<br>
<br>
<h2>Creating Your Own Reports</h2>

SwitchList also allows you to create the standard reports in a style to match your switch lists.  Custom reports can be used for the Car Report, Industry Report, Reserved Car Report, and Yard Report.<br>
<ul><li>Car Report: defined in a file called car-report.html<br>
</li><li>Industry Report: industry-report.html<br>
</li><li>Reserved Car Report: reserved-car-report.html<br>
</li><li>Yard Report: yard-report.html</li></ul>

The custom reports go in the same folder as the corresponding custom switchlist.<br>
<br>
These reports all follow the same form as a switchlist.  Most will access the layout variable to extract details on any part of your model railroad.  Check out the custom reports in the Line Printer template or Southern Pacific Narrow report for more details.<br>
<br>
<h2>What's Easy, and What's Hard</h2>

There are limitations to what can be done in HTML switchlists; generally, typography, graphic layout, and what is printed for each car can be changed easily.  Changing the order that cars are listed, adding special instructions if a particular car is present, and other more complex tasks that require examining all cars is harder to do.  If you run into problems, raise your suggestions on the mailing list, and I'll see what can be done.<br>
<br>
On the other hand, I was a bit surprised what could be done just in HTML.  The HTML versions of the built-in templates show how handwriting fonts can be downloaded from Google Web Fonts, and jittery text placed at slightly different locations be done with some random scattering of spaces.<br>
<br>
<h2>Making Switchlists Easy to Read on iPhone and iPad</h2>

Showing switchlists on smaller screens - for example an iPhone or iPad - can be tough, but SwitchList lets you change how a custom switchlist is displayed on these different devices.  There are two ways you can change the appearance of your switchlist: you can have different CSS files for your different devices, or you can define a separate iPhone-only version of the switchlist template.<br>
<br>
Web pages already have the idea of drawing content differently on different devices.  Typically, this is done using CSS so that portions of the document have different sizes, different font choices, or have portions disappear completely.  By placing all the position and size style attributes in the CSS, loading a different file can make a page more acceptable for an iPad or iPhone.<br>
<br>
Here's an example used by SwitchList (and also used by real web pages):<br>
<pre><code>&lt;link href='switchlist-iphone.css' rel='stylesheet' type='text/css' media='only screen and (max-device-width: 480px)'/&gt;<br>
&lt;link href='switchlist-ipad.css' rel='stylesheet' type='text/css' media='only screen and (min-device-width: 481px) and (max-device-width: 1024px)'/&gt;<br>
&lt;link href='switchlist.css' rel='stylesheet' type='text/css' media='all and (min-device-width: 1025px)'&gt;<br>
</code></pre>
These three lines look like normal references to a CSS file, but each has a 'media' attribute that indicates which kinds of devices should load which link.  Different css files are loaded for each size of screen, and a different css file could be loaded when printing (as opposed to viewing) a page.<br>
<br>
Your custom switchlist style can use the same trick, adding these three lines at the top of the HTML file, then defining three separate css files documenting sizes and positions of elements when shown on the corresponding device.<br>
<br>
The second way you can change how switchlists appear is to define a different HTML file.  SwitchList already does this; if you look at the Handwriting switchlist style from the web interface, you'll see a very different screen showing only the reporting marks, from, and to locations, all in large letters.  This couldn't easily be done in css alone; instead, when an iPhone connects to SwitchList's web server, SwitchList will check if a file called switchlist-iphone.html exists in the current style's folder, and if so uses that file instead of the normal switchlist.html.<br>
<br>
The separate iPhone HTML file can draw the switchlist in any way that's handy for small devices.  That HTML file can also load a separate CSS file if you want to hide some details of layout and style in the other file.<br>
<br>
For security, the SwitchList web server will only allow you to reference and view files in your template's directory.  It won't allow reading arbitrary files on your computer.  You can also reference any web pages or resources on other web sites to get images or fonts.<br>
<br>
<h2>Other Examples</h2>

Although all the standard switchlist styles are handled<br>
by the SwitchList program,<br>
the HTML versions of these switchlist styles (as seen in the Web interface)<br>
use HTML templates, just like you'd use for your custom template.<br>
Check out those examples in the <a href='http://code.google.com/p/switchlist/source/browse/#svn%2Ftrunk%2Fsrc%2Fbuiltin-templates'>src/builtin-templates directory</a> in the SwitchList sources.<br>
The template for the stock "Handwritten" template<br>
is in the <a href='http://code.google.com/p/switchlist/source/browse/#svn%2Ftrunk%2Fsrc%2Ftemplates%253Fstate%253Dclosed'>src/templates directory</a> in the<br>
SwitchList source.  You can examine these files from code.google.com's<br>
code browser.<br>
<br>
<h2>Template Variables</h2>

The key to defining templates is knowing what values you can print in a switchlist template.  Each switch list gets a set of variables that can be accessed via templates.  The templates can actually access lots of other internals about your layout, but these are the ones that are the most useful and guaranteed not to change.<br>
<br>
<h2>Variables Accessible to Switch List Template</h2>
The following variables are accessible from templates:<br>
<table><thead><th> train </th><th> current train. (Train object) </th></thead><tbody>
<tr><td> firstStation </td><td> Station where the train starts. </td></tr>
<tr><td> lastStation </td><td> Station where the train ends. </td></tr>
<tr><td> freightCars </td><td> list of freight cars being carried in visiting order (contains FreightCar objects) </td></tr>
<tr><td> layout </td><td> current layout. (Layout object) </td></tr>
<tr><td> randomValue </td><td> integer value between 0 and 9, used for putting fingerprints, dogear or other bits of realism on switchlists. </td></tr>
<tr><td> interactive </td><td> defined and set to 1 if switchlist is used in web interface.  Undefined if template is being used to print a switchlist.  This allows you to hide controls when printing.</td></tr></tbody></table>

Some of these variables are plain string or number values (such as firstStation or randomValue), while others are objects containing their own values.  Here is a list of the kinds of objects visible in a template, and the values attached to each.<br>
<br>
<table><thead><th> <b>Layout</b> </th><th> </th></thead><tbody>
<tr><td> currentDate   </td><td>  Usually formatted with a template filter to draw the date in a sane manner.  </td></tr>
<tr><td> layoutName    </td><td>  The layout description as entered into the Layout tab of SwitchList. </td></tr>
<tr><td> allStationsSortedOrder </td><td>   List of all stations / places on the layout, sorted alphabetically. </td></tr>
<tr><td> allIndustries </td><td> List of all industries.  Unsorted. </td></tr>
<tr><td> stationsWithWork </td><td> List of all the stations where the train has cars to pick up or drop off.  Each station is just a collection of industries with cars to pick up and drop off.  Used for drawing the PICL report. </td></tr>
<tr><td> <b>Train</b>  </td><td> </td></tr>
<tr><td> name        </td><td>string name for train. </td></tr>
<tr><td> freightCars   </td><td>   list of freight cars in the train in no order. </td></tr>
<tr><td> freightCarsInVisitOrder: </td><td> list of freight cars in the train, sorted by the order that towns will be visited. </td></tr>
<tr><td> stationsInOrder </td><td> list of the stations visited by this train in the order visited.  Contains duplicates if the same town is visited twice. </td></tr>
<br>
<tr><td> <b>Freight Car</b> </td><td> </td></tr>
<tr><td> reportingMarks </td><td> String containing owning railroad and car number ("SP 27310") </td></tr>
<tr><td> initials      </td><td> String containing railroad initials only. (Car "SP 27310" would return "SP") </td></tr>
<tr><td> number        </td><td> String containing car number only.  (Car "SP 27310" would return "27310") </td></tr>
<tr><td> cargo         </td><td> Current cargo being carried or assigned to freight car. </td></tr>
<tr><td> isLoaded      </td><td> True if car is carrying cargo.  If false, car may not have a cargo assigned, or may be travelling to the location where the cargo is picked up. </td></tr>
<tr><td> carType       </td><td> Abbreviation for the freight car's type of car (T for tank car, XM for short boxcar, etc.) </td></tr>
<tr><td> currentLocation </td><td> industry or yard where car is currently located. (Industry object.) </td></tr>
<tr><td> nextStop      </td><td> Destination this session.  (Industry object.) </td></tr>
<tr><td> nextDoor      </td><td> Specific door where car should be placed at next industry. (Version 1.3 and after) </td></tr>
<tr><td> <b>Cargo</b>  </td><td> </td></tr>
<tr><td> name          </td><td> short description of cargo. </td></tr>
<tr><td> description   </td><td>  long description of cargo, including what kind of car and how many cars per week.  </td></tr>
<tr><td> <b>Industry</b> </td><td> </td></tr>
<tr><td> location      </td><td> Town where industry is located. </td></tr>
<tr><td> name          </td><td> Name of industry. </td></tr>
<tr><td> <b>Station</b> (Also called place or town) </td><td> </td></tr>
<tr><td> name          </td><td> Name of station. </td></tr>
<tr><td> allIndustriesSortedOrder </td></tr></tbody></table>
