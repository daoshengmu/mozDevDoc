> This is an edited extract from https://codesugar.wordpress.com/2014/12/13/building-the-mozilla-central

## Mozilla-Central
Mozilla-central is the source code of the well-known Firefox-Nightly, which is one version of the firefox browser built for testing the new features. Mozilla-central is actually a Mercurial repository of the Mozilla code: https://hg.mozilla.org/mozilla-central . You can read more information <a name="moz-cen" title="mozilla-central" target="_blank" href="https://developer.mozilla.org/en-US/docs/mozilla-central">here</a>.


## About this article
This is a short note of [Simple Firefox build](#Fxbuild) to setup and run the mozilla-central. With this article, you can build this project faster than doing it by yourself after reading the [whole guide](#Fxbuild).


<!--more-->

## My environment
Ubuntu 14.04 64-bit


## Build prerequisites
- Recommended: 4GB of RAM (having only 2GB RAM and 2GB swap may give memory errors during compile)
- High speed internet

## Go to your working directory
You may want to organize all your projects in one directory, so go there first
<pre>
$ cd path/of/working/directory
</pre>

## One-Line Bootstrapping
<pre>
# This command include Mercurial installation already
$ wget -q https://hg.mozilla.org/mozilla-central/raw-file/default/python/mozboot/bin/bootstrap.py &amp;&amp; python bootstrap.py
</pre>


## Getting Source
<pre>
$ hg clone https://hg.mozilla.org/mozilla-central moz_cen
# if you already have mozilla-central code
$ cd moz_cen
$ ./mach bootstrap
</pre>


## Build Confiduration
<pre>
# create a configuration file
$ touch mozconfig
</pre>


## Building
<pre>
$ ./mach build
</pre>


## Running
<pre>
$ ./mach run
</pre>


## Updating the hg environment
<pre>
$ ./mach mercurial-setup
</pre>


## Reference
<a name="Fxbuild" title="Simple Firefox build" target="_blank" href="https://developer.mozilla.org/en-US/docs/Simple_Firefox_build">Simple Firefox build</a>


## Manuscript
<a title="Google Doc" target="_blank" href="https://docs.google.com/a/mozilla.com/document/d/1yNv92RLNajj39v94nc6agU9jhrqLjHlCzus-TCoDLfc/edit?usp=sharing">click here</a>
