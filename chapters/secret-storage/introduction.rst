Introduction
============

Let me start off this chapter by giving some background on why I've included it
and why I felt it was important to build. I'm currently preparing to present
this functionality to an internal team of administrators and engineers toward
solving a problem we've had for some time.

In 2014 there were some big changes surrounding my team and our
responsibilities at work. Since our inception, my team has been responsible for
architecting and maintaining enterprise-grade network services. Things like
DNS, SMTP, NTP, FTP--things that every company needs and uses every day. We
were good at this, and our successess led to more responsibilities. One of
these new responsibilities was the addition of a new project that would span
across more than a dozen teams, affect every product within our business unit,
and keep some of us busy for some time to come. That project was Identity
Management (IdM).

It all seems like ages ago now, but I held meeting after meeting with more than
a dozen teams to find out requirements they had in regards to IdM and related
issues. One issue that kept coming up (and something that each team seemed to
want sooner than later) was a secret storage solution. Something they could use
to securely store passwords, ssh keys, and other secrets. As it turns out,
there aren't a lot of solutions available in the market designed specifically
for this. There was one that we looked at, but it left a bad taste in my mouth.
I wanted to find something better.

One day, after spending time with yet another vendor, I found myself killing
some time and reading the release notes for the latest version of SaltStack. I
noticed the listing of a new renderer and the wheels in my head started
turning. It was then that I felt like I could design, using out of the box
components of SaltStack, a secret storage solution that would be more flexible,
better integrated, and faster than the proprietary solutions we had been
considering.

This chapter outlines the solution that I came up with.

