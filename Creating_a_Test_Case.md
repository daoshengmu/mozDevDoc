## Mochitest
Mochitest is an automated testing framework build on top of the Mochikit JavaScript libraries. It is one of the automated regression testing frameworks used by Mozilla.

-https://developer.mozilla.org/en-US/docs/Mochitest

## About this article
This is an article about how to create a test case and use mochitest. The more detailed information is mentioned in [Mochitest](#mochi)

<!--more-->

## Writing the test 
- Where to put the test <br>
Add your test file to the closest test folder you can find e.g, dom/alarm/test. Only create the new directory when you are creating the new feature.
The following article demostrates crating test in new directory.
\-b is the bug number and \-type decide the template type. The name of test file must start with "test_".

###Crate test template

<pre>
$ mkdir -p MOZ_CEN/dom/helloTest/test
$ perl MOZ_CEN/testing/mochitest/gen_template.pl -b=123456 -type=html > MOZ_CEN/dom/helloTest/test/test_bug123456.html
</pre>



###Add test content

Each test must contain some JavaScript that will run and tell Mochitest whether the test has passed or failed. [SimpleTest.js](#SimpleTest)provides a number of functions for the test to use that communicate the pass/fail to Mochitest. These include:


- ok(expressionThatShouldBeTrue, "Description of the check") -- tests a value for truthiness
- is(actualValue, expectedValue, "Description of the check") -- compares two values (using ==, not ===)
- isnot(actualValue, unexpectedValue, "Description of the check") -- opposite of is()
<br>
<pre>
$ vim MOZ_CEN/dom/helloTest/test/test_bug123456.html
</pre>

```html
...
...
  <script type="application/javascript">


  ok(true, "test passed");

  </script>

...
...
```

###Create mochitest.ini

<pre>
$ cd MOZ_CEN/dom/helloTest/test
$ vim mochitest.ini
</pre>

<pre>
[test_bug123456.html]
</pre>

###Add to build tree
<pre>
$ vim MOZ_CEN/dom/helloTest/moz.build
</pre>

<pre>
MOCHITEST_MANIFESTS += [
            'test/mochitest.ini',
]
</pre>

<pre>
$ vim MOZ_CEN/dom/moz.build
</pre>

<pre>
DIRS += [
    'animation',
    'apps',
...
...
    'manifest',
    'vr',
    'helloTest',
]
...
...
</pre>


##Running specific tests

<pre>
$ cd MOZ_CEN
$ ./mach mochitest-plain dom/helloTest/test/test_bug123456.html
</pre>

## Reference
<a name="mochi" title="Mochitest" target="_blank" href="https://developer.mozilla.org/en-US/docs/Mochitest">[1] Mochitest</a>
<br>
<a name="SimpleTest" title="SimpleTest.js" target="_blank" href="http://mxr.mozilla.org/mozilla-central/source/testing/mochitest/tests/SimpleTest/SimpleTest.js">[2] SimpleTest.js</a>
