- app
	- assets
	- controllers
	- models
		- callback, relationships, scopes, validations, methods
	- views
- bin
- config
	- database.yml
		- development, test, production 连接 db/.sqlite3
	- routes.rb
- coverage
	- index.html 网页版检查test coverage的工具
- db
	- migrate
		- YYYYMMDDHHMMSS_create_proverbs.rb
	- development.sqlite3
	- test.sqlite3
	- production.sqlite3
	- schema.rb
- docs
- lib
- log
- public
- storage
- test
	- controllers
	- factories
	- models
	- sets: where we create contexts
	- context.rb --take the subcontext and make them into one context
- tmp
- vendor
- Gemfile
- README.md

[Authorization](https://chat.openai.com/share/57a8886e-6bfe-487b-a3b8-c0369e61d118)

- To build web-based applications, 
- 1. you need to know the MVC pattern in software architecture

Model
- business logic 
- connect to the database (scope)

View
- given the right data, all about looking good
- should not doing any calculations

Controller
- connect the views and models
- take the correct data from models and give it to views
- like the cream in the oreoo, too large -- business logic creeps in
	- to clean it up -- refactor
- who did all the calculations? models

But why MVC?
- parallel -- specialization

- 2. you need to understand why and how to do software testing
Testing
- `simplecov` - false positives
- acceptance test -- views (cucumber, capibara)

Why testing?
- documentation
- reduce technical debt
- institutional memory

- 3. We need to use source code control to manage project development
- 4. We need to understand the OOP paradigm 
- 5. user-centered design
	- interactions
	- tradeoffs
	- mental models
	- gulf of execution: 
	- gulf of evaluation: what i think it will happen and what it actually happens
	- 1. keep users in control
	- 2. reduce cognitive load
	- 3. keep designs consistent

Real question: 
- model process the data
- controller
- view