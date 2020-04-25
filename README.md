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

```
$ rails g controller Homepage index
      create  app/controllers/homepage_controller.rb
       route  get 'homepage/index'
      invoke  erb
      create    app/views/homepage
      create    app/views/homepage/index.html.erb
      invoke  helper
      create    app/helpers/homepage_helper.rb
      invoke  assets
      invoke    scss
      create      app/assets/stylesheets/homepage.scss
```

##### Edit route
```
config/routes.rb

Rails.application.routes.draw do
  root 'homepage#index'
  # For details on the DSL available within this file, see http://guides.rubyonrails.org/routing.html
end
```

##### Run server
```
rails s --binding=127.0.0.1
```

##### Delete the contents of the ~/rails_react_recipebook/app/views/homepage/index.html.erb

```html
<h1>Homepage#index</h1>
<p>Find me in app/views/homepage/index.html.erb</p>
```

# Step 5 — Configuring React as Your Rails Frontend

First, rename the `~/rails_react_recipebook/app/javascript/packs/hello_react.jsx` file to `~/rails_react_recipebook/app/javascript/packs/Index.jsx`.

```
mv ~/rails_react_recipebook/app/javascript/packs/hello_react.jsx ~/rails_react_recipe/app/javascript/packs/Index.jsx
```

add;

`<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">`
`<%= javascript_pack_tag 'Index' %>`

below

```html
<!DOCTYPE html>
<html>
  <head>
    <title>RailsReactRecipe</title>
    <%= csrf_meta_tags %>
    <%= csp_meta_tag %>

    <%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track': 'reload' %>
    <%= javascript_include_tag 'application', 'data-turbolinks-track': 'reload' %>
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <%= javascript_pack_tag 'Index' %>
  </head>

  <body>
    <%= yield %>
  </body>
</html>
```

Adding the JavaScript pack to your application’s header makes all your JavaScript code available and executes the code in your Index.jsx file on the page whenever you run the app. Along with the JavaScript pack, you also added a meta viewport tag to control the dimensions and scaling of pages on your application.


Create a components directory in the app/javascript directory:

```
mkdir ~/rails_react_recipebook/app/javascript/components
```

create a Home.jsx file in the components directory:

```
$nano ~/rails_react_recipe/app/javascript/components/Home.jsx
```

Add the following code to the file:

```js
~/rails_react_recipe/app/javascript/components/Home.jsx
import React from "react";
import { Link } from "react-router-dom";

export default () => (
  <div className="vw-100 vh-100 primary-color d-flex align-items-center justify-content-center">
    <div className="jumbotron jumbotron-fluid bg-transparent">
      <div className="container secondary-color">
        <h1 className="display-4">Food Recipes</h1>
        <p className="lead">
          A curated list of recipes for the best homemade meal and delicacies.
        </p>
        <hr className="my-4" />
        <Link
          to="/recipes"
          className="btn btn-lg custom-button"
          role="button"
        >
          View Recipes
        </Link>
      </div>
    </div>
  </div>
);
```

In this code, you imported React and also the Link component from React Router. The Link component creates a hyperlink to navigate from one page to another. You then created and exported a functional component containing some Markup language for your homepage, styled with Bootstrap classes.


With your Home component in place, you will now set up routing using React Router. Create a routes directory in the app/javascript directory:

```
$ mkdir ~/rails_react_recipe/app/javascript/routes
```

The routes directory will contain a few routes with their corresponding components. Whenever any specified route is loaded, it will render its corresponding component to the browser.

In the routes directory, create an Index.jsx file:

```
 nano ~/rails_react_recipebook/app/javascript/routes/Index.jsx
```

Add;

```js
/rails_react_recipe/app/javascript/routes/Index.jsx

import React from "react";
import { BrowserRouter as Router, Route, Switch } from "react-router-dom";
import Home from "../components/Home";

export default (
  <Router>
    <Switch>
      <Route path="/" exact component={Home} />
    </Switch>
  </Router>
);
```

In this Index.jsx route file, you imported a couple of modules: the React module that allows us to use React, and the BrowserRouter, Route, and Switch modules from React Router, which together help us navigate from one route to another. Lastly, you imported your Home component, which will be rendered whenever a request matches the root (/) route. Whenever you want to add more pages to your application, all you need to do is declare a route in this file and match it to the component you want to render for that page.

Create an App.jsx file in the app/javascript/components directory:

```
$ nano ~/rails_react_recipebook/app/javascript/components/App.jsx
```

```js
/rails_react_recipe/app/javascript/components/App.jsx

import React from "react";
import Routes from "../routes/Index";

export default props => <>{Routes}</>;
```

In the App.jsx file, you imported React and the route files you just created. You then exported a component that renders the routes within fragments. This component will be rendered at the entry point of the aplication, thereby making the routes available whenever the application is loaded.

Now that you have your App.jsx set up, it’s time to render it in your entry file. Open the entry Index.jsx file:

```
$ nano ~/rails_react_recipe/app/javascript/packs/Index.jsx
```

Replace the code there with the following code:

```js

import React from "react";
import { render } from "react-dom";
import 'bootstrap/dist/css/bootstrap.min.css';
import $ from 'jquery';
import Popper from 'popper.js';
import 'bootstrap/dist/js/bootstrap.bundle.min';
import App from "../components/App";

document.addEventListener("DOMContentLoaded", () => {
  render(
    <App />,
    document.body.appendChild(document.createElement("div"))
  );
});
```

In this code snippet, you imported React, the render method from ReactDOM, Bootstrap, jQuery, Popper.js, and your App component. Using ReactDOM’s render method, you rendered your App component in a div element, which was appended to the body of the page. Whenever the application is loaded, React will render the content of the App component inside the div element on the page.


Open up your `application.css` in your `~/rails_react_recipe/app/assets/stylesheets` directory:

```
$ nano ~/rails_react_recipebook/app/assets/stylesheets/application.css
```

```css
/app/assets/stylesheets/application.css

.bg_primary-color {
  background-color: #FFFFFF;
}
.primary-color {
  background-color: #FFFFFF;
}
.bg_secondary-color {
  background-color: #293241;
}
.secondary-color {
  color: #293241;
}
.custom-button.btn {
  background-color: #293241;
  color: #FFF;
  border: none;
}
.custom-button.btn:hover {
  color: #FFF !important;
  border: none;
}
.hero {
  width: 100vw;
  height: 50vh;
}
.hero img {
  object-fit: cover;
  object-position: top;
  height: 100%;
  width: 100%;
}
.overlay {
  height: 100%;
  width: 100%;
  opacity: 0.4;
}
```

# Step 6 — Creating the Recipe Controller and Model

Create a Recipe model and controller. The recipe model will represent the database table that will hold information about the user’s recipes while the controller will receive and handle requests to create, read, update, or delete recipes. When a user requests a recipe, the recipe controller receives this request and passes it to the recipe model, which retrieves the requested data from the database. The model then returns the recipe data as a response to the controller. Finally, this information is displayed in the browser.

Create a Recipe model by using the generate model subcommand provided by Rails and by specifying the name of the model along with its columns and data types.

```
$ rails generate model Recipe name:string ingredients:text instruction:text image:string

      invoke  active_record
      create    db/migrate/20200425194512_create_recipes.rb
      create    app/models/recipe.rb
```

