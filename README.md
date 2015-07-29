

# Sinatra Final Project

Now that we know how to build Sinatra applications, use ActiveRecord as an ORM, and render data on a page with ERB templating, let's put that all together to build your first stand-alone Sinatra app!

## Setup

You'll need to build this from scratch, but your project directory should be in the MVC structure and look something like this:

```bash
├── Gemfile
├── README.md
├── Rakefile
├── app
│   ├── controllers
│   │   └── application_controller.rb
│   ├── models
│   └── views
│       └── layout.erb
├── config
│   └── environment.rb
├── config.ru
├── db
│   ├── migrate
│   └── seeds.rb
├── public
│   └── stylesheets
└── spec
    ├── controllers
    ├── features
    ├── models
    └── spec_helper.rb
```

### Gemfile

Look at the labs you've done to see what gems are usually included in a Sinatra app. At the very least, it should look something like this:

```ruby
source 'https://rubygems.org'

gem 'sinatra'
gem 'shotgun'
gem 'activerecord', :require => 'active_record'
gem 'sinatra-activerecord', :require => 'sinatra/activerecord'
gem 'require_all'
gem 'rake'
gem 'sqlite3'
gem 'thin'
gem 'pry'

gem 'simplecov'
gem 'rspec'
gem 'capybara'
gem 'database_cleaner', git: 'https://github.com/bmabey/database_cleaner.git'
gem 'rack-test'
```

### `config/environment.rb`

This file should be:

1. establishing your SINATRA_ENV variable (default "development")
2. loading Bundler to require all of the dependencies listed in your Gemfile
3. establishing a Database connection with ActiveRecord. That should look something like this:

```ruby
ActiveRecord::Base.establish_connection(
  :adapter => "sqlite3",
  :database => "db/#{ENV['SINATRA_ENV']}.sqlite"
)
```

4. requring all (using the handy require_all gem) the files in the `/app` directory


### `Rakefile`

Require the module in the active-record gem to gain access to the database rake tasks, like so:

```ruby
require 'sinatra/activerecord/rake'
```

Then, when you run `rake -T` you should see the tasks available to you.

### `config.ru`

This is where your app will be racked up. It should look something like this:

```ruby
require './config/environment'


#this allows us to use HTTP methods like puts/patch
use Rack::MethodOverride

# this requires any images and css that are stored in the /public directory
use Rack::Static, :root => 'public', :urls => ['/images', '/stylesheets']

# You might need to mount your controller(s) as middelware
use MyController

# Mount the main controller as our Rack Application
run ApplicationController
```

## Ideas 

This project can be anything you want! The best projects are the ones where you build something useful to yourself, so think about something that you'd like as a tool or something that you'd use. Be good to yourself: this should be a challenging yet realistic learning opportunity and something that you're excited about.

At the very least your app should incorporate the following:

1. accept user input via forms
2. persist data to the database
3. have a logical flow and routes
4. have some styling; you can use Bootstrap

## Timeframe

This project should take you the weekend to complete.

## Deploying

When you're done, think about deploying it to Heroku. Signing up is free, and deploying a Sinatra app is about as easy as pushing code to Github. 

After you've downloaded the [Heroku Toolbelt](https://toolbelt.heroku.com/), make the app on your Heroku account, and add it as a remote link to your project's repository (just like Github). Then make sure everything is staged and commited, and then push it up.

Check out this [quickguide](https://devcenter.heroku.com/articles/git) on deploying to Heroku.

## Bonus

Spec it out! Use [Simplecov](https://github.com/colszowka/simplecov) to monitor your test coverage. At this point, you should be familiar with writing some tests and definitely passing them. Use the labs as guides for how to write tests.
