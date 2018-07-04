# Section 3

all applications have user

devise

# 45. user sign up

- topic branch
- create spec

Signing up users

- visit root
- click on sign-up link
- email
- password
- password confirmation

- sign up

invalid signup

- do an invalid sign-up and ensure that if fails

spec/features/signing_up_users_spec.rb


```ruby
require "rails_helper"
# spec/features/signing_up_users_spec.rb


RSpec.feature "Signup users" do

	scenario "with valid credentials" do
		visit "/"
		click_link "Sign up"
		fill_in "Email", with: "user@example.com"
		fill_in "Password", with: "password"
		fill_in "Password confirmation", with: "password"
		click_button "Sign up"

		expect(page).to have_content("Welcome! You have signed up successfully.")
	end

	scenario "with invalid credentials" do
		visit "/"
		click_link "Sign up"
		fill_in "Email", with: ""
		fill_in "Password", with: ""
		fill_in "Password confirmation", with: ""
		click_button "Sign up"

		# expect(page).to have_content("You have not signed up succesfully.")
	end
end
```







# devise

rails generate devise:views
# 47. signin



```ruby
require "rails_helper"
# signing_users_in_spec.rb

RSpec.feature "Users signin" do
	before do
		@john = User.create!(email: "john@example.com", password: "password")
	end

	scenario "with valid credentials" do
		visit "/"

		click_link "Sign in"
		fill_in "Email", with: @john.email
		fill_in "Password", with: @john.password
		click_button "Log in"

		expect(page).to have_content("Signed in successfully.")
		expect(page).to have_content("Signed in as #{@john.email}")
		expect(page).not_to have_link("Sign in")
		expect(page).not_to have_link("Sign up")
	end
end
```


# 53. sign out


```ruby
require "rails_helper"
# signing_users_out_spec.rb

RSpec.feature "Signing out signed-in users" do
	before do
		@john = User.create!(email: "john@example.com", password: "password")
	
		visit "/"

		click_link "Sign in"
		fill_in "Email", with: @john.email
		fill_in "Password", with: @john.password
		click_button "Log in"
	end

	scenario "signed out successfully" do
		visit "/"
		click_link "Sign out"

		expect(page).to have_content("Signed out successfully.")
		expect(page).not_to have_link("Sign out")
	end
end
```
















































