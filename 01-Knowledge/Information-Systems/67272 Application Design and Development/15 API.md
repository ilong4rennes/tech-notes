- Def: a set of protocols that enable different software components to communicate and transfer data
- Types
	- Private
	- Public
	- Partner
- result of api: a .json file

## Step 1: Define a Route

- Define a route in `config/routes.rb` to map requests to controller actions
```
Rails.application.routes.draw do
  get '/users', to: 'users#index'
end
```
### General Structure of API routes
```
Rails.application.routes.draw do
	# API routing
	scope module: 'api', defaults: {format: 'json'} do
		namespace :v1 do
			# provide the routes for the API here
			get 'investigations', 
				to: 'investigations#index', as: :investigations
			get 'investigations/:id', 
				to: 'investigations#detail', as: :investigations_detail
			post 'investigations/:id/notes', 
				to: 'investigations#new_note', as: :investigations_note
			post 'investigations/:id/add_assignment', 
				to: 'investigations#add_assignment', as: :add_assignment
	end
end
```

- `scope module: 'api'` - controllers for these routes in `app/controllers/api/` 
- `defaults: {format: 'json'}` - routes are formatted as JSON by default
- `namespace :v1 do` - routes defined within this block will be prefixed with `/v1/`
	- Versioning API: future changes can be made without breaking the current functionality

这四个routes分别对应的URL与功能是：
- `https://yourdomain.com/v1/investigations`
- `https://yourdomain.com/v1/investigations/1`
- `https://yourdomain.com/v1/investigations/1/notes`
- `https://yourdomain.com/v1/investigations/1/add_assignment`

- `v1`后面的URL内容格式和routes里引号的内容一样
- method则是引号前面的get/post
- to后面的是controller#action

- as: alias for the route
- `as: 'some_name'` - generates helper methods you can use throughout your application:
	- `some_name_path` returns the relative path (e.g., `/some_name`).
	- `some_name_url` returns the full URL (e.g., `http://yourdomain.com/some_name`).
- Example:
	- `investigations_detail_path(id: 1)` = `/investigations/1`.
	- `investigations_detail_url(id: 1)` = `http://yourdomain.com/investigations/1`

You might use these helpers in your views to create links:
```
<%= link_to 'View Details', investigations_detail_path(id: @investigation.id) %>
```

Or in your controllers to redirect:
```
redirect_to investigations_detail_path(id: @investigation.id)
```

## Step 2: Create a Controller

- Generate a controller named `UsersController`. 
- In `app/controllers/users_controller.rb`, define an `index` action to fetch users and render them as JSON.
```
class UsersController < ApplicationController
  def index
    users = User.all
    render json: users
  end
end
```
## Step 3: Use a Serializer

```
class DepartmentSerializer 
	include FastJsonapi::ObjectSerializer 
	attributes :id, :name 
	
	attribute :faculty do |object| 
		object.faculties.alphabetical.map do |faculty| 
		  DepartmentFacultySerializer.new(faculty).serializable_hash 
		end 
	end 
end
```

## Normal vs API Routes

**Normal Routes**
- **Primary Use**: To serve web pages to the browser.
- **Interaction**: Directly with end-users.
- **Response Type**: HTML / XML 
- **Example Scenario**: A user navigates to `https://example.com/about`, and the server responds by rendering the "About Us" page in HTML.

**API Routes**
- **Primary Use**: To handle data requests and responses, usually in a RESTful manner.
- **Interaction**: Between different parts of an application (e.g., frontend and backend) or with external services.
- **Response Type**: JSON / XML
- **Example Scenario**: A frontend application makes a POST request to `https://example.com/api/users` to create a new user. The server processes the request and returns a JSON object with the details of the newly created user.

## `render` vs `redirect_to`

`render` 
- `render` is used to display a view template to the user, sending the generated HTML back to the client's browser
- `render` does not create a new request; it simply generates and sends the response for the current request

You can use `render` in several ways:
- To display the default view for the current action (implicitly or explicitly).
- To render a specific template or file.
- To render inline code.
- To render with a specific layout, or without any layout at all.
- To render data in other formats, like JSON or XML.

```
def show
  @book = Book.find(params[:id])
  render 'special_show' # Renders the 'special_show.html.erb' view template
end
```

`redirect_to`
- `redirect_to` tells the browser to make a new request to a different URL. It's a way to send the user from one action to another action or even to a different website entirely. 
- This method is commonly used after performing operations like creating or updating a database record, as part of the Post/Redirect/Get pattern to prevent duplicate submissions. The server responds to the original request with an HTTP status code of 302 (by default) or 301 for a permanent redirect, along with the new URL to request.
```
def create
  @book = Book.new(book_params)
  if @book.save
    redirect_to @book
  else
    render :new
  end
end
```





- application_controller.rb
```
# handle 404 errors
  rescue_from ActiveRecord::RecordNotFound do |exception|
    render json: "This record could not be found."
  end
```

- swagger
- when i create the owner, i serialize the pet
- can have nice json for owner but not pet
- 
- this () is better because it has more info about pets


lib/filters 里面有一些generic method???

curl -X GET -H “” “大概是网址” jsonpp   --这是搞到json的command line instruction

filters -- let users have more flexibility
看pets_controller.rb
using
filterable.rb

## Serializer

- serializer: process that converts data into a format that can be easily stored or transmitted and then reconstructs it back into its original state / new state
- use ActiveModel Serializers to define how your Ruby objects are converted to JSON. A serializer in this context would specify which attributes of an object to include in the JSON output, and how related objects are nested or included.

swagger api


before_create :generate_api_key
- 为什么不是before save：因为不然的话 每次edit之后 都有一个新的api key 这就不行
- generate new api key every time I edit the user 
- log in once, authorize, get a token, save a token on your device, don't have to log in every time

## Why API?

- why do we customize apis
	- meet the needs of customers
- how do we customize apis
	- serializers
- what will happen if we are given all the owners?
	- cost too much time and space
- how can we give users only the data they want?
	- add filters
- how should we keep track of which user can get the data?
	- use token

how do i get the key?
- basic authentication
	- pass username and password as a base64 encoded string
	- use https lib, to decode the strings
	- authenticate
- token authentication before action except from :token
- cannot check login at the login page 不然会有循环
- don't have to pass in the token when i was requesting the token 

versioning
- why? (test question)


unlike in a normal Rails application, in a RESTful one, you will only need 5 (**index, show, create, update, and destroy**) actions instead of 7. We won't be needing the new or edit action since those were only used to display the form, and with only JSON responses, the form will no longer be needed









