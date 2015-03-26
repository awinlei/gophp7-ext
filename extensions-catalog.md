---
layout: page
title: Go PHP 7 Extensions
---

# Extensions Catalog

This is a mammoth page on our wiki: <https://github.com/gophp7/gophp7-ext/wiki/extensions-catalog>.

It is divided into sections by extension type:

 * Core
 * pecl.php.net
 * pecl (not php.net)
 * Other extensions

## How you can help

We need to list as many extensions here as we can find information about.  Each extension needs a row in the table with the following (mostly yes/no) information:

 * Name
 * Homepage/location
 * Has maintainers
 * Has tests
 * Has docs
 * Works on PHP5 (newest PHP 5)
 * Works on PHP7
 * Is good on PHP 7
 * Details - link out to a new wiki page and write more about the state of the project and what needs doing before the answers are all "yes"

You can edit our existing template line (the pipes don't need to line up) to create your row entry:

```
| Homepage/location | Has maintainers | Has tests | Has docs | Works on PHP5 | Works on PHP7 | Is good on PHP 7 | Details |
```

Please fill in as much information as you can ... and where there are gaps, guess whose job it is to fill them?

## Finding Extensions

Are you using a PHP extension?  Check it's on our list, and if not, add it!

## Contacting maintainers

Please get in touch with maintainers and check whether they are still active - their contact details should go on the details page for the extension.  If there are no maintainers, or a sole maintainer, see if you can find someone else to get involved with the extension.  If you don't get a response, feel free to fork the extension in order to be able to continue to work on it.

## Has tests

Run the tests.  Bonus points if you can add more tests.

We have more detailed documentation on doing this on the [testing extensions page](testing-extensions.html)

## Has documentation

Where is the documentation?  Some resources for you if you need to write or update the docs for an extension

 * Take a look at <http://doc.php.net/> to learn how to write php.net documentation
 * Take a look at <http://readthedocs.org> to learn how to create offsite documentation

## Works on PHP5

Try <http://www.lornajane.net/posts/2014/compiling-php-extensions> as a starting resource (which should probably be forked to a page on this site so we can improve and update it)

## Works on PHP7

Make it build on PHP7 (while writing down how it's done) - this is probably the hardest part, the part for people who want to learn extensions or know C to work on, - this includes brain dumps of best practices, things to watch out for,  how to migrate will help later folks get up to speed

There is a [guide for upgrading extensions](https://wiki.php.net/phpng-upgrading) on the php.net wiki and also a page about the [implementation of PHP7](https://wiki.php.net/phpng-int) that may be useful for reference.

## Is good on PHP

Make it run RIGHT on PHP7 - this is more than just C code.... if they don't have tests, add tests, if they don't have docs, write docs, if their apis were designed 10 years ago and use resources and hurt you to touch them - redesign the apis, set up travis and appveyor(windows ci) so that they are continously tested, fix bugs that have been outstanding, add new features - keep the extensions alive and well

## Details

Additional page with more information, any outanding issues etc.
