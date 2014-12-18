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

The template code will be named **Hello-example.{h,cpp}**
in **MOZ_CEN/obj-xxxxx/dom/bindings/**.
After generating, you should now complete the implementation of this WebIDL.

<pre>
# move files outputed to “MOZ_CEN/dom/hello”
$ cd dom/
$ mkdir hello
$ cd hello
$ mv ../../obj-xxxxx/dom/bindings/Hello-example.* .

# Rename
$ mv Hello-example.h Hello.h
$ mv Hello-example.cpp Hello.cpp
</pre>

<pre>
$ vim Hello.h
</pre>

```cpp
```


<pre>
$ vim Hello.cpp
</pre>

```cpp
```


### Adding information about your WebIDL to firefox build system

<pre>
# add the moz.build in folder hello
$ vim moz.build
</pre>

```
EXPORTS.mozilla.dom += [
    'Hello.h',
]
UNIFIED_SOURCES += [
    'Hello.cpp',
]

FAIL_ON_WARNINGS = True
FINAL_LIBRARY = 'xul'
```

<pre>
# add folder “hello” to dom/moz.build
$ cd ..
$ vim moz.build
</pre>

```
..
..
DIRS += [
  ..
  ..
  'geolocation',
  'hello',
  'html',
   ..
   ..
]
..
..
```


## Testing your WebIDL from web page

### Writing a html file for testing

<pre>
$ cd MOZ_CEN
$ vim testHello.html
</pre>

```html
<!doctype html>

<html lang="en">
<head>
    <meta charset="utf-8">
    <title>hello test</title>
    <script>
        funcInit = function(){
            // After DOM has been loaded....
            console.log("DOM has been loaded...");
            var btnElement = document.querySelector('#btn');
            btn.addEventListener('click', clickHandler);

        }
        document.addEventListener('DOMContentLoaded', funcInit);

        console.log("test page is starting...");

        var clickHandler = function(){
            console.log("click trigger");
            var h = new Hello();
            //var h = new Hello("Init in JS");

            console.log("sayHello : " + h.sayHello());

            console.log("helloStr : " + h.helloStr);
            h.helloStr = null;
            console.log("helloStr : " + h.helloStr);
            h.helloStr = "Be modified by web";
            console.log("helloStr : " + h.helloStr);

            console.log("readonlyStr : " + h.readonlyStr);
            h.readonlyStr = "Also be modified by web";
            console.log("readonlyStr : " + h.readonlyStr);

            var x = h.randNum();
            var y = h.randNum();
            console.log(x + " + " + y + " = " + h.add(x,y));
            console.log(x + " - " + y + " = " + h.sub(x,y));
            console.log(x + " * " + y + " = " + h.mul(x,y));
            try{
                console.log(x + " / " + y + " = " + h.div(x,y));
                y = 0;
                console.log(x + " / " + y + " = " + h.div(x,y));
            }catch(err){
                console.log(err);
                if(err.name == "NS_ERROR_ILLEGAL_VALUE"){
                    console.log("Can NOT divide by zero!");
                }
            }finally{
                console.log("Finish testing");
            }
        }

    </script>
</head>

<body>
    <H1>Test Page</H1>
    <button id="btn">click me</button>
</body>
</html>
```

### Getting the results
<pre>
$ ./mach run testHello.html
</pre>

After the firefox nightly start running, 
you need to open the **browser console**.
You can press **F12** or mouse rightclick->Inspect Element 
and select **console** to open it.

<pre>
</pre>


## Reference

<a title="WebIDLBindings" target="_blank" href="https://developer.mozilla.org/en-US/docs/Mozilla/WebIDL_bindings">[1] WebIDL bindings</a>

<a title="GeckoWebIDL" target="_blank" href="https://github.com/mozilla/gecko-dev/blob/master/dom/bindings/docs/index.rst">[2] Gecko WebIDL</a>

<a title="W3CWebIDL" target="_blank" href="http://www.w3.org/TR/WebIDL/">[3] W3C Web IDL</a>


## Manuscript

<a title="Google Doc" target="_blank" href="https://docs.google.com/a/mozilla.com/document/d/1iM89Tmsh7PmohOq5R4-ZtHMKVAw1qPHc_sC4aZ1XU6E/edit?usp=sharing">click here</a>
