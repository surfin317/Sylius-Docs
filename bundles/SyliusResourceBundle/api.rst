Building a API
==============

Header parameter parser
-----------------------

The header parameter parser will find the version and the validation group in the HTTP headers to give them to the `JmsSerializerBundle`

.. code-block:: bash

    Accept: application/json; version=1.0.1; groups=Sylius,Details

The previous header will be translated to :

.. code-block:: php

    $handler = $this->get('fos_rest.view_handler');
    $handler->setExclusionStrategyVersion('1.0.1');
    $handler->setExclusionStrategyGroups(array('Sylius', 'Details'));

In serializer configuration files, you can filter the data that you want to send by using the field called ``since_version``, ``until_version`` and ``group``.
The following example show you how it works. The ``lastname`` won't be return after the version ``0.3`` and the ``firstname`` will be returned after the version ``0.2``.
You can play with groups to return a part of data depending on specifics cases like the symfony form.

.. code-block:: php

    # src/App/Bundle/YourBundle/Resources/serializer/Entity.User.yml

    App\Bundle\YourBundle\Entity\User:
        exclusion_policy: ALL
        properties:
            id:
                expose: true
                type: integer
                groups: [Sylius, Details]
            firstName:
                expose: true
                type: string
                since_version: 0.2
                groups: [Sylius]
            lastName:
                expose: true
                type: string
                until_version: 0.3
                groups: [Details]

More details about serializer can be found in its `official documentation  <http://jmsyst.com/libs/serializer>`_.

Api Loader
----------

Routes for your API will be automatically generated by ApiLoader class. The ``resource`` is the concatenation of the application and resource name.

.. code-block:: yml

    api_resource:
        resource: my_api.resource
        type: sylius.api

It will generate the following routes :

.. code-block:: bash

    my_app_api_resource_index            GET       ANY    ANY  /resources/
    my_app_api_resource_show             GET       ANY    ANY  /resources/{id}
    my_app_api_resource_create           POST      ANY    ANY  /resources/
    my_app_api_resource_update           PUT|PATCH ANY    ANY  /resources/{id}
    my_app_api_resource_delete           DELETE    ANY    ANY  /resources/{id}