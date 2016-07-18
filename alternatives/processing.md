---
layout: post
title:  processing
date:   2015-12-15 14:22:31 +0000
categories: jekyll update
---
### Installation

Install `jEdit` and the `console` plugin at a minimum.

Either clone this repro or use a zipped file, copy the files in the `.jedit` folder to wherever your `.jedit` folder is typically `~/.jedit` and edit them suit your system see below for processing installed on ArchLinux.

### The macro file processing.bsh

This file lives in `~/.jedit/macros/` folder use this so you you can `run`, `watch` from the `macros` menu 

{% highlight java %}
// This macro displays the processing command gui
new console.commando.CommandoDialog(view,"commando.processing");
{% endhighlight %}

### The commando file processing.xml

This file lives in `~/.jedit/console/commando/` you should tune this to match your OS setup

{% highlight xml %}

<?xml version="1.0"?>
<!DOCTYPE COMMANDO SYSTEM "commando.dtd"><!-- processing commando -->
<!-- Monkstone, 2015-Dec-15 -->
<COMMANDO>
  <UI>
    <CAPTION LABEL="Run Processing">

      <FILE_ENTRY LABEL="File Name" VARNAME="file" EVAL="buffer.getName()"/>
      <DIR_ENTRY LABEL="Running Directory" VARNAME="pde" EVAL="buffer.getDirectory()"/>

    </CAPTION>
    <CAPTION LABEL="Path to processing-java">
      <ENTRY LABEL="(no spaces)" VARNAME="processing"/>
    </CAPTION>
    <CAPTION LABEL="Output folder">
      <ENTRY LABEL="(no spaces)" VARNAME="output"/>
      <TOGGLE LABEL="Overwrite" VARNAME="force" />
    </CAPTION>
    <CHOICE LABEL="Command" VARNAME="cmd" DEFAULT=" --run">
      <OPTION LABEL="Run" VALUE=" --run"/>
      <OPTION LABEL="Present" VALUE=" --present"/>
      <OPTION LABEL="Compile" VALUE=" --build"/>
      <OPTION LABEL="Export" VALUE=" --export"/>
    </CHOICE>
    <CAPTION LABEL="Export Options">
      <CHOICE LABEL="Platform" VARNAME="platform" DEFAULT="windows">
        <OPTION LABEL="Windows" VALUE="windows"/>
        <OPTION LABEL="MacOSX" VALUE="macosx"/>
        <OPTION LABEL="linux" VALUE="linux"/>
      </CHOICE>
      <TOGGLE LABEL="64 bit" VARNAME="bit" />
    </CAPTION>
  </UI>
  <COMMANDS>
    <COMMAND SHELL="System" CONFIRM="FALSE">
      buf = new StringBuilder(100);	
      if (file.endsWith(".pde")){
        buf = new StringBuilder(100);
        buf.append(processing);
        buf.append(" --sketch=").append(pde);
        <!-- Be careful specifying your output/tmp folder with force option -->
        if(force){
          buf.append(" --force --output=").append(output);	
        }
        else{
          buf.append(" --output=").append(output);
        }
        <!-- begin export -->
        if (cmd.equals(" --export")){
          buf.append(" --platform=").append(platform);
          if (bit){
            buf.append(" --bits=64");
          }
          else{
            buf.append(" --bits=32");
          }	
          
        }
        <!-- end export -->	    
      }
      else{
        throw new RuntimeException("Save the sketch first in a folder, bearing the same name!!!!");
      } 
      buf.append(cmd);
      buf.toString();
    </COMMAND>
  </COMMANDS>
</COMMANDO>

{% endhighlight %}

### keyboard shortcuts

The macro menu is accessible with `Alt c`.  However you should create a direct shortcut to the macro say `Alt p` see how in `Utilities/Global Options` menu.

### The rp5 popup menu

Use this menu to run/watch your sketch (the current buffer), you only need to enter the full path to the processing-java executable once (this, the choice of run/watch, and the checkbox setting are 'remembered' for you). The filename of the current buffer is displayed for your convenience and need not be entered. You can use the `Commands` tab to see what commands get sent (eg checked jruby-complete, inserts `--nojruby` flag). You can also check JRubyArt version and configuration using version _remember to change back to run or watch though_.

<img src="{{ site.github.url }}/assets/processing.png" />

### Syntax highlighting for processing

Put this file in the modes directory, and remember to update `catalog`, also be sure not to mix up the two processing.xml files.

{% highlight xml %}
<?xml version="1.0"?>
...
<!DOCTYPE MODE SYSTEM "xmode.dtd">
<MODE>
...ellipsed to save space
</MODE>

{% endhighlight %}

The catalog file entry

{% highlight xml %}

<?xml version="1.0"?>
<!DOCTYPE MODES SYSTEM "catalog.dtd">

<MODES>
<!-- Add lines like the following, one for each edit mode you add: -->
<!-- <MODE NAME="foo" FILE="foo.xml" FILE_NAME_GLOB="*.foo" /> -->
<MODE NAME="processing" FILE="processing.xml" FILE_NAME_GLOB="*.pde" /> 
</MODES>

{% endhighlight %}

