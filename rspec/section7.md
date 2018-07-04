# list members on home page

touch spec/features/dashboards/listing_members_spec.rb

```ruby
require "rails_helper"

RSpec.feature "Listing Members" do

	before do
		@john = User.create(first_name: "John", last_name: "Doe", email: "john@example.com", password: "password")
		@sarah = User.create(first_name: "Sarah", last_name: "Jones", email: "sarah@example.com", password: "password")
	end

	scenario "shows a list of registered members" do
		visit "/"

		expect(page).to have_content("List of Members")
		expect(page).to have_content(@john.full_name)
		expect(page).to have_content(@sarah.full_name)
	end
end
```

# Devise

cat log/test.log

we have to permit firstname and lastname in registration controller, we dont have it, we need to create it


touch app/controllers/registrations_controller.rb



```ruby
class RegistrationsController < Devise::RegistrationsController

	private

	def sign_up_params
		params.require(:user).permit(:first_name, :last_name, :email, :password, :password_confirmation)
	end

	def account_update_params
		params.require(:user).permit(:first_name, :last_name, :email, :password, :password_confirmation, :current_password)
	end
end
```

in routes.rb

```ruby
Rails.application.routes.draw do
  devise_for :users, :controllers => { registrations: 'registrations' }
  get 'dashboards/index'

  resources :users do
	resources :exercises  	
  end

  # For details on the DSL available within this file, see http://guides.rubyonrails.org/routing.html

  root to: "dashboards#index"
end
```


# validates first name and last name


```ruby
require "rails_helper"
# signing_users_up_spec.rb
 
RSpec.feature "Users signup" do
 
  scenario "with valid credentials" do
    visit "/"
   
    click_link "Sign up"
   	fill_in "First name", with: "John"
   	fill_in "Last name", with: "Doe"
    fill_in "Email", with: "john@example.com"
    fill_in "Password",  with: "password"
    fill_in "Password confirmation",  with: "password"
    click_button "Sign up"
    expect(page).to have_content("You have signed up successfully.")

    visit "/"
    expect(page).to have_content("John Doe")
  end

  scenario "with invalid credentials" do
    visit "/"
   
    click_link "Sign up"
    fill_in "First name", with: ""
    fill_in "Last name", with: ""
    fill_in "Email", with: "john@example.com"
    fill_in "Password",  with: "password"
    fill_in "Password confirmation",  with: "password"
    click_button "Sign up"

    expect(page).to have_content("First name can't be blank")
    expect(page).to have_content("Last name can't be blank")
  end
end
```

we need to update all the specs, adding the first name and last name while creating a user

# pagination

gem "will_paginate-bootstrap"

```ruby
class DashboardsController < ApplicationController
  def index
  	@athletes = User.paginate(:page => params[:page])
  end
end
```

user.rb

```ruby
  self.per_page = 10
```

index.html.erb

```ruby
<h1>List of Members</h1>
<%= will_paginate @athletes, renderer: BootstrapPagination::Rails, class: "pull-left paginate" %>
```

# search for users feature rspec

spec/features/dashboards/searching_for_users_spec.rb

```ruby
require "rails_helper"

RSpec.feature "Searching for User" do

	before do
		@john = User.create(first_name: "John", last_name: "Doe", email: "john@example.com", password: "password")
		@sarah = User.create(first_name: "Sarah", last_name: "Doe", email: "sarah@example.com", password: "password")
	end

	scenario "with existing name returns all those users" do
		visit "/"

		fill_in "search_name", with: "Doe"
		click_button "Search"

		expect(page).to have_content(@john.full_name)
		expect(page).to have_content(@sarah.full_name)
		expect(current_path).to eq("/dashboards/search")
	end
end
```


```html
    <%= form_tag(search_dashboards_path, role: "search") %>
      <%= text_field_tag :search_name, nil, placeholder: "Search for friends" %>
      <%= submit_tag "Search", class: "btn btn-default" %>
    <% end %>
```

form_tag has no backing database!

```ruby
  resources :dashboards, only: [:index] do
  	collection do
  		post :search, to: "dashboards#search"
  	end
  end
```

dashboards controller

```ruby
  def search
  	@athletes = User.search_by_name(params[:search_name]).paginate(page: params[:page])
  end
```

user.rb

```ruby
  def self.search_by_name(name)
    names_array = name.split(' ')

    if names_array.size == 1
      where('first_name lIKE ? or last_name LIKE ?',
        "%#{names_array[0]}%", "%#{names_array[0]}%").order(:first_name)
    else
      where('first_name LIKE ? or first_name LIKE ? or last_name LIKE ? 
        or last_name LIKE ?', "%#{names_array[0]}",
        "%#{names_array[1]}%", "%#{names_array[0]}%",
        "%#{names_array[1]}%").order(:first_name)
    end
  end
```




# following friends spec

spec/features/followings/following_friends.spec.rb

```ruby
require "rails_helper"

RSpec.feature "Following Friends" do

	before do
		@john = User.create!(email: "john@example.com", password: "password", first_name: "John", last_name: "Doe")
		@peter = User.create!(email: "peter@example.com", password: "password", first_name: "Peter", last_name: "Corn")
		login_as(@john)
	end

	scenario "if signed in" do
		visit "/"

		# see all the users
		expect(page).to have_content(@john.full_name)
		expect(page).to have_content(@peter.full_name)

		href = "/friendships?friend_id=#{@john.id}"
		expect(page).not_to have_link("Follow", :href => href)

		# when the user clicks follow, the link should dissapear
		link = "a[href='/friendships?friend_id=#{@peter.id}']"
		find(link).click

		href = "/friendships?friend_id=#{@peter.id}"
		expect(page).not_to have_link("Follow", :href => href)
	end
end
```




```html
<% if current_user %>
	<%= link_to "Follow", friendships_path(friend_id: :athlete.id), method: :post, class: "btn btn-success" unless current_user.follows_or_same?(athlete) %>
<% end %>
```

user.rb

```ruby
def follows_or_same?(new_friend)
  friendships.map(&:friend).include?(new_friend) || self == new_friend
end
```

```ruby
  has_many :exercises
  has_many :friendships
  has_many :friends, through: :friendships, class_name: "User"
  # friend is user, is an alias. 
```

```ruby
rails g model friendship user:references friend:references
```

in routes.rb

```ruby
  resources :friendships, only: [:show, :create, :destroy]

```

```ruby
touch app/controllers/friendships_controller.rb
```

```ruby
class FriendshipsController < ApplicationController
	before_action :authenticate_user!

	def create
		friend = User.find(params[:friend_id])

		params[:user_id] = current_user.id

		Friendship.create(friendship_params) unless current_user.follows_or_same?(friend)
		redirect_to root_path
	end

	private

	def friendship_params
		params.permit(:friend_id, :user_id)
	end
end
```

friendship.rb

```ruby
class Friendship < ApplicationRecord
  belongs_to :user
  belongs_to :friend, class_name: "User"
  # there is no model called friend, a friend is an alias for the model user
end
```







































