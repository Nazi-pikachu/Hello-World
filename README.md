# HELLO DJANGO

 1. Use the `django-admin` tool to create the project folder, basic file templates, and project management script (**manage.py**).
 2. Use **manage.py** to create one or more _applications_.
 3. Register the new applications to include them in the project.
 4. Hook up the url/path mapper for each application.
## Creating the catalog app

    py manage.py startapp catalog
## Registering the catalog app
open the **locallibrary/locallibrary/settings.py** and find the definition for the `INSTALLED_APPS` list. Then add a new line at the end of the list, as shown in bold below.

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
 **'catalog',** 
]
## Using Virtualenv

 -   `deactivate` — Exit out of the current Python virtual environment
 -   `workon` — List available virtual environments
 -   `workon name_of_environment` — Activate the specified Python virtual environment
 -   `rmvirtualenv name_of_environment` — Remove the specified environment.

The fields to match and the type of match are defined in the filter parameter name, using the format: `field_name__match_type`

```
wild_books = Book.objects.filter(title__contains='wild')
number_wild_books = wild_books.count()
```
There are many other types of matches you can do: `icontains` (case insensitive), `iexact` (case-insensitive exact match), `exact` (case-sensitive exact match) and `in`, `gt` (greater than), `startswith`, etc. The [full list is here](https://docs.djangoproject.com/en/2.1/ref/models/querysets/#field-lookups).
The pattern matching provided by  `path()`  is simple and useful for the (very common) cases where you just want to capture  _any_  string or integer. If you need more refined filtering (for example, to filter only strings that have a certain number of characters) then you can use the  [re_path()](https://docs.djangoproject.com/en/2.1/ref/urls/#django.urls.re_path)  method.
## RE(Regular Expression)
This method is used just like `path()`  except that it allows you to specify a pattern using a [Regular expression](https://docs.python.org/3/library/re.html). For example, the previous path could have been written as shown below:

**re_path(r'^book/(?P<pk>\d+)$', views.BookDetailView.as_view(), name='book-detail'),**

_Regular expressions_  are an incredibly powerful pattern mapping tool. They are, frankly, quite unintuitive and can be intimidating for beginners. Below is a very short primer!

The first thing to know is that regular expressions should usually be declared using the raw string literal syntax (i.e. they are enclosed as shown:  **r'<your regular expression text goes here>'**).

The main parts of the syntax you will need to know for declaring the pattern matches are:

Symbol

Meaning

^

Match the beginning of the text

$

Match the end of the text

\d

Match a digit (0, 1, 2, ... 9)

\w

Match a word character, e.g. any upper- or lower-case character in the alphabet, digit or the underscore character (_)

+

Match one or more of the preceding character. For example, to match one or more digits you would use  `\d+`. To match one or more "a" characters, you could use  `a+`

*

Match zero or more of the preceding character. For example, to match nothing or a word you could use  `\w*`

( )

Capture the part of the pattern inside the brackets. Any captured values will be passed to the view as unnamed parameters (if multiple patterns are captured, the associated parameters will be supplied in the order that the captures were declared).

(?P<_name_>...)

Capture the pattern (indicated by ...) as a named variable (in this case "name"). The captured values are passed to the view with the name specified. Your view must therefore declare an argument with the same name!

[ ]

Match against one character in the set. For example, [abc] will match on 'a' or 'b' or 'c'. [-\w] will match on the '-' character or any word character.

Most other characters can be taken literally!

Let's consider a few real examples of patterns:

Pattern

Description

**r'^book/(?P<pk>\d+)$'**

This is the RE used in our URL mapper. It matches a string that has  `book/`  at the start of the line (**^book/**), then has one or more digits (`\d+`), and then ends (with no non-digit characters before the end of line marker).

It also captures all the digits  **(?P<pk>\d+)**  and passes them to the view in a parameter named 'pk'.  **The captured values are always passed as a string!**

For example, this would match  `book/1234`  , and send a variable  `pk='1234'`  to the view.

**r'^book/(\d+)$'**

This matches the same URLs as the preceding case. The captured information would be sent as an unnamed argument to the view.

**r'^book/(?P<stub>[-\w]+)$'**

This matches a string that has  `book/`  at the start of the line (**^book/**), then has one or more characters that are  _either_  a '-' or a word character (**[-\w]+**), and then ends. It also captures this set of characters and passes them to the view in a parameter named 'stub'.

This is a fairly typical pattern for a "stub". Stubs are URL-friendly word-based primary keys for data. You might use a stub if you wanted your book URL to be more informative. For example  `/catalog/book/the-secret-garden`  rather than  `/catalog/book/33`.

You can capture multiple patterns in the one match, and hence encode lots of different information in a URL.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIxNDgyOTIyMl19
-->