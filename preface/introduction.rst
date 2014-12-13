============
Introduction
============

The digital world as we know it is constantly changing. New technologies, both
in hardware and software, are released daily. The number of connected systems
to manage grows exponentially day after day. The Internet itself is larger than
all of us--it's the biggest thing mankind has ever built. If you think about the
scale at which the technologies we work with are expanding it can be daunting.
The unending amounts of data created, transferred, stored and analyzed is more
than any one of us can keep up with. It's all pretty amazing if you really think
about it.

Imagine for a minute the vastness of it all. The *entire* Internet. All the
systems. Servers old and new. Clouds, public and private, constantly forming,
shifting and expanding. Billions of websites around the world. Overwhelming
really. At that scale there's not one of us that could manage it. Not a hundred
of us or a thousand of us. The only reason it works is because of skilled
admins dutifully maintaining their little sections individually.  That's really the
only way it can be done.. or is it?

Every now and then something changes the game. Some brilliant visionary among us
comes up with an elegant solution to a problem. Something that, for those who
see it, changes everything. These solutions are sometimes software. Sometimes
they are hardware. Sometimes it's just a new way to think about a problem. I'm
sure you can come up with a few examples. A piece of software you discovered
that makes your life so much easier you wonder how you ever got by without it.
Once something has improed your life that much, how could you ever go back?
Once you've seen the potential it's really hard to not be excited.

This is the way I feel about SaltStack. I see a revolutionary tool unlike any
other currently available. I see the collective contributions of brilliant
developers around the world creating solutions to problems nearly as fast as
they arrive. I see a modular, flexible and lightweight alternative to the
legacy systems and methodologies that we've been using for years. What I see
with SaltStack is the future of computing.

History
-------

The history of SaltStack goes back for years. It is the constantly improving
culmination of many attempts at solving a common problem. The author and lead
developer, Thomas S.  Hatch, tried solving this problem many times for many
companies. Each time improving on the last, and each time gaining valuable
insight into the most efficient ways to manage systems. Let me take you back a
few years to when my history with Salt began.

It was 2011 and I had just been hired by Thomas at an online music startup. He
had recruited a few of us that had worked together in the past in what I
thought of as a dream team. If I could work with anyone again, it would be
these guys. We got along great. We worked hard. We solved complex problems that
nobody had solved before. We were breaking new ground and growing fast.

Thomas started working on a tool to manage our private cloud infrastructure.
The idea was, using some home-grown tools, we would be able to spin up machines
on the fly, provision them and drop them into different environments. When we
had updates to systems we would simply spin up a new machine with the latest
code on it and transition it into place, recycling the old machine. The vision
was there. It was, at the time, very exciting. The only problem was that the
tools weren't there. The vision was there, but we didn't yet have the tools to
take us from point A to point B.

Remember, we were a fledgling startup with grand ideas but not a lot of money.
We had to come up with our own solutions and build our own tools. Everything
from scratch, with a small team.

By the time I hired on work had begun on these tools. We had a tool called
"Bacon", another called "Butter" and a third called "Salt". These were
in-house, python-based tools for provisioning and managing our private cloud.
At the time these three were very rudimentary, but for the most part they got
the job done. They had little bugs here and there, but we also had a very
talented development team that squashed those about as quickly as they came up.

I fondly remember times working in what couldn't even be called a conference
room, shared with five other system engineers. I'd run into an issue with one
of our tools and called it out. I'd promptly hear Thomas call back "give me one
minute!", and like clockwork the bug would be fixed. Time and time again,
improvements implemented at the speed only a startup can seem to manage. Day by
day our tools become more mature, our infrastructure became larger, and the
company seemed to be doing great.

Somewhere along the line here Thomas had the foresight to get the company to
allow him to open source these tools. He had developed most of it on his own
time at home in his basement (yes, the old cliche), and primarily fixed bugs
with our feedback while at the office. He placed the code on github and work
continued.. but not for long.

Before we knew it months had passed and it was the Holidays. Like any startup
we had minimal time off during the season. There were constantly things to fix
and systems to build. We were preparing for a big deal with a mobile telecom
company and hoping to expand. All of our hard work was finally paying off! We
were all very excited. We were supposed to be getting a dedicated NOC team, new
hires, data center expansion--all the things we needed to support this upcoming
deal. Unfortunately I don't think any of us really saw what was really coming.

I remember it clearly to this day. One of the perks of working at this startup
was flexible working and office hours. We spent most of our time working from
home and only met as a team in the office once or twice a week. This was a
Wednesday, the week between Christmas and New Years, and I was working from
home in an old beatup chair in my living room. Our lead network engineer and
my roommate at the time was upstairs with the flu, taking the day off.

Then an email arrived. "Staff meeting conference call @5:00pm". I didn't think
much of it, and dialed in at the suggested time. This, depending on how you
look at it, is either where it all ended or where it all began.

Layoffs. Everyone. The investors had pulled out. There would be no severance.
There would be no more work. Report to the office tomorrow to turn in any
equipment you have and good luck to you. I was floored! I remember being so
shocked and sitting up so abruptly in surprise that my laptop tumbled to the
floor. My company laptop that I was expected to return in the morning. What had
just happened? Was he for real? I--we--were all shocked. What were we going to
do? I'd never been laid off before. The rest of that afternoon is a bit of a
blur, but I'll always remember that moment as a crossroads for a lot of us.

As the days and the weeks went on some of us found jobs right away. Others took
a bit longer. For some people it was a great move. They landed even better jobs
with better companies. Some people found temporary consulting work. If you've
ever been part of a massive layoff you know how it goes. It's difficult, but
people usually make it. During this time we all kept in touch pretty well. We'd
share job leads and keep tabs on who was ending up where, and did that company
need anyone else. During all of this there was one person that didn't seem
worried, and didn't seem to be as concerned about finding a new job. Thomas.

Thomas had a plan. Something bigger than where we came from or where a lot of
us ended up. He was going to strike out on his own with a new tool that had
been slowly picking up popularity in the open source community. He had decided
he was going to form SaltStack, the company behind the Salt tool we had begun
with at the startup.

If you're reading this I think you have some idea how that turned out.
SaltStack is now a growing, successful company filled with talented people.
Some of the people from our original dream team now work for Thomas again, this
time at a much more successful company.

To give you an idea of the usefulness of the Salt tool, every one of the
systems engineers from that original startup have deployed Salt for our new
employers. I'm currently architecting a Salt based solution across multiple
products and nearly fourty-thousand servers. Salt is a game changer and I hope
to be able to work with it long into the future.
