# Django Crash Course Commands

```bash
# Install pipenv
pip install pipenv
```
```bash
# Create Venv
pipenv shell
```
```bash
# Install Django
pipenv install django
```
```bash
# Create project
django-admin startproject pollster
cd pollster
```
```bash
# Run server on http: 127.0.0.1:8000 (ctrl+c to stop)
python manage.py runserver
```
```bash
# Run initial migrations
python manage.py migrate
```
```bash
# Create polls app
python manage.py startapp polls
```
```bash
# Create polls migrations
python manage.py makemigrations polls
```
```bash
# Run migrations
python manage.py migrate
```
```bash
# Using the shell
python manage.py shell

>>>  from polls.models import Question, Choice
>>>  from django.utils import timezone
>>>  Question.objects.all()
>>>  q = Question(question_text="What is your favorite Python Framework?", pub_date=timezone.now())
>>>  q.save()
>>>  q.id
>>>  q.question_text
>>>  Question.objects.all()
>>>  Question.objects.filter(id=1)
>>>  Question.objects.get(pk=1)
>>>  q = Question.objects.get(pk=1)
>>>  q.choice_set.all()
>>>  q.choice_set.create(choice_text='Django', votes=0)
>>>  q.choice_set.create(choice_text='Flask', votes=0)
>>>  q.choice_set.create(choice_text='Flask', votes=0)
>>>  q.choice_set.all()
>>>  quit()
```
```bash
# Create admin user
python manage.py createsuperuser
```
```bash
# Create polls app
python manage.py startapp polls
```
```bash
# Write models in models.py
from django.db import models

class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')

    def __str__(self):
        return self.question_text

class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)

    def __str__(self):
        return self.choice_text
```
```bash
# Add config for polls in pollster>settings.py INSTALLED_APPS section 
'polls.apps.PollsConfig',
```
```bash
# Create migration files and tables
python manage.py makemigrations polls
```
```bash
# Migrate to make the tables in the database
python manage.py migrate
```bash
# (Optional) Manipulate the data from shell
python manage.py shell
```
```bash
# Import models
from polls.models import Question, Choice
```
```bash
# Make queries. It is empty now as we have not written anything in yet
Question.objects.all()
```
```bash
# Import time zone
from django.utils import timezone
```bash
# Write a question with the timing tag 'now'
q = Question(question_text="What is your favourite Python framework?", pub_date=timezone.now())
```
```bash
# Save it
q.save()
```
```bash
# Get question id
q.id
```
```bash
# Get info of the question
q.question_text
```
```bash
# Check the question
Question.objects.all()
```
```bash
# Filter question by id 
Question.objects.filter(id=1)
```
```bash
# Instead of filter, we can get the question by primary key (pk)
Question.objects.get(pk=1)
```
```bash
# Assign q the first question
q = Question.objects.get(pk=1)
```
```bash
# We can check choices of q question. There is no choice for now.
q.choice_set.all()
```
```bash
# Add choices to the question
q.choice_set.create(choice_text:"Django", votes0)
q.choice_set.create(choice_text:"Flask", votes0)
q.choice_set.create(choice_text:"Web2py", votes0)
```
```bash
# We can now see the choices of q question
q.choice_set.all()
```
```bash
# Quiting shell
quit()
```
```bash
# Setting up admin area
python manage.py createsuperuser
```
```bash
# When promoted, fill in your personal username, email, password and press enter

```
```bash
# Now you can run the server and login to the linked port 8000 on your browser.
python manage.py runserver
```
```bash
# Add admin functionality in polls>admin.py
from django.contrib import admin

from .models import Question, Choice

class ChoiceInline(admin.TabularInline):
    model = Choice
    extra = 3

class QuestionAdmin(admin.ModelAdmin):
    fieldsets = [(None, {'fields': ['question_text']}),
    ('Date information', {'fields': ['pub_date'], 'classes': ['collapse']}), ]
    inlines = [ChoiceInline]

# admin.site.register(Question)
# admin.site.register(Choice)
admin.site.register(Question, QuestionAdmin)
```
```bash
# Change the header and title of the admin page in polls>admin.py
admin.site.site_header = "Pollster Admin"
admin.site.site_title = "Pollster Admin Area"
admin.site.index_title = "Welcome to the Pollster Admin Area"
```
```bash
# Now we have worked enough on backend. We will move on working on fronend.
# Bring the models to pollster>polls>views.py
from .models import Question, Choice
```
```bash
# Get questions and display them in views.py
def index(request):
    return render(request, 'polls/index.html')
```
```bash
# Create a new file in polls called urls.py and write in
from django.urls import path

from . import views

app_name = 'polls'
urlpatterns = [
    path('', views.index, name='index')
]
```bash
# There is another urls.py file in pollster, edit it to include as follows
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('polls/', include('polls.urls')),
    path('admin/', admin.site.urls),
]
# This is to include the polls.urls whenever we go to polls
```
```bash
# Create a template folder in pollster and another folder called polls inside it

```
```bash
# Inside the newly created polls file, create index.html and write in
POLLS
```
```bash
# Import os in pollster>settings.py on top of the file
import os
```
```bash
# Write in DIRS of TEMPLATES section in polls>setting.py
        'DIRS': [os.path.join(BASE_DIR, 'templates')],
# Now, the the system is referring to the directory of the templates
```
```bash
# Create base.html inside templates folder and write in
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content"width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content=ie=edge">
  <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.1/css/bootstrap.min.css" integrity="sha384-VCmXjywReHh4PwowAiWNagnWcLhlEJLA5buUprzK8rxFgeH0kww/aWY76TfkUoSX" crossorigin="anonymous">
  <title>Pollster{% block title %}{% endblock %}</title>
</head>

<body>
    <div class="container">
        <div class="row">
            <div class="col-md-6 m-auto">
                {% block content %}{% endblock %}
            </div>
        </div>
    </div>
</body>
</html>
```
```bash
# Write in pollster>templates>polls>index.html
{% extends 'base.html' %} 
{% block content %} 
   POLLS
{% endblock %}
# Bootstrap is enabled and the title of the page is displayed above browser address
```
```bash
# Edit pollster>polls>views.py as follows

# Get questions and display them
def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    context = {'latest_question_list': latest_question_list}
    return render(request, 'polls/index.html', context)
```
```bash
# Edit pollster>templates>polls>index.html as follows
{% extends 'base.html' %} 
{% block content %} 
    <h1 class="text-center mb-3">Poll Questions</h1>
    {% if latest_question_list %}
        {% for question in latest_question_list %}
            <div class="card mb-3">
                <div class="card-body">
                   <p class="lead">{{ question.question_text }}</p>
                   <a href="{% url 'polls:detail' question.id %}" class="btn 
                   btn-primary btn-sm">Vote Now</a> # link to votes
                   <a href="{% url 'polls:results' question.id %}" class="btn 
                   btn-secondary btn-sm">Results</a> # link to results
                </div>
            </div>            
        {% endfor %}
    {% else %}
        <p>No polls available</p>
    {% endif %}
{% endblock %}
```
```bash
# Add the following lines to pollster>polls>views.py

# Show specific question and choices
def detail(request, question_id):
    try:
      question = Question.objects.get(pk=question_id)
    except Question.DoesNotExist:
        raise Http404("Question does not exit")
    return render(request, 'polls/detail.html', { 'question': question})
```
```bash
# Edit pollster>polls>url.py as follows
from django.urls import path

from . import views

app_name = 'polls'
urlpatterns = [
    path('', views.index, name='index'),
    path('<int:question_id>/', views.detail, name='detail')
)
]
```
```bash
# Add the following lines to pollster>polls>views.py
# Get question and display results
def results(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    return render(request, 'polls/results.html', { 'question': question })
```
```bash
# # Edit by adding result line to pollster>polls>url.py as follows
from django.urls import path

from . import views

app_name = 'polls'
urlpatterns = [
    path('', views.index, name='index'),
    path('<int:question_id>/', views.detail, name='detail'),
    path('<int:question_id>/results/', views.results, name='results')
]
```
```bash
# Create detail.html under pollster>templates>polls and write in
{% extends 'base.html' %}
{% block content %}
<a class="btn btn-secondary btn-sm mb-3" href="{% url 'polls:index' %}">Back To Polls</a>
<h1 class="text-center mb-3">{{ question.question_tex}}</h1>

{% if error_message %}
<p class="alert alert-danger">
    <strong>{{ error_message }}</strong>
</p>
{% endif %}

<form action="{% url 'polls:vote' question.id %}" method="post">
    {% csrf_token %} # forgery protection
    {% for choice in question.choice_set.all %}
        <div class="form-check">
            <input
             type="radio"
             name="choice"
             class="form-check-input"
             id="choice{{ forloop.counter }}"
             value="{{ choice.id }}"
            />
            <label for="choice{{ forloop.counter }}">{{ choice.choice_text }}</label
            >
        </div>
    {% endfor %}
    <input type="submit" value="Vote" class="btn btn-success btn-lg btn-block mt-4" />
</form>
{% endblock %}
```
```bash
# Create results.html under pollster>templates>polls and write in
{% extends 'base.html' %}
{% block content %}
<h1 class="mb-5 text-center">{{ question.question_text }}</h1>

<ul class="list-group mb-5">
    {% for choice in question.choice_set.all %}
    <li class="list-group-item">
        {{ choice.choice_text }} <span class="badge badge-success float-right">{{
        choice.votes }} vote{{ choice.votes | pluralize }}</span>
    </li>
    {% endfor %}
</ul>

<a class="btn btn-secondary" href="{% url 'polls:index' %}>Back To Polls"></a>
<a class="btn btn-dark" href="{% url 'polls:detail' question.id %}">Vote again?</a>
{% endblock %}
```
```bash
# Add the following lines to pollster>polls>views.py
# Vote for a question choice
def vote(request, question_id):
    # print(request.POST('choice'))
    question = get_object_or_404(Question, pk=question_id)
    try:
        selected_choice = question.choice_set.get(pk=request.POST('choice'))
    except (KeyError, Choice.DoesNotExist):
        # Redisplay the question voting format
        return render(request, 'polls/detail.html', {
            'question': question,
            'error_message': "You didn't select a choice."
        })
    else:
        selected_choice.votes += 1
        selected_choice.save()
        # Always return an HttpResponseRedirect after successfully dealing
        # with POST data. This prevents data from being posted twice if a 
        # user hits the Back button.
        return HttpResponseRedirect(reverse('polls:results', args=(question.id,)))
 
```
```bash
# Add the following lines below the first import on the top of pollster>polls>views.py
from django.shortcuts import get_object_or_404, render
from django.template import loader
from django.http import HttpResponse, HttpResponseRedirect
from django.urls import reverse
```
```bash
# Add the vote path to pollster>polls>urls.py
from django.urls import path

from . import views

app_name = 'polls'
urlpatterns = [
    path('', views.index, name='index'),
    path('<int:question_id>/', views.detail, name='detail'),
    path('<int:question_id>/results/', views.results, name='results'),
    path('<int:question_id>/vote/', views.vote, name='vote')
]
```
```bash
# Create a new folder called partials under templates
# Create a file inside partials called _navbar.html and write in as follows:
<nav class="navbar navbar-dark bg-primary mb-4">
    <div class="container">
        <a class="navbar-brand" href="/">pollster</a>
    </div>
</nav>
```
```bash
# Add the lines above the container class in pollster>templates>base.html
    <!-- NavBar -->
    {% include 'partials/_navbar.html' %}
```
```bash
# Add the lines to the newly created pollster>pages>views.py
def index(request):
    return render(request, 'pages/index.html')
```
```bash
# Create a file called urls.py in pollster>pages and write in 
from django.urls import path

from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
```
```bash
# Add the urls line to pollster>pollster>urls.py
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('', include('pages.urls')),
    path('polls/', include('polls.urls')),
    path('admin/', admin.site.urls),
]
```
```bash
# Create a file called index.html in pollster>templates>pages and write in
{% extends 'base.html' %}
{% block content %}

<div>class="card text_center">
    <div class="card-body">
        <h1>Welcome to Pollster</h1>
        <p>This is an example Django polling app</p>
        <a class="btn btn-dark" href="{% url 'polls:index' %">
            View Available Polls</a
            >
    </div>
</div>
{% endblock %}
```