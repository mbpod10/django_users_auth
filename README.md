# Django User Authorization and Authentication

### Setup

- Create django project
  - `django-admin startproject learning_users`
- Create django app
  - `django-admin startapp basic_app`
- Go to settings.py and put app in INSTALLED_APPS
  - Migrate the installed app `python3 manage.py migrate`
- Install Libraries (make sure you are in ENV)
  - `pip3 install bcrypt` password hash
  - `pip3 install argon2` password hash
  - `pip3 install argon2_cffi`
  - `pip3 install pillow` work with media files
  - make sure that libraries are istalled with `pip3 list`
- In `settings.py`
  - add a list that uses password hashsers. This increases the level of security in passwords

```python
PASSWORD_HASHERS = [
    'django.contrib.auth.hashers.Argon2PasswordHasher',
    'django.contrib.auth.hashers.BCryptSHA256PasswordHasher',
    'django.contrib.auth.hashers.BCryptPasswordHasher',
    'django.contrib.auth.hashers.PBKDF2PasswordHasher',
    'django.contrib.auth.hashers.PBKDF2SHA1PasswordHasher',
]
```

In `AUTH_PASSWORD_VALIDATORS` you have the options to validate user passwords; go to https://docs.djangoproject.com/en/3.1/ref/settings/#auth-password-validators for more.

### Add Folders

At the top level of the project, add folders:

1. `media`
2. `templates`
3. `static`

Now we need configure paths of the folders we just made in `settings.py`

```python
# Build paths inside the project like this: BASE_DIR / 'subdir'.
BASE_DIR = Path(__file__).resolve().parent.parent
TEMPLATE_DIR = BASE_DIR / 'templates'
STATIC_DIR = BASE_DIR / 'static'
MEDIA_DIR = BASE_DIR / 'media'


TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [TEMPLATE_DIR, ], # <<<<<<
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

# GO TO THE BOTTOM OF SETTINGS

STATIC_URL = '/static/'
STATICFILES_DIRS = [STATIC_DIR, ]

# MEDIA
MEDIA_ROOT = MEDIA_DIR
MEDIA_URL = '/media/'

```

## User Models

We will use Django built-in tools for authentication and authorization <br  />

Django User object has key features

1. Username
2. Email
3. Password
4. First Name
5. Surname
6. Other Attributes

Go to `models.py` file in your app

```python
from django.db import models
from django.contrib.auth.models import User
# Create your models here.


class UserProfileInfo(models.Model):

    user = models.OneToOneField(User, on_delete=models.CASCADE)

    # additional
    portfolio_site = models.URLField(blank=True)

    profile_pic = models.ImageField(upload_to='profile_pics', blank=True)

    def __str__(self):
        return self.user.username

```

## Set Up Model Forms

```python
from django import forms
from django.contrib.auth.models import User
from basic_app.models import UserProfileInfo


class UserForm(forms.ModelForm):
    password = forms.CharField(widget=forms.PasswordInput())

    class Meta():
        model = User
        fields = ('username', 'email', 'password')


class UserProfileInfoForm(forms.ModelForm):
    class Meta():
        model = UserProfileInfo
        fields = ('portfolio_site', 'profile_pic')

```
