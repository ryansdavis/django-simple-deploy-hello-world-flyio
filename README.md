Used as a walk-through hello world to understand how django-simple-deploy (DSD) should work
https://django-simple-deploy.readthedocs.io/en/latest/quick_starts/quick_start_flyio/

To build/setup django project (from the original DSD repo):
```
uv run tests/e2e_tests/utils/build_dev_env.py
```
outputs:
```
...
cmd: git tag -am '' 'ADDED_DSD'


 --- Finished setup ---
  Your project is ready to use at: /Users/ryan_s_davis/projects/dsd-dev-project_ekwxx
```
the `cp`'ed to here...


ran `python manage.py deploy`:

```
...

DEPRECATED: Unmanaged Fly Postgres is deprecated in favor of 'fly mpg' (Managed Postgres).
Please visit https://fly.io/docs/mpg/overview/ for more information about Managed Postgres.

automatically selected personal organization: Ryan Davis
Creating postgres cluster in organization personal
Creating app...
Setting secrets on app cool-app-bro-db...
Provisioning 1 of 1 machines with image flyio/postgres-flex:17.2@sha256:f4301ae20d193ab3c3539eb9df9a8f8d3736dd331aeec1bfb2e34b539dc353c5
Waiting for machine to start...
^[^[Machine 6830ddda7411e8 is created
==> Monitoring health checks
  Waiting for 6830ddda7411e8 to become healthy (started, 2/3)

Postgres cluster cool-app-bro-db created
  Username:    postgres
  Password:    pP9qyvih3njtdoJ
  Hostname:    cool-app-bro-db.internal
  Flycast:     fdaa:19:9949:0:1::2
  Proxy port:  5432
  Postgres port:  5433
  Connection string: postgres://postgres:pP9qyvih3njtdoJ@cool-app-bro-db.flycast:5432

Save your credentials in a secure place -- you won't be able to see them again!

...
t 
  Added whitenoise to requirements file.

--- Your project is now configured for deployment on Fly.io ---

To deploy your project, you will need to:
- Commit the changes made in the configuration process.
    $ git status
    $ git add .
    $ git commit -am "Configured project for deployment."
- Push your project to Fly.io's servers:
    $ fly deploy
- Open your project:
    $ fly apps open
- As you develop your project further:
    - Make local changes
    - Commit your local changes
    - Run `fly deploy` again to push your changes.

- You can find a full record of this configuration in the dsd_logs directory.
```


```
uv run test_deployed_app_functionality.py --url https://cool-app-bro.fly.dev/
```
works! 

There is no way to automatically destroy cloud resources by design (intended audience may delete important data)
```
ryan_s_davis@Ryans-MacBook-Pro:~/Desktop/git_repos/django-simple-deploy-hello-world-flyio$ fly apps list
NAME            OWNER           STATUS          LATEST DEPLOY
cool-app-bro    personal        deployed        53m32s ago
cool-app-bro-db personal        deployed

ryan_s_davis@Ryans-MacBook-Pro:~/Desktop/git_repos/django-simple-deploy-hello-world-flyio$ fly apps destroy cool-app-bro cool-app-bro-db
Destroying an app is not reversible.
? Destroy app cool-app-bro? Yes
Destroyed app cool-app-bro
Destroying an app is not reversible.
? Destroy app cool-app-bro-db? Yes
Destroyed app cool-app-bro-db

```

