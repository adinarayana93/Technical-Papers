# Django Technical Paper 
## 1. Django Settings File

The main configuration file of a Django project is `settings.py`.

It contains settings such as:

```python
SECRET_KEY = "..."
DEBUG = True
ALLOWED_HOSTS = []

INSTALLED_APPS = []
MIDDLEWARE = []
DATABASES = {}
ROOT_URLCONF = "project.urls"
TEMPLATES = []
WSGI_APPLICATION = "project.wsgi.application"
```

---

## 1.1 What is `SECRET_KEY`?

`SECRET_KEY` is a secret value used by Django for cryptographic signing and security-related operations.

It is used by features such as:

- Sessions
- Password-reset related tokens
- Cryptographic signing
- Other security mechanisms

Example:

```python
SECRET_KEY = "django-insecure-example"
```

### Important

The secret key should not be exposed publicly or committed to a public repository.

For production, it is better to load it from an environment variable:

```python
import os

SECRET_KEY = os.environ.get("DJANGO_SECRET_KEY")
```

### Code-review answer

> `SECRET_KEY` is a secret cryptographic signing key used by Django for security-related operations. It should be kept private, especially in production.

---

# 2. `INSTALLED_APPS`

`INSTALLED_APPS` tells Django which applications are enabled in the project.

A default Django project commonly contains:

```python
INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
]
```

## What are these apps?

### `django.contrib.admin`

Provides Django's built-in administration interface.

Usually available at:

```text
/admin/
```

It allows authorized users to manage registered models.

### `django.contrib.auth`

Provides authentication and authorization features:

- Users
- Passwords
- Login/logout
- Groups
- Permissions

### `django.contrib.contenttypes`

Provides Django's content type framework. It is used by some generic relationships and permission-related features.

### `django.contrib.sessions`

Provides session support.

Example:

```python
request.session["username"] = "Adi"
```

### `django.contrib.messages`

Provides temporary messages to users.

Example:

```python
messages.success(request, "Saved successfully!")
```

### `django.contrib.staticfiles`

Helps manage static files such as:

- CSS
- JavaScript
- Images

---

## Are there more Django apps?

Yes.

Django has many reusable components, and third-party packages can also be installed.

Your own application is also normally added:

```python
INSTALLED_APPS = [
    ...
    "polls",
]
```

For example, Django REST Framework can be added as a third-party application:

```python
INSTALLED_APPS = [
    ...
    "rest_framework",
]
```

---

# 3. What is Middleware?

Middleware is a layer that processes HTTP requests and responses.

Simplified flow:

```text
Browser
   ↓
Request
   ↓
Middleware
   ↓
URL Resolver
   ↓
View
   ↓
Response
   ↓
Middleware
   ↓
Browser
```

Middleware can:

- Process requests before the view
- Process responses after the view
- Handle security
- Manage sessions
- Support authentication
- Add security headers
- Handle some exceptions

---

# 4. Default Django Middleware

A typical Django project contains:

```python
MIDDLEWARE = [
    "django.middleware.security.SecurityMiddleware",
    "django.contrib.sessions.middleware.SessionMiddleware",
    "django.middleware.common.CommonMiddleware",
    "django.middleware.csrf.CsrfViewMiddleware",
    "django.contrib.auth.middleware.AuthenticationMiddleware",
    "django.contrib.messages.middleware.MessageMiddleware",
    "django.middleware.clickjacking.XFrameOptionsMiddleware",
]
```

## 4.1 SecurityMiddleware

```python
django.middleware.security.SecurityMiddleware
```

Provides several security-related protections and can add security headers depending on configuration.

It works with settings related to:

- HTTPS
- HSTS
- Content-Type sniffing
- Referrer policy
- SSL security

---

## 4.2 SessionMiddleware

```python
django.contrib.sessions.middleware.SessionMiddleware
```

Enables sessions.

It makes session data available through:

```python
request.session
```

Example:

```python
request.session["user_id"] = 10
```

---

## 4.3 CommonMiddleware

```python
django.middleware.common.CommonMiddleware
```

Provides common request/response processing.

One familiar setting associated with it is:

```python
APPEND_SLASH = True
```

which can help normalize URLs that do not contain a trailing slash.

---

## 4.4 CsrfViewMiddleware

```python
django.middleware.csrf.CsrfViewMiddleware
```

Provides CSRF protection.

CSRF means:

> Cross-Site Request Forgery

It protects authenticated users from malicious websites causing unwanted state-changing requests.

---

## 4.5 AuthenticationMiddleware

```python
django.contrib.auth.middleware.AuthenticationMiddleware
```

Associates the authenticated user with the request.

It allows:

```python
request.user
```

Example:

```python
if request.user.is_authenticated:
    print(request.user.username)
```

---

## 4.6 MessageMiddleware

```python
django.contrib.messages.middleware.MessageMiddleware
```

Enables Django's messages framework.

Example:

```python
messages.success(request, "Profile updated!")
```

---

## 4.7 XFrameOptionsMiddleware

```python
django.middleware.clickjacking.XFrameOptionsMiddleware
```

Helps protect against clickjacking by adding an `X-Frame-Options` response header.

---

# 5. CSRF

## What is CSRF?

CSRF stands for:

> Cross-Site Request Forgery

Suppose a user is logged into a website.

A malicious website could attempt to make that user's browser send an unwanted request to the trusted website.

Django protects normal POST forms using CSRF tokens.

Example:

```html
<form method="post">
    {% csrf_token %}

    <input type="text" name="name">

    <button type="submit">
        Save
    </button>
</form>
```

Django verifies the token on the server.

### Code-review answer

> CSRF protection makes sure that a state-changing request came from a trusted page rather than a malicious website.

---

# 6. XSS

XSS means:

> Cross-Site Scripting

XSS occurs when an attacker gets malicious JavaScript executed in another user's browser.

Example malicious input:

```html
<script>alert("Hacked")</script>
```

Django templates automatically escape normal variable output.

Example:

```django
{{ username }}
```

Django normally escapes dangerous HTML characters.

### Important

Be careful with:

```django
{{ content|safe }}
```

The `safe` filter tells Django not to escape the content.

Therefore, never mark untrusted user input as safe without proper sanitization.

### Code-review answer

> XSS is an attack where malicious scripts are injected into content and executed in another user's browser. Django template auto-escaping helps protect against common XSS cases.

---

# 7. Clickjacking

Clickjacking is an attack where a malicious page attempts to trick a user into clicking something different from what they believe they are clicking.

A common technique is using a hidden or disguised iframe.

Django provides:

```python
django.middleware.clickjacking.XFrameOptionsMiddleware
```

It can add the:

```text
X-Frame-Options
```

header.

This helps control whether a page can be loaded inside a frame.

---

# 8. Other Important Django Security Issues

## SQL Injection

SQL injection occurs when untrusted input is incorrectly inserted into SQL.

Bad example:

```python
query = "SELECT * FROM users WHERE name = '" + name + "'"
```

Django ORM normally uses parameterized queries.

Safer:

```python
User.objects.filter(username=name)
```

---

## HTTPS

HTTPS encrypts communication between the browser and server.

Production applications should normally use HTTPS.

Useful settings include:

```python
SECURE_SSL_REDIRECT = True
```

---

## Secure Cookies

Important settings include:

```python
SESSION_COOKIE_SECURE = True
CSRF_COOKIE_SECURE = True
```

These help ensure cookies are sent only over HTTPS.

---

## DEBUG

During development:

```python
DEBUG = True
```

In production:

```python
DEBUG = False
```

Debug mode can expose detailed error information, so it should not normally be enabled in production.

---

## ALLOWED_HOSTS

Example:

```python
ALLOWED_HOSTS = [
    "example.com",
]
```

It specifies the host/domain names that Django is allowed to serve.

---

# 9. What is WSGI?

WSGI means:

> Web Server Gateway Interface

It is a standard interface between Python web applications and web servers.

Django creates a `wsgi.py` file.

Example:

```python
import os

from django.core.wsgi import get_wsgi_application

os.environ.setdefault(
    "DJANGO_SETTINGS_MODULE",
    "project.settings"
)

application = get_wsgi_application()
```

Simplified flow:

```text
Browser
   ↓
Web Server
   ↓
WSGI
   ↓
Django
   ↓
View
   ↓
Response
```

### Code-review answer

> WSGI is a standard interface that allows a web server to communicate with a Python web application such as Django.

---

# 10. Models

A Django model is a Python class that represents data stored in the database.

Example:

```python
from django.db import models

class Student(models.Model):
    name = models.CharField(max_length=100)
    age = models.IntegerField()
```

Django uses this model to create and work with a corresponding database table.

---

# 11. What is `on_delete=models.CASCADE`?

It is used with relationships such as `ForeignKey`.

Example:

```python
class Author(models.Model):
    name = models.CharField(max_length=100)


class Book(models.Model):
    author = models.ForeignKey(
        Author,
        on_delete=models.CASCADE
    )
```

If an `Author` is deleted, related `Book` objects are also deleted.

### Simple answer

> `CASCADE` means that deleting the referenced parent object also deletes related child objects.

---

# 12. Other `on_delete` Options

## CASCADE

Deletes related objects.

```python
on_delete=models.CASCADE
```

## PROTECT

Prevents deletion if related objects exist.

```python
on_delete=models.PROTECT
```

## SET_NULL

Sets the foreign key to `NULL`.

Requires:

```python
null=True
```

Example:

```python
on_delete=models.SET_NULL
```

## SET_DEFAULT

Sets the relationship to its default value.

## DO_NOTHING

Does not perform deletion handling at the Django level. Database constraints may still cause an error.

---

# 13. Important Django Model Fields

## CharField

Used for relatively short strings.

```python
name = models.CharField(max_length=100)
```

---

## TextField

Used for larger text.

```python
description = models.TextField()
```

---

## IntegerField

Stores integers.

```python
age = models.IntegerField()
```

---

## PositiveIntegerField

Stores non-negative integers.

```python
count = models.PositiveIntegerField()
```

---

## BooleanField

Stores true/false values.

```python
is_active = models.BooleanField(default=True)
```

---

## FloatField

Stores floating-point numbers.

```python
rating = models.FloatField()
```

---

## DecimalField

Stores fixed-precision decimal values.

Useful for financial values.

```python
price = models.DecimalField(
    max_digits=10,
    decimal_places=2
)
```

---

## DateField

Stores a date.

```python
birth_date = models.DateField()
```

---

## DateTimeField

Stores date and time.

```python
created_at = models.DateTimeField(
    auto_now_add=True
)
```

`auto_now_add=True` sets the value when the object is first created.

```python
updated_at = models.DateTimeField(
    auto_now=True
)
```

`auto_now=True` updates the value whenever the object is saved.

---

## EmailField

Used for email addresses.

```python
email = models.EmailField()
```

---

## ForeignKey

Represents a many-to-one relationship.

```python
author = models.ForeignKey(
    Author,
    on_delete=models.CASCADE
)
```

Many books can belong to one author.

---

## OneToOneField

Represents a one-to-one relationship.

```python
profile = models.OneToOneField(
    User,
    on_delete=models.CASCADE
)
```

---

## ManyToManyField

Represents a many-to-many relationship.

```python
courses = models.ManyToManyField(Course)
```

Many students can take many courses.

---

# 14. Important Field Options

## `null`

Controls whether the database can store SQL `NULL`.

```python
age = models.IntegerField(null=True)
```

## `blank`

Controls whether a field can be empty during validation/forms.

```python
name = models.CharField(
    max_length=100,
    blank=True
)
```

### Remember

```text
null  → database level
blank → validation/form level
```

---

## `default`

Provides a default value.

```python
is_active = models.BooleanField(default=True)
```

## `unique`

Requires values to be unique.

```python
email = models.EmailField(unique=True)
```

## `primary_key`

Makes a field the primary key.

```python
student_id = models.IntegerField(
    primary_key=True
)
```

---

# 15. Django Validators

Validators check whether a value satisfies a condition.

Example:

```python
from django.core.validators import MinValueValidator

age = models.IntegerField(
    validators=[
        MinValueValidator(18)
    ]
)
```

The value must be at least 18 when validation is performed.

Common validators include:

- `MinValueValidator`
- `MaxValueValidator`
- `MinLengthValidator`
- `MaxLengthValidator`
- `RegexValidator`
- `EmailValidator`

Example:

```python
from django.core.validators import MaxValueValidator

marks = models.IntegerField(
    validators=[
        MaxValueValidator(100)
    ]
)
```

### Important

A validator is different from a database constraint.

Validators primarily perform application-level validation. If data must be guaranteed at the database level, appropriate database constraints should also be considered.

---

# 16. Python Module vs Python Class

This is an important basic Python question.

## Python Module

A module is normally a Python `.py` file containing Python code.

Example:

```text
models.py
```

`models.py` is a module.

A module can contain:

- Classes
- Functions
- Variables
- Imports

---

## Python Class

A class is a blueprint for creating objects.

Example:

```python
class Student:
    def __init__(self, name):
        self.name = name
```

`Student` is a class.

Create an object:

```python
student = Student("Adi")
```

### Simple difference

```text
models.py       → Python module/file

class Student   → Python class inside the module
```

---

# 17. Django ORM

ORM means:

> Object-Relational Mapping

Django ORM allows us to interact with database records using Python instead of manually writing SQL for every operation.

Example:

```python
Student.objects.all()
```

Conceptually corresponds to:

```sql
SELECT * FROM student;
```

---

# 18. Using ORM in Django Shell

Start the shell:

```bash
python manage.py shell
```

Import the model:

```python
from polls.models import Question
```

Get all records:

```python
Question.objects.all()
```

Filter:

```python
Question.objects.filter(id=1)
```

Get one:

```python
Question.objects.get(id=1)
```

Create:

```python
Question.objects.create(
    question_text="What is Django?"
)
```

Count:

```python
Question.objects.count()
```

Order:

```python
Question.objects.order_by("-id")
```

Exclude:

```python
Question.objects.exclude(id=1)
```

---

# 19. `get()` vs `filter()`

## `get()`

Used when exactly one object is expected.

```python
Student.objects.get(id=1)
```

If no object exists, Django raises:

```text
DoesNotExist
```

If multiple objects match:

```text
MultipleObjectsReturned
```

## `filter()`

Returns a QuerySet.

```python
Student.objects.filter(age=20)
```

It can return zero, one, or many records.

### Easy answer

> `get()` returns one object and expects exactly one match. `filter()` returns a QuerySet and can contain multiple objects.

---

# 20. Common ORM Lookups

```python
Student.objects.filter(age__gt=18)
```

Greater than.

```python
Student.objects.filter(age__gte=18)
```

Greater than or equal.

```python
Student.objects.filter(age__lt=30)
```

Less than.

```python
Student.objects.filter(age__lte=30)
```

Less than or equal.

```python
Student.objects.filter(
    name__icontains="adi"
)
```

Case-insensitive contains.

```python
Student.objects.filter(
    age__in=[18, 20, 22]
)
```

Matches values in the given list.

---

# 21. Turning ORM into SQL

In Django Shell:

```python
qs = Student.objects.filter(age__gte=18)
```

Then:

```python
print(qs.query)
```

Django displays the SQL representation generated for the QuerySet.

Conceptually it may look like:

```sql
SELECT ...
FROM ...
WHERE age >= 18;
```

This is useful for understanding and debugging ORM queries.

### Important

`query` is useful for inspecting the SQL. It is not normally used as a replacement for the ORM in application code.

---

# 22. QuerySet and Lazy Evaluation

Example:

```python
students = Student.objects.filter(age__gte=18)
```

A QuerySet is generally lazy.

This means Django can build the query without immediately executing it against the database.

The database is queried when the results are needed, for example:

```python
for student in students:
    print(student.name)
```

or:

```python
list(students)
```

---

# 23. What are Aggregations?

Aggregation calculates a summary value from multiple database rows.

Common aggregation functions:

- `Count`
- `Sum`
- `Avg`
- `Min`
- `Max`

Import:

```python
from django.db.models import (
    Count,
    Sum,
    Avg,
    Min,
    Max
)
```

Example:

```python
Student.objects.aggregate(
    average_age=Avg("age")
)
```

Possible result:

```python
{
    "average_age": 21.5
}
```

Another example:

```python
Student.objects.aggregate(
    total_students=Count("id")
)
```

### Simple answer

> Aggregation produces summary information from a group of database records.

---

# 24. What are Annotations?

Annotation adds a calculated value to each object in a QuerySet.

Example:

```python
from django.db.models import Count

authors = Author.objects.annotate(
    book_count=Count("book")
)
```

Now each author can have:

```python
author.book_count
```

### Aggregation vs Annotation

Aggregation:

```python
Author.objects.aggregate(
    total_books=Count("book")
)
```

Produces a summary.

Annotation:

```python
Author.objects.annotate(
    book_count=Count("book")
)
```

Adds a calculated value to each object.

### Easy memory trick

```text
aggregate  → overall summary

annotate   → value attached to each object
```

---

# 25. `select_related()` and `prefetch_related()`

These are important ORM optimization techniques.

## `select_related()`

Mainly used with:

- ForeignKey
- OneToOneField

It generally uses SQL JOINs.

Example:

```python
Book.objects.select_related("author")
```

---

## `prefetch_related()`

Useful for:

- ManyToMany relationships
- Reverse ForeignKey relationships
- Related collections

Example:

```python
Author.objects.prefetch_related("book_set")
```

It generally performs separate queries and combines the results in Python.

### Easy difference

```text
select_related
→ JOIN
→ ForeignKey / OneToOne

prefetch_related
→ separate queries
→ ManyToMany / reverse relationships
```

---

# 26. What is a Migration File?

A migration is a Python file that describes changes to the database schema.

Suppose we add:

```python
class Student(models.Model):
    name = models.CharField(max_length=100)
```

Run:

```bash
python manage.py makemigrations
```

Django creates a migration file.

Then:

```bash
python manage.py migrate
```

applies the migration to the database.

---

# 27. Why are Migrations Needed?

Migrations keep track of database schema changes over time.

Example:

```text
0001_initial.py
       ↓
Create Student table
       ↓
0002_add_email.py
       ↓
Add email field
       ↓
0003_change_name.py
       ↓
Change field
```

They allow developers to reproduce database schema changes across environments.

For example:

```text
Developer machine
       ↓
Testing
       ↓
Production
```

---

# 28. `makemigrations` vs `migrate`

## `makemigrations`

Creates migration files.

```bash
python manage.py makemigrations
```

Think:

> Create instructions for changing the database.

## `migrate`

Applies those migration files.

```bash
python manage.py migrate
```

Think:

> Perform those database changes.

---

# 29. What is a SQL Transaction?

A transaction is a group of database operations treated as one logical unit.

Example: transferring money.

```text
1. Subtract ₹100 from Account A
2. Add ₹100 to Account B
```

Both operations should succeed together.

If the second operation fails, the first operation should also be rolled back.

---

# 30. ACID Properties

Transactions are commonly described using ACID.

### Atomicity

All operations succeed, or none are applied.

### Consistency

The database remains in a valid state.

### Isolation

Concurrent transactions should not improperly interfere with each other.

### Durability

Committed changes remain stored according to the database's durability guarantees.

---

# 31. What are Atomic Transactions?

Django provides:

```python
from django.db import transaction
```

Use:

```python
with transaction.atomic():
    account_a.balance -= 100
    account_a.save()

    account_b.balance += 100
    account_b.save()
```

The operations inside the block are treated as one atomic transaction.

If an exception causes the transaction to fail, Django can roll back the changes made within the transaction.

### Code-review answer

> `transaction.atomic()` groups database operations into an atomic transaction so that they can be committed together or rolled back if the transaction fails.

---

# 32. SQL Transaction vs `transaction.atomic()`

SQL transaction is a database concept.

Example:

```sql
BEGIN;

UPDATE account
SET balance = balance - 100
WHERE id = 1;

UPDATE account
SET balance = balance + 100
WHERE id = 2;

COMMIT;
```

Django provides a Python API:

```python
with transaction.atomic():
    ...
```

So:

```text
SQL transaction
    ↓
Database-level concept

transaction.atomic()
    ↓
Django API for transaction management
```

---

# 33. Important ORM Operations to Remember

| Operation | Purpose |
|---|---|
| `all()` | Get all records |
| `filter()` | Get matching records |
| `get()` | Get exactly one object |
| `exclude()` | Exclude matching records |
| `create()` | Create and save an object |
| `save()` | Save object |
| `delete()` | Delete object |
| `count()` | Count records |
| `exists()` | Check whether records exist |
| `first()` | First object |
| `last()` | Last object |
| `order_by()` | Sort records |
| `values()` | Return dictionaries |
| `values_list()` | Return tuples/values |
| `aggregate()` | Calculate summary |
| `annotate()` | Add calculated values |

---

# 34. Django Request-Response Flow

A very useful flow to remember for code review:

```text
Browser
   |
   | HTTP Request
   ↓
Django
   |
   ↓
Middleware
   |
   ↓
URL Resolver
   |
   ↓
View
   |
   ↓
Django ORM
   |
   ↓
Database
   |
   ↓
View
   |
   ↓
Template / HTTP Response
   |
   ↓
Middleware
   |
   ↓
Browser
```

For example:

```text
GET /polls/
     ↓
urls.py
     ↓
views.index()
     ↓
Question.objects.all()
     ↓
Database
     ↓
Template
     ↓
HTML Response
```

---
