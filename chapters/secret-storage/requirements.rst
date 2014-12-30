Requirements
============

Let me start off by listing some of the requirements that were outlined by some
of our admin and engineering teams in this solution. I like to think that S4
covers all of the bases here, and handles them efficiently.

 - password storage
 - ssh-key storage
 - token storage
 - user/host based access control
 - efficiency / caching

We basically needed something that would not only securely store user
credentials, but a solution that could pass off credentials to applications
that needed access to other applications. For example, a web server needing
access to a database, without the need to hardcode passwords in applications or
config files. The idea being when the application needs a password it can query
the secret storage for it, cache it, and use it as needed until expiration.

I was working from home one day when I had the thought that I could start
creating something like this. I remember sitting in my la-z-boy thinking about
how we could solve the issue of hard-coding passwords in config files. This got
me thinking about how Salt can securely store data in its pillar
infrastructure. The idea of pillar in combination with the GPG renderer came
together in my mind and BAM I was on to something! I immediately started on a
proof of concept.

The proof would be a user needing access to a mysql server with the following
requirements:

 - The user doesn't know the mysql password
 - The user is trusted to connect to the mysql server
 - The host is trusted to connect to the mysql server
 - The password is never stored in plain text

In its current state, S4, solves all of these problems and meets all of the
requirements. With only a dozen or so lines of code I was able to write a
script that would take a secret or token as an argument and query S4 for the
contents. Assuming the user had access to the script (root or sudo privileges),
and the machine was within the appropriate group(s) to query for those secrets,
the token would be fetched, decrypted and passed through to mysql.

The user never learns the passphrase, it is never stored in plain text, and
access is limited per user per machine. This amount of granularity would prove
to be important, but as you can see, this was all built using out of the box
open source solutions.

Required Packages
-----------------

In order to build this secret storage solution you'll need:

 - Salt Master
 - Salt Minion(s)
 - ``python-gnupg`` installed
 - Public and private GPG keys
 - GPG Agent

For a really high level overview here's how each of these components fit
together.

The Salt Master will be the central storage server. All secrets will be stored
within the pillar data on the master (usually ``/srv/pillar/``). Each secret is
stored on the master in a GPG encrypted cipher. The public and private GPG keys
are only ever stored on the master. GPG keys never need to be shared to
minions. The ``python-gnupg`` package needs to be installed on any minion
wanting to request secure keys. Lastly, assuming you'll want to secure your
encrypted secrets with a GPG key passphrase, you'll need a configured GPG agent
to unlock these secrets upon request, without the requirement of entering a
passphrase each time.
