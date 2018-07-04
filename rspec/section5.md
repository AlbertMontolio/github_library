# Section 5. Comments and Real-time features

- create a branch
- spec
- building the functionality

# build

```ruby
@comment = @article.comments.build
```

rails routes | grep comments

# build


```ruby
  def show
    @comment = @article.comments.build
  end
```

returns empty object

```ruby
	def create
		@comment = @article.comments.build(comment_params)
	end
```

# app/helpers

we add methods in helpers that we will use in our views!!!

```ruby
module ArticlesHelper
	def persisted_comments(comments)
		comments.reject { |comment| comment.new_record? }
	end
end
```

# adding comments spec

```ruby
require "rails_helper"
# adding_comments_spec.rb

RSpec.feature "Adding reviews to Articles" do

	before do
		@john = User.create(email: "john@example", password: "password")
		@fred = User.create(email: "fred@example", password: "password")
		@article = Article.create(title: "The first article", body: "Lorem ipsum dolor sit amet, consectetur.", user: @john)
	end

	scenario "permits a signed in user to write a review" do
		login_as(@fred)
		visit "/"

		click_link @article.title
		fill_in "New Comment", with: "An amazing article"
		click_button "Add Comment"

		expect(page).to have_content("Comment has been created")
		expect(page).to have_content("An amazing article")
		expect(current_path).to eq(article_path(@article.id))
	end
end

```

# sign in to comment spec

comments feature

- request spec
- a signed in user gets to comment
- a non-signed in user gets redirected to sign in page

spec/requests/comments_spec.rb

```ruby

require "rails_helper"
# spec/requests/comments_spec.rb

RSpec.describe "Comments", type: :request do
	before do
		@john = User.create(email: "john@example.com", password: "password")
		@fred = User.create(email: "fred@example.com", password: "password")
		@article = Article.create!(title: "Title One", body: "Body of article one", user: @john)
	end

	describe "POST /articles/:id/comments" do
		context "with a non signed-in user" do
			before do
				post "/articles/#{@article.id}/comments", params: { comment: { body: "Awesome blog" } }
			end

			it "redirect user to the signing page" do
				flash_message = "Please sign in or sign up first"
				expect(response).to redirect_to(new_user_session_path)
				expect(response.status).to eq 302
				expect(flash[:alert]).to eq flash_message
			end
		end

		context "with a logged in user" do
			before do
				login_as(@fired)
				post "/articles/#{@article.id}/comments", params: { comment: { body: "Awesome blog" } }
			end

			it "create the comment successfully" do
				flash_message = "Comment has been created"
				expect(response).to redirect_to(article_path(@article.id))
				expect(response.status).to eq 302
				expect(flash[:notice]).to eq flash_message
			end
		end
	end
end
```

# remove helpers that you don't use

git rm spec/helpers/comments_helper_spec.rb

# 43. Implement real-time comments with ActionCable

cable.yml

setup actioncable connection

channels/application_cable/connection.rb


```ruby
module ApplicationCable
  class Connection < ActionCable::Connection::Base
  	identified_by :current_user

  	def connect
  		self.current_user = find_current_user
  	end

  	def disconnect
  		
  	end

  	protected

  	def find_current_user
  		if current_user == User.find_by(id: cookies.signed['user.id'])
  			current_user
  		else
  			reject_unauthorized_connection
  		end
  	end

  end
end
```


# authenticate user on the websocket server

config/initializers
new file warden.rb

```ruby
# sets the cookie when the user logs in, makes it available for the find current uesr method in connection.rb
Warden::Mananger.after_set_user do |user, auth, opts|
	scope = opts[:scope]
	auth.cookies.signed["#{scope}.id"] = user.id
end

# clears cookie when the user signs out
Warden::Manager.before_logout do |user, auth, opts|
	auth.cookies.signed["#{scope}.id"] = nil
end
```

access action cable server via rails

config, application.rb

```ruby
module BlogApp
  class Application < Rails::Application
  	config.action_cable.mount_path = "/cable"
```

routes.rb

# create messaging channels

our channel is comments

rails g channel comments


```ruby
Running via Spring preloader in process 16624
      create  app/channels/comments_channel.rb
   identical  app/assets/javascripts/cable.js
      create  app/assets/javascripts/channels/comments.coffee
```

comments_channel.rb

```ruby
class CommentsChannel < ApplicationCable::Channel
  def subscribed
  	# this method is called whenever a client subscribes to a channel
    stream_from "comments"
  end

  def unsubscribed
    # Any cleanup needed when channel is unsubscribed
  end
end
```

now we need to broadcast the comment, to send, to the channel

when the comment is created, we need to broadcast it to the channel

the comment is created in our controller!

```ruby
class CommentsController < ApplicationController
	before_action :set_article

	def create
		unless current_user
			flash[:alert] = "Please sign in or sign up first"
			redirect_to new_user_session_path
		else
			@comment = @article.comments.build(comment_params)
			@comment.user = current_user

			if @comment.save
				ActionCable.server.broadcast "comments",
				render(partial: "comments/comment", object: @comment)
				flash[:notice] = "Comment has been created"
			else
```

now we need to create the partial!

new file
views/comments/_ comment.html.erb

display with same format as in show


```ruby
<div class="comment-body">
	<%= comment.body %>
</div>
<div class="comment-time">
	<%= time_ago_in_words(comment.created_at) %>
	ago by <%= comment.user.email %>
</div>
<hr>
```
now we need ajax to send the formed data to the server

show.html.erb

```html
	<%= form_for [@article, @comment], remote: true, :html => { class: "form-horizontal", role: "form" } do |f| %>
```


comments.coffe

```ruby
App.comments = App.cable.subscriptions.create "CommentsChannel",
  connected: ->
    # Called when the subscription is ready for use on the server

  disconnected: ->
    # Called when the subscription has been terminated by the server

  received: (data) ->
    # Called when there's incoming data on the websocket for this channel
    $('#messages').prepend(data)
```


indentation is important in .coffe files

show.html.erb

```html
<div class="col-md-12">
	<header>
		<h2>Comments</h2>
	</header>
	<div class="col-md-10">
		<% if @article.comments.any? %>
			<div id="messages">
				<% persisted_comments(@comments).each do |comment| %>
					<div class="comment-body">
						<%= comment.body %>
					</div>
					<div class="comment-time">
						<%= time_ago_in_words(comment.created_at) %>
						ago by <%= comment.user.email %>
					</div>
					<hr>
				<% end %>
			</div>
		<% else %>
			There are no comments to show.
		<% end %>
	</div>
</div>
```

it works!!!

environments/development.rb
config.action_cable.disable_request_forgery_protection = true

it allows more ports than 3000



















