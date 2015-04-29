title: "Willin: jQuery InstantPjax"
date: 2015-04-29 14:31:33
categories: Dev
tags: [performance,fe,jquery,plugin]
---

## InstantPjax - A jQuery Instant Pjax Plugin

### Not for noob.

Demo: this site.

## Installation

```shell
bower install instantpjax
```

or download here: 

[https://github.com/willin/InstantPjax/releases](https://github.com/willin/InstantPjax/releases)

or use directly: 

[https://rawgit.com/willin/InstantPjax/dist/instantpjax.min.js](https://rawgit.com/willin/InstantPjax/dist/instantpjax.min.js)


## Usage

### 1. Basic

Use: `$.fn.ipjax`

```js
	/*selector,container,config*/
	$(document).ipjax('a','#main',{
		timeout: 2000, //ms
		cache:  3600*24*7, //s, 0 for disable cache
		storage: true, //false for disable localStorage
		delay: 300, //ms, 0 for disable mouse over preloading
		push: true, // true is push, false is replace, null for do nothing
		titleSuffix: '', 
		show: '', // _default/fade or Animation Function(data, callback, isCached)
	});
	$(document).on('ipjax.start',function(){
		//start animation here
		// Example:
		/*
		$('#loading').show();
		*/
	});
	$(document).on('ipjax.end',function(){
		//end animation here
		// Example:
		/*
		$('#loading').hide();
		*/
	});
	$(document).on('ipjax.cached',function(){
		//if cached animation here
		// Example:
		/*
		$($.ipjax.options.element).addClass('cached');
		$($.ipjax.options.element).one('mouseleave',function(){
		  $(this).removeClass('cached');
		});
		*/
	});
```

### 2. Manual Request

See [jQuery.ajax/#jQuery-ajax-settings](http://api.jquery.com/jQuery.ajax/#jQuery-ajax-settings)

```js
	$.ipjax({
		url: '',
		container: '#main',
		timeout: 2000, //ms
        cache:  3600*24*7, //s, 0 for disable cache
        storage: true, //false for disable localStorage
        delay: 300, //ms, 0 for disable mouse over preloading
        push: true, // true is push, false is replace, null for do nothing
        titleSuffix: '', 
        show: '', // _default/fade or Animation Function(data, callback, isCached)
        type:'GET',
        dataType: 'html',
        data:{
            ipjax: true
        }
	});
```



Feel free to star or fork it:

<div class="github-widget" data-repo="willin/InstantPjax"></div>
