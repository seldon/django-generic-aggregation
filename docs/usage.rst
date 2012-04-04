.. -*- mode: rst -*-

=================
 Getting started
=================

Just to get you started, below we list some basic usage examples; for a detailed description of the generic DB
aggregation API provided by ``django-generic-aggregation``, see :doc:`here <api>`.

Examples
========

Suppose you have a ``BlogEntry`` model and want to retrieve the most commented on blog entries:

.. sourcecode:: python

    >>> from django.contrib.comments.models import Comment
    >>> from django.db.models import Count
    >>> from blog.models import BlogEntry
    >>> from generic_aggregation import generic_annotate

    >>> annotated = generic_annotate(BlogEntry.objects.all(), Comment.content_object, Count('id'))

    >>> for entry in annotated:
    ...    print entry.title, entry.score

    The most popular 5
    The second best 4
    Nobody commented 0


Now, suppose you have a ``Food`` model and a generic ``Rating`` model; if you want to figure out which items have the
highest rating, you can proceed as follows:

.. sourcecode:: python

    from django.db.models import Sum, Avg

    apple = Food.objects.create(name='apple')
    
    # create some ratings on the food
    Rating.objects.create(content_object=apple, rating=3)
    Rating.objects.create(content_object=apple, rating=5)
    Rating.objects.create(content_object=apple, rating=7)

    >>> aggregate = generic_aggregate(Food.objects.all(), Rating.content_object, Sum('rating'))
    >>> print aggregate
    15

    >>> aggregate = generic_aggregate(Food.objects.all(), Rating.content_object, Avg('rating'))
    >>> print aggregate
    5



   
