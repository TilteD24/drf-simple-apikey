# Django REST Framework Simple API Key 🔐

Django REST Framework Simple API Key is a fast and secure API Key authentication plugin for REST API built with [Django Rest Framework](https://www.django-rest-framework.org/).

<div>
  <a href="https://badge.fury.io/py/djangorestframework-simple-apikey">
      <img src="https://badge.fury.io/py/djangorestframework-simple-apikey.svg" alt="Version"/>
  </a>
  <a href="https://github.com/koladev32/djangorestframework-simple-apikey/actions/workflows/ci-cd.yml">
      <img src="https://github.com/koladev32/djangorestframework-simple-apikey/actions/workflows/ci-cd.yml/badge.svg" alt="Testing"/>
  </a>
</div>

For the full documentation, visit [https://djangorestframework-simple-apikey.readthedocs.io/en/latest/](https://djangorestframework-simple-apikey.readthedocs.io/en/latest/).

## Introduction

Django REST Simple Api Key is a package built upon Django, Django REST Framework, and the fernet cryptography module to generate, encrypt, and decrypt API keys. It provides fast, secure and customizable API Key authentication.

### Benefits

Why should you use this package for your API Key authentication?

* ⚡**️Fast**: We use the [fernet](https://cryptography.io/en/latest/fernet/) cryptography module to generate, encrypt, and decrypt API keys. Besides the security facade, it is blazing fast allowing you to treat requests quickly and easily.
    
* 🔐 **Secure**: Fernet guarantees that a message encrypted using it cannot be manipulated or read without the key, which we call `FERNET_KEY`. As long as you treat the fernet key at the same level you treat the Django `SECRET_KEY` setting, you are good to go.
    
* 🔧 **Customizable**: The models, authentication backend, and permissions classes can be rewritten and fit your needs. We do our best to extend Django classes and methods, so you can easily extend our classes and methods.😉 Your Api Key authentication settings are kept in a single configuration dictionary named `SIMPLE_API_KEY` in the `settings.py` file of your Django project. It can be customized to fit your project needs.
    

## Quickstart

1 - Install with `pip`:

```bash
pip install djangorestframework-simple-apikey
```

2 - Register the app in the `INSTALLED_APPS` in the `settings.py` file:

```python
# settings.py

INSTALLED_APPS = [
  # ...
  "rest_framework",
  "rest_framework_simple_api_key",
]
```

3- Add the `FERNET_KEY` setting in your `SIMPLE_API_KEY` configuration dictionary. You can easily generate a fernet key using the `python manage.py generate_fernet_key` command. Keep in mind that the fernet key plays a huge role in the api key authentication system.

```python
SIMPLE_API_KEY = {
    "FERNET_SECRET": "sVjomf7FFy351xRxDeJWFJAZaE2tG3MTuUv92TLFfOA="
}
```

4 - Run migrations:

```bash
python manage.py migrate
```

In your view then, you can add the authentication class and the permission class.

> ⚠️ **Important Note**: By default, authentication is performed using the `AUTH_USER_MODEL` specified in the settings.py file. However, you can specify a different model as the entity for authentication by configuring the `AUTHENTICATION_MODEL` in the `SIMPLE_API_KEY` setting.

```python
from rest_framework import viewsets

from rest_framework_simple_api_key.backends import APIKeyAuthentication
from rest_framework.response import Response

class FruitViewSets(viewsets.ViewSet):
    http_method_names = ["get"]
    authentication_classes = (APIKeyAuthentication, )

    def list(self, request):
        return Response([{"detail": True}], 200 )
```

## Generate a Fernet Key
We've made it easier for you by creating a custom Django command to quickly generate a fernet key, which is a **crucial component** in the authentication system. Make sure to keep the key secure and store it somewhere safely (ie: environment variable). 

**Important ⛔️** : You should treat the `FERNET_KEY` security at the same level as the Django `SECRET_KEY`. 🫡

To generate the fernet key use the following command:

```python
python manage.py generate_fernet_key
```
or 

```python
django-admin generate_fernet_key
```

## Custom attribute on the request object

By default, when using Django, you can access the entity associated with an API key using `request.user`. However, 
we understand that this may not always be convenient for your use case. Therefore, we have provided you with the 
flexibility to choose your own custom attribute name.

1 - Add the `rest_framework_simple_api_key.middleware.ApiKeyAuthenticationMiddleware` middleware in the `MIDDLEWARE` 
list in your Django settings.

```python
MIDDLEWARE = (
    # ...
    "rest_framework_simple_api_key.middleware.ApiKeyAuthenticationMiddleware"
)
```

2 - Define a value for the `custom_entity` key in the `SIMPLE_API_KEY` dictionnary.

```python
SIMPLE_API_KEY={
    # ...
    "CUSTOM_ENTITY_NAME": "organization"
}
```

That's it! With these changes in place, you can retrieve your entity using your custom attribute name on every 
authenticated API request. For example, if you set `CUSTOM_ENTITY_NAME` to `organization`, you can access the entity with 
`request.organization`. If you don't define a `CUSTOM_ENTITY_NAME`, you can still access the entity using the default 
attribute name `request.entity`.

## Changelog

See [CHANGELOG.md](https://github.com/koladev32/djangorestframework-simple-apikey/blob/main/CHANGELOG.md).

## Contributing

See [CONTRIBUTING.md](https://github.com/koladev32/djangorestframework-simple-apikey/blob/main/CONTRIBUTING.md).