context.rb --take the subcontext and make them into one context

## CRUD Operations

![[Screen Shot 2024-02-13 at 11.24.34.png]]

RESTful Actions: ![[Pasted image 20240311234018.png]]

7 controller actions --need a route to the controller action
- `GET /posts` for `index`
- `GET /posts/new` for `new`
- `POST /posts` for `create`
- `GET /posts/:id` for `show`
- `GET /posts/:id/edit` for `edit`
- `PATCH/PUT /posts/:id` for `update`
- `DELETE /posts/:id` for `destroy`

- dispatcher -- routes table -- name a controller -- controller#action 比如 owner#index
- In `app/controller.rb` file, we define the `index` action:
```
def index --不叫methods，叫actions
	@active_owners = Owner.active.alphabetical --找对应model里的method
	然后就是找到对应的template
	layout application -- view/layout/ 把结果放到yield里面去
	这里知道怎么找到template，因为这个是从一个superclass里inherit下来的
	自动地知道应该有一个template
```

### Routes and URL Patterns

- In a Rails application, routes define the URL patterns that your application can respond to. 
- They map incoming requests to controllers and actions. 

### HTTP Verbs and Actions

- **GET**: This HTTP method is used to request data from a specified resource. 
	- In the context of Rails routes, using `get` as a shortcut allows you to define a route that handles GET requests, typically to read or retrieve a resource.
- **PUT and PATCH**: Both are used for updating resources. 
	- In Rails, `put` is traditionally used to update a resource entirely, while `patch` is for partially updating it. Modern Rails applications use `patch` more commonly for updates.
- **Redirects and Rendering**: Redirecting to a different page (`redirect_to @owner`) or going back to a previous page as part of the control flow in a Rails controller. This is often used after a database operation like insert, update, or delete (reflected in SQL terms) to navigate users to the appropriate view based on the outcome of their request.

### SQL Operations and Rails

- While SQL directly uses `INSERT`, `UPDATE`, and `DELETE` commands to modify data, Rails abstracts these into methods like `.save`, `.update`, and `.destroy`. 
- You don't see SQL directly; you see its effects through changes in the database, much like how routes abstract the details of HTTP requests and responses.

### Part of a Controller action
```
if ()
	redirect to some place
	redirect_to @owner -- shortcut of owner_path(@owner)
else
	go back to the new page
```

- If the condition is true, it redirects to the `@owner` resource, using a Rails path helper (`owner_path(@owner)` as a shortcut). 
- If the condition is false, it suggests going back to the 'new' page, likely to correct form inputs. This pattern is common in create or update actions where a form submission's success leads to a redirect, and failure re-renders the form.

get a shortcut -- use the GET verb
read something - create a shortcut to get this path
最右边那一栏就是shortcut

## `show.html.erb`

- Location: typically found in the `app/views/model_name` directory, where `model_name` is the name of the model associated with the controller
- Def: a template used to display the details of a specific instance of a model
- The `.html.erb` extension indicates that it is an HTML file that can contain embedded Ruby code (ERB).
- The `show.html.erb` file is part of the view layer in the MVC (Model-View-Controller) architecture pattern used by Rails. The controller fetches the model data, passes it to the view, and then the view uses this data to render the HTML page to the user.
- render partial: "something" 有时会省略partial这个词 默认就是partial

## `routes.rb`

- def: file that defines how your application maps incoming HTTP requests to specific controllers and actions
	- E.g., create the routes for get
- example: Blog application
	- `PostsController` with actions
		- `index` (to list all posts)
		- `show` (to show a specific post)
		- `new` (to display a form for creating a new post)
		- `create` (to save the new post)
		- `edit` (to edit a post) 
		- `update` (to update a post)
	- `routes.rb`:
		- This simple line, `resources :posts`, tells Rails to create seven different routes in your application, all mapping to the `PostsController`. These routes correspond to the standard CRUD (Create, Read, Update, Delete) operations, including:
			- `GET /posts` for `index`
			- `GET /posts/new` for `new`
			- `POST /posts` for `create`
			- `GET /posts/:id` for `show`
			- `GET /posts/:id/edit` for `edit`
			- `PATCH/PUT /posts/:id` for `update`
			- `DELETE /posts/:id` for `destroy`
```
Rails.application.routes.draw do
  # This creates routes for PostsController actions
  resources :posts
end
```

### Basic Structure of `routes.rb`

```
Rails.application.routes.draw do
  # Defines the root path route ("/")
  root "pages#home"

  # Example of a regular route
  get 'about', to: 'pages#about', as: 'about'

  # Example of a resource route (maps HTTP verbs to controller actions automatically)
  resources :articles

  # Example of a nested resource
  resources :articles do
    resources :comments
  end

  # Example of a namespace route
  namespace :admin do
    resources :articles
  end

  # Example of a custom named route
  get 'login', to: 'sessions#new', as: :login

  # Direct routes to a specified path
  direct(:homepage) { "http://www.example.com" }
  
  # You can have the root of your site routed with "root"
  # just remember to delete public/index.html.
  root :to => 'welcome#index'
end
```

### Step by Step Explanation

1. **Root Route**: `root "pages#home"` sets the root route of your application, which is the default page displayed when someone visits the base URL of your site. Here, it routes to the `home` action in the `pages` controller.
    
2. **Regular Route**: `get 'about', to: 'pages#about', as: 'about'` defines a simple, custom route. It maps a GET request to `/about` to the `about` action in the `pages` controller. The `as: 'about'` part creates a URL helper named `about_path` which can be used in your views.
    
3. **Resource Route**: `resources :articles` creates RESTful routes for the `articles` resource, automatically mapping standard HTTP verbs (GET, POST, PUT/PATCH, DELETE) to corresponding actions (`index`, `show`, `new`, `create`, `edit`, `update`, `destroy`) in the `articles` controller.
    
4. **Nested Resource**: The nested resources syntax allows you to express relationships between resources. In the example, `resources :articles do resources :comments end` creates routes for comments that are nested within articles, reflecting the hierarchical relationship between articles and their comments.
    
5. **Namespace Route**: `namespace :admin do resources :articles end` groups routes under an `admin` namespace, segregating them from the rest of your application routes. This is useful for organizing and restricting parts of your application, like an admin panel.
    
6. **Custom Named Route**: `get 'login', to: 'sessions#new', as: :login` maps a GET request to `/login` to the `new` action in the `sessions` controller, creating a `login_path` URL helper. This is useful for custom authentication flows.
    
7. **Direct Routes**: `direct(:homepage) { "http://www.example.com" }` provides a way to create custom URL helpers directly, without needing an associated route within the application.
    
8. **Root Route Redefined**: Although having multiple root definitions is not typical and can be confusing, the last line, `root :to => 'welcome#index'`, shows another way of defining the root route, overriding any previous root definition.
    

### What It Does

- **Routing Requests**: The `routes.rb` file maps URLs to controllers and actions. When a request comes in, Rails uses this file to determine which controller and action to send the request to.
    
- **URL Helpers**: It also defines URL helpers (like `about_path` or `login_path`) that you can use throughout your application to generate URLs. This makes your views and controllers cleaner and more maintainable.
    
- **Resourceful Routing**: By using resourceful routing, Rails encourages RESTful design, making it easier to create consistent and predictable URLs for interacting with resources.

### Semi-static Page Routes

These routes are likely intended for pages that rarely change. They are prefixed with `home/` and directed to various actions within the `home` controller:

- `get 'home', to: 'home#index', as: :home` routes to the main homepage of the application.
- The following routes (`about`, `contact`, `privacy`, `search`) are similarly structured, directing to their respective actions within the `home` controller and named for easy reference (`as: :about`, `as: :contact`, etc.).

### Authentication Routes

This section deals with user authentication and session management:

- `resources :sessions` and `resources :users` create the full set of RESTful routes for sessions and users, respectively, facilitating actions like creating, viewing, editing, and deleting user sessions and user profiles.
- Additional specific routes for user signup (`users/new`), editing the current user's profile (`user/edit`), logging in (`login`), and logging out (`logout`) provide clear, named paths for these common authentication actions.

### Resource Routes

These routes use the `resources` method to automatically generate RESTful routes for various models in the application:

- Models such as `owners`, `animals`, `pets`, `visits`, `dosages`, `treatments`, `medicines`, and `procedures` are all set up with routes that map HTTP verbs to controller actions, covering the standard CRUD (Create, Read, Update, Delete) operations and more.

### Routes for Medicine and Procedure Costs

These routes handle the creation of new medicine and procedure costs:

- `get` routes for `medicine_costs/new` and `procedure_costs/new` provide forms to create new costs.
- `post` routes for `medicine_costs` and `procedure_costs` handle the form submission to actually create the costs in the database.

### Other Custom Routes

- An example of a custom route is `get 'visits/:id/dosages', to: 'visits#dosages', as: :visit_dosages`, which likely shows all dosages for a specific visit. The `:id` segment captures the visit's ID from the URL.

### Commented-Out Routes for Searching

These commented-out routes (`medicines/search`, `owners/search`, `pets/search`) suggest that the application had or planned to have search functionality for medicines, owners, and pets, but these routes are currently not in use.

### Root Route

- `root 'home#index'` sets the application's root path to the `index` action of the `home` controller, meaning this is the default page shown when visiting the base URL of the application.
- In the provided `routes.rb` file, the `home_path` is defined by this line:
	- `get 'home', to: 'home#index', as: :home`

Breaking this down:

- **HTTP Verb (`get`)**: Specifies that this route responds to HTTP GET requests.
- **Path (`'home'`)**: The URL path that the route matches. In this case, it matches the `/home` URL.
- **Controller and Action (`to: 'home#index'`)**: This part specifies that the `index` action of the `home` controller should handle the request. The `home` controller is likely responsible for rendering the application's homepage or a similar landing page.
- **Named Route (`as: :home`)**: This creates a named route helper, `home_path`. In your views and controllers, you can use `home_path` to generate the URL `/home` programmatically, ensuring that your code does not break if the URL structure changes later.


