
# light-trans
A light weight multilingual & localization framework for your web application.

Light-trans is zero dependency multilingual & localization framework for vanilla javaScript. It is worked for any web based app build on Cordova, Node Webkit, Electron, etc. It is also great for normal websites.

# Installation
With NPM :

    npm i light-trans

# How to use

## Step 1. Installation

Include light-trans into your application.

NW (Node Webkit) and other Node embedded engine
```javascript
// create a new instance of Light Trans
const LT = require("light-trans");
```
Browser 
```html
<script type="text/javascript" src="light-trans.js"></script>
```

## Step 2. Initialization

Initialization
```javascript
// create a new instance of Light Trans
// run this at the top most of your code. You want to run light-trans before all of your code
const lt = new LT();
```
Light-trans will load the last selected language by user. Rest assured, no cookie are involved.

## Step 3. Marking for translation

**Marking your JavaScript code :**
```javascript
// instead of writing your codes directly like this :
alert("Hello world");
document.getElementById('text').innerHTML = "some string"

// wrap all your translatable string with the listener you defined before (by default the listener is "_t" function)
alert(_t("Hello world"));
document.getElementById('text').innerHTML = _t("some string")
// notice the "_t" function that encapsulate the texts that marked for translation.
// you can change the listener function as you like.
```

**Marking your HTML code :**
To mark inner html for translation, use "**data-tran**" attribute to the target element :
```html
<p data-tran="">This text between p tags will be subject for translation</p>
```

To mark attributes for translation, add **"data-tranattr"** attributes into target elements referring to the attribute name. Separate attribute name with spaces for multiple attributes.
```html
<input type="button" title="this is a very cool button" data-tranattr="title" />
<img src="coolimg.png" title="a very cool image" alt="cool image" data-tranattr="title alt" />
<input type="text" value="search" title="search" placeholder="Start searching" data-tranattr="title value placeholder" />
```

## Step 4. Create a translation table

There is several way to create translation table.
The most straight forward way is to define **translationPair** in the instance of light-trans.
```javascript
lt.set('translationPair', {
	"text to be translated" : "translation result",
	"hello" : "こんにちは",
	"this is a very cool button" : "これはとてもクールなボタンです",
	"This text between p tags will be subject for translation" : "pタグ間のこのテキストは翻訳されます"
})
```

Or, you can import from external .trans format.
```javascript
lt.from("lang/jp.trans");
```
You can use [Translator++](http://dreamsavior.net/download), [a free GUI editor to create and edit .trans document.](http://dreamsavior.net)
![enter image description here](https://github.com/Dreamsaviors/light-trans/blob/master/doc/translator++.png?raw=true)


## Collecting translatable text
You can manually collecting the translatable text and made it an object of source-translation pair :

    {
	    "text to translate" : "translation"
    }

Or you can fetch them programmatically by activating recorder mode on light-trans
```javascript
// activating recorder mode on initialization
var lt = new LT({
	recordMode:true,
	listener:"t"
});
```
```javascript
// activating recorder mode via setter
lt.set("recordMode", true)
```

After that, browse through all the windows of your application. Every text passed on the listener and marked HTML element will be recorded.
After that, download the result by running this command **in your console window** :
```javascript
lt.dump()
```
Rename *"dump.trans"* and save to the location that accessible from the script.

# Options
Below is the options and their default value.
```javascript
var lt = new LT({
	// enable record mode. Always disable this on production environment
	recordMode:false,
	
	// marker for HTML elements to fetch the attributes
	attrGetter:"data-tranattr",
	 
	// marker for HTML elements to fetch the innerHTML 
	innerHTMLGetter:"data-tran", 

	// listener for javaScripts 
	defaultListener:"_t", 
	
	// translation pair
	translationPair : {}
});
```

# Method

## set(key, value)
Attributes / method setter

**Returned value** 
Instance of LT

**Example** 
```javascript
lt.set("translationPair", {
	"hello" : "こんにちは"
});
```

## get(key, value)
Attributes / method getter

**Returned value** 
Any value by "key" keyword

**Example** 
```javascript
lt.get("translationPair");
```

## getConfig(key)
Get the save-able configuration data.

**Returned value** 
Any value by "key" keyword

**Example** 
```javascript
lt.getConfig("lang");
```

## save(key, value)
Save the save-able configuration data on the client

**Returned value** 
none

**Example** 
```javascript
lt.getConfig("lang", "EN-US");
```

## createListener(name[,  function])
Create a string wrapper for translatable texts within javaScript script.

**Returned value** 
none

**Example** 
```javascript
lt.createListener("t");
```
usage :
```javascript
alert(t("some cool text"))
```

You can also create numbering / date format localization by declaring the second argument
```javascript
lt.createListener("money", function(value) {
	// do something with the value here
	return newValue
});
```
usage :
```javascript
document.querySelectorAll("#theMoney")[0] = money("2.000.000,00")
```

## from(source)
Generate translation pairs from the "source"
if source is url, then this method will fetch the url and parse the contents.

**Returned value** 
none

**Example** 
```javascript
lt.from("/lang/en-US.trans");
```
```javascript
lt.from("https://yourapiurl.net/lang/en-US.trans");
```

## dump()
Dump collected translation into a file. A download file dialog will appear.

**Returned value** 
none

**Example** 
```javascript
lt.dump();
```
if you want to get the data without downloading it, you can access the registered translatable texts in "reg" attributes.
```javascript
lt.reg;
```

## getTrans()
Dump collected translation into a [trans](http://dreamsavior.net/docs/translator-developers-guide/trans-format/) formatted object.

**Returned value** 
object: [.trans](http://dreamsavior.net/docs/translator-developers-guide/trans-format/) formatted object.

**Example** 
```javascript
lt.getTrans();
```


## translate(string)
Translate a string using predefined translation pairs.

**Returned value** 
string: translated text

**Example** 
```javascript
lt.translate("hello world")
```

## translateHTML()
Trigger HTML translation

**Returned value** 
none

**Example** 
```javascript
lt.translateHTML()
```

## init()
initialize the light-trans

**Returned value** 
none

**Example** 
```javascript
lt.init()
```

# License

MIT License

Copyright (c) 2020 Dreamsaviors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.