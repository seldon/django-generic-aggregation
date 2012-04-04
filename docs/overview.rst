.. -*- mode: rst -*-
 
Motivations
===========

Since version 1.1, the Django ORM provides an `aggregation API`_, allowing for generation of summary information (counts, sums,
averages, etc.) from data contained within database tables without having to write any SQL by hand.  

This API also supports aggregation across related tables, when data relationships are implemented -- at the model level
-- by ``ForeignKey``\ s or ``ManyToManyField``\ s.

Django also provides `generic relations`_, a machinery allowing a model instance to be linked to other, arbitrary, model
instances, even belonging to different model classes.  Currently, though, Django's database aggregation API
doesn't support aggregating across generic relations.  Quoting from `the docs`_::

        The generic relation adds extra filters to the queryset to ensure the correct content type, but the ``aggregate()`` method
        doesn't take them into account. For now, if you need aggregates on generic relations, you'll need to calculate them
        without using the aggregation API.

However, the need for aggregating data across generic relations arises quite often, for example in common scenarios like the
following ones:

* retrieving the set of most commented/voted/rated/bookmarked objects within an application
* annotating each tag with the number of times it was used to categorize an item within an application
* selecting "hot" topics in a forum
* and so on

Since Django's ORM doesn't (yet [*]_ ]) provides support for these sort of queries, we are forced to resort to writing custom SQL
that, beyond violating the DRY principle, is an error-prone task and may lead to compatibility issues with some of the
database backends supported by Django.  

``django-generic-aggregation`` is meant to be a temporary workaround with respect to this issue, providing a cross-DB [*]_
aggregation API supporting generic relations. Internally, it relies on the `extra()`_  ``QuerySet`` method for assembling a
custom SQL query, given some user-provided parameters.

See also `this post`_ by Charles Leifer, this application's author. 

 
.. _`aggregation API`: http://docs.djangoproject.com/en/dev/topics/db/aggregation/
.. _`generic relations`: https://docs.djangoproject.com/en/dev/ref/contrib/contenttypes/#generic-relations
.. _`the docs`: http://docs.djangoproject.com/en/dev/ref/contrib/contenttypes/#generic-relations-and-aggregation
.. _`extra()`: https://docs.djangoproject.com/en/1.3/ref/models/querysets/#extra
.. _`this post`: http://charlesleifer.com/blog/generating-aggregate-data-across-generic-relations/

.. [*] See e.g. tickets #10461 and #10870.
.. [*] Currently, ``django-generic-aggregation`` has been tested against **PostgreSQL** and **SQLite**.
