## New way

## 1. Install Heroku Toolbelt
Sign up to Heroku. Then install the Heroku Toolbelt. It is a command line tool to manage your Heroku apps.

After installing the Heroku Toolbelt, open a terminal and login to your account:

```
$ heroku login
heroku: Enter your login credentials
Email: your_email@gmail.com
Password (typing will be hidden):
Authentication successful.
```
## 2. Preparing the Application

In this tutorial we will deploy the blog app from Module 13.

The Heroku deployment process is done through Git. Your application will be stored in a remote Git repository in the Heroku Cloud.

**Create new virtual env and clone your project from Git inside it.**

for example:
```
git clone https://github.com/CleverProgrammer/my_django_site
```
then
```
cd my_django_site
```
Anyway, here is the two things you will need to add to your project:

Add a Procfile in the project root.

Add requirements.txt file with all the requirements in the project root
> Note that requirements are already in project root, bellow is explained how they looks if you don't have them.

#### The Procfile
Create a file inside your cloned project, named **Procfile** in the root with the following content:
```
web: gunicorn mysite.wsgi --log-file -
```
and commit the changes.

#### The requirements.txt
This step is required if by some accident you don't have requirements.txt file.

This is what requirements.txt should contain for now:

certifi==2018.4.16
Django==2.1
django-crispy-forms==1.7.2
Markdown==2.6.11
mistune==0.8.3
Pygments==2.2.0
pytz==2018.5

### Get the requirements for heroku
Make sure your virtual env is activated and run:
```
$ pip install django-heroku
```
after installation is done, run:
```
$ pip install -r requirements.txt
```
and then run:
```
$ pip install gunicorn
```
Now type:
```
$ pip freeze > requirements.txt
```
The requirements.txt should be ready now.

Open mysite/settings.py file and at the top make the import django_heroku under the import os line:
```python
import django_heroku
```

Find DEBUG in the same file and make it False:
```python
# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = False
```
Then go to ALLOWED_HOSTS and add '*':
```python
ALLOWED_HOSTS = ['*']
```

And at the bottom under STATIC_URL add:
```python
# Activate Django-Heroku.
django_heroku.settings(locals())
```
Save the changes.

## 3. Deployment
Let’s get the deployment started.

Login to Heroku using the toolbelt (from Step 1):
```
$ heroku login
```

Inside the project root, create a Heroku App with unique name for example:
```
$ heroku create my-django-site
```
If name is taken just try another one.

Output:

```
Creating ⬢ my-django-site... done
https://my-django-site.herokuapp.com/ | https://git.heroku.com/my-django-site.git
```
You can omit the app name parameter (in my case, my-django-site), then Heroku will pick a name for you.

**Add a PostgreSQL database to your app:**
```
heroku addons:create heroku-postgresql:hobby-dev
```

Output:
```
Creating heroku-postgresql:hobby-dev on ⬢ my-django-site... free
Database has been created and is available
 ! This database is empty. If upgrading, you can transfer
 <https://p4n4ua.dm.files.1drv.com/y4msVMtLR85W6ihpVqFSO-VEMudjS5Y_TLIN_tvNhjGHgJgU1zDfza4XLPU0OUMz7sZvWbYfpLkl9aC-oAMpTV7bpyOjIurhWlY2W5nNaJ325w7Rr8ErO1agsbyvqc6I9wUEWrRQmZuxsK0ddxeQwvB2I09ixddJKbYqg-JgKhyIG31tIv_au9wxkB0ETj300n2V3XbXl1FF2wNa_IY6wstxA/heroku_app.jpg?psid=1>! data from another database with pg:copy
Created postgresql-flat-70197 as DATABASE_URL
Use heroku addons:docs heroku-postgresql to view documentation
```
Now login to [Heroku](https://dashboard.heroku.com/apps) and access your recently created app:

![heroku_app.jpg](https://dm2302files.storage.live.com/y4m6LQtYEv1zPb41BYv3NwNxa9kbw7bI9XjJOwtiIFvDKrzGZibIgUEMRKQZnT84jOZqgsFOdlgcoqcBTlqSzYSPSVUsRUBxfmQqmaOjM2u41q_DLtN3X4OlNfVpQ-lDAlDTICv2jG7BQnGQYmGlBif4iWRr86iPteVlqSxfTTfbt-sf3mB9WBYF7jvkxgB3QV2LDv1pUBxb-9--S9CjcIobA/heroku_app.jpg?psid=1&width=1915&height=536)


Click on the Settings menu and then on the button Reveal Config Vars:

![heroku_settings.jpg](https://p4n4ua.dm.files.1drv.com/y4mGiUh_DyTzpkE2ZMlaY3MznIYewmByoF8z-mT0rTiDhKd0_R5ewaMcuvSK3GoiXpZ-3kfGJvg4g2RMftlkwbsWtH8mlsqSHg1xS9FSpACaiR0sgoedvhd7RFsMZO2-1Z5R0pP3dtzWFIjxUXTS6dzPI7Y8L9cX-45PoYPQDFIt_MYJ3jkWknqqpD2Gvl9fVptURMMbijuqKdp3uPNlCrL0A/heroku_settings.jpg?psid=1)

Click Reveal Config Vars:

![config_vars.jpg](https://dm2302files.storage.live.com/y4mnuRhy_k3C9tgWPzWGq5SdwEk5mnQJkKlBuMLznvjkCgXcd_Ik46ENbvCg7tMAb3ZH_P8DutHta4kn2bes5lJH6TkdjwuzqjRDsh4Klg6JmyC__TjW1MfQkmO72THhLDQTyuxfDFGdNxg4EJb40j92BvvvHiubMZr-oUYSI8MSepwp8B-5nN2N7-e3Av2LiXnsBi8bHq0mLgVsc8YHFJWpQ/config_vars.jpg?psid=1&width=1678&height=808))


Add the SECRET_KEY from mysite/settings.py:

![secret_key.jpg](https://dm2302files.storage.live.com/y4mg1yQmaf_kj7jAHEjs0hmQIaM2f-C5nOGloDXLdtx0vjmWgL7kwgKqUkZVTsNFJO5Q-eUeWRShKsJYlqMrW8-cwsC9hyth7XxCNUSsErXxR00W8HgFqP5_fFrkHXcZk6zBj99Pw4ejIcEsKIE3eYlhm9Y-uTKWm0RBehpU-yKfmM_tKgkemO513BhL4kJmPM9ZHZrW9k3dcZMsKfVV5e3Ig/secret_key.jpg?psid=1&width=1915&height=790)

Now in terminal push to deploy:
```
git push heroku master
```

Output:
```
Counting objects: 691, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (285/285), done.
Writing objects: 100% (691/691), 103.17 KiB | 9.38 MiB/s, done.
Total 691 (delta 377), reused 611 (delta 333)
remote: Compressing source files... done.
remote: Building source:
remote:
remote: -----> Python app detected
remote:  !     The latest version of Python 3.6 is python-3.6.6 (you are using python-3.6.1, which is unsupported).
remote:  !     We recommend upgrading by specifying the latest version (python-3.6.6).
remote:        Learn More: https://devcenter.heroku.com/articles/python-runtimes
remote: -----> Installing python-3.6.1
remote: -----> Installing pip
remote: -----> Installing requirements with pip
remote:        Collecting bleach==2.1.3 (from -r /tmp/build_cffc96e75dc797fc8a508e3ff5ee449a/requirements.txt (line 1))
remote:          Downloading https://files.pythonhosted.org/packages/30/b6/a8cffbb9ab4b62b557c22703163735210e9cd857d533740c64e1467d228e/bl
each-2.1.3-py2.py3-none-any.whl
remote:        Collecting dj-database-url==0.5.0 (from -r /tmp/build_cffc96e75dc797fc8a508e3ff5ee449a/requirements.txt (line 2))
remote:          Downloading https://files.pythonhosted.org/packages/d4/a6/4b8578c1848690d0c307c7c0596af2077536c9ef2a04d42b00fabaa7e49d/dj
_database_url-0.5.0-py2.py3-none-any.whl
remote:        Collecting dj-static==0.0.6 (from -r /tmp/build_cffc96e75dc797fc8a508e3ff5ee449a/requirements.txt (line 3))
remote:          Downloading https://files.pythonhosted.org/packages/2b/8f/77a4b8ec50c821193bf9682c7896f12fd0418eb3711a7d66796ede59c23b/dj
-static-0.0.6.tar.gz
...
remote:        Successfully installed Django-2.0.7 Markdown-2.6.11 Pillow-5.2.0 Pygments-2.2.0 Unipath-1.1 bleach-2.1.3 dj-database-url-0.
5.0 dj-static-0.0.6 django-crispy-forms-1.7.2 django-model-utils-3.1.2 django-notifications-hq-1.5.0 gunicorn-19.9.0 html5lib-1.0.1 humani
ze-0.5.1 jsonfield-2.0.2 mistune-0.8.3 psycopg2-2.7.5 python-decouple-3.1 pytz-2018.5 six-1.11.0 static3-0.7.0 webencodings-0.5.1 whitenoi
se-3.3.1
remote:
remote: -----> $ python manage.py collectstatic --noinput
remote:        /app/.heroku/python/lib/python3.6/site-packages/psycopg2/__init__.py:144: UserWarning: The psycopg2 wheel package will be r
enamed from release 2.8; in order to keep installing from binary please use "pip install psycopg2-binary" instead. For details see: <http:
//initd.org/psycopg/docs/install.html#binary-install-from-pypi>.
remote:          """)
remote:        121 static files copied to '/tmp/build_cffc96e75dc797fc8a508e3ff5ee449a/static', 151 post-processed.
remote:
remote: -----> Discovering process types
remote:        Procfile declares types -> web
remote:
remote: -----> Compressing...
remote:        Done: 71.1M
remote: -----> Launching...
remote:        Released v5
remote:        https://my-django-site.herokuapp.com/ deployed to Heroku
remote:
remote: Verifying deploy... done.
To https://git.heroku.com/my-django-site.git
```

**Migrate the database:**
```
$ heroku run python manage.py migrate
```

Output:
```
Running python manage.py migrate on ⬢ my-django-site... up, run.5369 (Free)
/app/.heroku/python/lib/python3.6/site-packages/psycopg2/__init__.py:144: UserWarning: The psycopg2 wheel package will be renamed from release 2.8; in order to ke
ep installing from binary please use "pip install psycopg2-binary" instead. For details see: <http://initd.org/psycopg/docs/install.html#binary-install-from-pypi>
.
  """)
Operations to perform:
  Apply all migrations: admin, auth, blog, contenttypes, notifications, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying blog.0001_initial... OK
  Applying blog.0002_comment... OK
  Applying notifications.0001_initial... OK
  Applying notifications.0002_auto_20150224_1134... OK
  Applying notifications.0003_notification_data... OK
  Applying notifications.0004_auto_20150826_1508... OK
  Applying notifications.0005_auto_20160504_1520... OK
  Applying notifications.0006_indexes... OK
  Applying sessions.0001_initial... OK
```

Try the URL in a web browser: 
https://my-django-site.herokuapp.com/

