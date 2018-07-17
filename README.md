

[![License: CC BY-NC-ND 4.0](https://licensebuttons.net/l/by-nc-nd/4.0/80x15.png)](https://creativecommons.org/licenses/by-nc-nd/4.0/)


YDigital Media Client Side Libraries
====

This set of libraries makes integration to YDigital Media systems a breeze.



## Creatives Lib
This library is intended to be used in creatives developed by YDigital Media and will allow the creative to take advantage of some advanced YD tracking system features like click and interaction tracking, while turning the development easy and fun.
To start using the library you'll need to:

1. [Include the library in your page](#include-the-library-in-your-page);
2. [Change some configuration options (optional)](#configuration-options);
3. [Change your HTML markup to compile with the lib standards](#html-markup);
4. [Learn how to use the library in your js documents, in case you need some advanced features](#javascript-api).

**Note:** This documentation assumes that you have basic knowledge of Javascript and HTML.



#### Include the library in your page
Include the script (at cdn.jsdelivr.net/gh/ydigitalmedia/creatives@4/yd-creatives.js) at the end of the body tag of your HTML document, ie:
```
        ...
        <div>Some content</div>
        <script type="text/javascript" src="//cdn.jsdelivr.net/gh/ydigitalmedia/creatives@4/yd-creatives.js"></script>
    </body>
</html>
```
After inserting the script tag inside the HTML document, there's some options you can use to change the default behavior of the script, please see the next section for details about this.



#### Configuration Options
Almost every part of the lib can be configured. Bellow there's a list of all the configurations you can change and how.

##### By declaring the YD variable
List of available configuration directives:


* **creatives.ydUrlParameter** (string, 'yd')
	Expected YD URL parameter name on input


* **creatives.trackNavigation** (boolean, true)
	If set to false all calls to the trackNavigation method will be ignored


* **creatives.clickClass** (string, '.yd-click')
	The CSS class name responsible for the navigation tracking


* **creatives.openClass** (string, '.yd-open')
	The CSS class name responsible for the click through tracking

Example:
```
        ...
        <div>Some content</div>
        <script type="text/javascript">
            window.YD = {
                logging: true,
                creatives: {
                    ydUrlParameter: 'yd',
                    trackNavigation: true,
                    clickClass: '.yd-click',
                    openClass: '.yd-open'
                }
            };
        </script>
        <script type="text/javascript" src="//cdn.jsdelivr.net/gh/ydigitalmedia/creatives@4/yd-creatives.js"></script>
    </body>
</html>
```



##### By passing parameters in the URL
List of available configuration directives:


* **ydParam** (string, 'yd')
	Expected YD URL parameter name on input


* **clickClass** (string, '.yd-click')
	The CSS class name responsible for the navigation tracking


* **openClass** (string, '.yd-open')
	The CSS class name responsible for the click through tracking


* **navTrack** (boolean, true)
	If set to false all calls to the trackNavigation method will be ignored

Example:
```
<script type="text/javascript" src="//cdn.jsdelivr.net/gh/ydigitalmedia/creatives@4/yd-creatives.js?ydParam=yd"></script>
```
You can also pass any of this configuration parameters in the top link of the page.



#### HTML markup
In order for the lib to track clicks and interactions of the user in your creative you'll need to tell the lib in which page elements you want to track those interactions. You can track **any event** in **any element** inside your page.
For example purposes we will only demonstrate it with link elements.

##### Track user interactions
To track user interactions on a given HTML element, just add the CSS class ***yd-click*** to that HTML element (the name of the CSS class can be configured, see [Configuration Options](#configuration-options) section for more details). See the example below on how to do this in a link element.
```
<!-- Short example
        Action: click
        Screen: home
        Label : Click me please
        Will send to the server: home > click > Click me please
-->
<a href="javascript:void(0)" class="link yd-click">Click me please</a>


<!-- Full example
        Action: click
        Screen: Custom screen
        Label : Custom "Label"
        Will send to the server: Custom screen > click > Custom "Label"
-->
<a href="javascript:void(0)" class="link yd-click" data-ydaction="click" data-ydscreen="Custom screen" data-ydlabel="Custom &quot;Label&quot;">Click me quick</a>
```
###### **Available HTML properties**
None of the following options are required and in case of omission their default values will be used.
* **data-ydaction**  - The name of the action performed by the user, available values are: *click*, *shake*, *share*, *swipe*. The default is *click*.
* **data-ydscreen**  - The name of the screen the user are in. The default is *home*
* **data-ydlabel**   - This is the name / label of the button the user clicked in. By default the lib will try to determine the label by analyzing the html element. If the element is an input of type button then the lib will use the *title* attribute if exists or the *value* attribute otherwise.
If the element is none of the previous types the first existing property will be used from the following list: *title*, *alt*, *textContent*, *innerText*.

**Note:** If you need to use one of the following characters & (ampersand), " (double quote), < (less than) and > (greater than) inside of any property, please use their corresponding HTML entities: *\&amp;*, *\&quot;*, *\&lt;* and *\&gt;*.

---

##### Track clicks (and go to the LP)
A "click" in the advertising context is a special user interaction. It represents a user escape from the ad to the campaign landing page. So, here in our lib we also treat it differently, and we call it ***clickThrough***. To tell the lib that a click on a given button (or at any other element) should open the campaign landing page, we just add the CSS class ***yd-open*** to that HTML element (the name of the CSS class can be configured, see [Configuration Options](#configuration-options) section for more details). See the example below on how to do this in a link element.
```
<!-- Short example
        Screen: home
        Label : Tell me more
        Target: _blank
        Will send to the server: home > open > Click me please
-->
<a href="javascript:void(0)" class="link yd-open">Tell me more</a>


<!-- Full example
        Screen: Custom screen
        Label : Custom "Label"
        Target: _self
        Will send to the server: Custom screen > open > Custom "Label"
-->
<a href="http://www.google.com" target="_blank" class="link yd-open" data-ydscreen="Custom screen" data-ydlabel="Custom &quot;Label&quot;" data-ydtarget="_self" data-ydstop="false">Buy now</a>
```
You need to pay attention to the fact that we, in the full example link, just used *data-ydstop="false"* and *data-ydtarget="_self"* which means that the LP will open in the current window or frame and a new window will also be opened, pointing to *http://www.google.com*. Keep reading to understand why.

###### **Available HTML properties**
None of the following options are required and in case of omission their default values will be used.
* **data-ydscreen**  - The name of the screen the user are in. The default is *home*
* **data-ydlabel**   - This is the name / label of the button the user clicked in. By default the lib will try to determine the label by analyzing the html element. If the element is an input of type button then the lib will use the *title* attribute if exists or the *value* attribute otherwise.
If the element is none of the previous types the first existing property will be used from the following list: *title*, *alt*, *textContent*, *innerText*.
* **data-ydtarget**  - By default the LP will always be opened in a new window, but you can change this behavior by giving another target. You can give any target you want, but there's some reserved tokens that have special meanings. ***_self*** will open the LP in the current window or frame, ***_blank*** will always open the LP in a new window and ***_none*** will not open the LP at all, but will silently register the click, this is useful when the campaign has no LP.
* **data-ydstop**    - By default this is *true*, which means that the event default action will be canceled, without stopping further propagation of the event. See this for more details: https://developer.mozilla.org/en/docs/Web/API/Event/preventDefault

**Note:** If you need to use one of the following characters & (ampersand), " (double quote), < (less than) and > (greater than) inside of any property, please use their corresponding HTML entities: *\&amp;*  *\&quot;*  *\&lt;* and *\&gt;*.




#### Javascript API
Sometimes the HTML markup can be a little restrictive in more complex scenarios, that's why we allow you to use the Javascript API directly.

##### Track user interactions
```
YD.trackNavigation(action = 'click', screen = 'home', btnLabel, useDOM = false)
```
###### Parameters
* **action**    - Optional, the name of the action performed by the user, available options are: *click*, *shake*, *share* and *swipe*. The default is *click*.
* **screen**    - Optional, the name of the screen the user are in. The default is *home*
* **btnLabel**  - This is the name / label of the button the user clicked in. This parameter is required.
* **useDOM**    - Optional, this parameter is *false* by default and in case it's true, a real HTML image tag will be written to the document, otherwise (the default) the Image object will be used to make the server call.

###### Returns
Returns a Promise object. Read more about promises in here: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise

###### Examples
```
// Simple example
YD.trackNavigation(null, null, 'Label');

// Complete example
YD.trackNavigation('click', 'Screen', 'Label', false);

// example with a callback
YD.trackNavigation('click', 'Screen', 'Label', false).then(function(event){
    window.alert('Success!');
});
```

---

##### Track clicks (and go to the LP)
```
YD.clickThrough(screen = 'home', btnLabel, target = '_blank', preventDefault = false, useDOM = false)
```
###### Parameters
* **screen**          - Optional, the name of the screen the user are in. The default is *home*
* **btnLabel**        - This is the name / label of the button the user clicked in. This parameter is required.
* **target**          - Optional, by default the LP will always be opened in a new window, but you can change this behavior by giving another target. You can give any target you want, but there's some reserved tokens that have special meanings. ***_self*** will open the LP in the current window or frame, ***_blank*** will always open the LP in a new window and ***_none*** will not open the LP at all, but will silently register the click, this is useful when the campaign has no LP.
* **preventDefault**  - Optional, by default this parameter is *true*, which means that the event default action will be canceled, without stopping further propagation of the event. See this for more details: https://developer.mozilla.org/en/docs/Web/API/Event/preventDefault
* **useDOM**          - Optional, this parameter is *false* by default and in case it's true, a real HTML image tag will be written to the document, otherwise (the default) the Image object will be used to make the server call.

###### Returns
Returns a Promise object. Read more about promises in here: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise

###### Examples
```
// Simple example
YD.clickThrough(null, 'Label');

// Complete example
YD.clickThrough('Screen', 'Label', '_blank', false, false);

// example with a callback
YD.clickThrough('Screen', 'Label', '_blank', false, false).then(function(event){
    window.alert('Success!');
});
```

---

##### Get tracking-code
Obtain the current tracking-code
```
YD.getTrackingCode()
```
###### Returns
Returns the detected tracking-code or null if none was detected.

---

##### Get impression ID
Obtain the current impression ID
```
YD.getImpressionId()
```
###### Returns
Returns the detected impression ID or null if none was detected.




## License
#### Attribution-NonCommercial-NoDerivatives 4.0 International
[creativecommons.org/licenses/by-nc-nd/4.0/](https://creativecommons.org/licenses/by-nc-nd/4.0/)
