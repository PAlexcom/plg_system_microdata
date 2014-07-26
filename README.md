Microdata
==============
A Joomla ```3.2+``` system plugin that uses the [JMicrodata](https://github.com/joomla/joomla-cms/tree/master/libraries/joomla/microdata "JMicrodata") library.  
Created during the Google Summer of Code 2014.


If you want to keep your views separated from the logic, ```plg_system_microdata``` is system plugin for parsing the HTML markup and convert the ```data-*``` HTML5 attributes in Microdata semantics.  

The ```data-*``` attributes are new in HTML5, they gives us the ability to embed custom data attributes on all HTML elements. So if you disable the library output, the HTML will still be validated.  

Installation
============
[Download the last version of the plugin](https://github.com/PAlexcom/plg_system_microdata/archive/master.zip "Download plg_system_microdata") and install it. __Don't forget to publish this plugin!__

Usage
=====
##### Syntax
![plg_system_microdata Syntax](https://palexcom.github.io/PHPStructuredData/images/parser-plugin-syntax-v1.1.0.png)

Publisher
=========
![plg_system_microdata content editor usage](https://palexcom.github.io/plg_system_structureddata/images/plg_system_structureddata-editor.png)

Developer
=========
```html
<div data-sd="Article">
    <!-- Title -->
    <span data-sd="name">
        How to Tie a Reef Knot
    </span>
    <!-- Author -->
    <span data-sd="author">
        Written by
        <span data-sd="Person">
            <span data-sd="name">John Doe</span>
        </span>
    </span>
    <!-- Date published -->
    <meta data-sd='Article.datePublished' content='2014-01-01T00:00:00+00:00'/>1 January 2014
    <!-- Content -->
    <span data-sd='articleBody'>
        Lorem ipsum dolor sit amet...
    </span>
<div>
```
The ```Microdata``` output will be:
```html
<div itemscope itemtype='https://schema.org/Article'>
    <!-- Title -->
    <span itemprop='name'>
        How to Tie a Reef Knot
    </span>
    <!-- Author -->
    <span itemprop='author'>
        Written by
        <span itemscope itemtype='https://schema.org/Person'>
            <span itemprop='name'>John Doe</span>
        </span>
    </span>
    <!-- Date published -->
    <meta itemprop='datePublished' content='2014-01-01T00:00:00+00:00'/>1 January 2014
    <!-- Content -->
    <span itemprop='articleBody'>
        Lorem ipsum dolor sit amet...
    </span>
<div>
```
Currently ```plg_system_microdata``` doesn't support fallbacks or nested displays.


Todos
-----
* Add multiple data-suffix support.
* Add fallbacks support.

License
-------
plg_system_microdata is licensed under the MIT License – see the LICENSE file for details.