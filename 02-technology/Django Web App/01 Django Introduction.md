### Step 1: Install Django and Set Up Virtual Environment

1. Create a virtual environment to isolate your project dependencies:
    ```bash
    virtualenv venv
    source venv/bin/activate  # On Windows use `venv\Scripts\activate`
	deactivate # 退出虚拟环境
    ```

2. Install Django in the virtual environment:
    ```bash
    pip install django
    ```

### Step 2: Create a Django Project

1. Create a new Django project:
    ```bash
    django-admin startproject recsys
    ```

2. Navigate into the project directory:
    ```bash
    cd recsys
    ```

Your project structure will look like this:
```
recsys/
    manage.py
    recsys/
        __init__.py
        settings.py
        urls.py
        asgi.py
        wsgi.py
```

### Step 3: Start the Local Development Server

You can start your Django development server to test if everything is working:

```bash
python manage.py runserver
```

Visit `http://127.0.0.1:8000/` in your browser to verify the server is running.

### Step 4: Create a Django App

1. Create a new app within your project:
    ```bash
    python manage.py startapp reviewmaster
    ```

Your app structure will look like this:
```
reviewmaster/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
```

### Step 5: Add the App to Your Project

In `recsys/settings.py`, add the new app to the `INSTALLED_APPS` section:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'reviewmaster.apps.ReviewmasterConfig',
]
```

### Step 6: Define Models for the App

In `reviewmaster/models.py`, define the models for `User`, `Business`, and `Review`:

```python
from django.db import models

class User(models.Model):
    name = models.CharField(max_length=200)

    def __str__(self):
        return self.name

class Business(models.Model):
    name = models.CharField(max_length=200)
    city = models.CharField(max_length=200)
    users = models.ManyToManyField(User, through='Review')

    def __str__(self):
        return self.name

class Review(models.Model):
    RATING_STARS = [
        (1, 'One Star'),
        (2, 'Two Star'),
        (3, 'Three Star'),
        (4, 'Four Star'),
        (5, 'Five Star'),
    ]
    user = models.ForeignKey(User, on_delete=models.CASCADE)
    business = models.ForeignKey(Business, on_delete=models.CASCADE)
    rating = models.IntegerField(choices=RATING_STARS)

    def __str__(self):
        return f'{self.user} rates {self.business} at {self.rating} star.'
```

### Step 7: Apply Migrations

Generate the migrations for your models and apply them:

```bash
python manage.py makemigrations
python manage.py migrate
```

### Step 8: Create Views for the App

In `reviewmaster/views.py`, create views to list users, businesses, and their details:

```python
from django.shortcuts import render, get_object_or_404
from .models import User, Business

def user_index(request):
    users = User.objects.all()
    return render(request, 'reviewmaster/user_index.html', {'users': users})

def user_detail(request, user_id):
    user = get_object_or_404(User, pk=user_id)
    return render(request, 'reviewmaster/user_detail.html', {'user': user})

def business_index(request):
    businesses = Business.objects.all()
    return render(request, 'reviewmaster/business_index.html', {'businesses': businesses})

def business_detail(request, business_id):
    business = get_object_or_404(Business, pk=business_id)
    return render(request, 'reviewmaster/business_detail.html', {'business': business})
```

### Step 9: Set Up URLs

1. In `recsys/urls.py`, include the URLs from the `reviewmaster` app:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('reviewmaster/', include('reviewmaster.urls')),
    path('admin/', admin.site.urls),
]
```

2. In `reviewmaster/urls.py`, define the URLs for your views:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('users/', views.user_index, name='user_index'),
    path('users/<int:user_id>/', views.user_detail, name='user_detail'),
    path('businesses/', views.business_index, name='business_index'),
    path('businesses/<int:business_id>/', views.business_detail, name='business_detail'),
]
```

### Step 10: Create Templates

1. Create a `templates/reviewmaster/` folder for your HTML templates.

2. Add templates such as `user_index.html`, `user_detail.html`, `business_index.html`, and `business_detail.html`:

- **`user_index.html`:**
    ```html
    {% if users %}
    <ul>
        {% for user in users %}
        <li><a href="{% url 'user_detail' user.id %}">{{ user.name }}</a></li>
        {% endfor %}
    </ul>
    {% else %}
    <p>No users are available.</p>
    {% endif %}
    ```

- **`user_detail.html`:**
    ```html
    <p>User Id: {{ user.id }}</p>
    <p>User Name: {{ user.name }}</p>
    ```

### Step 11: Admin Setup

1. Register the models in `reviewmaster/admin.py` so that you can manage them via the Django admin interface:

```python
from django.contrib import admin
from .models import User, Business, Review

admin.site.register(User)
admin.site.register(Business)
admin.site.register(Review)
```

2. Create a superuser to access the Django admin panel:

```bash
python manage.py createsuperuser
```

Follow the prompts to create your admin account.

### Step 12: Run the Development Server

Finally, run the server again to test your app:

```bash
python manage.py runserver
```

You can now visit:

- `http://127.0.0.1:8000/reviewmaster/users/` to view the list of users.
- `http://127.0.0.1:8000/reviewmaster/businesses/` to view the list of businesses.
- `http://127.0.0.1:8000/admin/` to manage users, businesses, and reviews via the admin interface.

![[Screen Shot 2024-10-08 at 22.24.56.png]]