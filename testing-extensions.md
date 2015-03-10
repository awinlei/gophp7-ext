---
layout: page
title: Testing extensions
---

# Testing extensions

When you have "adopted" an extension it is likely you are going to want to have a look at the tests that exist for it. Basic information considering these
tests can be found here: http://qa.php.net/

This page is aimed at answering the most likely questions on testing extension you will have. This page considers you succeeded to configure and compile PHP
from source already. Obviously with your "adopted" extension(s) included.

## How do I run tests

The easy way is running "make test". This will test all tests for all extensions (including standard, which has stuff like array_*) that where included
with ./configure before you compiled PHP.

## Can I run tests for a single extension?

Yes. Run "make test TESTS=path/to/extension/*.phpt" for instance. Obviously you can run a single test file by replacing the wild card with the actual filename.

## My test fails and I did not expect this

If a test fails "make test" automatically provides you with a couple of files that can give you information on what went wrong. These files are placed in the
same folder as the test file. The following files are added (considering your test file is named foo.phpt):

 * foo.diff - A diff file between the expected output (be it in EXPECT, EXPECTF or another option) and the actual output.
 * foo.exp - The expected output.
 * foo.log - A log containing expected output, actual output and results. Most likely very similar to info in the other files.
 * foo.out - The actual output of your .phpt test part.
 * foo.php - The php code that was executed for this test.
 * foo.sh - An executable file that executes the test for you as it was executed during failure.
