# DjangoRestStartingTemplate
Just a starting template for django rest api



Needed
firstly we installed django
Then we created a project with django-admin startproject <name>
Then we installed djangorestframework by using pip install djangorestframework
Then we create a new app by python manage.py startapp <name>

Then in the settings.py of project file we will add 'rest_framework' andn '<NameOfApp>' in the INSTALLED APPS section

Then we will create a new folder and name api
    Inside the api make a python file named '__init__.py' and views.py

In views.py:
    
    from rest_framework.response import Response
    from rest_framework.decorators import api_view
    
    @api_view(['GET'])
    def getData(request):
        #create a dictionary
        return Response(dictionary)

Then create a new file named urls.py:
    from django.urls import path
    from . import views
    
    urlpatterns = [
        path('',views.getData)
    ]

In Projects urls.py append include along with path and append:
   
    path('',include('api.urls')),


Then You may run the server and see if it works !!


In the app created
Go to models.py:
    
    class Item(models.Model):
        name = models.CharField(max_length=200)
        created = models.DateTimeField(auto_now_add=True)

Then you can make migrations with python manage.py makemigrations
and migrate with python manage.py migrate

TO ADD some data manually you can:
    
    python manage.py shell
    >> from <name>.models import Item
    >> Item.objects.create(name="Item #1")
    >> exit()

create new folder 'serializers.py' in the api folder:
    
    from rest_framework import serializers
    from base.models import Item
    class ItemSerializer(serializers.ModelSerializer):
        class Meta:
            model = Item
            fields = '__all__'



open views.py on api folder and append:
    
    from base.models import Item
    from .serializers import ItemSerializer
    #change the following as:
        @api_view(['GET'])
        def getData(request):
            items = Item.objects.all()
            serializer = ItemSerializer(items,many=True)
            return Response(serializer.data)
        
        @api_view(['POST"])
        
        def addItem(request):
            serializer = ItemSerializer(data=request.data)
            if serializer.is_valid():
                serializer.save()
            return Response(serializer.data)


add in urls.py of api:
   
    path('add/',views.addItem)

AGAIN CHECK WITH /add



























    
