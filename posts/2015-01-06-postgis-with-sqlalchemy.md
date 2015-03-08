---
layout: post
title: PostGIS with SQLAlchemy
date: 2015-01-06 15:08
comments: true
categories: 
author: Paul Meng
---

Postgres is often said as the open-source world Oracle DB. It is shipped with different kind of index type. The default is B-tree, the others are R-Tree and GIST and GIN respectively.  [PostGIS](http://postgis.net/docs/manual-2.1/) is built for indexing geometry objects, and it is built on Postgres’s GIST type index. On AWS RDS you could also switch it on by following the [instructions](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.PostgreSQL.CommonDBATasks.html#Appendix.PostgreSQL.CommonDBATasks.PostGIS)

To take the most of it and build fast prototypes, we still need ORM abstraction for PostGIS extension. The popular Sqlalchemy ORM in python world comes with geoalchemy extension. It provides functions from PostGIS.

To use it, you have to import it into your model file
```
from geoalchemy2 import Geometry
```

And define the column with ``Geometry`` class
```
geom = Column(Geometry(‘POINT’), nullable=False)
```

And insert to the model by assigning it a ``WKTElement`` with correct srid
```
s = ‘POINT(%s %s)’ %(geocode[“latitude”], geocode[“longitude”])
l.geom = WKTElement(s, srid=4326)
```

Then you could do the common operation using [spatial functions](http://geoalchemy-2.readthedocs.org/en/latest/spatial_functions.html)

```
from geoalchemy2 import functions
```
