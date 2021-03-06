# Working Efficiently with Big Data in Text Formats Using Free Software

One of our first commercial software products at Continuum Analytics was a product called *IOPro* which we have sold continuously since 2009. Now we are releasing the code under a liberal open source license. 

Following the path of widely adopted projects like conda, Blaze, Dask, odo, Numba, Conda, Bokeh, datashader, DataShape, DyND and other software that Continuum has created, we hope that the code in IOPro becomes valuable to open source communities and data scientists.  

We don't only hope this code is useful to you, however, we also hope you—or your colleagues—will be able to enhance, to refine and to develop the code further to increase its utility for the entire Python world.

## What IOPro does

IOPro loads NumPy arrays and Pandas DataFrames directly from files, SQL databases and NoSQL stores–including ones with millions (or billions) of rows. It provides a drop-in replacement for NumPy data loading functions but dramatically improves performance and starkly reduces memory overhead.

The key concept in our code is that we access data via *adapters* which are something like enhanced file handles or database cursors. An adapter does not read data directly into memory, but rather provides a mechanism to use familiar NumPy/Pandas slicing syntax to load manageable segments of a large dataset. Moreover, an adapter provides fine-grained control over exactly *how* data is eventually read into memory, whether using custom patterns for how a line of data is parsed, choosing the precise data type of a textually represented number, or exposing data as "calculated fields" (that is,  "virtual columns").

As well as local CSV, JSON or other textual data sources, IOPro can load data from Amazon S3 buckets. When accessing large datasets—especially ones too large to load into memory—from files that do not have fixed record sizes, IOPro's indexing feature allows users to seek to a specific collection of records tens, hundreds or thousands of times faster than is possible with a linear scan.

## Our release schedule

The intial release of our open source code will be of the TextAdapter component that makes up the better part of the code in IOPro. This code will be renamed—straightforwardly enough—as **TextAdapter**. The project will live at https://github.com/ContinuumIO/TextAdapter. We will make this forked project available by October 15, 2016 under a BSD 3-Clause License. 

Continuum is evaluating the details of our release of the database adapters, but will definitely make the code (though possibly unrefined) available by December 31, 2016. Our main hesitations with releasing the database adapters is that the state of the art in Python database adapters has advanced considerably since 2009, and we do not want to advocate a codebase unless it is currently best-of-breed (at very least for some niche use case). At worst, we will still release the code as an historical artifact. Those projects will live at https://github.com/ContinuumIO/DBAdapter, https://github.com/ContinuumIO/PostgresAdapter, https://github.com/ContinuumIO/AccumuloAdapter, and https://github.com/ContinuumIO/MongoAdapter.

If you are a current paid customer of IOPro, and are due for renewal before January 1, 2017, your sales rep will get in touch with you for renewal arrangements. We will continue to monitor and reply to issues and discussion about these successor projects at their GitHub repositories.

Thank you to prior contributors at Continuum, especially Jay Bourque (jayvius), but notably also Francesc Alted (FrancescAlted), Óscar Villellas Guillén (ovillellas), Michael Kleehammer (mkleehammer) and Ilan Schnell (ilanschnell) for their wonderful contributions. Any remaining bugs are my responsibility alone as current maintainer of the project.

## The Blaze ecosystem

As part of the open source release of TextAdapter, we plan to integrate TextAdapter into the Blaze ecosystem. Blaze itself, as well as odo, provide translation between data formats and querying of data within a large variety of formats. Putting TextAdapter clearly in this ecosystem will let an *adapter* act as one such data format, and hence leverage the indexing speedups and data massaging that TextAdapter provides.




## TextAdapter

TextAdapter is a Python module containing optimized data adapters for
importing data from a variety of data sources into NumPy arrays and Pandas
DataFrame. Current data adapters include TextAdapter for JSON, free-form,
and CSV-like text files. DBAdapter, also based on IOPro, accesses
MongoAdapter for mongo databases, PostgresAdapter for PostgreSQL databases,
AccumuloAdapter for Accumulo databases, and an optimized pyodbc module for
accessing any relational database that supports the ODBC interface (SQL
Server, PostgreSQL, MySQL, etc).

## Build Requirements

Building TextAdapter requires a number of dependencies. In addition to a
C/C++ dev environment, the following modules are needed, which can be
installed via conda.

* NumPy
* Pandas
* zlib 1.2.8 (C lib)
* pcre 8.31 (C lib)

## Building Conda Package

Note: If building under Windows, make sure the following commands are issued
within the Visual Studio command prompt for version of Visual Studio that
matches the version of Python you're building for. Python 2.6 and 2.7 needs
Visual Studio 2008, Python 3.3 and 3.4 needs Visual Studio 2010, and Python
3.5 needs Visual Studio 2015.

1. Build TextAdapter using the following command:

  ```
  conda build buildscripts/condarecipe --python 3.5
  ```

1. TextAdapter can now be installed from the built conda package:

  ```
  conda install textadapter --use-local
  ```

## Building By Hand

Note: If building under Windows, make sure the following commands are issued
within the Visual Studio command prompt for version of Visual Studio that
matches the version of Python you're building for. Python 2.6 and 2.7 needs
Visual Studio 2008, Python 3.3 and 3.4 needs Visual Studio 2010, and Python
3.5 needs Visual Studio 2015.

For building TextAdapter for local development/testing:

1. Install most of the above dependencies into environment called
   'textadapter':

  ```
  conda env create -f environment.yml
  ```

   Be sure to activate new TextAdapter environment before proceeding.


1. Build TextAdapter using Cython/distutils:

  ```
  python setup.py build_ext --inplace
  ```

## Testing

Tests can be run by calling the iopro module's test function. By default
only the TextAdapter tests will be run:

```python
python -Wignore -c 'import textadapter; textadapter.test()'
```

(Note: `numpy.testing` might produce a FurtureWarning that is not directly
relevant to these unit tests)


Related projects
----------------

- DBAdapter (SQL derivatives): https://github.com/ContinuumIO/DBAdapter
- PostgresAdapter (PostgreSQL): https://github.com/ContinuumIO/PostgresAdapter
- AccumuloAdapter (Apache Accumulo): https://github.com/ContinuumIO/AccumuloAdapter
- MongoAdapter (MongoDB): https://github.com/ContinuumIO/MongoAdapter


## Other open source tools

Other open source projects for interacting with large datasets provide either competitors or collaborative capabilities.  

* The **ParaText** from Wise Technology looks like a very promising approach to accelerating raw reads of CSV data. It doesn't currently provide regular expression matching nor as rich data typing as IOPro, but the raw reads are shockingly fast. Most importantly, perhaps, ParaText does not address indexing, so as fast as it is at linear scan, it remains stuck with big-O inefficiencies that TextAdapter addresses. I personally think that (optionally) utilizing the underlying reader of ParaText as a layer underneath TextAdapter would be a wonderful combination. Information about ParaText can be found at http://www.wise.io/tech/paratext

Database access is almost always I/O bound rather than CPU bound, and hence the likely wins are by switching to asynchronous frameworks. This *does* involve using a somewhat different programming style than synchronous adapters, but some recent ones look amazingly fast. I am not yet sure whether it is worthwhile to create IOPro style adapters around these `asyncio`-based interfaces.

* **asyncpg** is a database interface library designed specifically for PostgreSQL and Python/asyncio. asyncpg is an efficient, clean implementation of PostgreSQL server binary protocol. Information about asyncpg can be found at https://magicstack.github.io/asyncpg/current/.

* **Motor** presents a callback- or Future-based API for non-blocking access to MongoDB from Tornado or asyncio. Information about Motor can be found at http://motor.readthedocs.io/en/stable/.
