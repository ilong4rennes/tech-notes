# Model

## Components

- ActiveRecord does the heavy lifting 
- Key Idea: Models hold all the data & business logic

- Callback
	- `before_save`, `before_create`, `before_update`
	- `after_save`, `after_create`, `after_update`
- Relationships 
	- `has_many`
	- `belongs_to`
- Scopes 
	- `scope :alphabetical, -> { order('last_name, first_name') }`
- Validations: to ensure data integrity 
	- `validates_presence_of`
	- `validates_format_of`
	- `validates_inclusion_of`
- Methods
	- privates methods - store callback code

## Goals of Active Record

- Make working with databases easier 
- Reduce repetition in code 
- Cut down on configuration needed to make applications work
## Primary Key and Active Record

- Active Record gives you a way of overriding the default name of the primary key for a table.
	- isbn = candidate key `self.primary_key = “isbn”`

- Table names are plural and class names are singular
	- pet model
		- `belongs_to :owner `
		- `@pet.owner`
	- OwnersController / owners table
		- `has_many :pets`
		- `@owner.pets`
## Associations

- Def: a relationship or an association is a connection between two Active Record models
- ActiveRecord creates SQL
- `Pet.last.owner` 找到pet的最后一个的owner，相当于join table
- `belong_to` = have the foreign key
- `Pet.last.visits` - 须有一个table叫visits，然后里面有一个field叫pet_id

我们现在在pet.rb file里面
用.visit是因为有 belongs_to :visits

Pet.active.class => Pet::ActiveRecord_Relation
然后再输入Pet.active.alphabetical 就会return一个table

Pet.alphabetical.active 应该会fail，因为group by之后就不能go back to where 但是他行了。。it doesn't execute until the user press enter. 

relationship on array 不行，会有error

eg. 一个owner有多个pets，有owner model 和 pet model
- To add a new pet for an existing owner, we’d need to include the owner_id as a foreign key (in pet model).
- When we’d like to delete owner having id=1, we should ensure that all his pets get deleted as well.

Active Record supports the three common types of relationship between tables: 
1. one-to-one 
2. one-to-many 
3. many-to-many

Rails supports six types of associations: 
1. belongs_to 
2. has_many 
3. has_many :through 
4. has_one 
5. has_one :through 
6. has_and_belongs_to_many

# Database

## ORM

- Object-relational mapping (ORM): simplifies the use of databases in applications
- ActiveRecord: The Rails ORM
### `ActiveRecord` Demo
- `rails console`
- `Animal.all`  = select *
- `Animal.first`
- `Animal.last`
- `Animal.find 3`

Q: 这些method都是从哪里来的？
A: active record: the rails ORM 

Q: Animal.find_by_name "Dog" 我们有这个method嘛？
A: 好在有meta programming 发生了某些神奇的东西，可以find by (field name) 就可以去找这个field

- `dog = Animal.find(3)`  -- error
- `dog.name`
- `dog.active`
- 这些点后面的都是field name, return 这些field里面的值

- `dog.name = 'Dog!!'`
	- change in the memory
	- not change in the database

- `dog.save` / `dog.save!`
- `dog.reload`
	- save in the memory

## Connecting to a database

**Key 1: database.yml**
- in config/ directory
- contains connection information for 3+ levels
	- development: `db/development.sqlite3`
	- test
	- production
	- others possible, eg.staging
- 今天全是讲的development level，之后还有test level，test完就到了production level

**Key 2: `Gemfile` provides adapter**
- use sqlite3 in development & test 
- use Postgres or MySQL in production

- production就不用sqlite3（因为工业界不太在用）用oracle之类的
- sqlite3 -- 适合 embedded systems（一两百个record很适合，几百万个就不太适合）
- sqlite太慢了 比Postgres慢多了
## Rails migrations

```
class CreatePets < ActiveRecord::Migration[5.2]
	def change
		create_table :pets do |t|
			t.string :name
			t.references :animal, foreign_key: true
			t.references :owner, foreign_key: true
			t.boolean :female
			t.date :date_of_birth
			t.boolean :active, default: true
			
			# t.timestamps
		end
	end
end
```

- migrations = instructions for how to build the database
	- create / change / drop tables
- Migrations are stored as files in the `db/migrate` directory, one for each migration class.
- File name: `db/migrate/YYYYMMDDHHMMSS_create_proverbs.rb`
- 不能rerun migration 不然会error; only run migrations that haven't run yet!!
	- 这个东西可能会考
- migration datatypes

### Six operations of Rails Migration
1. add a new table (associated to a model)
	1. `YYYYMMDDHHMMSScreatetable.rb`
2. add a column
	1. `rails generate migration AddColumnToTable column1:integer`
	2. generates `YYYYMMDDHHMMSS_add_column_to_table.rb`
	
	3. `rails generate migration AddDetailsToOwners age:integer ssn:string`
	4. `class AddDetailsToOwners < ActiveRecord::Migration[5.2] 
		   def change 
			   add_column :owners, :age, :integer 
			   add_column :onwers, :ssn, :string 
			end 
		end`
3. remove a column
	1. `rails generate migration RemoveColumnToTable column:string`
4. drop a table
	1. `rails destroy model ModelName`
5. add data to a table
	1. `rake db:seed`
	2. `rake db:populate` and `rake db:contexts`
	3. via migrations: create a default user at the time db is created
```
class AddDefaultUser < ActiveRecord::Migration[5.2] 
	def change 
		# create a new instance of User and populate 
		vet = User.new 
		vet.username = "vet" 
		vet.email = "vet@example.com" 
		vet.password = "yodel" 
		vet.password_confirmation = "yodel" 
		# save with bang will throw exception on failure 
		vet.save! 
	end 
end
```
6. add functions and triggers to the database
```
class AddPgSearchDmetaphoneSupportFunctions < ActiveRecord::Migration[5.2 ] 
	def change 
		# add the extension first... 
		say_with_time("Adding psql extensions if not existing:") do 
			execute <<-'SQL' 
			CREATE EXTENSION IF NOT EXISTS fuzzystrmatch; 
			SQL 
	end 

	# now add the search function needed on the database side... 
	say_with_time("Adding support functions for pg_search :dmetaphone") do 
		execute <<-'SQL' 
		CREATE OR REPLACE FUNCTION pg_search_dmetaphone(text) 
			RETURNS text LANGUAGE SQL IMMUTABLE STRICT AS $function$ 
			SELECT array_to_string(ARRAY(SELECT dmetaphone(unnest(regexp_split_to_array($1, E'\\s+')))), ' ') 
			$function$; 
		SQL 
		end 
	end 
end
```