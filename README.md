# Django User Authorization and Authentication

### Setup

- Create django project
  - `django-admin startproject learning_users`
- Create django app
  - `django-admin startapp basic_app`
- Go to settings.py and put app in INSTALLED_APPS
  - Migrate the installed app `python3 manage.py migrate`
- Install Libraries (make sure you are in ENV)
  - `pip3 install bcrypt`
  - `pip3 install argon2`
  - make sure that libraries are istalled with `pip3 list`
- In `settings.py`
  - add a dictionary that incrates password hashsers

```python red
PASSWORD_HASHERS = [
    'django.contrib.auth.hashers.Argon2PasswordHasher',
    'django.contrib.auth.hashers.BCryptSHA256PasswordHasher',
    'django.contrib.auth.hashers.BCryptPasswordHasher',
    'django.contrib.auth.hashers.PBKDF2PasswordHasher',
    'django.contrib.auth.hashers.PBKDF2SHA1PasswordHasher',
]
```
