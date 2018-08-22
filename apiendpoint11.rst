API Endpoint v1.1
#################

| Using this API, Users will receive results as bounding boxes (a box on each item in the image) 
| and offers for each box. 

This version of the API introduces the concepts of the catalog param, which adds the option of getting results for fashion items, home items or both.

.. important:: Retailers who wish to use our API and receive offers from their own product catalog, need to send us a product data feed (see :ref:`productfeed` section).

The API is comprised of two different requests:
 - **bounds** - will return an object mapping from image URL to list of bounding boxes.
 - **offers** - from the list of bounding boxes, you can request the URL under the ``offers`` attribute, and get a list of offers for this bounding box.


**API Base URL**

.. code-block:: html

 https://syteapi.com/v1.1



Bounds Request
**************

You can **upload an image** *or* send an **image URL**.

Image Upload
============

| You can ask for bounds of an **image binary** -
| Run the following command **replace** "my_test_image.jpg" with the path to the image on the file system:

.. code-block:: bash

 curl -v 'https://syteapi.com/v1.1/offers/bb?account_id=[YOUR_ACCOUNT_ID]&feed=[YOUR_FEED_NAME]&sig=[YOUR_ACCOUT_SIGNATURE]&payload_type=image_bin' --data-binary @my_test_image.jpg

Image URL
=========

You can also ask for bounds of an **image URL**. See example below:

.. code-block:: bash

 curl -v 'http://syteapi.com/v1.1/offers/bb?account_id=[YOUR_ACCOUNT_ID]&feed=[YOUR_FEED_NAME]sig=[YOUR_ACCOUT_SIGNATURE]' --data-binary '["http://wearesyte.com/syte_docs/images/1.jpeg"]'

Users of this API should replace the ``[account_id]``, ``[sig]`` and ``[feed]`` params, with the corresponding credentials as provided to you by Syte.

To get bounds for fashion items, add the **catalog** param to the call like so: ``&catalog=fashion``

To get bounds for home items, add the **catalog** param to the call like so: ``&catalog=home``

To get bounds for both fashion and home, add the **catalog** param to the call like so: ``&catalog=fashion,home``

The API response for ``Bounds`` request will look like `this
<http://wearesyte.com/apiexample/v1.1/example_bb.json>`_.

Every bound includes the following:

 - ``gender`` - if relevant, the gender of the person in the image.
 - ``offers`` link - the link used for getting offers on this bounding box.
 - ``label`` - the product category this bound assumes. this category is an indicator (some with typos).
 - ``catalog`` - the catalog (fashion or home) that the specific bounds belongs to.
 - Three points in the format of ``[x, y]``:
      1. ``center`` - the center of the box
      2. ``b0`` - the top left corner of the box
      3. ``b1`` - the bottom right corner of the box.

``x`` and ``y`` of any of the points are in fractions. numbers in the range ``[0..1]``.

To make an actual coordinate on an image, just multiply image height by ``y`` s and image width by ``x`` s.

All images assume origin (``[0,0]``) of the image to be top-left, and the bottom right is ``[1,1]``.


Offers Request
**************

Once the bounds were obtained, you can call the ``offers`` link provided in the bounds response to get a list of offers related to the bound selected.

The response for offers returns an object, where the attribute ``ads`` has a list of products to offer the user. It looks like `this
<http://wearesyte.com/apiexample/v1.1/example_offers.json>`_.

Every product contains the following:

- ``description`` - some text about the product as provided by merchant/affiliate.
- ``floatPrice`` - the current price for this product as float
- ``floatOriginalPrice`` - the original price for this product as float. having both allows you to present discounts, for example.
- ``price`` - the formatted ``floatPrice`` with currency symbol (of type text).
- ``originalPrice`` - the formatted ``floatOriginalPrice`` with currency symbol (of type text).
- ``brand`` - the brand of the product
- ``merchant`` - the seller of the product
- ``imageUrl`` - the image for the product
- ``offer`` - the link to use to get to the product page at the sellers site.


Overrides
*********

You can override the gender or the category by appending a param to the ``offers`` link:


Gender Override
===============


Forcing a gender to the ``offers`` link will force the results to fetch only products from the selected gender.

To force results for male add: ``&force_gender=male``.

To force results for female add: ``&force_gender=female``.

See example for forcing gender=female:

.. code-block:: bash

 curl 'http://syteapi.com/offers?image_url=aHR0cDovL3dlYXJlc3l0ZS5jb20vc3l0ZV9kb2NzL2ltYWdlcy8yLmpwZWc%3D&crop=eyJ5MiI6MS4wLCJ5IjowLjU3ODU0MjgxMjkxMzY1NjMsIngyIjowLjU5MTI4Mzc5ODIxNzc3MzQsIngiOjAuMDUwNzk1NTU1MTE0NzQ2MTE1fQ%3D%3D&cats=WyJUcm91c2VycyJd&prob=0.5739&gender=female&feed=default&country=IL&account_id=[YOUR_ACCOUNT_ID]&sig=[YOUR_ACCOUT_SIGNATURE]&force_gender=female'


Category Override
=================

Forcing a category to the ``offers`` link will force the results to fetch only products from the selected category.

For example, to force results for dresses add: ``&force_cats=Dresses``.

See example for forcing cats=Dresses:

.. code-block:: bash

 curl 'http://syteapi.com/offers?image_url=aHR0cDovL3dlYXJlc3l0ZS5jb20vc3l0ZV9kb2NzL2ltYWdlcy8yLmpwZWc%3D&crop=eyJ5MiI6MS4wLCJ5IjowLjU3ODU0MjgxMjkxMzY1NjMsIngyIjowLjU5MTI4Mzc5ODIxNzc3MzQsIngiOjAuMDUwNzk1NTU1MTE0NzQ2MTE1fQ%3D%3D&cats=WyJUcm91c2VycyJd&prob=0.5739&gender=female&feed=default&country=IL&account_id=[YOUR_ACCOUNT_ID]&sig=[YOUR_ACCOUT_SIGNATURE]&force_cats=Dresses'

You can find the full list of categories here_.

.. _here: http://wearesyte.com/apiexample/force_cats.json


Related Looks
*************

You can send an image and get related looks from social networks. To enable this feature, please contact Syte.

To use this feature, please add ``&features=related_looks`` to the bounds request. This feature will only work with version v1.1 of the API.

Here is an example of a bounds call with the related_looks feature:

.. code-block:: bash

 curl -v 'http://syteapi.com/v1.1/offers/bb?account_id=[YOUR_ACCOUNT_ID]&sig=[YOUR_ACCOUT_SIGNATURE]&features=related_looks' --data-binary '["http://wearesyte.com/syte_docs/images/1.jpeg"]'

The response to this call includes a "related_looks" link (similar to the offers link), that when followed, will return a list of image URLs with similar looks from social networks.

Deep Tagging
************

You can send an image and get a break down of each garment in the image to its most nuanced attributes. To enable this feature, please contact Syte.

To use this feature, please add ``&features=deeptags`` to the bounds request. This feature will only work with version v1.1 of the API.

Here is an example of a bounds call with the related_looks feature:

.. code-block:: bash

 curl -v 'http://syteapi.com/v1.1/offers/bb?account_id=[YOUR_ACCOUNT_ID]&sig=[YOUR_ACCOUT_SIGNATURE]&features=deeptags' --data-binary '["http://wearesyte.com/syte_docs/images/1.jpeg"]'

The response to this will add an array of tags to each bound returned from the bounds API call.
