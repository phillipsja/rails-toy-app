# react-toy-app

This corresponds to chapter 2 of the (rails book)[https://www.railstutorial.org/book]

Highlights thus far: 

- install the local gems while suppressing the installation of production gems
#bundle install --without production

- using heroku for the "production" version
#heroku create
#git push heroku master

still don't understand why we need this when we can local server

- using scaffolding to set up models,
#rails generate scaffold User name:string email:string

- migrate the database
#rails db:migrate

Note: in every version of Rails before Rails 5, the db:migrate command used rake in place of rails. 

Probably the two most common Rake commands in a Rails context are rake db:migrate (to update the database with a data model) and rake test (to run the automated test suite). In these and other uses of rake, it’s important to ensure that the command uses the version of Rake corresponding to the Rails application’s Gemfile, which is accomplished using the Bundler command bundle exec. Thus, the migration command

#rake db:migrate
would be written as

#	bundle exec rake db:migrate
  
...


Pretty cool, the scafolding setups up basic crud and views for the model. 

New routes entry in config/routes.rb, i.e.
#resources :users
maps user URLs to controller actions


