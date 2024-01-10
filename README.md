![logo](https://soshace.com/wp-content/uploads/2021/01/879-png-3.png)


<details>
<summary><h3 style="color: blue;">Installation & Essential Stuf </h3></summary>

  
## Run 

 - Install Django  REST Framework

```bash
pip install djangorestframework
pip install markdown       # Markdown support for the browsable API.
pip install django-filter  # Filtering support
```

 - Go to Sattings.py and register the app

```bash
 INSTALLED_APPS = [
    ...
    'rest_framework',
]
```

 - Create A new App

```bash
  django-admin startapp myApp
```

 - Migrations

```bash
 python3 manage.py makemigrations
```
 - Migrate

```bash
 python3 manage.py migrate
```

- Create Superuser 
```bash
  python3 manage.py createsuperuser
```
- Run Server
 ```bash
  python3 manage.py runserver
 ```


</details>
