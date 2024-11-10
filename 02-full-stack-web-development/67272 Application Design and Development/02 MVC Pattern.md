- Model: Taking Care of Business
- View: Looking Good / Consists of partials
- Controller: Holding It All Together / Process
![[Screen Shot 2024-02-15 at 01.54.20.png]]

Advantages:
1. Easy to know where in the project to find relevant code
2. Allow for parallel development of the system
3. Smaller components are easier to test and debug
4. Allows for division of labor based on expertise
5. Easy to extend functionality of the system
6. Can reuse components in other applications

## Journey of a request in Ruby on Rails web application

1. **User Enters URL**
	1. Uniform Resource Locator (网址): address of a given unique resource on the Web
	2. In Ruby, URL is a request to view a certain resource or perform an action on a web application built with Ruby on Rails
	3. structure of URL -- REST architecture ```http[s]://www.example.com/controller/action/id```
		-  **HTTP/HTTPS**: This is the protocol used for the request. HTTPS is the secure version of HTTP and is recommended for all web applications.
		- **Domain**: This is the domain name of the application (e.g., `www.example.com`).
		- **Controller**: In Rails, the controller is responsible for handling requests that come to the application. It's named after the resource it manages (e.g., `articles`, `users`). The controller name in the URL signifies which controller the routing system should dispatch the request to.
		- **Action**: The action represents a specific function within the controller. It defines what should be done with the request (e.g., `show`, `edit`, `create`). In a RESTful design, standard actions correspond to CRUD (Create, Read, Update, Delete) operations.
		- **ID**: This is an optional parameter, typically used to specify a particular resource instance. For example, in a URL like `/articles/show/1`, `1` would be the ID of the article to be displayed by the `show` action.
	4. Nested resources 
		1. For resources that are logically contained within another resource, Rails allows you to **define nested routes**. For example, if each article can have many comments, you might have a route like `/articles/:article_id/comments/:id`
	5. Query strings `https://www.example.com/articles?search=rails&sort=recent`

2. **Routing: Rails router**
	1. Router: parse the URL and decide which controller and action (method) to dispatch the request to
	2. based on the routing rules defined in `config/routes.rb`
	3. **RESTful Routes**
		- **GET `/articles`**: Maps to the `index` action, displays a list of articles.
		- **GET `/articles/new`**: Maps to the `new` action, returns a form for creating a new article.
		- **POST `/articles`**: Maps to the `create` action, creates a new article.
		- **GET `/articles/:id`**: Maps to the `show` action, displays a specific article.
		- **GET `/articles/:id/edit`**: Maps to the `edit` action, returns a form for editing an article.
		- **PATCH/PUT `/articles/:id`**: Maps to the `update` action, updates a specific article.
		- **DELETE `/articles/:id`**: Maps to the `destroy` action, deletes a specific article.
	
3. **Dispatcher**
	1. Technically, the Rails router itself acts as a dispatcher. Once it determines the appropriate controller and action, it forwards the request to that controller.

4. **Controller**
	- It's responsible for making sense of the request, interacting with the model to retrieve, update, or create data, and then deciding how to respond. This could involve rendering a view or redirecting to another action based on business logic.
		- **Parameters**: The controller receives parameters from the request (such as form data or query strings), which can influence how it interacts with the model or which view is rendered.
		- **Session and Cookies**: The controller also has access to session and cookie data, enabling it to track user state across requests.

5. **Model**
	1. For resources that are logically contained within another resource, Rails allows you to define nested routes. For example, if each article can have many comments, you might have a route like

6. **View**
	1. Once the controller has decided on the response, it typically renders a view.

7. **Response**
	1. The compiled HTML page (or other responses like JSON or XML, depending on the request) is sent back through the controller to the user's browser.

8. **Rendering or Redirecting**
	- If the controller decides to render a view, it combines the template with any instance variables to produce an HTML response.
	- If it decides to redirect, the browser is instructed to make a new request to a different URL.