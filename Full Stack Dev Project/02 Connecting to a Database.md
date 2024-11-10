### Step 1: **Download and Install MySQL**
- Visit the official MySQL website: [https://dev.mysql.com/downloads/mysql/](https://dev.mysql.com/downloads/mysql/).
- During the installation, set a password for the root user and remember it for later use.

### Step 2: **Install MySQL Python Client**
- Use the following command to install the MySQL client for Python: 
  ```
  pip install mysqlclient
  ```

- `export PATH="/usr/local/mysql/bin:$PATH”`
- add to `.bash_profile` or `.zshrc`
- https://www.macports.org/install.php
- `sudo port install pkgconfig`
- `export PKG_CONFIG_PATH="/usr/local/mysql/lib/pkgconfig"`
- add to `.bash_profile`or `.zshrc`
### Step 3: **Connecting to MySQL**
- Open a terminal and connect to the MySQL server using:
  ```
  mysql --user=root -p
  ```
- You’ll be prompted to enter the password set during installation.

### Step 4: **Create a Database**
- In the MySQL command line, create a new database:
  ```
  CREATE DATABASE mydb;
  ```

### Step 5: **Configure Django to Use the MySQL Database**
- In your Django project, update the `DATABASES` configuration in `recsys/settings.py`:
  ```python
  DATABASES = {
      'default': {
          'ENGINE': 'django.db.backends.mysql',
          'NAME': 'mydb',
          'USER': 'root',
          'PASSWORD': 'password',
          'HOST': 'localhost',
          'PORT': 3306,
      },
  }
  ```

### Step 6: **Update Django Models**
- Define models in `reviewmaster/models.py` for `User`, `Business`, and `Review`:
  ```python
  from django.db import models
  
  class User(models.Model):
      name = models.CharField(max_length=200)
      
  class Business(models.Model):
      name = models.CharField(max_length=200)
      city = models.CharField(max_length=200)
      users = models.ManyToManyField(to=User, blank=True, through='Review')
      
  class Review(models.Model):
      RATING_STARS = [
          (1, 'One Star'),
          (2, 'Two Stars'),
          (3, 'Three Stars'),
          (4, 'Four Stars'),
          (5, 'Five Stars'),
      ]
      user = models.ForeignKey(User, on_delete=models.CASCADE)
      business = models.ForeignKey(Business, on_delete=models.CASCADE)
      rating = models.IntegerField(choices=RATING_STARS)
  ```

### Step 7: **Migrate Database Changes**
- Run the following commands to create database tables based on the Django models:
  ```
  python manage.py makemigrations
  python manage.py migrate
  ```

### Step 8: **Interacting with the Database**
- Use Django's interactive shell or the MySQL command line for operations:
  - **Show databases:** 
    ```
    mysql> show databases;
    ```
  - **Select a database:** 
    ```
    mysql> use mydb;
    ```
  - **Show tables in the database:**
    ```
    mysql> show tables;
    ```

### Step 9: **Using Django ORM vs. Raw SQL**
- Examples include filtering, retrieving, and saving records using both Django's ORM and equivalent raw SQL queries:
  - **Django ORM:** 
    ```python
    from reviewmaster.models import User
    User.objects.filter(name='John Doe')
    ```
  - **Equivalent SQL:** 
    ```
    mysql> select * from reviewmaster_user where name = 'John Doe';
    ```
