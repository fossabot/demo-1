Search API
##########

Using our Search API, You are able to send rich text queries and recieve relevant results for these queries.

Here's the API call:

.. code-block:: bash

 https://search.syteapi.com/search/items?q=dress&account_id=[YOUR_ACC_ID_HERE]&feed=[YOUR_FEED_HERE]&sig=[YOUR_SIG_HERE]

As in every API request, You need to replace the [] with the credentials provided by Syte. (``[YOUR_ACC_ID_HERE]`` , ``[YOUR_FEED_HERE]`` , ``[YOUR_SIG_HERE]``)

| In the example above, the word ``dress`` is searched, as it appears after the parameter ``q``.
| In order to search multiple words, add ``+`` between each word. 
| (e.g. ``blue+maxi+dress``)

In conclusion, to search for a blue maxi dress, use this API call:

.. code-block:: bash

 https://search.syteapi.com/search/items?q=blue+maxi+dress&account_id=[YOUR_ACC_ID_HERE]&feed=[YOUR_FEED_HERE]&sig=[YOUR_SIG_HERE]