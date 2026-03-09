## Authorization using `CanCanCan`
### Step 1: Add `CanCanCan` to `Gemfile`

- `gem 'cancancan', '~> 3.0'`
- `bundle install`

### Step 2: User Model - Define User roles

ENUM - Usage in `user.rb`
- Defines a set of possible roles a user can have using the `enum` directive.
```
enum :role, { vet: 1, assistant: 2, owner: 3}, scopes: true, default: :owner

ROLES = [
['Vet', User.roles[:vet]],
['Assistant', User.roles[:assistant]],
['Owner', User.roles[:owner]]
]
```
- `User.last.role` returns"owner" (integer 3)
- Automatically generated Boolean Methods
	- `mark.owner?` --true
	- `mark.vet?` --false
	- 没有这些method，但是enum自动create了这些method 比如owner？

### Step 3: Ability Model - Define Abilities of roles

1. Create file `app/models/ability.rb` via `rails g cancan:ability`
	- the file to define authorization rules
2. Define abilities
```
class Ability
	include CanCan::Ability
	
	def initialize(user)
		# set user to new User if not logged in
		user ||= User.new # i.e., a guest user
		
		# set authorizations for different user roles
		if user.vet?
			# they get to do it all
			can :manage, :all
		
		elsif user.assistant?
			# can manage owners, pets, and visits
			can :manage, Owner
			can :manage, Pet
			can :manage, Visit
			
			# can create and destroy dosages and treatments
			can :manage, Dosage
			can :manage, Treatment
			
			# can read (costs for) medicines and procedures
			can :read, Medicine#Cost
			can :read, Procedure#Cost
			
			# they can read their own profile
			can :show, User do |u|
				u.id == user.id
			end
			
			# they can update their own profile
			can :update, User do |u|
				u.id == user.id
			end
		
		elsif user.owner?
			# they can read their own data
			can :show, Owner do |this_owner|
				user.owner == this_owner
			end
			
			# they can see lists of pets and visits (controller filters automatically)
			can :index, Pet
			can :index, Visit
			
			# they can read their own pets' data
			can :show, Pet do |this_pet|
				my_pets = user.owner.pets.map(&:id)
				my_pets.include? this_pet.id
			end
			
			# they can read their own visits' data
			can :show, Visit do |this_visit|
				my_visits = user.owner.visits.map(&:id)
				my_visits.include? this_visit.id
			end
		
		else
			# guests can only read animals covered (plus home pages)
			# can :read, Animal
		end
	end
end
```

### Step 4: Basic Structure and Authorize Actions in Controllers

#### Callback
```
# A callback to set up an @owner object to work with
before_action :set_owner, only: [:show, :edit, :update, :destroy]
before_action :check_login

def set_owner
	@owner = Owner.find(params[:id])
end

def owner_params
	params.require(:owner).permit(:first_name, :last_name, :street, :city, :state, :zip, :phone, :email, :active, :username, :password, :password_confirmation)
end

def user_params
	params.require(:owner).permit(:first_name, :last_name, :active, :username, :password, :password_confirmation)
end
```
- `set_owner` is called before the `show`, `edit`, `update`, `destroy` action 
	- 1. retrieve the `Owner` instance based on the `id` from request
	- 2. set an `@owner` instance variable for use in these actions
		- crucial for authorization
- `check_login` ensures user is logged in before allowing them to perform any actions in this controller -- authentication -- `application_controller.rb`

#### Cancancan Authorization
##### Shortcut
```
authorize_resource
```
- a Cancancan macro 
- called as a before_action for the entire controller
- automatically applies authorization to all actions in this controller
- Cancancan checks these rules before every action to ensure the user has permission to perform it. 
- If the user doesn't have permission, Cancancan will raise an `CanCan::AccessDenied` exception.

##### Manual
```
def show
	# authorize! :show, @owner
	# get all the pets for this owner
	@current_pets = @owner.pets.alphabetical.active.to_a
end
```
- manually enforce authorization rules on a per-action basis
- `authorize_resource` already covers all actions for the `Owner` resource

#### Actions

- The `create`, `update`, and `destroy` actions are places where authorization is critical, as they change the state of the system. The implicit use of `authorize_resource` ensures that only authorized users can perform these actions.

```
def create
	@owner = Owner.new(owner_params)
	@user = User.new(user_params)
	@user.owner!
	if !@user.save
		@owner.valid?
		render action: 'new'
	else
		@owner.user_id = @user.id
		if @owner.save
			flash[:notice] = "Successfully created #{@owner.proper_name}."
			redirect_to owner_path(@owner)
		else
			render action: 'new'
		end
	end
end
```
- initialize owner and user objects
- `@user.owner!`
	- sets the role of the `User` instance to `:owner`
	- `owner!` method is a part of the enum functionality in Rails (enum见下)
-  The condition `if !@user.save` checks if the user instance cannot be saved to the database (e.g., due to validation failures). If the save fails:
	-  `@owner.valid?` is likely a check to trigger the validation of `@owner` without actually saving it. This might be used to populate `@owner` with any validation errors.
	- `render action: 'new'` renders the `new` template again, likely displaying the validation errors to the user.
- If the user is successfully saved:
	- `@owner.user_id = @user.id` associates the `Owner` instance with the `User` by setting the `Owner`'s `user_id` to the `User`'s `id`.
	- Then, it tries to save the `Owner` instance. If successful, it redirects to the `Owner`'s show page with a success message. If not, it renders the `new` action again to show errors.

### Step 5: Application Controller - Handle Unauthorized Access

```
# just show a flash message instead of full CanCan exception
rescue_from CanCan::AccessDenied do |exception|
	flash[:error] = "You are not authorized to take this action. Go away or I shall taunt you a second time."
	rediect_to home_path
end
```

### Step 6: Use in Views

- check permissions in your views to show or hide elements based on the user's abilities. Use `can?` and `cannot?` helpers:
```
<% if can? :update, @article %>
  <%= link_to 'Edit', edit_article_path(@article) %>
<% end %>
```


### Summary

Authorization happens in:
- Model: ability.rb 
- Controller: check if model can do it via ability file, throw error message otherwise
- View
## Testing
### Test user model

- FactoryBot.build -- save in memory without saving to database (before run the validation)
	- 所以我们test里面用build
- FactoryBot.create -- save to the database (go through validation process)

- `db/schema.rb`: auto-generated file that shows the structure of the db (tables, columns, data types)

### Test sessions controller

- **Controller Testing Components**:
    - **Routes**: routes -- correct actions in the sessions controller.
    - **Controller Actions**: Ensure that each action in the controller performs as expected, which includes setting or clearing the `session[:user_id]` during login and logout.
    - **Templates**: Verify that the correct view templates are rendered or redirected to, after the actions are performed.
- **Login Helper Method**: The `login_vet` method is defined in `test_helper.rb` and is used to simulate logging in a vet user during tests. This helps in testing actions that require authenticated users.
- It's crucial to test not just the successful paths (e.g., successful login) but also the failure cases (e.g., failed login due to incorrect credentials)
