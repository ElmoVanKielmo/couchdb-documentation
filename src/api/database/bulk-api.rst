.. Licensed under the Apache License, Version 2.0 (the "License"); you may not
.. use this file except in compliance with the License. You may obtain a copy of
.. the License at
..
..   http://www.apache.org/licenses/LICENSE-2.0
..
.. Unless required by applicable law or agreed to in writing, software
.. distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
.. WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the
.. License for the specific language governing permissions and limitations under
.. the License.

.. _api/db/all_docs:

=================
``/db/_all_docs``
=================

.. http:get:: /{db}/_all_docs
    :synopsis: Returns a built-in view of all documents in this database

    Returns a JSON structure of all of the documents in a given database.
    The information is returned as a JSON structure containing meta
    information about the return structure, including a list of all documents
    and basic contents, consisting the ID, revision and key. The key is the
    from the document's ``_id``.

    :param db: Database name
    :<header Accept: - :mimetype:`application/json`
                     - :mimetype:`text/plain`
    :query boolean conflicts: Includes `conflicts` information in response.
      Ignored if `include_docs` isn't ``true``. Default is ``false``.
    :query boolean descending: Return the documents in descending by key order.
      Default is ``false``.
    :query string endkey: Stop returning records when the specified key is
      reached. *Optional*.
    :query string end_key: Alias for `endkey` param.
    :query string endkey_docid: Stop returning records when the specified
      document ID is reached. *Optional*.
    :query string end_key_doc_id: Alias for `endkey_docid` param.
    :query boolean include_docs: Include the full content of the documents in
      the return. Default is ``false``.
    :query boolean inclusive_end: Specifies whether the specified end key
      should be included in the result. Default is ``true``.
    :query string key: Return only documents that match the specified key.
      *Optional*.
    :query string keys: Return only documents that match the specified keys.
      *Optional*.
    :query number limit: Limit the number of the returned documents to the
      specified number. *Optional*.
    :query number skip: Skip this number of records before starting to return
      the results. Default is ``0``.
    :query string stale: Allow the results from a stale view to be used,
      without triggering a rebuild of all views within the encompassing design
      doc. Supported values: ``ok`` and ``update_after``. *Optional*.
    :query string startkey: Return records starting with the specified key.
      *Optional*.
    :query string start_key: Alias for `startkey` param.
    :query string startkey_docid: Return records starting with the specified
      document ID. *Optional*.
    :query string start_key_doc_id: Alias for `startkey_docid` param.
    :query boolean update_seq: Response includes an ``update_seq`` value
      indicating which sequence id of the underlying database the view
      reflects. Default is ``false``.
    :>header Content-Type: - :mimetype:`application/json`
                           - :mimetype:`text/plain; charset=utf-8`
    :>header ETag: Response signature
    :>json number offset: Offset where the document list started
    :>json array rows: Array of view row objects. By default the information
      returned contains only the document ID and revision.
    :>json number total_rows: Number of documents in the database/view. Note
      that this is not the number of rows returned in the actual query.
    :>json number update_seq: Current update sequence for the database
    :code 200: Request completed successfully

    **Request**:

    .. code-block:: http

        GET /db/_all_docs HTTP/1.1
        Accept: application/json
        Host: localhost:5984

    **Response**:

    .. code-block:: http

        HTTP/1.1 200 OK
        Cache-Control: must-revalidate
        Content-Type: application/json
        Date: Sat, 10 Aug 2013 16:22:56 GMT
        ETag: "1W2DJUZFZSZD9K78UFA3GZWB4"
        Server: CouchDB (Erlang/OTP)
        Transfer-Encoding: chunked

        {
            "offset": 0,
            "rows": [
                {
                    "id": "16e458537602f5ef2a710089dffd9453",
                    "key": "16e458537602f5ef2a710089dffd9453",
                    "value": {
                        "rev": "1-967a00dff5e02add41819138abb3284d"
                    }
                },
                {
                    "id": "a4c51cdfa2069f3e905c431114001aff",
                    "key": "a4c51cdfa2069f3e905c431114001aff",
                    "value": {
                        "rev": "1-967a00dff5e02add41819138abb3284d"
                    }
                },
                {
                    "id": "a4c51cdfa2069f3e905c4311140034aa",
                    "key": "a4c51cdfa2069f3e905c4311140034aa",
                    "value": {
                        "rev": "5-6182c9c954200ab5e3c6bd5e76a1549f"
                    }
                },
                {
                    "id": "a4c51cdfa2069f3e905c431114003597",
                    "key": "a4c51cdfa2069f3e905c431114003597",
                    "value": {
                        "rev": "2-7051cbe5c8faecd085a3fa619e6e6337"
                    }
                },
                {
                    "id": "f4ca7773ddea715afebc4b4b15d4f0b3",
                    "key": "f4ca7773ddea715afebc4b4b15d4f0b3",
                    "value": {
                        "rev": "2-7051cbe5c8faecd085a3fa619e6e6337"
                    }
                }
            ],
            "total_rows": 5
        }

.. http:post:: /{db}/_all_docs
    :synopsis: Returns certain rows from the built-in view of all documents

    The ``POST`` to ``_all_docs`` allows to specify multiple keys to be
    selected from the database. This enables you to request multiple
    documents in a single request, in place of multiple :get:`/{db}/{docid}`
    requests.

    :param db: Database name
    :<header Content-Type: :mimetype:`application/json`
    :<json array keys: Return only documents that match the specified keys.
      *Optional*
    :>header Content-Type: - :mimetype:`application/json`
    :code 200: Request completed successfully

    **Request**:

    .. code-block:: http

        POST /db/_all_docs HTTP/1.1
        Accept: application/json
        Content-Length: 70
        Content-Type: application/json
        Host: localhost:5984

        {
            "keys" : [
                "Zingylemontart",
                "Yogurtraita"
            ]
        }

    **Response**:

    .. code-block:: javascript

        {
            "total_rows" : 2666,
            "rows" : [
                {
                    "value" : {
                        "rev" : "1-a3544d296de19e6f5b932ea77d886942"
                    },
                    "id" : "Zingylemontart",
                    "key" : "Zingylemontart"
                },
                {
                    "value" : {
                        "rev" : "1-91635098bfe7d40197a1b98d7ee085fc"
                    },
                    "id" : "Yogurtraita",
                    "key" : "Yogurtraita"
                }
            ],
            "offset" : 0
        }

.. _api/db/bulk_docs:

==================
``/db/_bulk_docs``
==================

.. http:post:: /{db}/_bulk_docs
    :synopsis: Inserts or updates multiple documents in to the database in
               a single request

    The bulk document API allows you to create and update multiple documents
    at the same time within a single request. The basic operation is similar
    to creating or updating a single document, except that you batch the
    document structure and information.

    When creating new documents the document ID (``_id``) is optional.

    For updating existing documents, you must provide the document ID, revision
    information (``_rev``), and new document values.

    In case of batch deleting documents all fields as document ID, revision
    information and deletion status (``_deleted``) are required.

    :param db: Database name
    :<header Accept: - :mimetype:`application/json`
                     - :mimetype:`text/plain`
    :<header Content-Type: :mimetype:`application/json`
    :<header X-Couch-Full-Commit: Overrides server's
      :config:option:`commit policy <couchdb/delayed_commits>`. Possible values
      are: ``false`` and ``true``. *Optional*
    :<json array docs: List of documents objects
    :<json boolean new_edits: If ``false``, prevents the database from
      assigning them new revision IDs. Default is ``true``. *Optional*
    :>header Content-Type: - :mimetype:`application/json`
                           - :mimetype:`text/plain; charset=utf-8`
    :>jsonarr string id: Document ID
    :>jsonarr string rev: New document revision token. Available
      if document has saved without errors. *Optional*
    :>jsonarr string error: Error type. *Optional*
    :>jsonarr string reason: Error reason. *Optional*
    :code 201: Document(s) have been created or updated
    :code 400: The request provided invalid JSON data
    :code 417: Occurs when at least one document was rejected by a
     :ref:`validation function <vdufun>`

    **Request**:

    .. code-block:: http

        POST /db/_bulk_docs HTTP/1.1
        Accept: application/json
        Content-Length: 109
        Content-Type:application/json
        Host: localhost:5984

        {
            "docs": [
                {
                    "_id": "FishStew"
                },
                {
                    "_id": "LambStew",
                    "_rev": "2-0786321986194c92dd3b57dfbfc741ce",
                    "_deleted": true
                }
            ]
        }

    **Response**:

    .. code-block:: http

        HTTP/1.1 201 Created
        Cache-Control: must-revalidate
        Content-Length: 144
        Content-Type: application/json
        Date: Mon, 12 Aug 2013 00:15:05 GMT
        Server: CouchDB (Erlang/OTP)

        [
            {
                "ok": true,
                "id": "FishStew",
                "rev":" 1-967a00dff5e02add41819138abb3284d"
            },
            {
                "ok": true,
                "id": "LambStew",
                "rev": "3-f9c62b2169d0999103e9f41949090807"
            }
        ]

Inserting Documents in Bulk
===========================

Each time a document is stored or updated in CouchDB, the internal B-tree
is updated. Bulk insertion provides efficiency gains in both storage space,
and time, by consolidating many of the updates to intermediate B-tree nodes.

It is not intended as a way to perform ``ACID``-like transactions in CouchDB,
the only transaction boundary within CouchDB is a single update to a single
database. The constraints are detailed in :ref:`api/db/bulk_docs/semantics`.

To insert documents in bulk into a database you need to supply a JSON
structure with the array of documents that you want to add to the database.
You can either include a document ID, or allow the document ID to be
automatically generated.

For example, the following update inserts three new documents, two with the
supplied document IDs, and one which will have a document ID generated:

.. code-block:: http

    POST /source/_bulk_docs HTTP/1.1
    Accept: application/json
    Content-Length: 323
    Content-Type: application/json
    Host: localhost:5984

    {
        "docs": [
            {
                "_id": "FishStew",
                "servings": 4,
                "subtitle": "Delicious with freshly baked bread",
                "title": "FishStew"
            },
            {
                "_id": "LambStew",
                "servings": 6,
                "subtitle": "Serve with a whole meal scone topping",
                "title": "LambStew"
            },
            {
                "servings": 8,
                "subtitle": "Hand-made dumplings make a great accompaniment",
                "title": "BeefStew"
            }
        ]
    }

The return type from a bulk insertion will be :statuscode:`201`,
with the content of the returned structure indicating specific success
or otherwise messages on a per-document basis.

The return structure from the example above contains a list of the
documents created, here with the combination and their revision IDs:

.. code-block:: http

    HTTP/1.1 201 Created
    Cache-Control: must-revalidate
    Content-Length: 215
    Content-Type: application/json
    Date: Sat, 26 Oct 2013 00:10:39 GMT
    Server: CouchDB (Erlang OTP)

    [
        {
            "id": "FishStew",
            "ok": true,
            "rev": "1-6a466d5dfda05e613ba97bd737829d67"
        },
        {
            "id": "LambStew",
            "ok": true,
            "rev": "1-648f1b989d52b8e43f05aa877092cc7c"
        },
        {
            "id": "00a271787f89c0ef2e10e88a0c0003f0",
            "ok": true,
            "rev": "1-e4602845fc4c99674f50b1d5a804fdfa"
        }
    ]

The content and structure of the returned JSON will depend on the transaction
semantics being used for the bulk update; see :ref:`api/db/bulk_docs/semantics`
for more information. Conflicts and validation errors when updating documents
in bulk must be handled separately; see :ref:`api/db/bulk_docs/validation`.

Updating Documents in Bulk
==========================

The bulk document update procedure is similar to the insertion
procedure, except that you must specify the document ID and current
revision for every document in the bulk update JSON string.

For example, you could send the following request:

.. code-block:: http

    POST /recipes/_bulk_docs HTTP/1.1
    Accept: application/json
    Content-Length: 464
    Content-Type: application/json
    Host: localhost:5984

    {
        "docs": [
            {
                "_id": "FishStew",
                "_rev": "1-6a466d5dfda05e613ba97bd737829d67",
                "servings": 4,
                "subtitle": "Delicious with freshly baked bread",
                "title": "FishStew"
            },
            {
                "_id": "LambStew",
                "_rev": "1-648f1b989d52b8e43f05aa877092cc7c",
                "servings": 6,
                "subtitle": "Serve with a whole meal scone topping",
                "title": "LambStew"
            },
            {
                "_id": "BeefStew",
                "_rev": "1-e4602845fc4c99674f50b1d5a804fdfa",
                "servings": 8,
                "subtitle": "Hand-made dumplings make a great accompaniment",
                "title": "BeefStew"
            }
        ]
    }

The return structure is the JSON of the updated documents, with the new
revision and ID information:

.. code-block:: http

    HTTP/1.1 201 Created
    Cache-Control: must-revalidate
    Content-Length: 215
    Content-Type: application/json
    Date: Sat, 26 Oct 2013 00:10:39 GMT
    Server: CouchDB (Erlang OTP)

    [
        {
            "id": "FishStew",
            "ok": true,
            "rev": "2-2bff94179917f1dec7cd7f0209066fb8"
        },
        {
            "id": "LambStew",
            "ok": true,
            "rev": "2-6a7aae7ac481aa98a2042718d09843c4"
        },
        {
            "id": "BeefStew",
            "ok": true,
            "rev": "2-9801936a42f06a16f16c30027980d96f"
        }
    ]

You can optionally delete documents during a bulk update by adding the
``_deleted`` field with a value of ``true`` to each document ID/revision
combination within the submitted JSON structure.

The return type from a bulk insertion will be :statuscode:`201`, with the
content of the returned structure indicating specific success or otherwise
messages on a per-document basis.

The content and structure of the returned JSON will depend on the transaction
semantics being used for the bulk update; see :ref:`api/db/bulk_docs/semantics`
for more information. Conflicts and validation errors when updating documents
in bulk must be handled separately; see :ref:`api/db/bulk_docs/validation`.

.. _api/db/bulk_docs/semantics:

Bulk Documents Transaction Semantics
====================================

CouchDB supports two different modes for updating (or inserting)
documents using the bulk documentation system. Each mode affects both
the state of the documents in the event of system failure, and the level
of conflict checking performed on each document. The two modes are:

-  **non-atomic**

   The default mode is `non-atomic`, that is, CouchDB will only guarantee
   that some of the documents will be saved when you send the request.
   The response will contain the list of documents successfully inserted
   or updated during the process. In the event of a crash, some of the
   documents may have been successfully saved, and some will have been
   lost.

   In this mode, the response structure will indicate whether the
   document was updated by supplying the new ``_rev`` parameter
   indicating a new document revision was created. If the update failed,
   then you will get an ``error`` of type ``conflict``. For example:

   .. code-block:: javascript

       [
           {
               "id" : "FishStew",
               "error" : "conflict",
               "reason" : "Document update conflict."
           },
           {
               "id" : "LambStew",
               "error" : "conflict",
               "reason" : "Document update conflict."
           },
           {
               "id" : "BeefStew",
               "error" : "conflict",
               "reason" : "Document update conflict."
           }
       ]

   In this case no new revision has been created and you will need to
   submit the document update, with the correct revision tag, to update
   the document.

Replication of documents is independent of the type of insert or update.
The documents and revisions created during a bulk insert or update are
replicated in the same way as any other document.

.. _api/db/bulk_docs/validation:

Bulk Document Validation and Conflict Errors
============================================

The JSON returned by the ``_bulk_docs`` operation consists of an array
of JSON structures, one for each document in the original submission.
The returned JSON structure should be examined to ensure that all of the
documents submitted in the original request were successfully added to
the database.

When a document (or document revision) is not correctly committed to the
database because of an error, you should check the ``error`` field to
determine error type and course of action. Errors will be one of the
following type:

-  **conflict**

   The document as submitted is in conflict. The new revision will not have been
   created and you will need to re-submit the document to the database.

   Conflict resolution of documents added using the bulk docs interface
   is identical to the resolution procedures used when resolving
   conflict errors during replication.

-  **forbidden**

   Entries with this error type indicate that the validation routine
   applied to the document during submission has returned an error.

   For example, if your :ref:`validation routine <vdufun>` includes
   the following:

   .. code-block:: javascript

       throw({forbidden: 'invalid recipe ingredient'});

   The error response returned will be:

   .. code-block:: http

       HTTP/1.1 417 Expectation Failed
       Cache-Control: must-revalidate
       Content-Length: 120
       Content-Type: application/json
       Date: Sat, 26 Oct 2013 00:05:17 GMT
       Server: CouchDB (Erlang OTP)

       {
           "error": "forbidden",
           "id": "LambStew",
           "reason": "invalid recipe ingredient",
           "rev": "1-34c318924a8f327223eed702ddfdc66d"
       }
