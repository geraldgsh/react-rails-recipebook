# Creating a New Rails Application

```
$ rails new rails_react_recipebook -T -d postgresql -T --webpack=react --skip-coffee
      create
      create  README.md
      create  Rakefile
.
.

├─ react@16.13.1
└─ scheduler@0.19.1
Done in 5.46s.
```

# Step 2 — Setting Up the Database

### DB Config file setup

```
# config/database.yml

default: &default
  adapter: postgresql
  encoding: unicode
  # For details on connection pooling, see Rails configuration guide
  # https://guides.rubyonrails.org/configuring.html#database-pooling
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  timeout: 5000

development:
  adapter: postgresql
  encoding: unicode
  database: recipebook_development
  username: postgres
  password: postgres
  host: 127.0.0.1

test:
  adapter: postgresql
  encoding: unicode
  database: recipebook_test
  username: postgres
  password: postgres
  host: 127.0.0.1

production:
  adapter: postgresql
  encoding: unicode
  database: recipebook_production
  username: postgres
  password: postgres
  host: 127.0.0.1
```

### DB setup with docker

```
docker run -d --name recipebook -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=postgres -d -p 5432:5432 postgres
89ea2315f3584d678726c44d6ad7b6f4db8a2c93e5e3863af99f84f6ec26fe11
```

###  Rails DB setup

```
$ rails db:create
Created database 'recipebook_development'
Created database 'recipebook_test'
````

### Start application

Now that the application is connected to a database, start the application by running the following command in you Terminal window:

```
$ rails s --binding=127.0.0.1

=> Booting Puma
=> Rails 6.0.2.2 application starting in development
=> Run `rails server --help` for more startup options
Puma starting in single mode...
* Version 4.3.3 (ruby 2.6.5-p114), codename: Mysterious Traveller
* Min threads: 5, max threads: 5
* Environment: development
* Listening on tcp://127.0.0.1:3000
Use Ctrl-C to stop
```

# Step 3 — Installing Frontend Dependencies

In this step, you will install the JavaScript dependencies needed on the frontend of your food recipe application. They include:

* `React Router`, for handling navigation in a React application.
* `Bootstrap`, for styling your front-end components.
* `jQuery` and `Popper`, for working with Bootstrap.

```
$ yarn add react-router-dom bootstrap jquery popper.js
yarn add v1.21.1
[1/4] Resolving packages...
[2/4] Fetching packages...
[3/4] Linking dependencies...
[4/4] Building fresh packages...
success Saved lockfile.
success Saved 11 new dependencies.
info Direct dependencies
├─ bootstrap@4.4.1
├─ jquery@3.5.0
├─ popper.js@1.16.1
└─ react-router-dom@5.1.2
info All dependencies
├─ bootstrap@4.4.1
├─ gud@1.0.0
├─ hoist-non-react-statics@3.3.2
├─ jquery@3.5.0
├─ mini-create-react-context@0.3.2
├─ popper.js@1.16.1
├─ react-router-dom@5.1.2
├─ react-router@5.1.2
├─ resolve-pathname@3.0.0
├─ tiny-warning@1.0.3
└─ value-equal@1.0.1
Done in 7.52s
```

# Step 4 — Setting Up the Homepage