+++
subtitle = ""
title = "Big Data Spain 2015"
bigimg = ""
date ="2015-10-18T18:58:49+02:00"

+++

This week I have attended the [Big Data Spain](http://www.bigdataspain.org/) 2015 conference. The format of this conference is:

- 2 tracks with no thematic separation
- Spanned over 2 days
- Talk duration around 45 minutes length, except some *lightning talks* (appropriately named `talkreduce()`) of 15 minutes or so

I would like to share some links and notes on some talks and ideas that I have picked.

<!-- TEASER_END -->

Paco Nathan ([@pacoid](https://twitter.com/pacoid)) did a news recap in his talk about *Data Science in 2016: Moving up* with (literally) tons of links to different sources. I have selected three:

- The [Project Euclid](https://projecteuclid.org/), an impressive platform that I did not know about, for maths and statistics publishers
- The fantastic [PyData Seattle 2015 keynote](https://www.youtube.com/watch?v=2YIZ2SY9mW4) by [Lorena Barba](https://twitter.com/lorenaabarba) titled *Data driven Education and the Quantified Student*
- The adoption of the [Jupyter](https://jupyter.org/) Notebooks as a first-class vehicle of publishing, by O'Reilly's [beta](https://www.oreilly.com/ideas/jupyter-at-oreilly) initiative

He also gave a *Crash introduction to Apache Spark* workshop. Although he did not have time to finish, the repo is [online](https://github.com/cetery/intro-spark), so we can do it self-paced.

[Kartik Paramasivam](https://www.linkedin.com/pub/kartik-paramasivam/11/b07/b71) from Linkedin, showed us some projects that I did not know:

- The [Apache Samza](http://samza.apache.org/) distributed stream processing framework that they are using to handle the incoming data from Kafka
- The embeddable [RocksDB](http://rocksdb.org/) persistent key-value storage. Their [benchmarks](https://github.com/facebook/rocksdb/wiki/Performance-Benchmarks) look impressive

[Matthias Braeger](http://strataconf.com/stratany2014/public/schedule/speaker/178011) spoke about a tool that the CERN has developed for monitoring systems called [c2mon](http://cern.ch/c2mon). Java-written (JMS-based), it is suited for high availability and high data volume.

Antonio Gallego ([@antoniogallego](https://twitter.com/antoniogallego)) from Pivotal, showed a [stock inference engine using Apache Geode and Spark ML](https://github.com/Pivotal-Open-Source-Hub/StockInference-Spark). Quite a bit demo effect, but since the code is in Github, I will try to re-run the demo and test it by myself.

Nicolás Poggi ([@ni_po](https://twitter.com/ni_po)) from the Barcelona Supercomputing Center, explained and demoed [Aloja](https://github.com/Aloja/aloja), a powerful Big Data benchmarking platform. Really impressive piece of software, and probably the greatest discovery in this event, at least for me. Congrats!

Kostas Tzoumas ([@kostas_tzoumas](https://twitter.com/kostas_tzoumas)) from Data Artisans, talked about [Apache Flink](https://flink.apache.org/). It seems that Flink is one of the hottest solutions for both streaming and batch processing, but focusing on streaming. Will have to keep an eye on it.

William Vambenepe ([@vambenepe](https://twitter.com/vambenepe)) from Google, explained [Dataflow](https://cloud.google.com/dataflow/). This is not only the newest paper of Google on Big Data architecture but also an open-source implementation that is already available on Google Cloud Platform. It works with a concept called *watermark* for windowing the reception of out-of-order real-time events and is able to use the same algorithms for processing both real-time and batch. Looks very cool, and cooler for me since he explained that a Python SDK is coming, and that the visualization can be done via Jupyter notebooks uploaded to their [Datalab](https://github.com/GoogleCloudPlatform/datalab) platform. Python rulez!
