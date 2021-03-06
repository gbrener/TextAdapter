-----
IOPro
-----

IOPro loads NumPy arrays (and Pandas DataFrames) directly from files, SQL
databases, and NoSQL stores--including ones with millions of rows--without
creating millions of temporary, intermediate Python objects, or requiring
expensive array resizing operations. 

IOPro provides a drop-in replacement for the 
NumPy functions :code:`loadtxt()` and :code:`genfromtxt()`, but dramatically
improves performance and reduces memory overhead.

The current version of IOPro 1.9 was released on July 30, 2016. 

How to get IOPro
----------------

IOPro is included with `Anaconda Workgroup and Anaconda Enterprise
subscriptions <https://www.continuum.io/content/anaconda-subscriptions>`_.

To start a 30-day free trial just download and install the IOPro package.

If you already have `Anaconda <http://continuum.io/downloads.html>`_ (free
Python platform) or `Miniconda <http://conda.pydata.org/miniconda.html>`_
installed::

    conda update conda
    conda install iopro

If you do not have Anaconda installed, you can `download it
<http://continuum.io/downloads.html>`_.

For more information about IOPro please contact `sales@continuum.io
<mailto:sales@continuum.io>`_.

Requirements
------------

* Python 2.7 or 3.4+
* NumPy 1.10+

Optional Python modules:

* Boto (for S3 support)
* Pandas (to use DataFrames)

What's new in version 1.9?
--------------------------

The documentation has been substantially updated for version 1.9.0. 
Numba has been removed and the code has been cleaned up, but no other 
features were added or removed. Some refactoring was done that didn't 
change functionality. We recommend that users not use older versions.
See :doc:`Release notes <release-notes>` for additional detail.


Getting started
---------------

Some of the basic usage patterns look like these.  Create TextAdapter object
for data source::

    >>> import iopro
    >>> adapter = iopro.text_adapter('data.csv', parser='csv')

Define field dtypes (example: set field 0 to unsigned int and field 4 to
float)::

    >>> adapter.set_field_types({0: 'u4', 4:'f4'})

Parse text and store records in NumPy array using slicing notation::

    >>> # read all records
    >>> array = adapter[:]

    >>> # read first ten records
    >>> array = adapter[0:10]

    >>> # read last record
    >>> array = adapter[-1]

    >>> # read every other record
    >>> array = adapter[::2]

User guide
----------

.. toctree::
    :maxdepth: 1

    install
    textadapter_examples
    eula
    release-notes

Reference guide
---------------

.. toctree::
    :maxdepth: 1

    TextAdapter
    loadtxt
    genfromtxt


Previous Versions
-----------------

This documentation is provided for the use of our customers who have not yet upgraded 
to the current version. 

NOTE: We recommend that users not use older versions of IOPro.

.. toctree::
   :maxdepth: 1

   IOPro 1.8.0 <1.8.0/index>
