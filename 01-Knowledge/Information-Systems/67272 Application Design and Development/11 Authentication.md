- def: verify the identity of a user
### Step 1: User Model
```
rails generate model User email:string password_digest:string
rails db:migrate
```

- built-in feature `has_secure_password` to securely hash and store passwords
	- To use it -  have `bcrypt` gem in your Gemfile 

- `Gemfile`:
```
gem 'bcrypt', '~> 3.1.7'
```
- User model:
```
class User < ApplicationRecord
  has_secure_password
end
```
- This adds methods to set and authenticate against a BCrypt password. This requires a `password_digest` attribute in your database.

```
def self.authenticate(email, password)
	find_by_email(email).try(:authenticate, password)
end
```
- login by email

### Step 2: User Registration

1. Create a form for users to register. You'll need a UsersController and corresponding views.
```
rails generate controller Users new
```

2. In `users_controller.rb`, add a `create` action:
```
def create
  @user = User.new(user_params)
  if @user.save
    # Log the user in and redirect to the user's show page
  else
    render 'new'
  end
end

private

def user_params
  params.require(:user).permit(:email, :password, :password_confirmation)
end
```
- The form for registration (`new.html.erb`) should include fields for email, password, and password_confirmation.

#### User Controller
- To manage user-related actions
- Acts as an intermediary between user models and views
##### User Management Actions

- **New**: Displays a form for creating a new user. This is usually the registration form where new users can sign up by providing their details.
```
def new
	@user = User.new
end
```
- **Create**: Handles the form submission for creating a new user. It takes the input from the "new" form, creates a new User record in the database, and handles success or failure scenarios such as showing validation errors or redirecting to a new page upon successful creation.
```
def create
	@user = User.new(user_params)	
	@user.assistant! if current_user.assistant?
	if @user.save
		flash[:notice] = "Successfully added #{@user.proper_name} as a user."
		redirect_to users_url
	else
		render action: 'new'
	end
end
```
- **Edit**: Displays a form for editing an existing user's details. This action is typically protected to ensure that only the user in question (or an administrator) can edit a user's details.
```
def edit
	@user.assistant! if current_user.assistant?
	# change the role of user to assistant if user has assistant role
end
```
- **Update**: Handles the form submission for updating a user's details. Similar to the "create" action, it takes input from the "edit" form, updates the User record in the database, and handles success or failure scenarios.
```
def update
	if @user.update(user_params)
		flash[:notice] = "Successfully updated #{@user.proper_name}."
		redirect_to users_url
		# maps to the index action of the UsersController
		# showing a list of users or a similar page
	else
		render action: 'edit'
		# renders the 'edit' action's view template again
		# allows the user to see the form with the previously submitted values 
		# still filled in, along with any validation error messages that were 
		# generated during the update attempt
	end
end
```
- **Show**: Displays a single user's details. This action is used to show a user's profile page, which might include their name, email, and other personal details.

- **Index**: (Optionally) Lists all users. Depending on the application's requirements, this action might be used to display a list of all users, often used in administrative interfaces.
```
def index
	# finding all the active owners and paginating that list (will_paginate)
	@users = User.all.paginate(page: params[:page]).per_page(15)
end
```
- **Destroy**: (Optionally) Deletes a user. This action allows for the removal of a user from the database, typically restricted to the user themselves or an administrator.
```
def destroy
	if @user.destroy
		redirect_to users_url, notice: "Successfully removed #{@user.proper_name} from the PATS system."
	## There is no 'else' for now; we have no restrictions on user deletion (although we probably should)
	# else
		# render action: 'show'
	end
end
```

### Step 3: Sessions Controller

1. Generate a Sessions controller for handling login and logout:
```
rails generate controller Sessions new
```

2. In `sessions_controller.rb`, add actions for creating (login) and destroying (logout) sessions:
```
def create
	user = User.authenticate(params[:username], params[:password])
	if user
		session[:user_id] = user.id
		redirect_to home_path, notice: "Logged in!"
	else
		flash.now.alert = "Username and/or password is invalid"
		render "new"
	end
end

def destroy
	session[:user_id] = nil
	redirect_to home_path, notice: "Logged out!"
end
```

3. The login form (`app/views/sessions/new.html.erb`) should have fields for email and password, and submit to the `create` action in `SessionsController`.

4. You'll need to set up a way to keep track of the current user's login state. This usually involves setting a session variable when the user logs in and clearing it when the user logs out.
	- In `sessions_controller.rb`, when logging in: `session[:user_id] = user.id`
	- And when logging out: `session.delete(:user_id)`

### Step 4: Access Control

- set up before actions in your controllers to restrict access to logged-in users:
```
before_action :set_user, only: [:show, :edit, :update, :destroy] 
before_action :check_login 
authorize_resource 

private 
def set_user 
	@user = User.find(params[:id]) 
end
```

- `before_action`: callback method run before other actions in the controller are executed.
- `only: [:show, :edit, :update, :destroy]` specifies that the `set_user` method should only be run before the `show`, `edit`, `update`, and `destroy` actions. This means that before any of these actions are performed, Rails will first execute the `set_user` method.
- `before_action :check_login `: a security measure to ensure that certain actions can only be performed by authenticated users

### Summary
1. User Model
	- `has_secure_password` -- `bcrypt` Gem + `password_digest` attr. in db
2. Users Controller
	- Usage: create form for users to **REGISTER**
	- Actions: new, create, edit, update, show, index, destroy
	- access control
3. Sessions Controller
	- Usage: handle **LOGIN / LOGOUT**
	- Actions: create, destroy

### Authentication vs. Authorization

- authentication 
	- verifying who a user is
	- Rails has built-in methods (`has_secure_password` method in models, which works in conjunction with the `bcrypt` gem to securely hash and store passwords)
- authorization 
	- determining what an authenticated user is allowed to do
	- about permissions and roles: deciding which parts of the application a user can access and what actions they can perform
	- Tools like CanCanCan in Rails help manage these permissions elegantly

### Routing to the Sessions Controller

- User visits `localhost:3000/login` 
- Dispatcher looking at `routes.rb` file
- Rails routes the request to Sessions controller's `new` action, via the line:
	- `get login, to: 'sessions#new', as: :login`
- Summary: dispatch requests for `/login` to the `new` action in the `SessionsController`, which typically renders a login form.

### Sessions and State

HTTP is inherently stateless, meaning it doesn't remember previous interactions. Sessions are a way to overcome this limitation by preserving state across requests. When a user logs in, a session is created to store their information, which can be held:

- In the database
- In memory
- On a specific server

The session is represented in Rails as a `session` hash, which persists for the duration of the user's visit to the website. Additionally, Rails uses:

- An `error` hash to store information about validations that failed.
- A `params` hash to hold parameters received from forms or the URL.
- 有一个session database

### Sessions Controller

- handling the login and logout
- doesn't require sessions model

- `create` method - log in
	- authenticate user: check user's credentials 
	- create a new session (if correct)
	- redirect to home page (so no view template for create action)
		- `get 'home', to: 'home#index', as: :home`
		- home_path 这个shortcut是在route里面定义的 用as: :home
	- generally speaking, 所有主要功能是redirect的action都没有view templates 比如create，update，destroy

### Application Controller and Current User

- `application_controller.rb` defines `current_user` method
	- determine the currently logged-in user across the application
	- checks the session to find the user's ID and then retrieves the user's information from the database. 
	- helper method for both authentication and authorization 根据role来决定干什么
