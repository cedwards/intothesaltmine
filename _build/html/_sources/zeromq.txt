======
ZeroMQ
======

I'd be remiss in my duties as an author if I did not start a book about
SaltStack by discussing ZeroMQ. This, I think, is the fundamental cornerstone
of what makes SaltStack so powerful. Without ZeroMQ SaltStack would not be
capable of what it does, and many of the performance-specific benefits would be
gone. In order to share with you the big picture vision of what SaltStack is
capable of, we'll first need to talk a little bit about ZeroMQ.

What is ZeroMQ?
---------------

What is ZeroMQ and why should I care? Let's just dive right in. First, ZeroMQ
is a socket-based high speed networking library that is leveraged by SaltStack
to allow for real-time communication between systems. SaltStack leverages
ZeroMQ to perform the high-speed communication between systems, while not
requiring a managed service to be running.

Technically ZeroMQ is a message queue, similar to those that you might be
familiar with. AMQP, rabbitmq, kafka, etc. These are all popular message queue
services that you can set up in your infrastructure. The primary difference
here is that ZeroMQ is not another service to setup and maintain. It is simply
a programming library that application developers can leverage in order to
achieve high speed network communication within their application. SaltStack's
requirement on ZeroMQ requires no effort or configuration on your behalf, but
provides all the benefits of a full-fledged messaging service.

So why ZeroMQ? ZeroMQ was developed initially for high-speed banking
transactions. A large banking institution determined that the network
transactions themselves were too expensive, costing the company millions of
dollars. They specifically set out to design a message queue interface that
would allow for high speed, low cost transactions on the network, saving them
money.

We're all familiar with the word 'latency'. We generally understand that the
lower the latency the better. This was essentially the goal here. Achieve the
lowest latency possible, in the simplest manner possible. The people behind
ZeroMQ initially created the AMQP system. This was designed for JP Morgan
Chase, specific to their network latency needs. AMQP has since been abandoned
by the initial developers (although still maintained by adopters), in
preference of what they suggest is an improved system with ZeroMQ.

ZeroMQ and Salt
---------------

How does Salt leverage ZeroMQ? To be honest it's all pretty transparent to the
end user. You could never learn anything more about ZeroMQ than what is
included here and still use SaltStack in an enterprise setting. While it is a
very critical and important piece, the way that it has been implemented is very
transparent to the user. All you have to worry about is starting the
``salt-master`` and ``salt-minion`` services (which we'll cover later), and the
ZeroMQ based network layer is automatically maintained. I think this fact is
both a credit to the ZeroMQ developers as well as the SaltStack developers.
Such an integral component is as transparent and maintenance-free as it is.

The reason that I wanted to cover ZeroMQ first is because it is a key reason
why SaltStack is different than any of its competitors. It is what sets it
apart from other remote execution and configuration management tools. Many of
these other tools leverage existing communication protocols, such as ``ssh`` or
a traditional serial ``TCP`` socket connection. While these solutions excel in
some areas, they simply weren't designed for the scale at which SaltStack
operates.

Let me see if I can describe this in a simple way.

SaltStack excels at high-speed, asynchronous communication between connected
systems. This communication--this underlying message bus--can be leveraged for
many different uses, only one of which is parallel remote execution. Let's
imagine for a moment how this all fits together.

Imagine your infrastructure the way it stands today. Dozens, hundreds or
thousands of servers tucked away nicely in data centers around the world. These
systems are connected to power and networked via switches and routers. it isn't
terribly complicated if you think about it. You can currently connect to any of
your systems from your management network and do any amount of administration
needed. That administration is generally done over ``ssh``, which is usually a
one-to-one connection. You connect securely to one system, do your maintenance,
and move on to the next. That's the way it's been done for years. That's the
way it's been done for as long as I can remember anyway.

Granted there are some tools that expand this functionality a bit. Tools like
``clusterssh`` allow you to mirror commands to multiple systems at once over
multiple ``ssh`` connections. This works well to an extent, but there is a
point at which this method doesn't scale. Unfortunately there are simply not a
lot of tools that allow you to control large pieces of your infrastructure in a
timely manner.

Now imagine that all of your systems were connected over a high speed,
asynchronous messaging system. Imagine being able to send commands to one
system or a thousand systems using the same method. Imagine being able to
leverage this high speed message bus to query real-time information about all
of your systems, all at once. This is what ZeroMQ provides for us in the
SaltStack world. A high-speed way to connect to not one-at-a-time, not
two-at-a-time but all-at-a-time with little overhead.

Just imagine for a second a high-speed network connecting all of your systems.
A network that is fully encrypted and sits on top of your existing physical
network infrastructure. This network can easily connect hundreds, thousands and
even tens of thousands of systems without issue.This network can provide you
with a low latency method by which you can query for or send information to
your entire network of servers.  I really hope you're paying attention here,
because this is the foundational piece of the entire Salt stack. This is the
piece that allows the rest of the magic to happen. This is the piece that
facilitates the high speed communication between systems, and not just minion
and master. This high speed network can be leveraged for one minion to query or
communicate with another minion. While it is traditionally a pub-sub
communication pattern it can be thought of more broadly in the sense that it
allows near instant communication between your entire infrastructure.

I want to again reiterate that this high speed network requires no additional
services to install and maintain, and little to know knowledge of the
underlying connectivity to use. It is truly high speed, low latency
connectivity at low cost. Low cost to the network and low cost to the
maintainers of the infrastructure. It is all behind the scenes. The magic
behind the curtain.

As you get deeper into this book I want you to keep in mind that everything
rides on top of this high speed encrypted network. Every command you post to
the message queue is instantly available to all of your systems. Every time you
want to update configuration on your servers the instructions are instantly
available everywhere. No more iterating through lists and sending commands over
ssh. No more sitting and waiting for networking protocols that weren't designed
for the scale of todays internet try and keep up. You will be leveraging a
modern, encrypted, asynchronous communication network actually designed just
for this.The Internet is growing. Your company is growing. Grow with it.
