# Elasticsearch Cheatsheet

Types of queries you can do.

## Create an index called "blog"

```
POST /blog
```

## Get a list of indices

```
GET /_cat/indices?v
```

## Delete an index

```
DELETE /blog
```

## Add a document of type "article", specifying an ID of "1"

```
PUT /blog/article/1
{
  "title": "First entry",
  "content": "My first entry"
}
```

## Add a document of type "article", specifying an ID of "1", but only if it does not already exist

```
PUT /blog/article/1/_create
{
  "title": "First entry",
  "content": "My first entry"
}
```

## Add a document of type "article", but let Elasticsearch autogenerate the ID

```
POST /blog/article
{
    "title": "Second entry",
    "content": "My second entry"
}
```

## Get a document by its ID

```
GET /blog/article/1
```

## Get a document by ID, but only return the "title" field

```
GET /blog/article/1?_source=title
```

## Delete a document by ID

```
DELETE /blog/article/1
```

## Return all documents in an index

```
GET /blog/_search
```

## Get a document by its version so that you can know whether you're trying to update the lastest version

```
GET /blog/article/1?version=1
```