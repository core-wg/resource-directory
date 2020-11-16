:css: style.css

.. title:: resource-directory 2020-10-22 interim

------

:data-y: start+0

``anchor=`` necessary all the time?
-----------------------------------

* Comment from Esko `#281`_
* Almost double the representation size in many examples
* "Many interpretations and implementations of RFC6690"…

..

* But it's really late

..

* … but the one that was most likely wrong is Christian's
* … and that's the one that kept anchor= in there

.. _`#281`: (https://github.com/core-wg/resource-directory/issues/281)

------

:data-y: start+1000

Server authorization
--------------------

* Mailing list thread_

* I'd need a statement about what OAuth does here (MCR?)

.. _thread: https://mailarchive.ietf.org/arch/msg/core/JyW0XAkXre1wvKoNxMwegOUCywc

------

:data-y: start+2000


Echo, ETag
----------

… or any other means to ensure replays and reshufflings don't kill registrations

PR `#291`_

Another option: Etag. I think it's even better, but can it recover?

.. _`#291`: https://github.com/core-wg/resource-directory/pull/291

------

:data-y: start+3000

The rest should "just" need doing
---------------------------------

although `#258`_ could still need a pair of eyes

.. _`#258`: https://github.com/core-wg/resource-directory/pull/258
