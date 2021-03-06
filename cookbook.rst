Cookbook
========

Recipes below are just suggestions, on how you can organize your workflow.when
using SimVer. Remember that you can always adapt this scheme and recipes to fit
best to your current needs.

Note
----

* Please remember, that this numbering scheme doesn't allow magic strings like
  ``alpha``, ``beta`` or ``rc`` in version number. If you think you really need
  them, you should probably use SemVer or different scheme. If you think you
  can replace it by using proper branch suffixes - well, that is possible, but
  for sake of `Automated updates`_ it is not recommended.

* Remember, that even if development is usually distributed, assigment of new
  version numbers must be centralized. If given branch was assigned to one
  developer, there is no problem, but if whole group is releasing product in
  gien branch, there must be one person responsible for give of new number.
  Otherwise there may occur problem, when two persons would give the same number
  to different revisions.

* If you are releasing only stable (and optionally unstable), and you don't
  release any development versions (or team with assigned task doesn't release
  any dev versions to public), then probably you won't even have any need for
  version branches.

* Trailing zero chunks (like here ``2.0.0``) are meaningless, so ``2.0.0 == 2
  == 2.0``.  Everytime I write something like "release version ``1``" you can
  also release it as ``1.0`` or ``1.0.0``.

Basic usage
-----------

First usable version should be ``0.1``, and later releases (not yet stable)
should be like ``0.1.1``, ``0.1.2`` and so on (or if there is a need
``0.1.1.3`` etc.). When there is a moment, that you want to release alpha, beta
and release candidate, just don't add any unnecessary suffixes like 'alpha' or
'beta', but just release next versions like ``0.1.5``, or ``0.1.5.1`` (for
knowing why there are no suffixes like 'alpha', 'beta' and so on see `Automated
updates`_).

When you have release candidate, that is accepted, with, for example version
number like ``0.1.5.3`` just release version ``1`` which points to the same
revision.

After that, anytime you release new version you need just add ``1`` to
non-first chunk, so you can release versions ``1.1``, ``1.2``, ``1.3`` and so
on. For smaller fixes you can increment third chunk (strategy from SemVer on
major, minor and patch would fit here very well).

Assume that current version is ``1``. Time comes when you need to release
development version to public, but you don't want anybody to assume that this
is version they should have. In that situation it's good to use suffixes,
and since next milestone is ``1.1`` you can release your version as
``1.0.1-dev``. Note that suffix ``-dev`` is just convention, you can name it
whatever you want, or have multiple development branches for different features
you want to release to get review or feedback on them before you merge them
into stable branch. If you want to release another version on that branch, for
example after fixing some bug, you can release ``1.0.2-dev`` or ``1.0.1.1-dev``
- in fact it is just your decision what fits better for you. After you are done
and new features are to be merged into stable branch, just do it and release
version ``1.1``. Remember that you can of course merge it later, into release
like ``1.4`` or further.

What is very important, first release that breaks backward compatibility must
be released under different series. If it is unstable release (since you want
to have some time for development) it should have number ``0.2``. Now you and
every user know it is unstable release and that he shouldn't use it yet. From
here on, the rest of the process is the same: releasint first stable version
from series ``2``, releasing dev versions like ``2.0.3-something``, releasing
next stable versions and at last releasing first version under next series.

Advanced usage
--------------

This recipe extends `Basic usage`_

When you have many developers, and all of them need to release development
and/or unstable versions everything you need to do is to use more branches.

For example when you have three developers, you must assign to them branch
names, under which they will release their versions, these may be like
``dev1``, ``dev2`` and ``dev3``. For convenience you can always assign them
branches like ``dev-steve`` to know who owns that branch, and make them work on
CVS branches (like in Git) with the same name (for simplicity).

When you have version ``0.4.3`` and each of developers will start work on new
features then versions they would release will be like ``0.4.3.1-dev1``,
``0.4.3.1-dev2``, ``0.4.3.1-dev3``. You can later merge those features and
release as new version.

Automated updates
-----------------

With this scheme it is easy to make automated updates of your server. Of course
you won't find here implementation in any language or for certain package
manager, just a general idea.  Since version number, besides branch suffix,
consist only of integers and dots, all you need is to set up updater, that will
install/fetch new version from given branch every time version with higher
precedence occur.

It comes handy when you have few development servers, which allows you to test
some features each from different branch. These servers can be even dedicated
to single developers. Each time developer releases version, which he find is
good enough, he releases some version in given series and branch. Server
dedicated to that certain series and branch now knows that new version was
released and can determine by version number if its newer release. If so, it
just updates it.

For example we could have server that install new package every time its
version number is from series ``1`` and from branch ``dev-steve`` (so matching
version number would be ``1.3.0.3-dev-steve``).

You can also have script for only stable releases (only for versions ``X.*``),
or only for unstable (``0.X.*``), but then you must remember that from releases
from series ``X`` takes both forms.
