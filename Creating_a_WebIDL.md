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
/* -*- Mode: C++; tab-width: 2; indent-tabs-mode: nil; c-basic-offset: 2 -*- */
/* vim:set ts=2 sw=2 sts=2 et cindent: */
/* This Source Code Form is subject to the terms of the Mozilla Public
 * License, v. 2.0. If a copy of the MPL was not distributed with this
 * file, You can obtain one at http://mozilla.org/MPL/2.0/. */

#ifndef mozilla_dom_Hello_h
#define mozilla_dom_Hello_h

#include "mozilla/Attributes.h"
#include "mozilla/ErrorResult.h"
#include "nsCycleCollectionParticipant.h"
#include "nsWrapperCache.h"

#include "nsCOMPtr.h" // For already_AddRefed
#include "mozilla/dom/BindingDeclarations.h" // For GlobalObject

struct JSContext;

namespace mozilla {
namespace dom {

class Hello MOZ_FINAL : public nsISupports /* Change nativeOwnership in the binding configuration if you don't want this */,
                        public nsWrapperCache /* Change wrapperCache in the binding configuration if you don't want this */
{
public:
  NS_DECL_CYCLE_COLLECTING_ISUPPORTS
  NS_DECL_CYCLE_COLLECTION_SCRIPT_HOLDER_CLASS(Hello)

public:
  //Hello();
  Hello(nsISupports* aParent);
  Hello(nsISupports* aParent, const nsAString& aStr);

private:
  nsCOMPtr<nsISupports> mParent;
  nsString hStr;

protected:
  ~Hello();

public:
  // TODO: return something sensible here, and change the return type
  //Hello* GetParentObject() const;
  nsISupports* GetParentObject() const { return mParent; }

  virtual JSObject* WrapObject(JSContext* aCx) MOZ_OVERRIDE;

  static already_AddRefed<Hello> Constructor(const GlobalObject& global, ErrorResult& aRv);
  static already_AddRefed<Hello> Constructor(const GlobalObject& global, const nsAString& str, ErrorResult& aRv);

  void GetHelloStr(nsString& aRetVal) const;

  void SetHelloStr(const nsAString& arg);

  void GetReadonlyStr(nsString& aRetVal) const;

  void SayHello(nsString& aRetVal);

  int32_t RandNum();

  int32_t Add(int32_t a, int32_t b);

  int32_t Mul(int32_t a, int32_t b);

  int32_t Sub(int32_t a, int32_t b);

  double Div(int32_t a, int32_t b, ErrorResult& aRv);
};

} // namespace dom
} // namespace mozilla

#endif // mozilla_dom_Hello_h
```


<pre>
$ vim Hello.cpp
</pre>

```cpp
/* -*- Mode: C++; tab-width: 2; indent-tabs-mode: nil; c-basic-offset: 2 -*- */
/* vim:set ts=2 sw=2 sts=2 et cindent: */
/* This Source Code Form is subject to the terms of the Mozilla Public
 * License, v. 2.0. If a copy of the MPL was not distributed with this
 * file, You can obtain one at http://mozilla.org/MPL/2.0/. */

#include "mozilla/dom/Hello.h"
#include "mozilla/dom/HelloBinding.h"

#include <cstdlib> // For rand() and srand()
#include <ctime> // For time()

namespace mozilla {
namespace dom {


NS_IMPL_CYCLE_COLLECTION_WRAPPERCACHE_0(Hello)
NS_IMPL_CYCLE_COLLECTING_ADDREF(Hello)
NS_IMPL_CYCLE_COLLECTING_RELEASE(Hello)
NS_INTERFACE_MAP_BEGIN_CYCLE_COLLECTION(Hello)
  NS_WRAPPERCACHE_INTERFACE_MAP_ENTRY
  NS_INTERFACE_MAP_ENTRY(nsISupports)
NS_INTERFACE_MAP_END

//Hello::Hello()
Hello::Hello(nsISupports* aParent):mParent(aParent)
{
}

Hello::Hello(nsISupports* aParent, const nsAString& aStr):mParent(aParent), hStr(aStr)
{
}

Hello::~Hello()
{
}

void
Hello::GetHelloStr(nsString& aRetVal) const
{
  aRetVal = hStr;
}

void
Hello::SetHelloStr(const nsAString& arg)
{
  hStr.Assign(arg);
}

void
Hello::GetReadonlyStr(nsString& aRetVal) const
{
  aRetVal = nsString(NS_LITERAL_STRING("This string can be read only!"));
}

void
Hello::SayHello(nsString& aRetVal)
{
  aRetVal = nsString(NS_LITERAL_STRING("Hello FireFox!"));
}

int32_t
Hello::RandNum()
{
  //std::srand(std::time(0));//Use current time as seed for random generator
  static bool init = false;
  if(!init){
    std::srand(std::time(0));//Use current time as seed for random generator
    init = true;
  }
  return std::rand()%100;
}

int32_t
Hello::Add(int32_t a, int32_t b)
{
  return a + b;
}

int32_t
Hello::Sub(int32_t a, int32_t b)
{
  return a - b;
}

int32_t
Hello::Mul(int32_t a, int32_t b)
{
  return a * b;
}

double
Hello::Div(int32_t a, int32_t b, ErrorResult& aRv)
{
  if(!b){
    //if b is zero, then throws an error
    aRv.Throw(NS_ERROR_ILLEGAL_VALUE);
    // See what kinds of error type you can throw here:
    // https://developer.mozilla.org/en-US/docs/Mozilla/Errors
  }
  return (double)a / b;
}

JSObject*
Hello::WrapObject(JSContext* aCx)
{
  return HelloBinding::Wrap(aCx, this);
}

/* static */ already_AddRefed<Hello>
Hello::Constructor(const GlobalObject& global, ErrorResult& aRv)
{
  nsRefPtr<Hello> obj = new Hello(global.GetAsSupports());
  return obj.forget();
}

/* static */ already_AddRefed<Hello>
Hello::Constructor(const GlobalObject& global, const nsAString& str, ErrorResult& aRv)
{
  nsRefPtr<Hello> obj = new Hello(global.GetAsSupports(), str);
  return obj.forget();
}


} // namespace dom
} // namespace mozilla
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

### Make it work
<pre>
$ cd MOZ_CEN
$ ./mach build
</pre>

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
"test page is starting..."
"DOM has been loaded..."
"click trigger"
"sayHello : Hello FireFox!"
"helloStr : "
"helloStr : null"
"helloStr : Be modified by web"
"readonlyStr : This string can be read only!"
"readonlyStr : This string can be read only!"
"48 + 36 = 84"
"48 - 36 = 12"
"48 * 36 = 1728"
"48 / 36 = 1.3333333333333333"
Exception { message: "", result: 2147942487, name: "NS_ERROR_ILLEGAL_VALUE", filename: "file:///home/chunmin/Work/moz_cen/testHello.html", lineNumber: 44, columnNumber: 0, inner: null, data: null, stack: "clickHandler@file:///home/chunmin/Work/moz_cen/testHello.html:44:52
" }
"Can NOT divide by zero!"
"Finish testing"
</pre>


## Reference

<a title="WebIDLBindings" target="_blank" href="https://developer.mozilla.org/en-US/docs/Mozilla/WebIDL_bindings">[1] WebIDL bindings</a>

<a title="GeckoWebIDL" target="_blank" href="https://github.com/mozilla/gecko-dev/blob/master/dom/bindings/docs/index.rst">[2] Gecko WebIDL</a>

<a title="W3CWebIDL" target="_blank" href="http://www.w3.org/TR/WebIDL/">[3] W3C Web IDL</a>


## Manuscript

<a title="Google Doc" target="_blank" href="https://docs.google.com/a/mozilla.com/document/d/1iM89Tmsh7PmohOq5R4-ZtHMKVAw1qPHc_sC4aZ1XU6E/edit?usp=sharing">click here</a>
