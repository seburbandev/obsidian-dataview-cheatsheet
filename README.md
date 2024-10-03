# Summary

This cheatsheet provides a handy reference guide for writing queries using **Dataview Query Language** (**DQL**) in the [dataview](https://github.com/blacksmithgu/obsidian-dataview) plugin for [Obsidian.md](https://obsidian.md) note-taking app.

# How to use this

Use it however you like but I would suggest you copy this file and simply paste it into your own Obsidian vault. You can then reference this from within your own vault by either accessing the file or searching in your vault for specific command.

 Star and follow this repository if you want to be updated when I add more examples to this list.

# Table of Contents

- [LIST](#list)
  - [Simple List](#simple-list)
  - [Table](#table)
- [Data Commands](#data-commands)
  - [FROM](#from)
    - [Tags](#tags)
    - [Excluding notes with a specific tag](#excluding-notes-with-a-specific-tag)
    - [Excluding notes from a specific folder](#excluding-notes-from-a-specific-folder)
    - [Folders](#folders)
    - [Single Files](#single-files)
  - [WHERE](#where)
    - [WHERE property is NOT empty](#where-property-is-not-empty)
    - [WHERE property is equal to something](#where-property-is-equal-to-something)
  - [GROUP BY](#group-by)
    - [GROUP BY category](#group-by-category)
  - [FLATTEN](#flatten)
    - [Multiple properties displayed in its own row](#multiple-properties-displayed-in-its-own-row)
  - [Bool property to custom display value](#bool-property-to-custom-display-value)
    - [Display Yes/No instead of True/False on bool properties](#display-yesno-instead-of-truefalse-on-bool-properties)
  - [Limit results in query](#limit-results-in-query)
- [Meta Data Examples](#meta-data-examples)
  - [JSON](#json)
  - [YAML](#yaml)

# Query Cheatsheet

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

## Tags

`FROM #tag`

Example

```js
TABLE
  file.cday as "Created Date"
FROM
  #my-tag
```

## Excluding notes with a specific tag

`!#tag-name`

Example

```js
TABLE
  Title,
  Rating,
  Seen,
  SeenDate as "Seen on"
FROM
  #movie AND !#template
```

The above example will return all notes with a tag `#movie` but exclude notes with a tag `#template`. This is handy if you have a note with pre-populated tags but it's only used as a template so you don't want to see it in your table view.

## Excluding notes from a specific folder

`FROM #tag AND !"FolderName"`

Example

```js
TABLE
  Title,
  Rating,
  Seen,
  SeenDate as "Seen on"
FROM
  #movie AND !"TemplatesFolder"
```

By including `!"FolderName"` we specify that we do not want to return any matches if the are located in the specified folder.

## Folders

`FROM "folder-name"`

Example

```js
TABLE
  file.cday as "Created Date"
FROM
  "my-folder-name"
```

## Single Files

`FROM "path/to/file-name"`

Example

```js
TABLE
  file.cday as "Created Date"
FROM
  "TopFolder/SubFolder/my-file-name"
```

## GROUP BY

### GROUP BY category

```js
GROUP BY <property-name>
```

Examples

```sql
TABLE 
rows.file.name as "File"
WHERE category
GROUP BY  category
```

```sql
LIST
rows.file.name
WHERE
category = "first-category"
GROUP BY category
```

NOTE: When using group by, the structure of the results changes. Instead of directly accessing `file.name`, you must use the `rows` property to access the file properties within each group. This is because results are now grouped into rows based on the group by field.

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

### WHERE property is equal to something

```js
WHERE <string-property-name> = "my-value"
```

```js
WHERE <digit-property-name> = 123
```

Examples

```sql
LIST
WHERE
Category = "my-value"
```

```sql
LIST
WHERE
DigitProperty = 123
```

# FLATTEN

## Multiple properties displayed in its own row

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

## JSON

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

## YAML

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