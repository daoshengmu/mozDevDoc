## Prerequisite

Before implementing XPCOM components from this tutorial, 
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


## Playing the existing XPCOM sample

There is an existing sample code for XPCOM component 
whose interface is **nsISample**.
The code is in **MOZ_CEN/xpcom/sample**.

<pre>
$ vim MOZ_CEN/xpcom/sample/nsSample.h
</pre>

```cpp
#include "nsISample.h"
#include "mozilla/Attributes.h"

/**
 * SampleImpl is an implementation of the nsISample interface.  In XPCOM,
 * there can be more than one implementation of an given interface.  Class
 * IDs (CIDs) uniquely identify a particular implementation of an interface.
 * Interface IDs (IIDs) uniquely identify an interface.
 *
 * The CID is also a unique number that looks just like an IID
 * and uniquely identifies an implementation
 * {7CB5B7A0-07D7-11d3-BDE2-000064657374}
 */

#define NS_SAMPLE_CID \
{ 0x7cb5b7a0, 0x7d7, 0x11d3, { 0xbd, 0xe2, 0x0, 0x0, 0x64, 0x65, 0x73, 0x74 } }

#define NS_SAMPLE_CONTRACTID "@mozilla.org/sample;1"


class nsSampleImpl MOZ_FINAL : public nsISample
{
public:
  nsSampleImpl();
...
...
}

...
...

```


- Run the **nsISample** XPCOM component

  - If you never built mozilla-central before
<pre>
$ ./mach build or ./mach build/ xpcom/
</pre>

  - Play it in **xpcshell**
<pre>
$ cd obj-YOUR-TARGET-XXXX/dist/bin
$ ./run-mozilla.sh ./xpcshell
</pre>
<pre>
js> const cSample = new Components.Constructor("@mozilla.org/sample;1", "nsISample");
js> var sample = new cSample(); 
js> sample.writeValue("Hello")
Hello initial value
foopy 5
GetValue 8
js> sample.value
initial value
js> sample.poke("nice to see you")
js> sample.value
nice to see you
js> sample.writeValue("yo!man!")
yo!man! nice to see you
foopy 5
GetValue 8
js> sample.value
nice to see you
js> sample.poke("high five");
js> sample.value
high five
js> sample.writeValue("yo!man!");
yo!man! high five
foopy 5
GetValue 8
js> quit()
</pre>
  

## Creating your own XPCOM component


## Reference
<a name="XPCOM" title="Creating XPCOM components" target="_blank" href="https://developer.mozilla.org/en-US/docs/Mozilla/Tech/XPCOM/Guide/Creating_components">[1] Creating XPCOM components</a>

<a name="" title="" target="_blank" href="">[2] abc</a>

<a name="" title="" target="_blank" href="">[3] abc</a>


## Manuscript
<a title="Google Doc" target="_blank" href="https://docs.google.com/a/mozilla.com/document/d/1rlAnNbVYUnI6PMWjn4eTB_As5tPfYObco9lDNkf8N6E/edit?usp=sharing">click here</a>
