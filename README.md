# erp-systemerp_system/
│── erp_system/        # main project settings
│── dashboard/         # overview dashboard
│── finance/
│── hr/
│── sales/
│── marketing/
│── templates/
│── static/
│── manage.py
django-admin startproject erp_system
cd erp_system

python manage.py startapp dashboard
python manage.py startapp finance
python manage.py startapp hr
python manage.py startapp sales
python manage.py startapp marketingINSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    'dashboard',
    'finance',
    'hr',
    'sales',
    'marketing',
]from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),

    path('', include('dashboard.urls')),
    path('finance/', include('finance.urls')),
    path('hr/', include('hr.urls')),
    path('sales/', include('sales.urls')),
    path('marketing/', include('marketing.urls')),
]from django.shortcuts import render

def dashboard_home(request):
    context = {
        "finance_summary": "Finance Data",
        "hr_summary": "HR Data",
        "sales_summary": "Sales Data",
        "marketing_summary": "Marketing Data",
    }
    return render(request, "dashboard/home.html", context)from django.urls import path
from . import views

urlpatterns = [
    path('', views.dashboard_home, name='dashboard'),
]from django.db import models

class Transaction(models.Model):
    title = models.CharField(max_length=255)
    amount = models.FloatField()
    date = models.DateField(auto_now_add=True)

    def __str__(self):
        return self.titlefrom django.shortcuts import render
from .models import Transaction

def finance_dashboard(request):
    transactions = Transaction.objects.all()
    total = sum(t.amount for t in transactions)

    return render(request, 'finance/dashboard.html', {
        'transactions': transactions,
        'total': total
    })from django.urls import path
from .views import finance_dashboard

urlpatterns = [
    path('', finance_dashboard, name='finance_dashboard'),
]from django.db import models

class Employee(models.Model):
    name = models.CharField(max_length=200)
    department = models.CharField(max_length=100)
    salary = models.FloatField()

    def __str__(self):
        return self.namefrom django.db import models

class Sale(models.Model):
    product = models.CharField(max_length=200)
    amount = models.FloatField()
    date = models.DateField(auto_now_add=True)from django.db import models

class Campaign(models.Model):
    name = models.CharField(max_length=200)
    budget = models.FloatField()
    leads_generated = models.IntegerField()<!DOCTYPE html>
<html>
<head>
    <title>ERP System</title>
</head>
<body>

<h1>ERP Dashboard</h1>

<nav>
    <a href="/">Overview</a> |
    <a href="/finance/">Finance</a> |
    <a href="/hr/">HR</a> |
    <a href="/sales/">Sales</a> |
    <a href="/marketing/">Marketing</a>
</nav>

<hr>

{% block content %}
{% endblock %}

</body>
</html>{% extends 'base.html' %}

{% block content %}

<h2>Overview Dashboard</h2>

<ul>
    <li>{{ finance_summary }}</li>
    <li>{{ hr_summary }}</li>
    <li>{{ sales_summary }}</li>
    <li>{{ marketing_summary }}</li>
</ul>

{% endblock %}
