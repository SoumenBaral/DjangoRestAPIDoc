![logo](https://soshace.com/wp-content/uploads/2021/01/879-png-3.png)

<details>
<summary><h3 style="color: blue;">REST API</h3></summary>
 
 REST API is  Representational State Transfer Application Programming Interface

 # REST API Have 4 points
  - End point ( last point from the base ulr where an API or service can be accessed)
  - Method ( POST, PUT/PATCH, GET, DELETE)
    
     - POST : POST for creating data --- C
     - PUT : PUT for updating data  --- U
     - GET : GET for retrieving data  --- R
     - DELETE : DELETE for deleting data --- D
       
     <p>Together these four operations make up the basic operations of storage management known as CRUD</p>   
  - Headers
        <p>headers provide additional information about the request or the response.</p>
        
     - Status Code:
        | Status      | Description     |
        | :--------   | :---------------|
        | 200         | ok              |
        | 404         | Page not Found  |
        | 505         | Server Error    |

          
    
    
  - Data 
  

</details>

<details>
<summary><h3 style="color: blue;">Installation & Essential Stuff </h3></summary>

  
## Run 

 - Install Django  REST Framework

```bash
pip install djangorestframework
```
 - Markdown support for the browsable API.
   
```bash
pip install markdown       
```
 - Filtering support
   
```bash
pip install django-filter  
```


 - Go to Sattings.py and register the app

```bash
 INSTALLED_APPS = [
    ...
    'rest_framework',
]
```
- Then Create A App 

```bash
django-admin startapp Myapp
```

- register the app
    - It is mendatory to register the app after create App Everytime 

```bash
 INSTALLED_APPS = [
    ...
    'Myapp',
]
```

- Then Create A Model
  
```bash
from django.db import models

class Service(models.Model):
    name = models.CharField(max_length =30)
    description =models.TextField()
    image = models.ImageField(upload_to='service/images/') 

    def __str__(self) -> str:
        return self.name
```
After Creating a Model Every time we have  makemigrations and migrate and register it in Admin panel  

</details>
<details>
<summary><h3 style="color: blue;">Serializers</h3></summary>

  
Serializers are used to convert complex data types, such as Django model instances, into Python data types that can be easily rendered into JSON, XML, or other content types.

 
![logo](https://phitron.gitbook.io/django/module_21/module_21_2)

</details>


