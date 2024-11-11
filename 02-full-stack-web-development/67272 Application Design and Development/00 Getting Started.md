1. Create a new rails application called "SimpleQuotes" and initialize a blank git repository inside the app: 
```
rails new SimpleQuotes 
cd SimpleQuotes 
git init . 
```

2. Add all the files generated by rails to the repo and verify that all files are staged with the command git status . Commit these files by typing to git and verify again with `git status . `
```
git add . 
git commit -m 'Base rails app' 
```

3. Checkout a new branch called "quote". We will merge this back into master later. 
```
git checkout -b quote 
```

4. Create the Quote model and scaffolding using the rails generator. (Substitute the correct values): 
```
rails generate model <Model> <attribute>:<datatype>
```
- foreign key datatype: `references`

5. Edit schema
```
rails generate migration RemoveCrimeIdFromInvestigations crime_id:integer
```

Operations with database:
```
rails db:contexts
rails db:drop
rails db:create
rails db:migrate
```

6. Start up the development web server. 
```
rails server
```

Debugging contexts:
```
require './test/contexts'
include Contexts
create_units
```