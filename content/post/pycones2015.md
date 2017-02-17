+++
subtitle = ""
title = "PyConES 2105"
bigimg = ""
date ="2015-12-10T23:28:23+01:00"

+++

Last month, I attended the 2015 edition of the [PyConES](http://2015.es.pycon.org), probably the most important national Python event of the year. The talks spanned the entire weekend and there were also workshops (as the one from [Django Girls](https://djangogirls.org/pycones2015/)) during the previous Friday.

<!-- TEASER_END -->

The conference started with the keynote for Yamila Moreno ([@yamila_moreno](https://twitter.com/yamila_moreno)) entitled [Python y Plutarco, el poder de una historia](http://yamila-moreno.github.io/Python-y-Plutarco-el-poder-de-una-historia/). A talk about diversity in the Python community that I recommend you to watch (hopefully the video will be online), because it is good food for thought.

I attended to a great talk of José Ignacio Galarza ([@igalarzab](https://twitter.com/igalarzab)) about [How to scale a web page](https://speakerdeck.com/igalarzab/how-to-scale-a-web-page), based upon his experience at Ticketea. A talk full packed of good advice such as:

- Use extensive caching (even nginx caching, at the risk of eventual consistency among an autoscaling group of instances)
- Finish the rendering of your site using javascript
- Move everything not essential to background jobs, especially I/O bound tasks
- Take a closer look on your database: check your slowlog, optimise your slow o recurring queries. Manage carefully your transaction mode and your isolation level
- Use faster key-value counters (such as the ones from Redis) whenever possible, instead of a database
- Move your bottlenecks to more appropiate tools, always having a plan B
- Send your selects to read replicas

At the end of the day, the lightning talks were a blast of humorous, provocative and mind-shattering condensed talks of less than 10 minutes each. For example:

- [Programación Python para Zombies](http://es.slideshare.net/reingart/programacion-python-para-zombis-charla-relampago), a talk from Mariano Reingart ([@reingart](https://twitter.com/reingart)) about a [MOOC](https://en.wikipedia.org/wiki/Massive_open_online_course) for teaching Python
- *Software y Ciencia* by Victor Terrón ([@pyctor](https://twitter.com/pyctor)) about fails using Python in scientific environments (really funny!)

On Sunday, another meaty talk from Miguel Araujo ([@maraujop](https://twitter.com/maraujop)) and Jose Ignacio Galarza, now about *AsyncIO, pónganse a la cola, por favor*, explaining how they used the Python 3.X library to create a VirtualLine implementation for their ticketing engine.

Alejandro Brito ([@ae_bm](https://twitter.com/ae_bm)) gave a talk entitled [Funcional para trollear](http://es.slideshare.net/ae_bm/funcional-para-trollear), covering the main functional programming libraries available in Python, in a hilarious talk.

Raúl Cumplido ([@raulcumplido](https://twitter.com/raulcumplido)) spoke about *Metaprogramación en Python*. Very good talk, although he only managed to cover the basics of decorators and metaclasses since the time was really limited.

David Barragán ([@bameda](https://twitter.com/bameda)) presented [Hacking the Taiga](http://bameda.github.io/slides/2015/pycones_2015/hacking_the_taiga). A comprehensive talk about extending the Taiga agile project management system in many different ways (I really liked the [curses client](http://bameda.github.io/slides/2015/pycones_2015/hacking_the_taiga/#/7/4)!)

Pau Freixes ([@pfreixes](https://twitter.com/pfreixes)) and Arnau Orriols ([@arnau_orriols](https://twitter.com/arnau_orriols)) spoke about [AMQP from Python, advanced design patterns](https://github.com/pfreixes/python-amqp-pycones/blob/master/slides.rst). Very interesting comparative of performance in very stressing environments of Pika, Celery librabbitmq and other libraries that implement AMQP.

Finally, Victor Terrón closed the conference with his keynote [Dijsktra es mi pastor: nada me falta](https://github.com/vterron/PyConES-2015). Algorithms, [Big O Notation](https://en.wikipedia.org/wiki/Big_O_notation), common sense and Pythonic code, with lots of humour.
