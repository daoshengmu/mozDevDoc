## Message Manager
Message managers provide a way for chrome-privileged JavaScript code 
to communicate across process boundaries. 
They are particularly useful for allowing chrome code, 
including the browser's own code and extension code, 
to access web content when the browser is running web content in a separate process.


## About this article
This is the implementation note of 
the message manager part of [[1]](#e10sOverview) and [[2]](#mm). 

## Prerequisite

Before operating the message manager, 
you need to get the firefox source code, 
which is known as **mozilla-central**, from Mercurial or Git.
The path of mozilla-central is denoted by **MOZ_CEN** from here.

- MDN : https://developer.mozilla.org/en-US/docs/Mozilla/Developer_guide/Source_Code/Mercurial
- Mercurial Menual : https://developer.mozilla.org/en-US/docs/Mozilla/Developer_guide/Source_Code/Mercurial
- Mercurial : https://hg.mozilla.org/mozilla-central/
- Git : https://github.com/mozilla/mozilla-central


## Terminology

- MOZ_CEN: The path of mozilla-central project


## Concept

<a target="_blank" href="https://docs.google.com/a/mozilla.com/presentation/d/1Lu3_1yvYN1dFGiHM6VVVpTPA9LXfH7ZgSGJfH07rHXA/edit?usp=sharing">Multiprocess Firefox(e10s)<a/>


## Try message manager in mochitest

The file used here is MOZ_CEN/dom/events/test/**bug989198_helper.js**.
It already use message manager to do some tests, 
so we can inject our code into it.

<pre>
$ cd MOZ_CEN/dom/events/test
$ vim bug989198_helper.js
</pre>


### Injecting your test code in frameScript()

```javascript
...
...
function frameScript()
{
  function handler(e) {
    var results = sendSyncMessage("forwardevent", { type: e.type });
    if (results[0]) {
      e.preventDefault();
    }
  
    var res = sendSyncMessage("DearDad", {say: "Good night"});
    dump( "[child] get: " + ((res[0])? res[0]:"nothing!"));
    dump('\n-------------------------------\n');
  }

  
  addEventListener('keydown', handler);
  ...
  ...
}
...
...
```

### Add event listener

```javascript
...
...
function prepareTest(useRemote)
{
...
...
    mm.loadFrameScript("data:,(" + frameScript.toString() + ")();", false);


    mm.addMessageListener("DearDad", function(m){
      dump('\n-------------------------------\n');
      //dump(JSON.stringify(m));
      dump("[Dad] Hi! Child! You say: " + m.data.say);
      /* mm.sendAsyncMessage('DearChild', {say:"good morning"}); */
      dump('\n-------------------------------\n');
      return "Good Child";
    });

    runTests();
...
...
}
...
...
```

<pre>
$ cd MOZ_CEN

# the bug989198_helper.js is included in 
# dom/events/test/test_dom_before_after_keyboard_event.html
$ ./mach mochitest-plain dom/events/test/test_dom_before_after_keyboard_event.html
…
…
-------------------------------
[Dad] Hi! Child! You say: Good night
-------------------------------
[child] get: Good Child
-------------------------------
…
…
</pre>


## Try message manager in FxOS(B2G)

If you're a FxOS developer, you can try message manager in it.
We plan to inject our code by following steps:

1.  Find the chrome code of FxOS and inject your code there
  
  1.1  Search **ppmm**(parent process message manager) on <a target="_blank" href="http://dxr.mozilla.org/mozilla-central/search?q=ppmm&case=false">dxr</a>
    we choose <a target="_blank" href="http://dxr.mozilla.org/mozilla-central/source/b2g/chrome/content/shell.js">b2g/chrome/content/shell.js</a> here
  
  1.2  Look <a target="_blank" href="">what kind of events</a> used in ppmm.addMessageListener

2.  Find the related content code and inject your code there




## Reference
<a name="e10sOverview" title="e10s overview" target="_blank" href="https://developer.mozilla.org/en-US/Firefox/Multiprocess_Firefox/Technical_overview">[1] e10s Technical Overview</a>

<a name="mm" title="The message manager" target="_blank" href="https://developer.mozilla.org/en-US/Firefox/Multiprocess_Firefox/The_message_manager">[2] The message manager</a>


## Manuscript
<a title="Google Doc" target="_blank" href="https://docs.google.com/a/mozilla.com/document/d/1yZ3X5XVzZws_14BYVj58tTKTnDMl8ZXY5z-vtTiP1_4/edit?usp=sharing">click here</a>
