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

#users_controller.rb
Shows the generated controller for user CRUD actions. 
A couple of notes: 
Instance variables begin with '@'
The UserController inherits from ApplicationController using the syntax: 
UserController < ApplicationController

Checkout the syntax for the create user method: 

  def create
    @user = User.new(user_params)

    respond_to do |format|
      if @user.save
        format.html { redirect_to @user, notice: 'User was successfully created.' }
        format.json { render :show, status: :created, location: @user }
      else
        format.html { render :new }
        format.json { render json: @user.errors, status: :unprocessable_entity }
      end
    end
  end
  
Other interesting things...

All these methods seem to be broken up into two sections: 

#before_action :set_user, only: [:show, :edit, :update, :destroy]
#private  


The section below private seems to be private methods used by the rest of the controller methods? 

    # Use callbacks to share common setup or constraints between actions.
    def set_user
      @user = User.find(params[:id])
    end

    # Never trust parameters from the scary internet, only allow the white list through.
    def user_params
      params.require(:user).permit(:name, :email)
    end

Other keywords: 

##Active Record - Enables getting the all the user models? 


Problems so far with scaffolding: 

##No data validations. Our User model accepts data such as blank names and invalid email addresses without complaint.
##No authentication. We have no notion of logging in or out, and no way to prevent any user from performing any operation.
##No tests. This isn’t technically true—the scaffolding includes rudimentary tests—but the generated tests don’t test for data validation, authentication, or any other custom requirements.
##No style or layout. There is no consistent site styling or navigation.
##No real understanding. If you understand the scaffold code, you probably shouldn’t be reading this book.


#2.3.1 Micropost Model and Scaffolding
Model and routes
##rails generate scaffold Micropost content:text user_id:integer
Update database
#rails db:migrate

Note the structure of the controller template generated for Microposts: 
```
class MicropostsController < ApplicationController
  .
  .
  .
  def index
    .
    .
    .
  end

  def show
    .
    .
    .
  end

  def new
    .
    .
    .
  end

  def edit
    .
    .
    .
  end

  def create
    .
    .
    .
  end

  def update
    .
    .
    .
  end

  def destroy
    .
    .
    .
  end
end
```
#2.3.2 Validation

In model file for Microposts, add length validation for max length: 

#validates :content, length: { maximum: 140 }

this create error message in view automatically

#2.3.3 has_many, belongs_to and `rails console`

express many-to-one relationship in User model
```
class User < ApplicationRecord
  has_many :microposts
end

class Micropost < ApplicationRecord
  belongs_to :user
  validates :content, length: { maximum: 140 }
end
```

`rails console` 

opens a rails console

sample instructions: 

#first_user = User.first
gets first user record
#first_user.microposts
gets user's microposts
#micropost = first_user.microposts.first
gets first micropost
#micropost.user
this just gives error, but supposed to give user
ahh, I forgot to add `belongs_to :user to Micropost model
quitting console and restarting, then for short typing: 
#User.first.microposts.first.user
gives me the user record, cool. 

also, make a "property" required: 
```
  validates :content, length: { maximum: 140 },
                      presence: true
```

#Edit the user view to show the user first micrpost: 
```
<p>
  <strong>First Post:</strong>
  <%= @user.microposts.first.content %>
</p>
```


#2.3.4 Inheritance Hierarchies