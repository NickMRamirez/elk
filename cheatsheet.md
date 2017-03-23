# Elasticsearch Cheatsheet

Types of queries you can do.

## Create an index called "website"

```
POST /website
```

## Get a list of indices

```
GET /_cat/indices?v
```

## Delete an index

```
DELETE /website
```

## Add a document of type "blog", specifying an ID of "1"

```
PUT /website/blogs/1
{
  "title": "First entry",
  "content": "My first entry"
}
```

## Add a document of type "blog", specifying an ID of "1", but only if it does not already exist

```
PUT /website/blogs/1/_create
{
  "title": "First entry",
  "content": "My first entry"
}
```

## Add a document of type "blog", but let Elasticsearch autogenerate the ID

```
POST /website/blogs
{
    "title": "Second entry",
    "content": "My second entry"
}
```

## Get a document by its ID

```
GET /website/blogs/1
```

## Get a document by ID, but only return the "title" field

```
GET /website/blogs/1?_source=title
```
