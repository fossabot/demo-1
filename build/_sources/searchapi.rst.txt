Search API
##########

Using our Search API, You are able to send rich text queries and recieve relevant results for these queries.

You can ask for search results using the following API call:

.. code-block:: bash

 https://search.syteapi.com/search/items?q=dress&account_id=[YOUR_ACCOUNT_ID_HERE]&feed=[YOUR_FEED_NAME]&sig=[YOUR_ACCOUT_SIGNATURE]

As in every API request, You need to replace the [] with the credentials provided by Syte. (``[YOUR_ACCOUNT_ID_HERE]`` , ``[YOUR_FEED_NAME]`` , ``[YOUR_ACCOUT_SIGNATURE]``)

| Put your desired search query after the param ``q=``. 
| In the example above, the word ``dress`` is searched. (``q=dress``)
| In order to search multiple words, add ``+`` between each word. 
| (e.g. ``blue+maxi+dress``)

In conclusion, to search for a blue maxi dress, use this API call:

.. code-block:: bash

 https://search.syteapi.com/search/items?q=blue+maxi+dress&account_id=[YOUR_ACCOUNT_ID_HERE]&feed=[YOUR_FEED_NAME]&sig=[YOUR_ACCOUT_SIGNATURE]