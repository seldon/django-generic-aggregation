 .. -*- mode: rst -*-
 
=========================
 Generic Aggregation API
=========================

Overview
--------

``django-generic-aggregation`` provides a DB aggregation API working across generic relations. This API is implemented by two functions,
:py:func:`generic_annotate` and :py:func:`generic_aggregate`, which may be imported from the ``generic_aggregation`` module.

For the sake of concreteness, we assume that we are dealing with the following data model: a set of *content models* (e.g. ``Article``,
``BlogEntry``, etc.) and a *generic model* (e.g. ``TaggedItem``, ``Bookmark``, ``Comment``, etc.), which describes a generic relationship 
involving instances of content models:

.. sourcecode:: python

   class GenericModel(models.Model):
          content_type = models.ForeignKey(ContentType)
          object_id = models.PositiveIntegerField()
          content_object = generic.GenericForeignKey()
          ... # other fields describing the relationship

    class ContentModel1(models.Model):
          ... # model fields

    class ContentModel2(models.Model):
          ... # model fields

          [...]

    class ContentModelN(models.Model):
          ... # model fields


API reference
-------------
     

.. py:function:: generic_annotate(queryset, gfk_field, aggregator[, generic_queryset=None, desc=True, alias='score'])
   
   Annotate a ``QuerySet`` of content objects with respect to a field, across a generic relation.

   :param queryset: the ``QuerySet`` of content objects to be annotated
   :param gfk_field: the ``GenericForeignKey`` (pseudo-)field of the generic model, pointing to content objects
   :param aggregator: an instance of one of the aggregator classes (``Sum``, ``Avg``, ``Max``, ``Min``, ``Count``, etc.) provided by
                      Django's DB aggregation API. The aggregator must be instantiated with the name of the field (of the generic model) with
                      respect to which the aggregation is to be performed
   :param generic_queryset: a ``QuerySet`` of generic model instances the aggregation should be restricted to
   :param desc: whether the result ``QuerySet`` should be ordered in descending order or not
   :type desc: boolean
   :param alias: an alias for the annotation attribute that will be added to the content model
   :type alias: string


.. py:function:: generic_aggregate(queryset, gfk_field, aggregator[, generic_queryset=None])
   
   Aggregate a ``QuerySet`` of content objects with respect to a field, across a generic relation.

   :param queryset: the ``QuerySet`` of content objects to be aggregated
   :param gfk_field: the ``GenericForeignKey`` (pseudo-)field of the generic model, pointing to content objects
   :param aggregator: an instance of one of the aggregator classes (``Sum``, ``Avg``, ``Max``, ``Min``, ``Count``, etc.) provided by
                      Django's DB aggregation API. The aggregator must be instantiated with the name of the field (of the generic model) with
                      respect to which the aggregation is to be performed
   :param generic_queryset: a ``QuerySet`` of generic model instances the aggregation should be restricted to
