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
     <p>Which Data we wil send frontend to BackEnd</p>
  

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


 - Go to Settings.py and register the app

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
    - It is mandatory to register the app after create App every time 

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

 - Model Data ------Serialization---> JSON
     - JSON -> JavaScript Object Notation
<p>Serializers are used to convert complex data types, such as Django model instances, into Python data types that can be easily rendered into JSON, XML, or other content types.</p>


![serializer](serialzer.webp)


 ## Model Serializer
 
   - we can work with multiple method (GET,POST,PUT/PATCH,DELETE) via ModelSerializer
       - PUT : We can update whole model via put method
       - PATCH : Slide change .

```bash
from rest_framework import serializers
from . import models 

class AppointmentSerializer(serializers.ModelSerializer):
#    patient = serializers.StringRelatedField(many=False)
   class Meta:
        model = models.Appointment
        fields = '__all__'
```

## Views for ModelViewSet

```bash
from django.shortcuts import render
from . import models , serializers
from rest_framework import viewsets

class AppointmentViewSet(viewsets.ModelViewSet):
    queryset = models.Appointment.objects.all()
    serializer_class = serializers.AppointmentSerializer
```

#### Custom Query set in Views.py
```bash
from django.shortcuts import render
from . import models , serializers
from rest_framework import viewsets

class AppointmentViewSet(viewsets.ModelViewSet):
    queryset = models.Appointment.objects.all()
    serializer_class = serializers.AppointmentSerializer

    def get_queryset(self):
        queryset = super().get_queryset() # Sob gula query nilam 6 no line thake 
        patient_id = self.request.query_params.get("patient_id")

        if patient_id:
            queryset = queryset.filter(patient_id=patient_id)
        return queryset
    
    def get_queryset(self):
        queryset = super().get_queryset() # Sob gula query nilam 6 no line thake 
        doctor_id = self.request.query_params.get("doctor_id")

        if doctor_id:
            queryset = queryset.filter(doctor_id=doctor_id)
        return queryset
```

## Urls.py
```bash
from django.urls import path, include
from rest_framework.routers import DefaultRouter
from .views import AppointmentViewSet
router = DefaultRouter() # Our Router
router.register(r'list', AppointmentViewSet) # Antenna Of Router
urlpatterns = [
    path('',include(router.urls))

]
```
#### Handle mupltiple urls for an App
```bash
from django.urls import path, include
from rest_framework.routers import DefaultRouter
from . import views
router = DefaultRouter() # Our Router
router.register(r'list', views.DoctorViewSet) # Antenna Of Router
router.register(r'designation', views.DesignationViewSet)
router.register(r'specialization', views.SpecializationViewSet)
router.register(r'availableTime', views.AvailableTimeViewSet)
router.register(r'review', views.ReviewViewSet)
urlpatterns = [
    path('',include(router.urls))

]
```

</details>

<details>
<summary><h3 style="color: blue;">Authentication</h3></summary>
  <details>
      <summary><h4 style="color: blue;">Registration</h4></summary>
   
  #### Task For Registration Serializer  : 
         
 1. Import User from django contrib auth models
 2. Make a class for registraion and inharit ModelSerializer from serializers
 3. Make a Meta class - (The Meta class is a conventional name for a class inside another 
             class that is used to hold metadata or configuration options.)
 4. In Meta Class we will Work with User models and take the field that we need . In User 
          modele there is no field for confirm Password that the reason we have to build it vai 
          serializers.CharFirld
 5. We have to save the info to database that why we have call the save funtion . Grap all the value from the field vai validated_data insted of cleaned_data in form class we use validated_data for  serializer
 6. Now we have to compaire the password and Confirm Password is they equal or not . if not then raise the error
 7. we have to filter the email and have to ensure that this is not exists in our database
 8. Then  We have to update the User and set the pass and by delault the creating account will be is_active false we set it trure via email and finally seve the acount and retun account

    #### Code :

```bash
from rest_framework import serializers
from django.contrib.auth.models import User


class RegistrationSerializer(serializers.ModelSerializer):
    confirm_password = serializers.CharField(required = True)
    class Meta:
        model = User
        fields = ['username','first_name','last_name','email','password','confirm_password']

    def save(self):
        username = self.validated_data['username']
        first_name = self.validated_data['first_name']
        last_name = self.validated_data['last_name']
        password = self.validated_data['password']
        password2 = self.validated_data['confirm_password']
        email = self.validated_data['email']

        if password!=password2:
            raise serializers.ValidationError({"error":"Password Does not Match "})
        if User.objects.filter(email=email).exists():
            raise serializers.ValidationError({"error":"Email Already Exist  "})
        
        account = User(username= username,email=email,first_name=first_name,last_name=last_name)
        print(account)
        account.set_password(password)
        account.is_active= False
        account.save()
        return account
```
   <h3> Views </h3>
        
   #### Task For Registration View   :  
     
  1. We will use  APIView for our Registration view class Just because we will play with Post reques not any kind of request . We also can use ModelviewSet for simplicity we will use APIview
  2. Then we need the Serilizer calss there we will inharit all the field from serializer
  3. We are paly with Post so we need to implement post function
  4. we need to to grap all the data from serializer calas via data.request and store it Serializer variable
  5. if siralizer Variable is currect all info for post request then we have to save the siralizer in data base and we will store it another variable that can be user
  6. We will genarate Token an Uid and make a confirmation mail via token and uid64 after endpoint we will use it active/{uid}/{token}
  7. Then we will work with sending email the is prity simple . we need to crate a app via 2 step vaifications and get the password for email and set it in .evn we need email body ,email that get the value from EmailMultiAlternatives function and this function take 3 parameter and attach alternative and finally send the email to activate the account
  8. if all is ok and send a ok response and if error then send and error Respose

  #### Code :

```bash
from .serializers import PatientSerializer,RegistrationSerializer
from rest_framework.views import APIView
from rest_framework.response import Response
from django.contrib.auth.tokens import default_token_generator
from django.utils.http import urlsafe_base64_encode,urlsafe_base64_decode
from django.utils.encoding import force_bytes
from django.contrib.auth.models import User
from django.contrib.auth import authenticate,login,logout
from rest_framework.authtoken.models import Token
# For sending Email

from django.core.mail import EmailMultiAlternatives
from django.template.loader import render_to_string


class UserRegistrationApiView(APIView):
    serializer_class  = RegistrationSerializer
    

    def post(self,request):
        serializer = self.serializer_class(data=request.data)

        if serializer.is_valid():
            user = serializer.save()
            print(user)
            token = default_token_generator.make_token(user)

            print('Token: ' ,token)
            uid = urlsafe_base64_encode(force_bytes(user.pk))
            print("Uid : ", uid)
            confirm_link = f"http://127.0.0.1:8000/patient/active/{uid}/{token}"
            email_subject = "Confirm Your Email"
            email_body = render_to_string('confirmEmail.html',{"ConfirmLink" : confirm_link})
            email = EmailMultiAlternatives(email_subject,'',to=[user.email])
            email.attach_alternative(email_body,'text/html')
            email.send()
            return Response('Check Your Mail for Confirmation')
      
        return Response(serializer.errors)
```
 
 
  

   </details>
 
  

</details>

