- some way to quickly find things
- Good Search should be: Fast / Accurate / Forgiving

## Types of Indexes

- Balanced tree: default in postgres, equality and range
- Hash indexes: only for equality, rarely used
- Generalized Inverted Indexes (GIN): useful when an index must map many values to one row
- Generalized Search Tree (GiST) Indexes: index the geometric data types + full-text seatch. ideal for smaller sets of records

## Trouble with Indexes

- no indexing at all
	- “Indexes add overhead and slow down inserts and updates” 
- Index shotgun 
	- “Index every field to make the database superfast to query” 
- Finding the middle ground

## Step 1: Creating Indexes

- Index helps you to search for information quickly without looking through every single entry. 

Using migrations
```
class AddIndexToBooksTitle < ActiveRecord::Migration[6.0]
  def change
    add_index :books, :title
  end
end
```
then `rails db:migrate`

Examples:
```
CREATE INDEX defects_sources_idx ON defects(source_id); 
CREATE INDEX users_names_idx ON users(last_name, first_name); 
```
- Creating index can take time depending on the number of records 
- Creating indexes adds time to insert and update commands, but saves time on selects 
- Query optimizer will determine if index exists that will help speed up query

### Partial Indexes

- Covers just a subset of the table’s data 
- Essentially a index with a where clause 

Example: 
```
CREATE INDEX defects_faculty_idx 
ON defects(reporter_id) 
WHERE reporter_id = 1 or reporter_id = 2
```

### Expression indexes 

- Can create an index which uses a function 
Eg.
- `CREATE INDEX users_lastname_idx ON users(lower(last_name)); `
- `CREATE INDEX articles_day ON articles(date(published_at)); `
can be used by a query containing WHERE date(articles.created_at) = date('2011-03-07')

### What to index 

- Unique fields: To ensure unique values and speed up their search.
- Foreign keys: To make joins quicker.
- Common queries: To enhance performance for frequent searches.
- Fields used together: To optimize searches that filter by multiple columns.
## Step 2: Using Searches in App

```
class BooksController < ApplicationController
  def search
    @books = Book.where("title LIKE ?", "%#{params[:query]}%")
  end
end
```

## Step 3: Using `pg_search` Gem for Advanced Searches

find similar strings:
```
SELECT * FROM names
WHERE LEVENSHTEIN(lower(name), 'william') < 3;
```

phonetically similar names:
```
WHERE DMETAPHONE(name) = DMETAPHONE('rob')
```

- Gem: `pg_search`
- `add_pg_search_function`: Use migrations to add search functions to your database
- Implementing the Search Feature in a controller action, typically with a `GET` request, where users can submit a search query and receive results.
```
def search
  @results = MyModel.pg_search(params[:query])
end
```
- then go to the show page of the owner

## Step 4: Testing Your Search

```
test 'should get index' do 
  get owner_path(@owner)
  assert_response :success # search results are displayed
end
```




