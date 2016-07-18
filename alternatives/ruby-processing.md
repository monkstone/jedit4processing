---
layout: post
title:  "ruby-processing"
date:   2015-12-11 21:12:48 +0000
categories: jekyll update
---
### Installation

Install `jEdit` and the `console` plugin at a minimum.

Either clone this repro or use a zipped file, copy the files in the `.jedit` folder to wherever your `.jedit` folder is typically `~/.jedit` and edit them suit your system see below for ruby-processing installed on ArchLinux.

### The macro file rp5.bsh

This file lives in `~/.jedit/macros/` folder use this so you you can `run`, `watch` from the `macros` menu 

{% highlight java %}

/**
* rp5.bsh by monkstone 24 October 2015 
* A jedit bean shell macro, to load environment, and call
* rp5 commando menu
*
* You must edit GEM_HOME/GEM_PATH to match your system
* if you use setenv (but usually you wont need to set it)
* example below for ArchLinux with user 'tux'
* Using Oracle jdk installed under /opt
*/

// setenv("JAVA_HOME", "/opt/jdk1.8.0_66");
// setenv("JRUBY_HOME", "/opt/jruby");
// setenv("GEM_HOME", "/home/tux/.gem/ruby/2.2.0");
// setenv("GEM_PATH", "/home/tux/.gem/ruby/2.2.0");
// setenv("PATH", "/usr/bin");
new console.commando.CommandoDialog(view,"commando.rp5");

{% endhighlight %}

### The commando file rp5.xml

This file lives in `~/.jedit/console/commando/` you should tune this to match your OS setup

{% highlight xml %}
<?xml version="1.0"?>
<!DOCTYPE COMMANDO SYSTEM "commando.dtd">
<!-- Monkstone, 2015-October-25 for ruby-processing -->
<COMMANDO>
<UI>
<CAPTION LABEL="Run">
<FILE_ENTRY LABEL="ruby file" VARNAME="file" EVAL="buffer.getName()"/>
</CAPTION>
<CAPTION LABEL="Path to rp5">
<ENTRY LABEL="path" VARNAME="rp5path" DEFAULT=""/>
</CAPTION>
<CAPTION LABEL="Choose Run/Watch/App">
<CHOICE LABEL="Select" VARNAME="type" DEFAULT="run" >
<OPTION  LABEL="run" VALUE="run"/>
<OPTION LABEL="watch" VALUE="watch"/>
<OPTION LABEL="app" VALUE="app"/>
</CHOICE>
</CAPTION>
<CAPTION LABEL="JRuby Opt">
<TOGGLE LABEL="jruby-complete" VARNAME="jruby" DEFAULT="FALSE"/>
</CAPTION>
</UI>

<COMMANDS>
<COMMAND SHELL="System" CONFIRM="FALSE">
<!-- cd to working dir -->

	  buf = new StringBuilder("cd ");
	  buf.append(MiscUtilities.getParentOfPath(buffer.getPath()));
	  buf.toString();
	
</COMMAND>
<COMMAND SHELL="System" CONFIRM="FALSE">

	  buf = new StringBuilder(rp5path);
	  buf.append("rp5 ");
	  if (jruby){
	      buf.append("--nojruby ");
	  }	 
	  buf.append(type);
	  buf.append(" "); 
	  switch(type){
	  case "run":
	  case "watch":
	  case "app":
	      buf.append(file);
	      break;

	  }
	  buf.toString();
	
</COMMAND>
</COMMANDS>
</COMMANDO>

{% endhighlight %}

### keyboard shortcuts

The macro menu is accessible with `Alt c`.  However you should create a direct shortcut to the macro say `Alt 5` see how in `Utilities/Global Options` menu.

### The rp5 popup menu

Use this menu to run/watch your sketch (the current buffer), you only need to enter the path to the rp5 executable once (this, the choice of run/watch, and the checkbox setting are 'remembered' for you). The filename of the current buffer is displayed for your convenience and need not be entered. You can use the `Commands` tab to see what commands get sent (eg checked jruby-complete, inserts `--nojruby` flag). You can also check JRubyArt version and configuration using version _remember to change back to run or watch though_.

<img src="{{ site.github.url }}/assets/rp5.png" />
