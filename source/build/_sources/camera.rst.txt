Camera Upload Intergration
##########################

Web and Mobile Web integration of the camera upload button is done by doing the following:

Design the camera icon that will lunch the feature and give it this class: 

.. code-block:: html

 <a href="javascript:void(0)" class="--syte-start-camera-upload">


Add the following HTML script tag to the section of the HTML:

.. code-block:: html

 <script src="//syteapi.com/assets/imajs/imajs.js?a=[YOUR_ACCOUNT_ID]&sig=[YOUR_ACCOUT_SIGNATURE]"></script>

Users of this service should provide an ``account_id`` and ``sig`` params, as provided by Syte.