# Summary

This cheatsheet provides a handy reference guide for writing queries using **Dataview Query Language** (**DQL**) in the [dataview](https://github.com/blacksmithgu/obsidian-dataview) plugin for [Obsidian.md](https://obsidian.md) note-taking app.

# How this repository came about

As I found myself frequently querying data within Obsidian, I created this cheatsheet to have quick access to the most common queries I use regularly. By adding this cheatsheet locally in Obsidian, I no longer need to leave Obsidian app and search the query syntax online.

While this cheatsheet is primarily for my own use, I'm sharing it here in case others find it useful as well. I would suggest you copy this readme and paste it in your Obsidian vault so you can search it locally just like I do.

# Query Cheatsheet

Examples for very specific queries.

# LIST

### Simple List

```js
LIST FROM <tag-name>
```

Example

```js
LIST
FROM
#games
```

### Table

```js
TABLE
	Title
FROM
	#tagName
```

# Data Commands

- FROM
- WHERE
- SORT (to do)
- GROUP BY (to do)
- FLATTEN
- LIMIT

## FROM

Selecting from different sources such as;

### Tags

`FROM #tag`

Example

```js
TABLE
	file.cday as "Created Date"
FROM
	#my-tag
```

### Folders

`FROM "folder-name"`

Example

```js
TABLE
	file.cday as "Created Date"
FROM
	"my-folder-name"
```

### Single Files

`FROM "path/to/file-name"`

Example

```js
TABLE
	file.cday as "Created Date"
FROM
	"TopFolder/SubFolder/my-file-name"
```

## WHERE

Examples of queries containing WHERE clause.

### WHERE property is NOT empty

```js
WHERE <property-name>
```

Example

```sql
TABLE
	file.cday as "Created",
	Category
FROM
	#books
SORT
	file.cday
WHERE
	Category
```

The above example ensures to show only results where the meta-data 'Category' is not empty.

## FLATTEN

### Multiple properties displayed in its own row

```js
FLATTEN <property-name>
```

Code example:

```js
TABLE
	Title,
	Action
FLATTEN Action
```

Result example:

| File Name | Created | Action        |
| --------- | ------- | ------------- |
| Note 1    | July    | Action name 1 |
| Note 1    | July    | Action name 2 |
| Note 2    | August  | My Action 123 |
| Note 2    | August  | Hello World   |

# Bool property to custom display value

## Display Yes/No instead of True/False on bool properties

Snippet

```js
CHOICE(<bool-property>, "Yes", "No") as "custom-name"
```

Example

```sql
TABLE
	Author as "Author",
	choice(read, "Yes", "No") as "Read",
FROM
	"Books"
```

# Limit results in query

```js
LIMIT 10
```

Example:

```js
TABLE
	Title,
	Rating
WHERE
	Rating > 3
LIMIT 10
```

# Meta Data Examples

Obsidian allows YAML and JSON for metadata.

### JSON

JSON

```
{
	"Author": "Author Name",
	"Genre": "Fiction",
	"DateRead": "2022-06-01",
	"Read": false,
	"Tags": [
		"Mind-blowing",
		"Interesting",
		"Science"
	]
}
```

### YAML

YAML

```
Author: Author Name
Genre: Fiction
DateRead: '2022-06-01'
Read: false
Tags:
- Mind-blowing
- Interesting
- Science
```
