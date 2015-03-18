---
title: Compiling and testing extensions
---

If you would like to help but have not compiled PHP, looked at an extension or written a test for PHP this blog aims to help you. It is intended to guide you to writing your first .phpt test step by step. 

So first of all, we need to compile php7. PHP's source code can be obtained from github here <https://github.com/php/php-src>. If you've never used github before take a look at these set of easy to follow articles that help you setup github and get used to the forking/pull request bits of github: <https://help.github.com/categories/bootcamp/>

Clone PHP into a directory you would like have it in and have a look :) Huge amounts of tags and branches are smiling at you right now, but you'll be wanting the master branch for PHP7. Essentially, master IS what will become PHP7 right now. To compile PHP you need a couple of steps:

1. cd /path/to/php7-src.git
2. ./buildconf
3. ./configure
4. make

That is basically all. You've now compiled PHP7 from scratch. You might run into errors along the way. My best advice is to use common sense and google or use irc. Usually though I've found errors to be caused by missing tools or development packages for underlying c-libraries that are used by the PHP extension I am trying to compile along with PHP. If you want to understand more of what just happened, go read: <http://www.phpinternalsbook.com/build_system/building_php.html>

## How about those extensions?

Now you've got yourself a brand new built PHP. You can find the PHP executable for cli in sapi/cli (it is obviously called php and executable :D ). This however is a PHP with nothing but the standard extensiond compiled along. Since we're looking to test extensions what you'd want to do is the following:

1. cd /path/to/php7-src.git
2. ./configure --with-enchant
3. make

This will cause PHP to be compiled with the enchant library included. This is the first extension I ever seriously looked at so it will come up a couple of more times this article. Other extensions have similar --with-extension flags though. After compilation with your extension included you can run the tests for this extension:

1. cd /path/to/php7-src.git
2. make test TESTS=ext/enchant/tests/

Output of these tests will look something along these lines:

![Test output](http://www.freeklijten.nl/l/library/download/g3DuuPhWsr-a-E-a-9nNPejdJs952RSp0O2Z/enchant-tests.png)

## Your first PHP contribution - writing tests

So now that we have a PHP with your favourite extension compiled along lets start contributing! A very good way is writing tests for PHP. PHP has its own way of writing tests for the language. To avoid this post becoming even longer than it already is I will advise you to take a look at a short primer found here: <http://qa.php.net/write-test.php>. After reading that, just have a look at a simple test-case in any standard extension.

Now that you've seen and understand phpt tests, lets find out what needs to be tested. One great (though not necessarily perfect) tool is checking code coverage for your extension. You need lcov and ggcov installed. That is literally the names of the packages on my ubuntu system, so no troubles there. Now generate your code coverage report as follows:

1. ./config.nice --enable-gcov (code coverage needs to be added to the configuration to work. config.nice is a trick that makes sure you do need to remember all the ./configure flags you added earlier. It appends to the already existing configuration)
2. make lcov

This generates a code coverage report in /path/to/php7-src.git/lcov_html/ which will look something like this:

![Red code coverage](http://www.freeklijten.nl/l/library/download/xcBDTl-a-Swk_a_omfxVGFOTLHQuiiDm2kHk/red.png)

Yup, thats a hell of a lot of red. So we've got work to do :)

When you keep on clicking, eventually you will arrive at annotated C files. These files show you which lines are touched how many times. A short example is below:

![A test](http://www.freeklijten.nl/l/library/download/fRkUk9Vqc34nxS-a-bI7RBBEcmEk8MAOiv/simple_function_cc.png)

Now don't get put off by this code if you're not used to C code. A lot of this is standard PHP api functions and macro's. Have a look and you'll see it is surprisingly easy to follow:

1. PHP_FUNCTION is a macro that expands into the correct code to define a new PHP function (this one is for enchant_broker_free_dict)
2. Then there are some variable definitions
3. zend_parse_parameters is a function that does multiple things. It checks types (the 2nd parameter is a mask, this time the r is for resource) and it loads incoming paramters into the local ones. Failing this function to trigger the return false can be achieved by passing in something else then a resouce.
4. PHP_ENCHANT_GET_DICT is another macro that loads the dictionary that is found in the resource and does some error checking around it. If you're wondering into what variable, this macro assumes the pdict variable of type enchant_dict is defined earliers (which it is as you can see in the image).
5. zend_list_close (probably) free's all resources used by the dictionary resource (the function does free a dict resource after all)
6. True is returned (another macro)

Even without prior c-knowledge you can understand what happens here. It is also obvious what line is not tested, although it might be a trivial test to add. You would achieve this by making a test that calls the function with an array or a string, anything but a resource. To make it succeed your FEXPECT section would look like this:

> Warning: enchant_dict_check() expects parameter 1 to be resource, string given in %s
> bool(false)

The %s is a wildcard since the last bit of the error string is dynamic. Now this test will not add an awful lot to PHP's stability but at the very least it will guard us against accidental function signature changes or unwanted BC-breaks.

## Proceeding

Now the above example is very simple, but most of PHP's source code is just as readable and you can generate code coverage for everything. Take a look at uncovered lines and see if you can come up with sensible tests. As you can see in our "red" image there must be a lot of core functionality untested, something you can change! This is obviously the hardest part, but you'll learn about PHP's internals and it is a ton of fun.

## Debugging tests

You will probably run into a new test case you added that fails while you didn't expect it. If a test fails “make test” automatically provides you with a couple of files that can give you information on what went wrong. These files are placed in the same folder as the test file. These files include actual output, a diff between the expected output from the phpt file and the actual output. You can find more information here: <http://gophp7.org/gophp7-ext/testing-extensions.html>

## Adding to this site

If you discover a bit of information that was not previously written down anywhere, this site is on github so you can add to the site by sending in pull-requests at github (branch gh-pages).

You can also help getting more information into this catalog: <https://github.com/gophp7/gophp7-ext/wiki/extensions-catalog> At the very least you can compile on php 7 with specific exceptions compiled along to see if it works on your system. Do what you can, do what you like.
