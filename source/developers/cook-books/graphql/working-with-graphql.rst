:is-up-to-date: True
:last-updated: 4.0.0

.. index:: Working with GraphQL

.. _working_with_graphql:

====================
Working with GraphQL
====================

CrafterCMS provides built-in support for GraphQL to query content in any project without writing additional code.
A GraphQL schema is generated independently for each project based on the content-type configuration that has been
created using Crafter Studio, and the schema is automatically updated after any change is detected.

To implement a project that uses GraphQL you would follow a workflow like this:

1. Create a new project (if needed, for existing projects skip to step number 3)
2. Define the content model for your project
3. Obtain the GraphQL schema for your project, you can use the provided GraphiQL client or any third party client
4. Develop GraphQL queries to use in your project or external app

All content changes made by authors in Crafter Studio will be immediately available in GraphQL queries.

When a change is made in the content model, for example adding a new field or creating a new content-type, the
GraphQL schema will be rebuilt to reflect the same changes. So for a CrafterCMS project that uses GraphQL queries the
development process would look like this:

1. Developers define the base content model
2. Developers define the project base GraphQL queries to use the latest schema
3. Content authors create content based on the model
4. Publishers review & approve the author's work
5. Publishers publish to live both the content model configuration & the content updates
6. Crafter Deployer will handle the GraphQL schema rebuild in delivery

You can also use the CrafterCMS GraphQL API from an external project or application, however in this case you will need to
handle the schema reload using third party tools.

-------------------------------
Using GraphQL in Crafter Studio
-------------------------------

GraphiQL is a simple GraphQL client that you can use in Crafter Studio to run GraphQL queries and explore the schema 
documentation for a project without the need of any other tool. To access GraphiQL follow these steps:

1. Login to Crafter Studio
2. Click on the CrafterCMS logo to toggle the sidebar open
3. Click ``Project Tools`` in the left sidebar
4. Click ``GraphQL`` in the left sidebar

To explore the GraphQL schema you can click the ``Docs`` icon on the right side:

.. image:: /_static/images/developer/graphql/graphql.png
        :width: 75%
        :alt: GraphQL
        :align: center

|

GraphiQL provides a convenient search navigation to quickly find a specific type or field:

.. image:: /_static/images/developer/graphql/graphiql-doc.png
        :width: 75%
        :alt: GraphiQL Schema Documentation Explorer
        :align: center

|

To test GraphQL queries type them in the left text editor, GraphiQL will provide suggestions and validate the query
against the schema in real time.

.. image:: /_static/images/developer/graphql/graphiql-query.png
        :width: 75%
        :alt: GraphiQL Query Editor
        :align: center

|

.. note::
    If the GraphQL server host name used is not ``localhost``, the ``<graphql-server-url />`` in your proxy configuration file needs to be set to the appropriate url.  For more information on the proxy configuration file, see: :ref:`proxy-configuration`

----------------
GraphQL Examples
----------------

Here you can find some examples on how to query content using GraphQL. The following examples use the built-in 
``Website Editorial`` blueprint but the same concepts apply to any CrafterCMS project.

For each content-type in the project you will find a field in the root Query, the name of the field is based on the
name of the content-type so for ``/page/article`` the field will be ``page_article``.
These fields contain two sub-fields, one is the ``total`` number of items found by the query and the other is a list
of ``items``.

.. note::
  Because GraphQL only supports the underscore ``_`` character besides alphanumeric for names, if your content-type or 
  field name contains the dash ``-`` character it will be replaced with a double underscore ``__``. To avoid 
  unnecessary long names it is suggested to use only ``_`` or ``camelCase`` notation if possible.

One of simplest GraphQL queries you can run in CrafterCMS projects is to find all items of a given content-type.

.. code-block:: text
  :linenos:
  :caption: Query for all ``/page/article`` items

  # root query
  {
    # query for content-type '/page/article'
    page_article {
      total # total number of items found
      items { # list of items found
        # content-type fields that will be returned 
        # (names are based on the content-type configuration)
        title_t
        author_s
        date_dt
      }
    }
  }

You can also run queries to find all pages, components or content items (both pages and components).

.. code-block:: text
  :linenos:
  :caption: Query for all pages

  # root query
  {
    # query for all pages
    pages {
      total # total number of items found
      items { # list of items found
        # the page fields that will be returned
        content__type
        localId
        createdDate_dt
        lastModifiedDate_dt
        placeInNav
        orderDefault_f
        navLabel
      }
    }
  }

.. code-block:: text
  :linenos:
  :caption: Query for all components

  # root query
  {
    # query for all pages
    components {
      total # total number of items found
      items { # list of items found
        # the component fields that will be returned
        content__type
        localId
        createdDate_dt
        lastModifiedDate_dt
      }
    }
  }

.. code-block:: text
  :linenos:
  :caption: Query for all content items

  # root query
  {
    # query for all pages
    contentItems {
      total # total number of items found
      items { # list of items found
        # the content item fields that will be returned
        content__type
        localId
        createdDate_dt
        lastModifiedDate_dt
      }
    }
  }

As you can expect if there are too many items for a given query the result will be too large, so you can also 
implement pagination using the ``offset`` and ``limit`` parameters. For example the following query
will return only the first five items found.

.. code-block:: text
  :linenos:
  :caption: Paginated query for content-type ``/page/article``

  # root query
  {
    # query for content-type '/page/article'
    page_article(offset: 0, limit: 5) {
      total # total number of items found
      items { # list of items found
        # content-type fields that will be returned 
        # (names are based on the content-type configuration)
        title_t
        author_s
        date_dt
      }
    }
  }

By default all items will be sorted using the ``lastModifiedDate_dt`` in descending order, you can change it by using
the ``sortBy`` and ``sortOrder`` parameters. For example you can use the ``date_dt`` field that is specific for the 
``/page/article`` content-type to sort.

.. code-block:: text
  :linenos:
  :caption: Paginated and sorted query for content-type ``/page/article``

  # root query
  {
    # query for content-type '/page/article'
    page_article (offset: 0, limit: 5, sortBy: "date_dt", sortOrder: ASC) {
      total # total number of items found
      items { # list of items found
        # content-type fields that will be returned 
        # (names are based on the content-type configuration)
        title_t
        author_s
        date_dt
      }
    }
  }

Besides finding all items for a specific content-type, it is also possible to filter the results using one or more 
fields in the query. Fields will have different filters depending on their type, for example you can find items for
a specific author.

.. code-block:: text
  :linenos:
  :caption: Paginated, sorted and filtered query for content-type ``/page/article``

  # root query
  {
    # query for content-type '/page/article'
    page_article (offset: 0, limit: 5, sortBy: "date_dt", sortOrder: ASC) {
      total # total number of items found
      items { # list of items found
        # content-type fields that will be returned 
        # (names are based on the content-type configuration)
        title_t
        # only return articles from this author
        author_s (filter: { equals: "Jane Doe" })
        date_dt
      }
    }
  }

Additionally you can create complex filters using expressions like ``and``, ``or`` and ``not`` for any field:

.. code-block:: text
  :linenos:
  :caption: Filtered query with complex conditions

  # Root query
  {
    page_article {
      total
      items {
        title_t
        author_s
        date_dt
        # Filter articles that are not featured
        featured_b (
          filter: {
            not: [
              {
                equals: true
              }
            ]
          }
        )
        # Filter articles from category style or health
        categories_o {
          item {
            key (
              filter: {
                or: [
                  {
                    matches: "style"
                  },
                  {
                    matches: "health"
                  }
                ]
              }
            )
            value_smv
          }
        }
      }
    }
  }

You can also include fields from child components in your model, this applies to fields like ``node-selector``,
``checkbox-group`` and ``repeat`` groups. Filters can also be added to fields from child components.

.. code-block:: text
  :linenos:
  :caption: Paginated, sorted and filtered query for content-type ``/page/article`` using child components

  # root query
  {
    # query for content-type '/page/article'
    page_article (offset: 0, limit: 5, sortBy: "date_dt", sortOrder: ASC) {
      total # total number of items found
      items { # list of items found
        # content-type fields that will be returned 
        # (names are based on the content-type configuration)
        title_t
        # only return articles from this author
        author_s (filter: { equals: "Jane Doe" })
        date_dt
        categories_o {
          item {
            # only return articles from this category
            key (filter: { matches: "health" }) 
            value_smv
          }
        }
      }
    }
  }

GraphQL ``aliases`` are supported on root level query fields (``contentItems``, ``pages``, ``components`` and content 
type fields).

.. code-block:: text
   :linenos:
   :caption: Query for 2020 and 2021 articles using aliases

   # root query
   {
     # query for 2020 articles
     articlesOf2020: page_article {
       items {
         localId(filter: {regex: ".*2020.*"})
       }
     },
     # query for 2021 articles
     articlesOf2021: page_article {
       items {
         localId(filter: {regex: ".*2021.*"})
       }
     }  
   }

GraphQL ``fragments`` are fully supported and can be used inline or as spreads. Using fragments you can simplify
queries by extracting repeated fields or request specific fields for different content-types in as single query:

.. code-block:: text
  :linenos:
  :caption: Using fragment spreads to simplify a query

  # Fragment definition
  fragment CommonFields on ContentItem {
    localId
    createdDate_dt
  }

  # Root query
  query {
    page_article {
      total
      items {
        # Fragment spread
        ... CommonFields
        title_t
        author_s
      }
    }
    
    component_feature {
      total
      items {
        # Fragment spread
        ... CommonFields
        title_t
        icon_s
      }
    }
  }

.. code-block:: text
  :linenos:
  :caption: Using inline fragments to request specific fields in a single query

  # Root query
  {
    contentItems {
      total
      items {
        # Query for fields from the interface
        localId
        createdDate_dt
        
        # Query for fields from specific types
        ... on page_article {
          title_t
          author_s
        }
        
        ... on component_feature {
          title_t
          icon_s
        }
      }
    }
  }


For more detailed information about GraphQL you can read the `official documentation <https://graphql.org/>`_.
