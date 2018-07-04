testing

unit tests
- models, views, routes etc.
- each individual component is tested one item at a time
- typically results in very good coverage

functional tests
- controllers
- testing multiple components and how they collaborate with each other
- multiple models in a process

integration tests
- integration test is when a business process is followed to completion to meet a business objective
- typically involves emulating a user, for example logging in and clicking on a purchase button or links etc.


testing framework Rspec

rails comes testing framework unitest or minitest

rspec is more popular



# lecture 11 install rspec and capybara

Rspec and capybara

create articles for our blog

a blog has different post

- write out the scenario in a test file

first step - feature will fail!

- build the features one by one till the test passes

gemfile

```ruby
group :development, :test do
  # Call 'byebug' anywhere in the code to stop execution and get a debugger console
  gem 'byebug', platforms: [:mri, :mingw, :x64_mingw]
  gem "rspec-rails"
end

group :test do
  gem "capybara"
end
```

```ruby
bundle
```

add rspec, install it for our app 

```ruby
rails generate rspec:install
```

create a stub for our rspec

```ruby
bundle binstubs rspec-core
```


# 13. Creating an article

process for creating articles feature test and feature

- create a branch to do the development work
- write a feature test
- build features to make test pass one by one
- once the feature test passes with no errors - merge branch with master branch

git checkout -b article-feature-success

when we install rspec, we have two folders
rails_helper.rb, spec_helper.rb

create folder features, we will store all our features

mkdir spec/features

new file, creating_article_spec.rb


we need to require rails_helper.rb in the files where we write the test

scenario:

- visit root page
- click on new article
- fill in title
- fill in body
- create article

expectacions
- Article has been created
- articles path


scenario

```ruby
# creating_article_spec.rb

require "rails_helper"

Rspec.feature "Creating Articles" do
	scenario "A user creates a new article" do
		visit "/"

		click_link "New Article"

		fill_in "Title", with: "Creating a blog"
		fill_in "Body", with: "lorem Ipsum"

		click_button "Create Article"

		# expectations
		expect(page).to have_content("Articel has been created")
		expect(page.current_path).to eq(articles_path)
	end

end
```

expectations:

```ruby
require "rails_helper"

RSpec.feature "Creating Articles" do
	scenario "A user creates a new article" do
		
		[...]

		expect(page).to have_content("Articel has been created")
		expect(page.current_path).to eq(articles_path)
	end

end
```

in the terminal, rspec

Failure/Error: visit "/"

running just one test

rspec spec/features/creating_article_spec.rb


in routes.rb

```ruby
Rails.application.routes.draw do
	root to: "articles#index"
end
```

```ruby
Failure/Error: visit "/"

     ActionController::RoutingError:
       uninitialized constant ArticlesController
```

```ruby
rails g controller articles index
```

# 15. new article template

```html
# articles/index.html.erb

<%= link_to "New Article", new_article_path %>

```

in routes.rb

```ruby
Rails.application.routes.draw do
	root to: "articles#index"
	resources :articles
end
```

```ruby
class ArticlesController < ApplicationController
  def index
  end

  def new
  	
  end
end
```

create a view for new

new.html.erb

```html
<h3 class="text-center">
	Adding New Article
</h3>

<div class="row">
	<div class="col-md-12">
		<%= form_for(@article, :html => {class: "form-horizontal", role: "form"}) do |f| %>
			<div class="form-group">
				<div class="control-label col-md-1">
					<%= f.label :title %>
				</div>
				<div class="col-md-11">
					<%= f.text_field :title, class: "form-control", placeholder: "Title of article", autofocus: true %>
				</div>
			</div>

			<div class="form-group">
				<div class="control-label col-md-1">
					<%= f.label :body %>
				</div>
				<div class="col-md-11">
					<%= f.text_area :body, rows: 10, class: "form-control", placeholder: "Body of article" %>
				</div>
			</div>

			<div class="form-group">
				<div class="col-md-offset-1 col-md-11">
					<%= f.submit class: "btn btn-primary btn-lg pull-right" %>
				</div>
			</div>

		<% end %>
	</div>
</div>
```

```ruby
Failure/Error: <%= form_for(@article, :html => {class: "form-horizontal", role: "form"}) do |f| %>

     ActionView::Template::Error:
       First argument in form cannot contain nil or be empty
```

create the model

# 17. 

```ruby
class ArticlesController < ApplicationController
  def index
  end

  def new
  	@article = Article.new
  end
end
```

lets create the model

rails g model article title:string body:text
rails db:migrate

```ruby
  def create
  	@article = Article.new(article_params)
  	@article.save
  	flash[:success] = "Article has been created"
  	redirect_to articles_path
  end
```

```ruby
  <body>
		<% flash.each do |key, message| %>
			<div class="text-center alert alert-<%= key == "notice" ? 'success' : "danger" %>">
				<%= message %>
			</div>
		<% end %>
    <%= yield %>
  </body>
```

```ruby
1 example, 0 failures
```


# 19. add bootstrap

in gem file

```ruby
gem "bootstrap-sass"
gem "autoprefixer-rails"
```

app assets, stylesheets

new file, custom.css.scss

```ruby
@import "bootstrap-sprockets";
@import "bootstrap";
```

javascripts/application.js

```js

// require jquery
// require jquery_ujs
//= require bootstrap-sprockets

//= require rails-ujs
//= require turbolinks
//= require_tree .
```

# guarddddddd

it runs the test whenever we make a change

# 21. Add guard to the app

git checkout -b adding-guard

```ruby
group :development do
  # Access an IRB console on exception pages or by using <%= console %> anywhere in the code.
  gem 'web-console', '>= 3.3.0'
  gem 'listen', '>= 3.0.5', '< 3.2'
  # Spring speeds up development by keeping your application running in the background. Read more: https://github.com/rails/spring
  gem 'spring'
  gem 'spring-watcher-listen', '~> 2.0.0'

  gem "guard"
  gem "guard-rspec"
  gem "guard-cucumber"
end
```

bundle bindstubs guard


guard init

it creates a Guardfile in our app

```
guard
```

testing framework keeps working in the background

issue with guard

in spec, we have the folder features.
but there are other folders

whenever i make a change, i want to run the features folder

in guard

```ruby
# rails files
  watch(%r{^app/controllers/(.+)_(controller)\.rb$}) { "spec/features" }
  watch(%r{^app/models/(.+)\.rb$}) { "spec/features" }
```

anytime i make a change to the contorller, its gonna run the features folder


```ruby
# Rails config changes
  watch(rails.spec_helper)     { rspec.spec_dir }
  watch(rails.routes)          { "spec" }  # { "#{rspec.spec_dir}/routing" }
  watch(rails.app_controller)  { "#{rspec.spec_dir}/controllers" }
```


```ruby
  # Capybara features specs
  watch(rails.view_dirs)     { "spec/features" } # { |m| rspec.spec.call("features/#{m[1]}") }
  watch(rails.layouts)       { |m| rspec.spec.call("features/#{m[1]}") }
```

awesome, when i do a mistake, the test are running!

without this changes, we dont run the specs that we created, guard runs automatically the x_controller_spec



cucumber --init
what the fuck does this?



# 18. adding validations


creating_article_spec

second scenario
a user fails to create a new article


```ruby
RSpec.feature "Creating Articles" do
	
	[...]

	scenario "A user fails to create a new article" do
		visit "/"

		click_link "New Article"

		fill_in "Title", with: ""
		fill_in "Body", with: ""

		click_button "Create Article"

		expect(page).to have_content("Article has not been created")
		expect(page).to have_content("Title can't be blank")
		expect(page).to have_content("Body can't be blank")
	end

end
```



```ruby
  def create
  	@article = Article.new(article_params)
  	if @article.save
  	 flash[:success] = "Article has been created"
     redirect_to articles_path
    else
     flash[:danger] = "Article has not been created"
     render :new
    end
  end
```

we need to add the validations, otherwise this never fails

```ruby
class Article < ApplicationRecord
	validates :title, presence: true
	validates :body, presence: true
end
```

in new.html.erb

```html

<%= form_for(@article, :html => {class: "form-horizontal", role: "form"}) do |f| %>

			<% if @article.errors.any? %>
				<ul>
					<% @article.errors.full_messages.each do |msg| %>
						<li>
							<%= msg %>
						</li>
					<% end %>
				</ul>

			<% end %>
```

the flash is there when you go to /new

solution

```ruby
  def create
  	@article = Article.new(article_params)
  	if @article.save
  	 flash[:success] = "Article has been created"
     redirect_to articles_path
    else
     flash.now[:danger] = "Article has not been created"
     render :new
    end
  end
```

# 29. lising articles feature test

- create branch

before my scenario
- create 2 articles to display

scenario
- list the two articles

- expect both article titles and bodies to be present


before do, before we run the scenario


features/listing_articles_spec.rb


```ruby
require "rails_helper"

RSpec.feature "Listing Articles" do

	before do
		@article1 = Article.create(title: "The first article", body: "Lorem ipsum dolor sit amet, consectetur.")
		@article2 = Article.create(title: "The second article", body: "Body of 2nd article.")
	end

	scenario "A user lists all articles" do
		visit "/"

		expect(page).to have_content(@article1.title)
		expect(page).to have_content(@article1.body)
		expect(page).to have_content(@article2.title)
		expect(page).to have_content(@article2.body)

		expect(page).to have_link(@article1.title)
		expect(page).to have_link(@article2.title)
	end
end
```



articles/index.html.erb

```html
<% @articles.each do |article| %>
	<div class="well well-lg">
		<div class="article-title">
			<%= link_to article.title, article_path(article) %>
		</div>
		<div class="article-body">
			<%= truncate(article.body, length: 500) %>
		</div>
	</div>
<% end %>
```

truncate method is cool!


```ruby
class Article < ApplicationRecord
	validates :title, presence: true
	validates :body, presence: true

	default_scope { order(created_at: :desc) }
end
```


DEFAULT SCOPE !!! for ever

what if there is no articles?

new test scenario!!!

```ruby

RSpec.feature "Listing Articles" do

	before do
		@article1 = Article.create(title: "The first article", body: "Lorem ipsum dolor sit amet, consectetur.")
		@article2 = Article.create(title: "The second article", body: "Body of 2nd article.")
	end

	[...]

	scenario "A user has no articles" do

		Article.delete_all

		visit "/"

		expect(page).not_to have_content(@article1.title)
		expect(page).not_to have_content(@article1.body)
		expect(page).not_to have_content(@article2.title)
		expect(page).not_to have_content(@article2.body)

		expect(page).not_to have_link(@article1.title)
		expect(page).not_to have_link(@article2.title)

		within ("h1#no-articles") do
			expect(page).to have_content("No Articles Created")
		end
	end

end

```

# 33. showing article feature

show article feature test

- create branch
- create 1 article to display

- show the article title and details

we also test the path!

features/show_article_spec.rb

```ruby
require "rails_helper"

RSpec.feature "Showing an article" do

	before do
		@article = Article.create(title: "The first article", body: "Lorem ipsum dolor sit amet, consectetur.")
	end

	scenario "A user shows an article" do
		visit "/"
		# visit "/articles/#{@article.id}"

		click_link @article.title

		expect(page).to have_content(@article.title)
		expect(page).to have_content(@article.body)
		expect(current_path).to eq(article_path(@article))
	end

end
```

# 35. deal with article not found exception

exception scecnario!

we want to catch the exception

http://localhost:3000/articles/4

this is a request that failed

spec/requests/articles_spec.rb

```ruby
require "rails_helper"

RSpec.describe "Articles", type: :request do
	
	before do
		@article = Article.create(title: "Title One", body: "Body of article one")
	end

	describe "GET /articles/:id" do
		context "with existing article" do
			before { get "/articles/#{@article.id}" }

			it "handles existing article" do
				expect(response.status).to eq 200
			end
		end

		context "with non-existing article" do
			before { get "/articles/xxxx" }

			it "handles non-existing article" do
				expect(response.status).to eq 302
				flash_message = "The article you are looking for could not be found"
				expect(flash[:alert]).to eq flash_message
			end
		end
	end
end
```


```ruby
class ApplicationController < ActionController::Base
  protect_from_forgery with: :exception

  rescue_from ActiveRecord::RecordNotFound, with: :resource_not_found

  protected

  def resource_not_found
  	
  end
end
```

empty method cause well overwrite it in the contorllers. in articles, but in others


in guard, if you write rspec, or press enter, you run all the specs

# 37. Editing an article


features/editing_article_spec.rb

```ruby
require "rails_helper"

RSpec.feature "Editing an article" do
	
	before do
		@article = Article.create(title: "Title One", body: "Body of article one")
	end

	scenario "A user updates an article" do
		visit "/"

		click_link @article.title
		click_link "Edit Article"

		fill_in "Title", with: "Updated Title"
		fill_in "Body", with: "Updated Body of Article"
		click_button "Update Article"

		# expectations
		expect(page).to have_content("Article has been updated")
		expect(page.current_path).to eq(article_path(@article))
	end

	scenario "A user fails to update an article" do
		visit "/"

		click_link @article.title
		click_link "Edit Article"

		fill_in "Title", with: ""
		fill_in "Body", with: "Updated Body of Article"
		click_button "Update Article"

		# expectations
		expect(page).to have_content("Article has not been updated")
		expect(page.current_path).to eq(article_path(@article))
	end
end
```

# 39. delete an article

features/delete_article_spec.rb

```ruby
require "rails_helper"

RSpec.feature "Deleting an Article" do

	before do
		@article = Article.create(title: "Title One", body: "Body of article one")
	end

	scenario "A user deletes an article" do
		visit "/"

		click_link @article.title
		click_link "Delete Article"

		expect(page).to have_content("Article has been delated")
		expect(current_path).to eq(articles_path)
	end
end
```



























































