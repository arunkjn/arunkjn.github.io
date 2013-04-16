---
layout: post
title: "Modular Javascript with require.js"
description: ""
category: 
tags: [js,AMD,requirejs]
---

I was going through [an excellent post](http://addyosmani.com/writing-modular-js/) by [Addy Osmani](http://twitter.com/addyosmani) recently and felt an urgent need to write a blog post of my own. This is my first blog post. I feel that writing about the things that you use will make you understand it better.

The web is changing at a fast pace. Javascript has grown to be the most popular language in the world in the last couple of years (specially last year). A few years back, the only good use that developers put to javascript was making fancy menus, dropdowns, popovers, text highlight, etc. Basically revolving around DOM manipulation. With the increase in the use of javascript in webapps over the years, and now with full blown complete *client side* javascript applications, plus complete server side javascript implementations pushed the world to rethink about javascript. People suddenly realized the importance and power of javascript. There was an urgent need for many missing features that other high level languages provide.

The new ES5 specification of Javascript addresses most of these issues, but it still very new. Browser vendors are working hard to implement these standards in the browsers. Most of them are already implemented.

One of the big issues that developers face while writing javascript is the missing out of the box module system. When we talk about modular code, we mean that it composed of a set of highly decoupled, distinct pieces of functionality stored in modules. The current iteration of Javascript(ECMA-262) does not provides a way to write modular javacript in a clean, organized manner. Developers use variations of object literal patterns to write modular code. There is also no clean way to handle dependencies while create a *module*, without any third party tools.

These concerns have led to the development of a few standard ways of writing modular javascript code today. Yes! you can start with these right now.

Most notable of them are -

* [AMD](https://github.com/amdjs/amdjs-api/wiki/AMD) (Asynchronous Module Definition)
* [CommonJS](http://www.commonjs.org/)
* [UMD](https://github.com/umdjs/umd) (Universal Module Definition)
* [ES5 Harmony](http://wiki.ecmascript.org/doku.php?id=harmony:harmony) (a proposed draft native solution in javascript)

We are going to touch upon AMD today. (Disclaimer- And no, it has got no relation whatsoever with the CPU/GPU manufacturer of the same name). AMD and commonJS both use script loaders to *asynchronously* load scripts(modules). The most popular script loader implementing AMD is [requireJS](http://requirejs.org/docs/whyamd.html) by [James Burke](https://twitter.com/jrburke). The project is very flexible and has got an amazing documentation (which is more or less the success factor of any open source project today). It is even used in large enterprise applications by giants like IBM.

Writing a module in AMD is as simple as *defining* it in a *define* statement.

	define(
	    module_id /*optional*/, 
	    [dependencies] /*optional*/, 
	    definition function /*function for instantiating the module or object*/
	);

Here *module_id* is an optional field and ommiting it makes the module *anonymous*.


Here is a mock code  for making a module in AMD.

	// A module_id (myModule) is used here for demonstration purposes only
	 
	define('myModule', 
	    ['foo', 'bar'], 
	    // module definition function
	    // dependencies (foo and bar) are mapped to function parameters
	    function ( foo, bar ) {
	        // return a value that defines the module export
	        // (i.e the functionality we want to expose for consumption)
	    
	        // create your module here
	        var myModule = {
	            doStuff:function(){
	                console.log('Yay! Stuff');
	            }
	        }
	 
	        return myModule;
	});

We have talked about creating modules by *defining* them. Now comes the part where you would probably want to use these modules somewhere.

This is done by a simple *require* statement.

	require(['app/myModule'], 
	    function( myModule ){
	        // start the main module which in-turn
	        // loads other modules
	        var module = new myModule();
	        module.doStuff();
	});