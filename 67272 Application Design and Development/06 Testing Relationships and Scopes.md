## ActiveSupport::TestCase

- Rails ships with three types of tests: unit (models), functional (controllers), and integration (full MVC stack).
- `def test_`: test method, 里面包含了一系列assertion
- regular expression: 某个string是否符合某个特定格式
- Before every test method the setup method is invoked, and afterwards the teardown method
- Every test method makes one or more assertions about the behavior of the class under test
## Unit testing assertions

`assert(actual, message)` # Asserts truth 
`assert_equal(expected, actual, message)`
`assert_in_delta(expected_float, actual_float, delta, message) `
`assert_match(pattern, string, message) `
`assert_nil(object, message)` / `assert_not_nil `
`assert_raise(Exception, ..., message) { block ... } `
`assert_difference(expressions, difference = 1, &block)`

shoulda
`should have_many(:pets)`
如果要test 一个 relationship非常困难（生成的是sql code），所以就用这个shoulda lib

simplecov --> coverage/index.html --检验coverage

scope -- generate segments of SQL
`rails db:test:preprare `--create test database for you

`factory `--blueprint of how to create an object of a certain type
```
factory :owner do
	association :user
	name {"Alex"}
	# some other attributes
	end
```
`turtle = FactoryBot.create(:animal)` --create turtle of animal type using default value
`turtle = FactoryBot.create(:animal, name: "Turtle")`

Tests should be independent because Rails run the test cases in random order. 
create - delete
Every test should test only one method
add records to the test database --> test them (keep the test database as small as possible) 每次都是加一点然后删一点

## Unit testing

- Unit testing is done on models only 
- All models have a default test created upon `rails generate model` or `rails generate scaffold `
- Unit tests are stored in the directory `test/models` within a Rails project 
- `rails test:models` --run just model tests 
- `rails test ` --run all tests
- `rails test test/models/owner_test.rb` -- executing a particular file
- test coverage --gems like `simple_cov` 
	- not perfect and especially vulnerable to false positive readings

## Why testing?
- Tests give us confidence the code works, as it should, in all cases 
- Tests helps us better understand the problem we are solving / method we are writing 
- Tests serve as a form of documentation 
- Tests serve as a form of institution memory 
- Tests make clear what performance expectations are 
- Tests identify ways in which we can clean up code and safely refactor 
- Tests save money in the long run (the Boeing answer) 
- Tests identify problems when they are easier/less costly to fix