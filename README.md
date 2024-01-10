![logo](https://soshace.com/wp-content/uploads/2021/01/879-png-3.png)

<details>
<summary><h3 style="color: blue;">REST API</h3></summary>
 
 REST API is  Representational State Transfer Application Programming Interface

 # REST API Have 4 points
  - End point ( last point from the base ulr where an API or service can be accessed)
  - Method ( POST, PUT/PATCH, GET, DELETE)
    
     - POST      : POST for creating data --- C
     - PUT/PATCH : PUT for updating data  --- U
     - GET       : GET for retrieving data  --- R
     - DELETE    : DELETE for deleting data --- D
       
       <p>Together these four operations make up the basic operations of storage management known as CRUD</p>   
  - Headers
  - Data 
  

</details>

<details>
<summary><h3 style="color: blue;">Installation & Essential Stuf </h3></summary>

  
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


</details>
<details>
<summary><h3 style="color: blue;">Serializers</h3></summary>

  
Serializers are used to convert complex data types, such as Django model instances, into Python data types that can be easily rendered into JSON, XML, or other content types.

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


</details>
