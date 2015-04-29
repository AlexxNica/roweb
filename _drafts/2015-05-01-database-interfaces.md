---
name: database-interfaces
layout: post
title: Database interfaces
date: 2015-05-01
authors:
  - name: Scott Chamberlain
tags:
- R
- database
- key-value
---



There are a lot of different databases. The most familiar may be the row-column SQL databases lie MySQL, SQLite, PostgreSQL, etc. Another type of database is the key-value store, which is very simple: you save a value specified by a key, and you retrieve a value by its key. One more type is the NoSQL database, or document database, which instead of storing rows and columns, stores blobs of text or even binary files. SQL clients for R have recieved a thorough treatment already, so we don't delve into those. But we are building a number of R clients for key-value stores and document stores.

Current rOpenSci database tools

* [elastic](https://github.com/ropensci/elastic) (document database) (on CRAN)
* [sofa](https://github.com/ropensci/sofa) (document database)
* [solr](https://github.com/ropensci/solr) (document database) (on CRAN)
* [etseed](https://github.com/ropensci/etseed) (key-value store)
* [rrlite](https://github.com/ropensci/rrlite) (key-value store)
* [rerddap](https://github.com/ropensci/rerddap) (SQL database as a service, open source)
* [ckanr](https://github.com/ropensci/ckanr) (SQL database as a service, open source)
* [docdbi](https://github.com/ropensci/docdbi)

If you aren't already using databases, why care about databases? We can answer this through a number of use cases. 

* Use case 1: Let's say you are producing a lot of data in your lab, like millions and millions of rows of data. Storing all this data in xls or csv files would definitely get cumbersome. If the data is traditional row-column spreadsheet data, a SQL database is perfect, perhaps PostgreSQL. Putting your data in a database allows the data to scale up easily while maintaining speedy data access, many tables can be linked together if needed, and more.
* Use case 2: A data provider gives dumps of data that you need for your research. You download the data and it's hundreds of csv files. It sure would be nice to be able to efficiently search this data. Simple searches like _return all records with variable X < 10_ are ideal for SQL database, but if you need more complicated searches, and/or if your data is unstructured text data, a document database is ideal. 
* Use case 3: Sometimes you just need to cache. Caching is a good use case for key-value stores. Let's say you are requesting data from a database online, and you want to make a derivative data thing from the original data, but you don't want to lose the original data. Simply storing the original data on disk in files is easy and does the job. Sometimes though, you may need something more structured. Redis and etcd are two key-value stores we make clients for and can make caching easy. 

### Get devtools

We'll need `devtools` to install some of these packages, as not all are on CRAN. If you are on Windows, see [these notes](https://github.com/hadley/devtools#updating-to-the-latest-version-of-devtools).


```r
install.packages("devtools")
```

### elastic

[elastic](https://github.com/ropensci/elastic) - Interact with Elasticsearch.


```r
install.packages("elastic")
```


```r
library("elastic")
```

`elastic` is a powerful document database with a built in query engine. It speaks JSON by default, has a nice HTTP API, which we use to communicate with `elastic` from R. What's great about `elastic` over e.g., `Solr` is that you don't have to worry about specifying a schema for your data. You can simply put data in, and then query on that data. Of course you can [specify configuration settings](http://www.elastic.co/guide/en/elasticsearch/reference/current/setup-configuration.html).

In a quick example, here's going from a data.frame in R, putting data into `elastic`, then querying on that data.


```r
"adfdfs"
#> [1] "adfdfs"
```

We have a package in developmented called [elasticdsl](https://github.com/ropensci/elasticdsl) that follows the lead of the Python client [elasticsearch-dsl-py](https://github.com/elastic/elasticsearch-dsl-py) to allow native R based ways to specify queries. The package focuses on querying for data, whereas other operations will remain in the lower level `elastic` client.

### sofa

[sofa](https://github.com/ropensci/sofa) - Interact with CouchDB.


```r
devtools::install_github("ropensci/sofa")
```


```r
library("sofa")
```

A quick example:

Create a database


```r
db_create(dbname = 'sofadb')
```

Create a document in that database


```r
doc_create('{"name":"sofa","beer":"IPA"}', dbname = "sofadb", docid = "a_beer")
```

Get the document


```r
doc_get(dbname = "sofadb", docid = "a_beer")
```

### solr

[solr](https://github.com/ropensci/solr) - A client for interacting with Solr.

> A note about the solr client. It focuses on reading data from Solr engines. We are working on adding functionality for working with more Solr features, including writing documents.


```r
install.packages("solr")
```


```r
library("solr")
```

A quick example:


```r
solr_search(q = '*:*', rows = 2, fl = 'id', base = 'http://api.plos.org/search')
```

### etseed

[etseed](https://github.com/ropensci/etseed) - A client for the [etcd](https://github.com/coreos/etcd) key-value store, developed by the folks at [coreos](https://coreos.com/), written in [Go](https://golang.org/).


```r
devtools::install_github("ropensci/etseed")
```


```r
library("etseed")
```

A quick example:



### rrlite

[rrlite](https://github.com/ropensci/rrlite) - An R client for the Redis C library [stuff](#)


```r
devtools::install_github("ropensci/rrlite")
```


```r
library("rrlite")
'xxx'
#> [1] "xxx"
```

### rerddap

[rerddap](https://github.com/ropensci/rerddap) - A general purpose R client for any ERDDAP server.


```r
devtools::install_github("ropensci/rerddap")
```


```r
library("rerddap")
'xxx'
#> [1] "xxx"
```

### ckanr

[ckanr](https://github.com/ropensci/ckanr) - A general purpose R client for any CKAN server.


```r
devtools::install_github("ropensci/ckanr")
```


```r
library("ckanr")
'xxx'
#> [1] "xxx"
```

### docdbi

[docdbi](https://github.com/ropensci/docdbi) - Like the DBI package, but for document and key-value databases.


```r
devtools::install_github("ropensci/docdbi")
```


```r
library("docdbi")
'xxx'
#> [1] "xxx"
```