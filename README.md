# Summary

[![Trajecta](https://img.shields.io/badge/Sponsored%20by-Trajecta-blue?style=flat-square)](https://trajecta.app)

This cheatsheet provides a handy reference guide for writing queries using 
**Dataview Query Language** (**DQL**) in the [dataview][dataview] plugin for 
[Obsidian.md][obsidian] note-taking app.

# How to use this

I recommend copying this file and including it in your Obsidian vault for easy 
reference. In this way, you can access the cheatsheet by pulling up the file 
or searching in your vault for a specific command.

Star and fork this repository to see updates and pull them when more examples 
are added or the list of commands is expanded.

# Table of Contents

- [Query Commands](#query-cheatsheet)
  - [LIST](#list)
    - [Simple List](#simple-list)
	- [Table](#table)
- [Data Commands](#data-commands)
	- [FROM](#from)
		- [Tags](#tags)
		- [Single Files](#single-files)
		- [Folders](#folders)
		- [Excluding Notes](#excluding-notes)
			- [Excluding notes with a specific tag](#excluding-notes-with-a-specific-tag)
			- [Excluding notes from a specific folder](#excluding-notes-from-a-specific-folder)
		- [Chaining Resources](#chaining-resources)
			- [AND operator](#and)
			- [OR operator](#or)
	- [WHERE](#where)
		- [WHERE property is NOT empty](#where-property-is-not-empty)
		- [WHERE property is equal to something](#where-property-is-equal-to-something)
	- [SORT](#sort)
	- [GROUP BY](#group-by)
		- [GROUP BY category](#group-by-category)
	- [FLATTEN](#flatten)
		- [Multiple properties displayed in its own row](#multiple-properties-displayed-in-its-own-row)
	- [Limit results in query](#limit-results-in-query)
	- [Extras](#extras)
		- [Bool property to custom display value](#bool-property-to-custom-display-value)
- [Metadata Reference](#metadata-reference)
	- [JSON](#json)
	- [YAML](#yaml)

# Query Cheatsheet

## LIST

### Simple List

```sql
LIST
FROM
  <tag-name>
```

Example

```sql
LIST
FROM
  #library
```

## Table

```sql
TABLE
  Title
  Author
FROM
  #library
```

[Back to Contents](#table-of-contents)

# Data Commands

- [FROM](#from)
- [WHERE](#where)
- [SORT](#sort)
- [GROUP BY](#group-by)
- [FLATTEN](#flatten)
- [LIMIT](#limit)

## FROM
Selecting from different sources such as;

### Tags

`FROM #tag`

Example

```sql
TABLE
  file.cday as "Created Date"
FROM
  #my-tag
```
### Single Files

`FROM "path/to/file-name"`

Example

```sql
TABLE
  file.cday as "Created Date"
FROM
  "TopFolder/SubFolder/my-file-name"
```
### Folders

`FROM "folder-name"`

Example

```sql
TABLE
  file.cday as "Created Date"
FROM
  "my-folder-name"
```

### Excluding Notes

#### Excluding notes with a specific tag

`!#tag-name`

Example

```sql
TABLE
  Title,
  Rating,
  Seen,
  SeenDate as "Seen on"
FROM
  #movie AND !#template
```

The above example will return all notes with a tag `#movie` but exclude notes 
with a tag `#template`. This is handy if you have a note with pre-populated 
tags but it's only used as a template so you don't want to see it in your 
table view.

#### Excluding notes from a specific folder

`FROM #tag AND !"FolderName"`

Example

```sql
TABLE
  Title,
  Rating,
  Seen,
  SeenDate as "Seen on"
FROM
  #movie AND !"TemplatesFolder"
```

By including `!"FolderName"` we specify that we do not want to return any 
matches if the are located in the specified folder.

### Chaining Resources

You can fine tune query parameters utilizing the `AND` and `OR` operators

#### AND
The `AND` operator queries notes that meet all criteria included in the query:

```sql
TABLE
  Title,
  Author,
  Publication
FROM
  "Books" AND "Magazines"
```

Using in conjunction with exclusion:

```sql
TABLE
  Title,
  Author,
  Publication
FROM
  "Books" AND "Books/assets"
```

#### OR
The `OR` operator queries notes that meet any of the provided criteria:

```sql
TABLE
  Title,
  Author,
  Publication
FROM
  #horror OR #comedy
```

Using in conjunction with exclusion:

```sql
TABLE
  Title
  Author
  Publication
FROM
  #horror OR #comedy AND !"Books/assets"
```

[Back to Contents](#table-of-contents)

## WHERE
Examples of queries containing WHERE clause.

### WHERE property is NOT empty

```sql
WHERE
  <property-name>
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

```sql
WHERE
  <string-property-name> = "my-value"
```

```sql
WHERE
  <digit-property-name> = 123
```

Examples

```sql
LIST
WHERE
  category = "my-value"
```

```sql
LIST
WHERE
  digitProperty = 123
```

[Back to Contents](#table-of-contents)

## SORT
Dataview offers simple ways to sort results. The most simplistic is by some 
property in ascending (asc) or descending (desc) order:

```sql
TABLE
  Title,
  Author,
  Published,
  Year
FROM
  #library
SORT
  Year asc
```

This should serve well for most use-cases. More complex sorting mechanisms 
will added here at a later time.

[Back to Contents](#table-of-contents)

## GROUP BY
The simplest method is to group by some property included in your frontmatter:

```sql
GROUP BY
  <property-name>
```

Group by category in a table:

```sql
TABLE 
  rows.file.name as "File"
WHERE
  category
GROUP BY
  category
```

Group by category in a list:

```sql
LIST
  rows.file.name
WHERE
  category = "first-category"
GROUP BY
  category
```

NOTE: When using `GROUP BY`, the structure of the results changes. Instead of 
directly accessing `file.name`, you must use the `rows` property to access the 
file properties within each group. This is because results are now grouped 
into rows based on the `GROUP BY` field.

[Back to Contents](#table-of-contents)

## FLATTEN
Use `FLATTEN` to display multiple properties in a single row

```sql
FLATTEN
  <property-name>
```

Code example:

```sql
TABLE
  Title,
  Action
FLATTEN
  Action
```

Result example:

| File Name | Created | Action        |
| --------- | ------- | ------------- |
| Note 1    | July    | Action name 1 |
| Note 1    | July    | Action name 2 |
| Note 2    | August  | My Action 123 |
| Note 2    | August  | Hello World   |

[Back to Contents](#table-of-contents)

## LIMIT
You can limit the results in a query:

```sql
LIMIT
  <numerical-value>
```

Example:

```sql
TABLE
  Title,
  Rating
WHERE
  Rating > 3
LIMIT
  10
```

## Extras

### Bool property to custom display value
Dataview provides options on how to display various forms of data. For 
example, Booleans can be displayed as Yes/No instead of True/False:

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

[Back to Contents](#table-of-contents)

# Metadata Reference

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

<!-- links -->
[obsidian]: https://obsidian.md
[dataview]: https://github.com/blacksmithgu/obsidian-dataview
[trajecta]: https://trajecta.app

---

