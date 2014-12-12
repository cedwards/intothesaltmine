=======
Outline
=======

I imagine there are going to be topics included in this book that are new to
you. Even if you've used Salt before there are likely components that you
haven't used or heard of. I have tried to include everything in this book, but
the difficulty there as with many actively developed software tools, is that
there are additions and fixes being committed even as I write this paragraph.
To that end I have designed this book around the 2014.7.x release. I will
include links and resources that you can follow to see what new features have
been added since publication. Even so, there are a lot of Salt components that
are not commonly published. It's not that they aren't useful, it's that most
blog posts on the Internet stick to the skin-deep, easy to replicate topics.
Basic remote execution and simple examples of configuration management.

In this book I want to cover more than that. I want to cover Salt from the
bottom up. I want to show you the real power of Salt by breaking it up into its
logical components and show how one builds on top of another. I want to
demonstrate how modular Salt really is and how it can be easily extended to
meet even the most complex use cases. I hope by describing Salt in this way
that you'll be able to catch the big picture vision.

I plan to outline Salt in the following sections:

Command & Control
-----------------

 - ZeroMQ
 - Key Management
 - targeting

   - grains
   - pillar

 - remote execution

   - modules
   - returners

 - configuration management

   - renderers
   - states

Events & Reactions
------------------

 - Event Bus
 - Reactor

Job Management
--------------

 - jid
 - job management

Web Interface & API
-------------------

 - Halite
 - salt-api

Scheduling Tasks
----------------

 - scheduler

Provisioning
------------

 - salt-bootstrap
 - salt-cloud
 - salt-virt

Salt at Scale
-------------

 - performance tuning
 - high availability (multi-master)
 - syndication masters

Configuration Management for Networking
---------------------------------------

 - proxy minions

Development
-----------

 - writing modules
 - writing states
 - writing returners
 - writing renderers
 - writing grains

Configuration
-------------

 - master config
 - minion config
