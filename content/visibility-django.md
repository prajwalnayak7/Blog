---
title: "Visibility and Profiling in Django (Python3)"
date: 2021-02-24T14:46:33+05:30
menu: ""
description: "django python profiling"
tags: ["tech", "django", "python", "profiling"]
slug: "visibility-django"
---


The 'batteries included' python framework will required a few tools to get insights into the application performance and other metrics.

This article is by no means exhaustive. But, it serves as a good start and simplifies geting started.

> `ncalls` is the total number of calls that the function makes.
> `tottime` is the total time spent in the function alone. 
> `cumtime` is the total time spent in the function plus all the functions that it in-turn calls.
> `percall` is the quotient of tot/cum`time` divided by `ncalls`


---

* [cProfile](https://docs.python.org/3/library/profile.html)

A python package for analysing the function performance as well as can be used along with the django dev server.

For instance, guess which is better? (Both give the same output)

`",".join(abc)`

OR

`str(abc)[1:-1]`
```python
>>> cProfile.run('",".join(abc)')
         4 function calls in 0.000 seconds

   Ordered by: standard name

   ncalls  tottime  percall  cumtime  percall filename:lineno(function)
        1    0.000    0.000    0.000    0.000 <string>:1(<module>)
        1    0.000    0.000    0.000    0.000 {built-in method builtins.exec}
        1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler' objects}
        1    0.000    0.000    0.000    0.000 {method 'join' of 'str' objects}


>>> cProfile.run('str(abc)[1:-1]')
         3 function calls in 0.000 seconds

   Ordered by: standard name

   ncalls  tottime  percall  cumtime  percall filename:lineno(function)
        1    0.000    0.000    0.000    0.000 <string>:1(<module>)
        1    0.000    0.000    0.000    0.000 {built-in method builtins.exec}
        1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler' objects}
```

Comand to monitor along with the Django Dev server:
```bash
python -m cProfile -o profile_output manage.py runserver
cprofilev -f profile_output
```

---

* [django-extensions](https://django-extensions.readthedocs.io/en/latest/)

It a heavyweight package to be included only in dev environments. This extends django capabilities in terms of profiling, better shell, schema generator and many other missing django features one would look for.


```bash
# Run the dev server with profiling enabled.
python manage.py runprofileserver --use-cprofile --prof-path=/tmp/my-profile-data
# Generate the output
snakeviz /tmp/my-profile-data/
```

In addition to it, the package also provides many cool commands, some of them listed below:
```bash
# Dev server with cleaner logs output and faster reloads
python manage.py runserver_plus
# Shell with hot reload, automatic imports, translate ORM queries to SQL query
python manage.py shell_plus --notebook --print-sql
# Generate the Model schema of apps
python manage.py graph_models app_name -o file_name.png
python manage.py graph_models -a --disable-abstract-fields --group-models -g --arrow-shape curve --theme original -o complete_schema.png
# Generate basic admin panels
python manage.py admin_generator Generate basic admin panels
# List all the url configs
python manage.py show_urls | wc -l
# List all the models
python manage.py list_model_info --db-type
```

---

* [Silk](https://github.com/jazzband/django-silk)

In an API driven world, this tool comes handy being one of the most powerful OSS for django out there. 

SetUp in django settings file:
```python
INSTALLED_APPS = [
    ‘silk’
]
MIDDLEWARE = [
    'silk.middleware.SilkyMiddleware',
]
urlpatterns = [
    url(r'^silk/', include('silk.urls', namespace='silk')),
]
```

Dashboard visible at : `http://localhost:8000/silk/`
Be sure to proxy the request in case routing via Nginx or any loadbalancers.


Catch: 
Silk understands the application from a middleware perspective. There could be scenarios for instance where changes in the ORM layer may proxy the request to a cache server. Silk in such cases won't be able to track the actual queries that are hitting the Database as opposed to the queries that are written in the application.
Ways to verify this: print the queries in 
`venv/lib/pythonx.x/site-packages/django/db/backends/mysql/base.py`
OR 
number of queries made in the DB connection 
`from django.db import connection; print(len(connection.queries))`


---

* [django-debug-toolbar](https://github.com/jazzband/django-debug-toolbar)

Another nice tool endorsed in the Django official documentation. Provides a detailed breakup of the queries.

---


It is advisible to have a `env` flag to enable or disable the configs related the profiling tools to avoid repetitive updates and better developer experience.


> The average network latency in the United States is about 200ms. Leaving a small room for the API's to make their way before the user is turned-off. 

> Avoid network calls as much as possible, joins between large tables, unused secondary indexes, indexes with abysmal cardinality.

> Everything is a trade-off, in the world of bits and atoms.
