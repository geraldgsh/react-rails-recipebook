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

The preceding command instructs Rails to create a Recipe model together with a name column of type string, an ingredients and instruction column of type text, and an image column of type string. This tutorial has named the model Recipe, because by convention models in Rails use a singular name while their corresponding database tables use a plural name.

Running the generate model command creates two files:

A recipe.rb file that holds all the model related logic.
A 20200426161357_create_recipes.rb file (the number at the beginning of the file may differ depending on the date when you run the command). This is a migration file that contains the instruction for creating the database structure.

. Open your recipe model located at `app/models/recipe.rb`:

```ruby
class Recipe < ApplicationRecord
  validates :name, presence: true
  validates :ingredients, presence: true
  validates :instruction, presence: true
end
```

To make sure that the migration works with the database you set up, it is necessary to make changes to the 20200426161357_create_recipes.rb file.

```ruby

class CreateRecipes < ActiveRecord::Migration[6.0]
  def change
    create_table :recipes do |t|
      t.string :name, null: false
      t.text :ingredients, null: false
      t.text :instruction, null: false
      t.string :image, default: 'https://raw.githubusercontent.com/do-community/react_rails_recipe/master/app/assets/images/Sammy_Meal.jpg'

      t.timestamps
    end
  end
end
```

Rub migration and actually create your table. In Terminal window, run the following command:

```
$ rails db:migrate

== 20200425194512 CreateRecipes: migrating ====================================
-- create_table(:recipes)
   -> 0.1280s
== 20200425194512 CreateRecipes: migrated (0.1287s) ===========================
```

Create recipes controller and add the logic for creating, reading, and deleting recipes. In Terminal window, run the following command:

```
$ rails generate controller api/v1/Recipes index create show destroy -j=false -y=false --skip-template-engine --no-helper
Running via Spring preloader in process 4333
      create  app/controllers/api/v1/recipes_controller.rb
       route  namespace :api do
  namespace :v1 do
    get 'recipes/index'
    get 'recipes/create'
    get 'recipes/show'
    get 'recipes/destroy'
  end
end
      invoke  assets
      invoke    scss
```

In this command, you created a Recipes controller in an api/v1 directory with an index, create, show, and destroy action. The index action will handle fetching all your recipes, the create action will be responsible for creating new recipes, the show action will fetch a single recipe, and the destroy action will hold the logic for deleting a recipe.

You also passed some flags to make the controller more lightweight, including:

* `-j=false` which instructs Rails to skip generating associated JavaScript files.
* `-y=false` which instructs Rails to skip generating associated stylesheet files.
* `--skip-template-engine`, which instructs Rails to skip generating Rails view files, since React is handling your front-end needs.
* `--no-helper`, which instructs Rails to skip generating a helper file for your controller.

Running the command also updated your routes file with a route for each action in the Recipes controller. To use these routes, make changes to your config/routes.rb file.

Open up the routes file in your text editor:

```
$ nano ~/rails_react_recipe/config/routes.rb
```

``` ruby
Rails.application.routes.draw do
  namespace :api do
    namespace :v1 do
      get 'recipes/index'
      post 'recipes/create'
      get '/show/:id', to: 'recipes#show'
      delete '/destroy/:id', to: 'recipes#destroy'
    end
  end
  root 'homepage#index'
  get '/*path' => 'homepage#index'
  # For details on the DSL available within this file, see http://guides.rubyonrails.org/routing.html
end
```

To see a list of routes available in your application, run the following command in your Terminal window:

```
$ rails routes
```

Running this command displays a list of URI patterns, verbs, and matching controllers or actions for your project.

Next, add the logic for getting all recipes at once. Rails uses the ActiveRecord library to handle database-related tasks like this. ActiveRecord connects classes to relational database tables and provides a rich API for working with them.

To get all recipes, you’ll use ActiveRecord to query the recipes table and fetch all the recipes that exist in the database.

Open the recipes_controller.rb file with the following command:

```ruby
$ nano ~/rails_react_recipebook/app/controllers/api/v1/recipes_controller.rb


class Api::V1::RecipesController < ApplicationController
  def index
    recipe = Recipe.all.order(created_at: :desc)
    render json: recipe
  end

  def create
  end

  def show
  end

  def destroy
  end
end
```

In your index action, using the all method provided by ActiveRecord, you get all the recipes in your database. Using the order method, you order them in descending order by their created date. This way, you have the newest recipes first. Lastly, you send your list of recipes as a JSON response with render.

Next, add the logic for creating new recipes. As with fetching all recipes, you’ll rely on ActiveRecord to validate and save the provided recipe details. Update your recipe controller with the following highlighted lines of code:

```ruby
app/controllers/api/v1/recipes_controller.rb

class Api::V1::RecipesController < ApplicationController
  def index
    recipe = Recipe.all.order(created_at: :desc)
    render json: recipe
  end

  def creater
    recipe = Recipe.create!(recipe_params)
    if recipe
      render json: recipe
    else
      render json: recipe.errors
  end

  def show
  end

  def destroy
  end
  
  private

  def recipe_params
    params.permit(:name, :image, :ingredients, :instruction)
  end
end

```

In the create action, you use ActiveRecord’s create method to create a new recipe. The create method has the ability to assign all controller parameters provided into the model at once. This makes it easy to create records, but also opens the possibility of malicious use. This can be prevented by using a feature provided by Rails known as strong parameters. This way, parameters can’t be assigned unless they’ve been whitelisted. In your code, you passed a recipe_params parameter to the create method. The recipe_params is a private method where you whitelisted your controller parameters to prevent wrong or malicious content from getting into your database. In this case, you are permitting a name, image, ingredients, and instruction parameter for valid use of the create method.

Your recipe controller can now read and create recipes. All that’s left is the logic for reading and deleting a single recipe. Update your recipes controller with the following code:


```ruby
app/controllers/api/v1/recipes_controller.rb

class Api::V1::RecipesController < ApplicationController
  def index
    recipe = Recipe.all.order(created_at: :desc)
    render json: recipe
  end

  def create
    recipe = Recipe.create!(recipe_params)
    if recipe
      render json: recipe
    else
      render json: recipe.errors
    end
  end

  def show
    if recipe
      render json: recipe
    else
      render json: recipe.errors
    end
  end

  def destroy
    recipe&.destroy
    render json: { message: 'Recipe deleted!' }
  end

  private

  def recipe_params
    params.permit(:name, :image, :ingredients, :instruction)
  end

  def recipe
    @recipe ||= Recipe.find(params[:id])
  end
end
```

In the new lines of code, you created a private recipe method. The recipe method uses ActiveRecord’s find method to find a recipe whose idmatches the id provided in the params and assigns it to an instance variable @recipe. In the show action, you checked if a recipe is returned by the recipe method and sent it as a JSON response, or sent an error if it was not.

In the destroy action, you did something similar using Ruby’s safe navigation operator &., which avoids nil errors when calling a method. This let’s you delete a recipe only if it exists, then send a message as a response.

Now that you have finished making these changes to recipes_controller.rb, save the file and exit your text editor.

In this step, you created a model and controller for your recipes. You’ve written all the logic needed to work with recipes on the backend. In the next section, you’ll create components to view your recipes.

# Step 7 — Viewing Recipes

Create components for viewing recipes. First create a page where you can view all existing recipes, and then another to view individual recipes.

Open up the seed file seeds.rb to edit:

```ruby
/db/seeds.rb

9.times do |i|
  Recipe.create(
    name: "Recipe #{i + 1}",
    ingredients: '227g tub clotted cream, 25g butter, 1 tsp cornflour,100g parmesan, grated nutmeg, 250g fresh fettuccine or tagliatelle, snipped chives or chopped parsley to serve (optional)',
    instruction: 'In a medium saucepan, stir the clotted cream, butter, and cornflour over a low-ish heat and bring to a low simmer. Turn off the heat and keep warm.'
  )
end
```

Seed the database with this data, run the following command in Terminal window:

```
rails db:seed
```

Running this command adds nine recipes to your database. Now you can fetch them and render them on the frontend.

The component to view all recipes will make a HTTP request to the index action in the RecipesController to get a list of all recipes. These recipes will then be displayed in cards on the page.

Create a Recipes.jsx file in the app/javascript/components directory:

Import the React and Link modules

```ruby
import React from "react";
import { Link } from "react-router-dom";
```

Create a Recipes class that extends the React.Component class. Add the following highlighted code to create a React component that extends React.Component:

```js
import React from "react";
import { Link } from "react-router-dom";

class Recipes extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      recipes: []
    };
  }

}
export default Recipes;
```

Inside the constructor, we are initializing a state object that holds the state of your recipes, which on initialization is an empty array ([]).

Next, add a componentDidMount method in the Recipe class. The componentDidMount method is a React lifecycle method that is called immediately after a component is mounted. In this lifecycle method, you will make a call to fetch all your recipes. To do this, add the following lines:

```js
/app/javascript/components/Recipes.jsx

import React from "react";
import { Link } from "react-router-dom";

class Recipes extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      recipes: []
    };
  }

  componentDidMount() {
      const url = "/api/v1/recipes/index";
      fetch(url)
        .then(response => {
          if (response.ok) {
            return response.json();
          }
          throw new Error("Network response was not ok.");
        })
        .then(response => this.setState({ recipes: response }))
        .catch(() => this.props.history.push("/"));
  }

}
export default Recipes;
```

In your componentDidMount method, you made an HTTP call to fetch all recipes using the Fetch API. If the response is successful, the application saves the array of recipes to the recipe state. If there’s an error, it will redirect the user to the homepage.

Finally, add a render method in the Recipe class. The render method holds the React elements that will be evaluated and displayed on the browser page when a component is rendered. In this case, the render method will render cards of recipes from the component state. Add the following highlighted lines to Recipes.jsx:

```ruby
/app/javascript/components/Recipes.jsx

import React from "react";
import { Link } from "react-router-dom";

class Recipes extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      recipes: []
    };
  }

  componentDidMount() {
    const url = "/api/v1/recipes/index";
    fetch(url)
      .then(response => {
        if (response.ok) {
          return response.json();
        }
        throw new Error("Network response was not ok.");
      })
      .then(response => this.setState({ recipes: response }))
      .catch(() => this.props.history.push("/"));
  }
  render() {
    const { recipes } = this.state;
    const allRecipes = recipes.map((recipe, index) => (
      <div key={index} className="col-md-6 col-lg-4">
        <div className="card mb-4">
          <img
            src={recipe.image}
            className="card-img-top"
            alt={`${recipe.name} image`}
          />
          <div className="card-body">
            <h5 className="card-title">{recipe.name}</h5>
            <Link to={`/recipe/${recipe.id}`} className="btn custom-button">
              View Recipe
            </Link>
          </div>
        </div>
      </div>
    ));
    const noRecipe = (
      <div className="vw-100 vh-50 d-flex align-items-center justify-content-center">
        <h4>
          No recipes yet. Why not <Link to="/new_recipe">create one</Link>
        </h4>
      </div>
    );

    return (
      <>
        <section className="jumbotron jumbotron-fluid text-center">
          <div className="container py-5">
            <h1 className="display-4">Recipes for every occasion</h1>
            <p className="lead text-muted">
              We’ve pulled together our most popular recipes, our latest
              additions, and our editor’s picks, so there’s sure to be something
              tempting for you to try.
            </p>
          </div>
        </section>
        <div className="py-5">
          <main className="container">
            <div className="text-right mb-3">
              <Link to="/recipe" className="btn custom-button">
                Create New Recipe
              </Link>
            </div>
            <div className="row">
              {recipes.length > 0 ? allRecipes : noRecipe}
            </div>
            <Link to="/" className="btn btn-link">
              Home
            </Link>
          </main>
        </div>
      </>
    );
  }
}
export default Recipes;
```

Now that you have created a component to display all the recipes, the next step is to create a route for it. Open the front-end route file located at `app/javascript/routes/Index.jsx`:

```js
/app/javascript/routes/Index.jsx

import React from "react";
import { BrowserRouter as Router, Route, Switch } from "react-router-dom";
import Home from "../components/Home";
import Recipes from "../components/Recipes";

export default (
  <Router>
    <Switch>
      <Route path="/" exact component={Home} />
      <Route path="/recipes" exact component={Recipes} />
    </Switch>
  </Router>
);
```

Verify that code is working correctly;

```
rails s --binding=127.0.0.1
```