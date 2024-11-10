# Rails Basic Commands

- `rails new SimpleQuotes ` = Create a new rails application called "SimpleQuotes"
- `rails generate scaffold <Model> <attribute>:<data type>` = Create the Quote model and scaffolding using the rails generator
- `rails generate controller Home index` = create a home controller with an index action
- `rails new `
- `rails generate` (or `rails g`)
- `rails server` or `rails s `
- `rails server -p` e.g., `rails server -p 3030 `
- `rails routes `
- `rails db:migrate `
- `rails console`  or `rails c `
- `rails —help` check all the rails commands available for you

# PATS dataset
- `git clone []` - clone the repo
- `bundle install` - install all the gems
- `rake db:populate` - set up the database
	- remove any old databases, create new development and test databases, run all the migrations to set up the structure, and then create 240 owners with over 450 pets and several thousand visits (run the `create_all` method in the context)
- `rails test` - verify model and controller tests are functioning
	- The `SimpleCov` gem will create a coverage directory with an index.html file in it; open this file in a web browser to see the coverage provided.

To load the testing context into development database:
1. if you have previously populated the database, drop it and rerun `rails db:migrate` to recreate a blank db.
2. open `rails console`
3. type `require 'factory_bot_rails'` (it will say 'false')
4. type `require './test/contexts'` (it will say 'true')
5. type `include Contexts` (it will say 'Object')
6. type in whatever context building method you wish (e.g., `create_animals`)