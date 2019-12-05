{%extends "BASIC-README.rst.jj2"%}

{%block features%}

Feature Highlights
===================

1. One application programming interface(API) to handle multiple data sources:

   * physical file
   * memory file
   * SQLAlchemy table
   * Django Model
   * Python data structures: dictionary, records and array
2. One API to read and write data in various excel file formats.
3. For large data sets, data streaming are supported. A genenerator can be returned to you. Checkout iget_records, iget_array, isave_as and isave_book_as. 

{% endblock %}

{%block usage%}

Usage
===============

Please note that you will have to use '.sortable.html' in order to replicate the example.

.. image:: https://github.com/pyexcel/pyexcel-sortable/raw/master/sortable.gif

.. code-block:: python

    >>> # pip install pyexcel-text==0.2.7.1
    >>> import pyexcel as p
    >>> ccs_insight2 = p.Sheet()
    >>> ccs_insight2.name = "Worldwide Mobile Phone Shipments (Billions), 2017-2021"
    >>> ccs_insight2.ndjson = """
    ... {"year": ["2017", "2018", "2019", "2020", "2021"]}
    ... {"smart phones": [1.53, 1.64, 1.74, 1.82, 1.90]}
    ... {"feature phones": [0.46, 0.38, 0.30, 0.23, 0.17]}
    ... """.strip()
    >>> ccs_insight2
    pyexcel sheet:
    +----------------+------+------+------+------+------+
    | year           | 2017 | 2018 | 2019 | 2020 | 2021 |
    +----------------+------+------+------+------+------+
    | smart phones   | 1.53 | 1.64 | 1.74 | 1.82 | 1.9  |
    +----------------+------+------+------+------+------+
    | feature phones | 0.46 | 0.38 | 0.3  | 0.23 | 0.17 |
    +----------------+------+------+------+------+------+



Suppose you have the following data in a dictionary:

========= ====
Name      Age
========= ====
Adam      28
Beatrice  29
Ceri      30
Dean      26
========= ====

you can easily save it into an excel file using the following code:

.. code-block:: python

   >>> import pyexcel
   >>> # make sure you had pyexcel-xls installed
   >>> a_list_of_dictionaries = [
   ...     {
   ...         "Name": 'Adam',
   ...         "Age": 28
   ...     },
   ...     {
   ...         "Name": 'Beatrice',
   ...         "Age": 29
   ...     },
   ...     {
   ...         "Name": 'Ceri',
   ...         "Age": 30
   ...     },
   ...     {
   ...         "Name": 'Dean',
   ...         "Age": 26
   ...     }
   ... ]
   >>> pyexcel.save_as(records=a_list_of_dictionaries, dest_file_name="your_file.xls")


And here's how to obtain the records:

.. code-block:: python
   
   >>> import pyexcel as p
   >>> records = p.iget_records(file_name="your_file.xls")
   >>> for record in records:
   ...     print("%s is aged at %d" % (record['Name'], record['Age']))
   Adam is aged at 28
   Beatrice is aged at 29
   Ceri is aged at 30
   Dean is aged at 26
   >>> p.free_resources()


Advanced usage :fire:
----------------------

If you are dealing with big data, please consider these usages:

   >>> def increase_everyones_age(generator):
   ...     for row in generator:
   ...         row['Age'] += 1
   ...         yield row
   >>> def duplicate_each_record(generator):
   ...     for row in generator:
   ...         yield row
   ...         yield row
   >>> records = p.iget_records(file_name="your_file.xls")
   >>> io=p.isave_as(records=duplicate_each_record(increase_everyones_age(records)),
   ...     dest_file_type='csv', dest_lineterminator='\n')
   >>> print(io.getvalue())
   Age,Name
   29,Adam
   29,Adam
   30,Beatrice
   30,Beatrice
   31,Ceri
   31,Ceri
   27,Dean
   27,Dean
   <BLANKLINE>

Two advantages of above method:

#. Add as many wrapping functions as you want.
#. Constant memory consumption

Available Plugins
=================

{% include "plugins-list.rst.jj2"%}


Acknowledgement
===============

All great work have been done by odf, ezodf, xlrd, xlwt, tabulate and other
individual developers. This library unites only the data access code.


.. testcode::
   :hide:
   
   >>> import os
   >>> os.unlink("your_file.xls")

{%endblock%}

{%block development_guide%}
{%endblock%}
