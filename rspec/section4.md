# section 4 associations

Articles will belong to users

Users will own articles

Users - 1 side of this

Articles - many

1 user can have many articles

an article can only belong to 1 user

foreign key of user_id in the articles table

steps:

- create the spec

# build one to many relationship

# creating articles

add in controller @article.user = current_user

```ruby
require "rails_helper"

RSpec.feature "Creating Articles" do

	before do
		@john = User.create!(email: "john@example.com", password: "password")
		login_as(@john)
	end

	scenario "A user creates a new article" do
		visit "/"

		click_link "New Article"

		fill_in "Title", with: "Creating a blog"
		fill_in "Body", with: "lorem Ipsum"

		click_button "Create Article"

		expect(Article.last.user).to eq(@john) # <---------
		expect(page).to have_content("Article has been created")
		expect(page.current_path).to eq(articles_path)
		expect(page).to have_content("Created by: #{@john.email}") # <-----------
	end

	# add validations in your model to fail
	[...]
end
```



```ruby
NoMethodError:
       undefined method `login_as' for #<RSpec::ExampleGroups::CreatingArticles:0x00007fefca3a6ad0>
```


```ruby
# rails_helper.rb
include Warden::Test::Helpers
# Add additional requires below this line. Rails is not loaded until this point!
```



# activerecord, add 

```ruby
rails g migration add_user_to_articles user:references
```

# 57.

rspec spec/features/editing_article_spec.rb

```ruby
Capybara::ElementNotFound:
       Unable to find visible link "Title One"
```

```ruby
require "rails_helper"

RSpec.feature "Editing an article" do
	
	before do
		john = User.create(email: "john@example", password: "password")
		login_as(john)
		@article = Article.create(title: "Title One", body: "Body of article one", user: john)
	end

	[...]
end
```

# show

rspec spec/features/show_article_spec.rb

```ruby
require "rails_helper"

RSpec.feature "Showing an article" do
	before do
		john = User.create(email: "john@example", password: "password")
		login_as(john)
		@article = Article.create(title: "The first article", body: "Lorem ipsum dolor sit amet, consectetur.", user: john)
	end

	[...]
end
```


# listing article spec

rspec spec/features/listing_articles_spec.rb

```ruby
require "rails_helper"

RSpec.feature "Listing Articles" do
	before do
		john = User.create(email: "john@example", password: "password")
		@article1 = Article.create(title: "The first article", body: "Lorem ipsum dolor sit amet, consectetur.", user: john)
		@article2 = Article.create(title: "The second article", body: "Body of 2nd article.", user: john)
	end

	[...]
end
```

# delete article

rspec spec/features/delete_article_spec.rb

```ruby
Capybara::ElementNotFound:
       Unable to find visible link "Title One"
```

```ruby
require "rails_helper"
# spec/features/delete_article.rb

RSpec.feature "Deleting an Article" do
	before do
		john = User.create(email: "john@example", password: "password")
		login_as(john)
		@article = Article.create(title: "Title One", body: "Body of article one", user: john)
	end

	[...]
end
```


# Restricting access

Restric access

- hide the "New Article" button from non-signed in users
- hide the "Edit" and "Delete" buttons from non-owners of the article
- permit the owners to edit or delete their own articles.

show new article button to signed in users

```ruby
require "rails_helper"

RSpec.feature "Listing Articles" do
	before do
		@john = User.create(email: "john@example", password: "password")
		@article1 = Article.create(title: "The first article", body: "Lorem ipsum dolor sit amet, consectetur.", user: @john)
		@article2 = Article.create(title: "The second article", body: "Body of 2nd article.", user: @john)
	end

	scenario "with articles created and user not signed in" do
		visit "/"

		expect(page).to have_content(@article1.title)
		expect(page).to have_content(@article1.body)
		expect(page).to have_content(@article2.title)
		expect(page).to have_content(@article2.body)

		expect(page).to have_link(@article1.title)
		expect(page).to have_link(@article2.title)

		expect(page).not_to have_link("New Article")
	end

	scenario "with articles created and user signed in" do
		login_as(@john)
		visit "/"

		expect(page).to have_content(@article1.title)
		expect(page).to have_content(@article1.body)
		expect(page).to have_content(@article2.title)
		expect(page).to have_content(@article2.body)

		expect(page).to have_link(@article1.title)
		expect(page).to have_link(@article2.title)

		expect(page).to have_link("New Article")
	end

	[...]
end
```

# hide the "edit" and "delete" buttons from non-owners of the article

show_article_spec.rb

```ruby
require "rails_helper"

RSpec.feature "Showing an article" do
	before do
		@john = User.create(email: "john@example", password: "password")
		@fred = User.create(email: "fred@example", password: "password")
		
		@article = Article.create(title: "The first article", body: "Lorem ipsum dolor sit amet, consectetur.", user: @john)
	end

	scenario "to non-signed in user hide the Edit and Delete buttons" do
		visit "/"

		click_link @article.title

		expect(page).to have_content(@article.title)
		expect(page).to have_content(@article.body)
		expect(current_path).to eq(article_path(@article))

		expect(page).not_to have_link("Edit Article")
		expect(page).not_to have_link("Delete Article")
	end

	scenario "to non-owner hide the Edit and Delete buttons" do
		login_as(@fred)
		visit "/"
		click_link @article.title

		expect(page).to have_content(@article.title)
		expect(page).to have_content(@article.body)
		expect(current_path).to eq(article_path(@article))

		expect(page).not_to have_link("Edit Article")
		expect(page).not_to have_link("Delete Article")
	end

	scenario "A signed in owner sees both the Edit and Delete buttons" do
		login_as(@john)
		visit "/"
		click_link @article.title

		expect(page).to have_content(@article.title)
		expect(page).to have_content(@article.body)
		expect(current_path).to eq(article_path(@article))

		expect(page).to have_link("Edit Article")
		expect(page).to have_link("Delete Article")
	end
end
```

update spec to verify owner sees edit delete btns

we need to restrict this also in our controllers! we made it just on the views

# Controller level restrictions

# spec/requests/articles_spec.rb


```ruby
require "rails_helper"
# spec/requests/articles_spec.rb

RSpec.describe "Articles", type: :request do
	before do
		@john = User.create(email: "john@example.com", password: "password")
		@fred = User.create(email: "fred@example.com", password: "password")
		@article = Article.create!(title: "Title One", body: "Body of article one", user: @john)
	end

	describe "GET /articles/:id/edit" do
		context "with non-signed in suer" do
			before { get "/articles/#{@article.id}/edit" }
			it "redirects to the signin page" do
				expect(response.status).to eq 302
				flash_message = "You need to sign in or sign up before continuing."
				expect(flash[:alert]).to eq flash_message
			end
		end

		context "with signed in user who is non-owner" do
			before do
				login_as(@fred)
				get "/articles/#{@article.id}/edit"
			end

			it "redirects to the home page" do
				expect(response.status).to eq 302
				flash_message = "You can only edit your own article."
				expect(flash[:alert]).to eq flash_message

			end
		end
	end

```

```ruby
1) Articles GET /articles/:id/edit with signed in user who is non-owner redirects to the home page
     Failure/Error: expect(response.status).to eq 302

       expected: 302
            got: 200
```

we were able to go to the edit page, although we are not the owner of the article. we need to fix this

```ruby
  def edit
    unless @article.user == current_user
      flash[:alert] = "You can only edit your own article."
      redirect_to root_path
    end
  end

  def update
    unless @article.user == current_user
      flash[:danger] = "You can only edit your own article."
      redirect_to root_path
    else
      if @article.update(article_params)
        flash[:success] = "Article has been updated"
        redirect_to @article
      else
        flash.now[:danger] = "Article has not been updated"
        render :edit
      end
    end
  end
```

```ruby

require "rails_helper"
# spec/requests/articles_spec.rb

RSpec.describe "Articles", type: :request do
	before do
		@john = User.create(email: "john@example.com", password: "password")
		@fred = User.create(email: "fred@example.com", password: "password")
		@article = Article.create!(title: "Title One", body: "Body of article one", user: @john)
	end

	describe "GET /articles/:id/edit" do
		
		[...]

		context "with signed in user as owner successful edit" do
			before do
				login_as(@john)
				get "/articles/#{@article.id}/edit"
			end
			
			it "successfully edits article" do
				expect(response.status).to eq 200
			end

		end
	end
	[...]
```






















