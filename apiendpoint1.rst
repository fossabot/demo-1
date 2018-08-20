API Endpoint v1.0
#################

Users of this API will receive results as bounding boxes (for each item in an image) and offers for each box. 

Retailers who wish to use our API and receive offers from their own product catalog, need to send us a product data feed (see :ref:`productfeed` section).

The API is comprised of two distinct requests:
 - **bounds** - will return an object mapping from image url to list of bounding boxes.
 - **offers** - from the list of bounding boxes, you can request the url under the ``offers`` attribute, and get a list of offers for this bounding box.

.. note:: Syte **strongly** recommends to use API Endpoint v1.1, if possible.



**API Base URL**

.. code-block:: html

 https://syteapi.com/



Bounds Request
**************

Image Upload
============

You can ask for bounds of an **image binary**, by running the following command and replacing "my_test_image.jpg" with the path to the image on the file system.



.. code-block:: bash

   curl -v 'https://syteapi.com/offers/bb?account_id=[YOUR_ACCOUNT_ID]&sig=[YOUR_ACCOUT_SIGNATURE]&feed=[YOUR_FEED_NAME]&payload_type=image_bin' --data-binary @my_test_image.jpg



Image URL
=========

You can also ask for bounds by sending an **image url**:

.. code-block:: bash

   curl -v 'https://syteapi.com/offers/bb?account_id=[YOUR_ACCOUNT_ID]&sig=[YOUR_ACCOUT_SIGNATURE]&feed=[YOUR_FEED_NAME]&payload_type=image_bin' --data-binary @my_test_image.jpg


Users of this API should replace the ``[account_id]``, ``[sig]`` and ``[feed]`` params, with the corresponding credentials as provided to you by Syte.


**The API response for** the ``Bounds`` **request will look like** `this
<http://wearesyte.com/apiexample/example_bb.json>`_.

Every bound includes the following:
 - ``offers`` link - the link used for getting offers on this bounding box.
 - ``label`` - the product category this bound assumes. this category is an indicator (some with typos).
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
<http://wearesyte.com/apiexample/example_offers.json>`_.

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

