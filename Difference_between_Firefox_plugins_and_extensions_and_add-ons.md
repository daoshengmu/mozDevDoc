Many people confuse about the term plugins, extensions and add-ons in the browser world. They sound like they should be the same thing, but in fact they are different.
<!--more-->

## Difference between Firefox add-ons,  extensions and plugins

- Plugins
    - plugins are **binary libraries**
    - plugins are **compiled**, **loadable modules**, originally descended from **NPAPI**
    - live **outside of the browser's process space**, which leads to all kinds of fun interoperability issues and vulerabilities
    - Plugins are the **links to external programs** that handle files that Firefox doesn't, like audio and video such as Flash or Java, or document platforms such as PDF files.
    - invoked if a website uses an ```<embed>``` or ```<object>``` tag with a type that is handled by the plugin
    - the **.so(shared object)** files in ```/usr/lib/mozilla/plugins``` (e.g. libflashplayer.so)
    - that's largely **independent** of the browser.
    - one plugin is compatible with **many** web browsers like IE, Firefox, and Opera.
- Extensions
    - extensions are written mostly in javaScript for dynamic actions and XUL, CSS for UI
    - recently, the Add-on SDK has been added as an alternative way to generate simple extensions, it uses HTML instead of XUL
    - since the extensions act as **part of the browser**, they have wider access privileges than JS-in-a-webpage
    - they have **full access to XPCOM** and can provide their own XPCOM components as well
    - one extension works with a **certain** browser
- Add-ons
    - Add-ons are essentially anything that can be installed into the browser
    - including extensions, plugins, themes, dictionaries, language packs, search engines.

## Reference
- <a href="http://www.differencebetween.net/technology/difference-between-add-on-and-plug-in/" target="_blank">Difference Between Add-on and Plug-in</a>
- <a href="http://colonelpanic.net/2010/08/browser-plugins-vs-extensions-the-difference/" target="_blank">Browser Plugins vs Extensions – the difference</a>
- <a href="http://stackoverflow.com/questions/2041261/what-is-the-difference-between-a-firefox-extension-and-a-firefox-add-on" target="_blank">What is the difference between a Firefox extension and a Firefox add-on?</a>
- <a href="http://stackoverflow.com/questions/4120106/what-is-the-difference-between-firefox-extension-and-plugin" target="_blank">What is The difference between Firefox extension and plugin?</a>
- <a href="http://stackoverflow.com/questions/7575658/firefox-add-on-vs-extensions-vs-plugins" target="_blank">firefox add-on vs. extensions vs. plugins</a>


