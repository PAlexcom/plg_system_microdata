Microdata
=========
A Joomla ```3.2+``` system plugin that uses the [JMicrodata](https://github.com/joomla/joomla-cms/tree/master/libraries/joomla/microdata "JMicrodata") library.  
Created during the Google Summer of Code 2014.

If you want to keep your views separated from the logic, ```plg_system_microdata``` is system plugin for parsing the HTML markup and convert the ```data-*``` HTML5 attributes in Microdata semantics.  

The ```data-*``` attributes are new in HTML5, they gives us the ability to embed custom data attributes on all HTML elements. So if you disable the library output, the HTML will still be validated. The default suffix the library will search for is ```data-sd```, but you can register more than one custom suffix.  

Installation
============
[Download the last version of the plugin](https://github.com/PAlexcom/plg_system_microdata/archive/master.zip "Download plg_system_microdata") and install it. __Don't forget to publish this plugin!__

Usage
=====
Since the system plugin runs right before the final content is send to the client, you can use the plugin syntax everywhere, including native and third-party extensions that allows html editing.
### Building blocks Syntax
##### setType
![ParserPlugin Syntax](https://palexcom.github.io/PHPStructuredData/images/parser-plugin-syntax-v1.3.0-setType.png)  
The _type_ must have the first character Uppercase. If the type is valid, the global scope is updated to the new one. Finally the params will be replaced with ```itemscope itemtype='https://schema.org/Type'```.  
  
##### global fallback proeprty
![ParserPlugin Syntax](https://palexcom.github.io/PHPStructuredData/images/parser-plugin-syntax-v1.3.0-global.png)  
The _property_ must have the first character lowercase. If the property is part of the current global scope, the params will be replaced with ```itemprop='property'```.  

##### specialized fallback proeprty
![ParserPlugin Syntax](https://palexcom.github.io/PHPStructuredData/images/parser-plugin-syntax-v1.3.0-specialized.png)  
A combination between both _Type_ and _property_, separated by a dot. In short, if the current global scope is equal to Type and the property is part of the Type, the params will be replaced ```itemprop='property'```.  
  
### Syntax
![ParserPlugin Syntax](https://palexcom.github.io/PHPStructuredData/images/parser-plugin-syntax-v1.3.0.png)  
A combination between the previous 3 building blocks. The order of the building blocks isn't significant and a white space is used as a separator.  
##### The Algorithm:
1. First the parser checks for __setTypes__. If one or more matches are found then the current global scope will be update with the first match. At this point if there are no specialized or global fallback properties the algorithm will finish and replace the params with the current scope. Otherwise continue to point 2.  
2. The parser checks for __specialized fallback properties__. If one or more valid matches are found, then the algorithm will finish and replace the params with the first match property. Otherwise go to point 3
3. The parser checks for __global fallback properties__. If one or more valid matches are found, then replace the params with the first match property and finish the algorithm.

Publisher
=========
![plg_system_structureddata content editor usage](https://palexcom.github.io/plg_system_structureddata/images/plg_system_structureddata-editor.png)

Developer
=========
Let's suppose that somewhere in your code you need to add Microdata semantics to the following HTML which is part of an article (_e.g._ ```$scope='Article';```).
```html
<div data-sd="<?php echo $scope;?>">
    <!-- Title -->
    <span data-sd="Review.itemReviewed name">
        How to Tie a Reef Knot
    </span>
    <!-- Author -->
    <span>
        Written by
        <span data-sd="author.Person">
            <span data-sd="name">John Doe</span>
        </span>
    </span>
    <!-- Date published -->
    <meta data-sd='<?php echo $scope;?> datePublished' content='2014-01-01T00:00:00+00:00'/>1 January 2014
    <!-- Content -->
    <span data-sd='reviewBody articleBody'>
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
    <span>
        Written by
        <span itemprop='author' itemscope itemtype='https://schema.org/Person'>
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
Instead, if you decide to change the current Type (_e.g._ ```$scope="Review";```).  
The ```Microdata``` output will be:
```html
<div itemscope itemtype='https://schema.org/Review'>
    <!-- Title -->
    <span itemprop='itemReviewed'>
        How to Tie a Reef Knot
    </span>
    <!-- Author -->
    <span>
        Written by
        <span itemprop='author' itemscope itemtype='https://schema.org/Person'>
            <span itemprop='name'>John Doe</span>
        </span>
    </span>
    <!-- Date published -->
    <meta itemprop='datePublished' content='2014-01-01T00:00:00+00:00'/>1 January 2014
    <!-- Content -->
    <span itemprop='reviewBody'>
        Lorem ipsum dolor sit amet...
    </span>
<div>
```

License
-------
plg_system_microdata is licensed under the GNU GPL v2 License – see the LICENSE file for details.