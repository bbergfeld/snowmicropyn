.. _api_usersguide:

API User's Guide
================

Data Files
----------

When performing a measurement with SnowMicroPen, the device writes the data
onto its SD card in a binary file with a :file:`pnt` extension. (Example:
:file:`S13M0067.pnt`). For each measurment process, a new pnt file is written.
Each pnt file consists of a header with meta information followed by the actual
data, the force samples.

.. note:: The *snowmicropyn* package never ever writes into a pnt file. Good to
   know your precious raw data is always safe.

Corresponding ini files
^^^^^^^^^^^^^^^^^^^^^^^

However, when using functionality of this package, an additional storage to save
other data is required. This storage is an :file:`ini` file, named like the pnt
file (example from previous section: :file:`S13M0067.ini`).

First steps
-----------

The core class of the API is the :class:`snowmicropyn.Profile` class. It
represents a profile loaded from a pnt file. By using its static load method,
you can load a profile:

.. code-block:: python

   import snowmicropyn
   p = snowmicropyn.Profile.load('./S13M0067.pnt')

In the load call, there's also a check for a corresponding ini file, in this
case for :file:`S13M0067.ini`.

Logging *snowmicropyn*'s Version and Git Hash
---------------------------------------------

As a scientist, you may be interested to keep a log so you can reproduce what
you calculated with what version of *snowmicropyn*. The package contains a
version string and a git hash identifier.

To access the package's version string, you do:

.. code-block:: python

   import snowmicropyn
   v = snowmicropyn.__version__

To access the git hash string of this release, you do:

.. code-block:: python

   import snowmicropyn
   gh = snowmicropyn.githash()

When exporting data using this module, the created CSV files will also contain a
comment as first line with the version string and git hash to identify which
version of *snowmicropyn* was used to create the file.

.. warning::

   However, this is no mechanism to protect a file from later alternation. It's
   just some basic information which maybe will be useful to you.

Examples
--------

Some examples will help you get an overview of *snowmicropyn*'s features.

.. hint::

   To get the code mentioned in this guide, Download_ the source code of
   *snowmicropyn*. You'll find the examples in the subfolder ``examples``
   and even some pnt files to play around with in the folder
   ``examples/profiles``.

Explore properties
^^^^^^^^^^^^^^^^^^

In our first example, we load a profile and explore its properties. We set some
markers and finally call the :meth:`snowmicropyn.Profile.save` so the markers
get saved in an ini file so we don't lose them.

.. literalinclude::  ../examples/explore.py

Batch exporting
^^^^^^^^^^^^^^^

You're just home from backcountry where you recorded a series of profiles with
your SnowMicroPen and now want to read this data with your tool of choice which
supports reading CSV files? Then this example is for you!

.. literalinclude::  ../examples/batch_export.py

After you executed this example, there will be a :file:`..._samples.csv` and a
:file:`..._meta.csv` for each pnt file in the directory.

Plotting
^^^^^^^^

In this example, we use the delightful matplotlib_ to explore the penetration
signal of a profile.

.. literalinclude::  ../examples/plot.py


When this code is executed, a window like the following should open:

.. figure:: images/screenshot_matplotlib.png
   :alt: Screenshot of matplotlib Window

Explore using the tool buttons below the plot! You can even modify the axes and
export the plot into an image file.

A Touch of Science
^^^^^^^^^^^^^^^^^^

Alright, let's do some science. In this example, we examine a profile recorded
at our test site Weissfluhjoch. There's a crust and a depth hoar layer in this
profile. By using :command:`pyngui`, we already identified the layers for you by
setting markers. Let's calculate the mean specific surface area (SSA) within the
crust and the weight that lies on the depth hoar layer.

.. literalinclude::  ../examples/weaklayer.py

This will print something like:

.. code-block:: console

   Mean SSA within crust: 5.5 m^2/m^3
   Weight above hoar layer: 98 kg/m^2


.. _matplotlib: https://www.matplotlib.org/
.. _Download: https://github.com/slf-dot-ch/snowmicropyn/
