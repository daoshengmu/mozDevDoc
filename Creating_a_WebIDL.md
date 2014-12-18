## WebIDL



## About this article
This is the implementation note of [WebIDL bindings](#WebIDLBindings). 

<!-- More -->

## Prerequisite

Before implementing your own WebIDL, 
you need to get the firefox source code, 
which is known as **mozilla-central**, from Mercurial or Git.
The path of mozilla-central is denoted by **MOZ_CEN** from here.

- MDN : https://developer.mozilla.org/en-US/docs/Mozilla/Developer_guide/Source_Code/Mercurial
- Mercurial Menual : https://developer.mozilla.org/en-US/docs/Mozilla/Developer_guide/Source_Code/Mercurial
- Mercurial : https://hg.mozilla.org/mozilla-central/
- Git : https://github.com/mozilla/mozilla-central



## Terminology

- MOZ_CEN


## Concept



## Creating your own WebIDL

### Defining your own WebIDL file
<pre>
$ cd MOZ_CEN/dom/webidl
$ vim Hello.webidl
</pre>

```
[Constructor,Constructor(DOMString str)]
interface Hello {
    attribute DOMString helloStr;
    readonly attribute DOMString readonlyStr;
    DOMString sayHello();
    long randNum();
    long add(long a, long b);
    long mul(long a, long b);
    long sub(long a, long b);
    [Throws]
    double div(long a, long b);
};
```


### Exposing your WebIDL file to the build system

<pre>
$ vim moz.build
</pre>

```
..
..
WEBIDL_FILES = [
…
…
    'GetUserMediaRequest.webidl',
    'Headers.webidl',
    'Hello.webidl',
    'History.webidl',
    'HTMLAllCollection.webidl',
…
…
]
..
..
```

After writing your own WebIDL, you should re-build the backend
<pre>
# Build backend : Generate a backend used to build the tree.
$ cd MOZ_CEN
$ ./mach build-backend
</pre>



### Generating the template files(.h, .cpp) from your WebIDL

<pre>
$ ./mach webidl-example Hello
</pre>



### Completing the implematation from template

The template code will be named **Hello-example.{.h,.cpp}**
in **MOZ_CEN/obj-xxxxx/dom/bindings/**


### Adding information about your WebIDL to firefox build system


## Testing your WebIDL from web page

### Writing a html file for testing

### Getting the results


## Reference

<a title="WebIDLBindings" target="_blank" href="https://developer.mozilla.org/en-US/docs/Mozilla/WebIDL_bindings">[1] WebIDL bindings</a>

<a title="GeckoWebIDL" target="_blank" href="https://github.com/mozilla/gecko-dev/blob/master/dom/bindings/docs/index.rst">[2] Gecko WebIDL</a>

<a title="W3CWebIDL" target="_blank" href="http://www.w3.org/TR/WebIDL/">[3] W3C Web IDL</a>


## Manuscript

<a title="Google Doc" target="_blank" href="https://docs.google.com/a/mozilla.com/document/d/1iM89Tmsh7PmohOq5R4-ZtHMKVAw1qPHc_sC4aZ1XU6E/edit?usp=sharing">click here</a>
