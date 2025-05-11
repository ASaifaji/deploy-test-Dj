# deploy-test-Dj

This example shows how to deploy Django on Vercel.

## Django Deployment on vercel

### STEP 1
Configure our app to be included in `INSTALLED_APPS` in `dev/settings.py`:

```python
# dev/settings.py
# ...
INSTALLED_APPS = [
    # ...,
    'myapp',  # <-- Add your app here
]
# ...
```

### STEP 2
We allow "*.vercel.app*" subdomains in `ALLOWED_HOSTS`:

```python
# dev/settings.py
# ...
ALLOWED_HOSTS = ['.vercel.app']  # <-- Allow any Vercel subdomain
# ...
```

### STEP 3
The `wsgi` module must use a public variable named `app` to expose the WSGI application:

```python
# dev/wsgi.py
#...
app = get_wsgi_application()  # <-- Expose the WSGI application as `app`
#...
```

### STEP 4
add dependencies list, requirements.txt

```terminal
pip freeze > requirements.txt
```

### STEP 5
add vercel.json, add wsgi path to builds `src`, and routes `dest`.

```json
{
    "builds": [{
        "src": "dev/wsgi.py",
        "use": "@vercel/python",
        "config": { "maxLambdaSize": "15mb", "runtime": "python3.9" }
    }],
    "routes": [
        {
            "src": "/(.*)",
            "dest": "dev/wsgi.py"
        }
    ]
}
```

### STEP 6
add .gitignore to every folder/file that's not in
```
your_repo/
|── dev/
|── myapp/
|── db.sqlite3 (if any)
|── requirements.txt
└── vercel.json
```

### STEP 7
- Sign up to vercel add your github account
- import your github repo, and click deploy.
- go to the project dashboard and click it's URL and you should see your web app running.