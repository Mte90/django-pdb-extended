Django PDB
==========

Make debugging Django easier to the (break)point.

This is a modded version based on https://github.com/cpontvieux-systra/django-pdb that add the support to Django 4.1.
The other difference is that automatically the input in console goes to the breakpoint and ignores the start of every view.
Also doesn't have the support for commands as they crashes with the latest Django releases, so only the URL parameter is supported.

Installation
------------

Install using pip::

    pip install git+https://github.com/mte90/django-pdb-extended

Add it to your settings.py.

.. code:: python

    # Order is important and depends on your Django version.
    # Put it towards the beginning, otherwise towards the end.
    INSTALLED_APPS = (
        ...
        'django_pdb_extended',
        ...
    )

    # Make sure to add PdbMiddleware after all other middleware.
    # PdbMiddleware only activates when settings.DEBUG is True.
    MIDDLEWARE_CLASSES = (
        ...
        'django_pdb_extended.middleware.PdbMiddleware',
    )

Usage
-----

``manage.py runserver``

Drops into pdb at the start of a view if the URL includes a `pdb` GET parameter.

Drops into ipdb at the start of a view if the URL includes a `ipdb` GET parameter.

This behavior is only enabled if ``settings.DEBUG = True``::

    bash: testproject/manage.py runserver
    Validating models...

    0 errors found
    Django version 4.1, using settings 'testproject.settings'
    Development server is running at http://127.0.0.1:8000/
    Quit the server with CONTROL-C.

    GET /test?pdb

    > /Users/tom/github/django-pdb/testproject/testapp/views.py(8)myview()
    -> a = 1
    (Pdb)

Filter
------

You can also use the template filter ``pdb`` or ``ipdb`` to explore a template variable in (i)pdb this way::

    {% load pdb %}

    {{ variable|pdb }}
    {{ variable|ipdb }}
    {{ variable|ipdb|a_filter_to_debug }}

Example::

    bash: testproject/manage.py runserver
    Validating models...

    0 errors found
    Django version 1.4, using settings 'testproject.settings'
    Development server is running at http://127.0.0.1:8000/
    Quit the server with CONTROL-C.
    > /Users/tom/github/django-pdb/django_pdb/templatetags/pdb_filters.py(14)pdb()
    -> return element
    (Pdb) element
    "I'm the variable"
    (Pdb) element = "another value"
    (Pdb) c
    [11/May/2012 11:22:53] "GET /filter/ HTTP/1.1" 200 37

This is useful to inspect a complex object that isn't behaving as expected or debug a filter.
