+++
subtitle = ""
title = "PyData 2016 Madrid"
bigimg = ""
date ="2016-04-10T22:45:37+02:00"

+++

Recently I have attended to the [PyData 2016](http://pydata.org/madrid2016/) conference held in Madrid. It has been a two-days single-tracked conference covering aspects of Python and *data science*. I will try to share some links and comments of that have been more relevant to me.

<!-- TEASER_END -->

Christine Doig ([@ch_doing](https://twitter.com/ch_doig)) opened the conference with a keynote entitled [The Hitchhiker's Guide to Data Science](https://speakerdeck.com/chdoig/the-hitchhickers-guide-to-data-science). Christine works at [Continuum](https://www.continuum.io/), one of the companies leading the *data science* revolution in the Python ecosystem. In her talk, she introduced the new products and services that Continuum is delivering. Many interesting things, such as:

- [Math Kernel Library optimizations](https://docs.continuum.io/mkl-optimizations/index) for supporting vectorized math routines hardware-accelerated by the Intel Math Kernel Library, now shipped in the [Anaconda](https://www.continuum.io/downloads) distribution

- [Anaconda navigator](https://docs.continuum.io/anaconda/navigator), a GUI for managing the packages, environments and channels

- [Conda-forge](https://conda-forge.github.io/): A community-curated collection of conda recipes and distributions

- Support for [R](https://www.r-project.org/about.html) language as a first-class citizen in Anaconda and [Jupyter](https://www.continuum.io/blog/developer/jupyter-and-conda-r)

- A [Jupyter Notebook](http://jupyter.org/) extension named [nbpresent](https://github.com/Anaconda-Server/nbpresent) for creating slideshows based on your notebooks

- A [Bokeh](http://bokeh.pydata.org/en/latest/) server to create beautiful visualizations

- Support for [Numba](http://numba.pydata.org/) for enabling JIT compilation of code and [Dask](http://dask.pydata.org/en/latest/) for distributed computing using the NumPy and Pandas interface

Juan Luis Cano ([@astrojuanlu](https://twitter.com/astrojuanlu)) talked about [packaging software with Conda](https://github.com/AeroPython/embrace-conda-packages/blob/master/Embrace%20conda%20packages.ipynb). Very interesting talk. I have really liked the idea of using Continuous Integration in data science, which I think that is not (unfortunately) a mainstream practice.

Manuel Garrido ([@manugarri](https://twitter.com/manugarri)) explained [A primer on recommendation systems](http://blog.manugarri.com/a-short-introduction-to-recommendation-systems/). The talk is great if you want to get hands-on with a simple yet functional first approximation to a recommendation system, and to grasp the concepts behind them.

[Tomás Gómez Álvarez-Arenas](https://www.linkedin.com/in/tom%C3%A1s-g%C3%B3mez-alvarez-arenas-55730b15) spoke about [inverse problems](https://en.wikipedia.org/wiki/Inverse_problem) and how to solve them using Python. Maybe you didn't know that you can reproduce the results of the [LIGO](https://en.wikipedia.org/wiki/LIGO) findings using a Jupyter Notebook and the [data](https://losc.ligo.org/s/events/GW150914/GW150914_tutorial.html) publicly released. Whoa!

[Jaime Fernández](https://www.linkedin.com/in/jaimefrio) gave a comprehensive talk on *The future of NumPy indexing*. Fancy indexing, orthogonal indexing, it all makes sense when Jaime explain them, including the three indexers (orthogonal, vectorized and legacy) that will eventually be incorporated to NumPy codebase.

Claudia Guirao ([@claudiaguirao](https://twitter.com/claudiaguirao)) spoke about [Whoosh: a fast pure-Python search engine library](https://github.com/intiveda/Conference-Info/blob/master/talks_materials/20160410_1215_Whoosh_a_fast_pure_Python_search_engine_library/whooshNotebook.ipynb). This talk shows that it is important to find the best tool for the job, and sometimes a simple search engine can be a better fit than a commercial-grade but complex one.

The team behind [AeroPython](https://twitter.com/aeropython) presented [Remove before flight: Analysing flight safety data with Python](https://github.com/AeroPython/remove-before-flight). Really cool Pandas magic to do *data massaging* with aerial accidents data.

In the section of the lightning talks:

- Gabi de Maeztu ([@gabimaeztu](https://twitter.com/gabimaeztu)) taught how to make [themes](https://github.com/merqurio/jupyter_themes) for Jupyter notebooks
- Juan Luis Cano explained [How to go to Mars using Python (with poliastro)](http://nbviewer.jupyter.org/github/Juanlu001/Lightning-talk-poliastro/blob/master/Going%20to%20Mars%20with%20Python%20in%205%20minutes.ipynb)
- Lluis Esquerda talked about his project [PyBikes](https://github.com/eskerda/pybikes)
- Javier Martin ([@jmartinter](https://twitter.com/jmartinter)) from CartoDB, gave a talk on how to [Create your geo-dashboard in 5 minutes](https://github.com/jmartinter/geo-dashboard)
- Biel Frontera ([@bielfrontera](https://twitter.com/bielfrontera)) presented [Folium](https://github.com/python-visualization/folium), an amazing library to interact with [Leaflet.js](http://leafletjs.com/) in order to create beautiful map overlays and its plugin Time Dimension to create time-based animations
- Yamila Moreno ([@yamila_moreno](https://twitter.com/yamila_moreno)) gave a talk about *Docker for Data Science 101* with an example of Jupyter, Pandas and PostgreSQL integration

Thanks for reading!
