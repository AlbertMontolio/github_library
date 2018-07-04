# workout app

create a new rails 5 project for the workout application

rvm use 2.3.0@workout --create --default

rvm list gemsets

gem install rails -v 5.0.0.1

# set rspec and capybara

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

rails g rspec:install

bundle binstubs rspec-core

rails_helper.rb

```ruby
  config.backtrace_exclusion_patterns = [
    /\/lib\d*\/ruby\//,
    /bin\//,
    /gems/,
    /spec\/spec_helper\.rb/,
    /lib\/rspec\/(core|expectations|matchers|mocks)/
  ]

end
```

# write creating home page feature spec

requirements

creating home page feature spec

1. create a folder within the spec fodler called features
2. in the features folder create a file called creating_homepage_spec.rb

details of the spec

when a user visits the home page, we want to see four things

1. we should see a link called 'home'
2. we should see a link called 'athletes den'
3. we should see content 'workout lounge'
4. we should see content 'show off your workout'

call the controller dashboardscontroller



# solution

```ruby
require "rails_helper"

RSpec.feature "Creating Home Page" do

	scenario do
		visit '/'

		expect(page).to have_link('Home')
		expect(page).to have_link('Athletes Den')
		expect(page).to have_content('Workout Lounge!')
		expect(page).to have_content('Show off your workout')
	end
end
```


# adding guard to gemfile

```ruby
group :development, :test do
  # Call 'byebug' anywhere in the code to stop execution and get a debugger console
  gem 'byebug', platforms: [:mri, :mingw, :x64_mingw]
  gem "rspec-rails"
  gem "guard"
  gem "guard-rspec"
end
```

bundle install

guard init



Gemfile

```ruby
  watch(%r{^app/models/(.+)\.rb$}) { |m| "spec/features/#{m[1]}s" }
  watch(%r{^app/controllers/(.+)_(controller)\.rb$}) { |m| "spec/features/#{m[1]}" }
  watch(rails.routes)          { "#{rspec.spec_dir}" }

  # Rails config changes
```


```ruby
rspec spec/features/creating_homepage_spec.rb
```

```ruby
require "rails_helper"

RSpec.feature "Creating Home Page" do
	scenario do
		visit '/'

		expect(page).to have_link('Home')
		expect(page).to have_link('Athletes Den')
		expect(page).to have_content('Workout Lounge!')
		expect(page).to have_content('Show off your workout')
	end
end
```

spec is failing :D


# add bootstrap to the project

gem "bootstrap-sass"

stylesheets/custom.css.scss

```body
@import "bootstrap-sprockets";
@import "bootstrap"
```

application.js

```js
//= require rails-ujs
//= require turbolinks

//= require bootstrap-sprockets

//= require_tree .
```

# setup devise


# signup a user

```ruby
require "rails_helper"
# signing_users_up_spec.rb
 
RSpec.feature "Users signup" do
 
  scenario "with valid credentials" do
   
    visit "/"
   
    click_link "Sign up"
   
    fill_in "Email", with: "john@example.com"
   
    fill_in "Password",  with: "password"
   
    fill_in "Password confirmation",  with: "password"
   
    click_button "Sign up"
   
    expect(page).to have_content("You have signed up successfully.")
 
  end
end
```

In the Guardfile we have the following line:

```ruby
watch(rails.view_dirs) { |m| "spec/features/#{m[1]}" }
```

This line tells us that when the files in the app/views folder change, we want to run the specs in 'spec/features/<folder>' folder, where <folder> is the subfolder under views in which the file changed. So if you modify a file in app/views/articles, our <folder> will be 'articles'.

We are doing this because we don't want to run specs unrelated to the view being modified.

```ruby
# Capybara features specs
# watch(rails.view_dirs)     { |m| rspec.spec.call("features/#{m[1]}") }
watch(rails.view_dirs)     { |m| "spec/features/#{m[1]}" }
```

# signing users in feature spec



users/signing_users_in_spec.rb

```ruby
require "rails_helper"
 
RSpec.feature "Users signin" do
 
  before do
   
    @john = User.create!(email: "john@example.com", password: "password")
 
  end
 
  scenario "with valid credentials" do
   
    visit "/"
   
    click_link "Sign in"
   
    fill_in "Email", with: @john.email
   
    fill_in "Password",  with: @john.password
   
    click_button "Log in"
   
    expect(page).to have_content("Signed in successfully.")
   
    expect(page).to have_content("Signed in as #{@john.email}")
 
  end
end
```


Gemfile

```ruby
  watch(%r{^app/controllers/(.+)_(controller)\.rb$}) { |m| "spec/features/#{m[1]}" }
  watch(rails.routes)          { "#{rspec.spec_dir}" }
  watch(%r{^app/views/layouts/application.html.erb$}) { "spec/features" }
```

No such file or directory - features

it's coming from a cucumber block

comment out cucumber in Guardfile

# sign out

features/users/signing_users_out_spec.rb

```ruby
require "rails_helper"
RSpec.feature "Signing users out" do
 before do
   @john = User.create!(email: "john@example.com",
                        password: "password")
   visit '/'
   click_link "Sign in"
   fill_in "Email", with: @john.email
   fill_in "Password",  with: @john.password
   click_button "Log in"
 end
 scenario do
   visit "/"
   click_link "Sign out"
   expect(page).to have_content("Signed out successfully.")
 end
end
```

# hiding links upon signin

1. make sure the user has signed up
2. make sure the user has signed in
3. upon successful signin, we should see only the "sign out" link
4. we should neither see the "sign in" link nor the "sign up" link

hiding_signin_signup_spec.rb

```ruby
require "rails_helper"
# hiding_signin_signup_spec.rb

RSpec.feature "Hiding signin link" do

	before do
		@john = User.create!(email: "john@example.com", password: "password")
	end

	scenario "upon successful signin" do
		visit "/"

		click_link "Sign in"
		fill_in "Email", with: @john.email
		fill_in "Password", with: @john.password
		click_button "Log in"

		expect(page).to have_link("Sign out")
		expect(page).not_to have_link("Sign in")
		expect(page).not_to have_link("Sign up")
	end
end
```

history|grep rails

# Exercise management

1. User to sign in
2. Upon successful sign in, user can click on a link "My Lounge" to go to an area that shows the details of her/his workouts
3. user can then click on another link (to be styled as a button)
4. user can fill out a form with the details of workout
5 alternively, user can click on a link to go back to the lounge

exercise facts:
1. has a duration in minutes
2. working details (essentially a description of the activity)
3. date of the activity
4. can only exist in the ontext of a user (exercise must be owned by a user)

exercise expectactions upon creation:
1. the new exercise's user_id has to be the same as the logged in user's
2. the current page should be the exercise's show page

# spec/features/exercises/creating_exercise_spec.rb

```ruby
require "rails_helper"

RSpec.feature "Creating Exercise"

	before do
		@john = User.create(email: "john@example.com", password: "password")
		loging_as(@john)
	end

	scenario "with valid inputs" do
		visit "/"

		click_link "My Lounge"
		click_link "New Workout"
		expect(page).to have_link("Back")

		fill_in "Duration", with: 70
		fill_in "Workout Details", with: "Weight lifting"
		fill_in "Activity date", with: "2016-07-26"
		click_button "Create Exercise"

		# expectactions
		expect(page).to have_content("Exercise has been created")
		exercise = Exercise.last
		expect(current_path).to eq(user_exercise_path(@john, exercise))
		expect(exercise.user_id).to eq(@john.id)
	end
end
```


```ruby
NoMethodError:
       undefined method `loging_as' for #<RSpec::ExampleGroups::CreatingExercise:0x00007fc6adb84130>
```

in rails helper

```ruby
# Add additional requires below this line. Rails is not loaded until this point!
include Warden::Test::Helpers
```

advanced rails

```ruby
	def show
		@exercise = current_user.exercises.find params[:id]
	end

	def new
		@exercise = current_user.exercises.new
	end

	def create
		@exercise = current_user.exercises.new(exercise_params)
```

# Unsuccessful creation of exercise

1. Flash message is displayed upon unsuccessful creation of an exercise
2. ensure that duration_in_min is a number
3. workout details field is required
4. activity date is required

Below the successful creation scenario add the following;

creating_exercise_spec.rb

```ruby
scenario "with invalid inputs" do
   visit "/"
 
   click_link "My Lounge"
   click_link "New Workout"
   expect(page).to have_link("Back")
 
   fill_in "Duration", with: ""
   fill_in "Workout details",  with: ""
   fill_in "Activity date",  with: ""
   click_button "Create Exercise"
 
   expect(page).to have_content("Exercise has not been created")
   expect(page).to have_content("Duration in min is not a number")
   expect(page).to have_content("Workout details can't be blank")
   expect(page).to have_content("Activity date can't be blank")
 end
 ```

git rm spec/models/exercise_spec.rb

use this in case names in db does not match with the names in the form

```ruby
  alias_attribute :workout_details, :workout
  alias_attribute :activity_date, :workout_date
  # to make same name as in the form
```


# add jquery-ui datepicker

```ruby
gem "jquery-ui-rails"
```

application.css

```css

 *= require jquery-ui

 *= require_tree .

 *= require_self
```



```js
//= require jquery-ui

//= require rails-ujs
// require turbolinks

//= require bootstrap-sprockets

//= require_tree .

```

disable turbolinks

in html application.html.erb remove all references to turbolinks


```html
    <%= stylesheet_link_tag    'application', media: 'all' %>
    <%= javascript_include_tag 'application' %>
```

in gemfile

```ruby
# gem 'turbolinks', '~> 5'
```

exercise.js

# listing workouts on page

spec/features/exercises/listing_exercises_spec.rb

```ruby
require "rails_helper"

RSpec.feature "Listing Exercises" do

	before do
		@john = User.create(email: "john@example.com", password: "password")
		login_as(@john)

		@e1 = @john.exercises.create(duration_in_min: 20,
			workout: "My body building activity,
			workout_date: Date.today")

		@e2 = @john.exercises.create(duration_in_min: 55,
			workout: "Weight lifting,
			workout_date: 2.days.ago")
	end

	scenario "shows user's workout for last" do
		visit "/"

		click_link "My Lounge"

		expect(page).to have_content(@e1.duration_in_min)
		expect(page).to have_content(@e1.workout)
		expect(page).to have_content(@e1.workout_date)

		expect(page).to have_content(@e2.duration_in_min)
		expect(page).to have_content(@e2.workout)
		expect(page).to have_content(@e2.workout_date)

		# expect(page).not_to have_content(@e3.duration_in_min)
		# expect(page).not_to have_content(@e3.workout)
		# expect(page).not_to have_content(@e3.workout_date)
	end
end
```

```ruby
	scenarion "shows no exercises if none created" do
		@john.exercises.delete_all

		visit "/"
		click_link "My Lounge"

		expect(page).to have_content("No Workouts Yet")
	end
```


# links in rails

```html
<%= truncate(exercise.workout, length: 100) %>
<%= exercise.workout_date %>
<%= link_to "Show", [current_user, exercise] %>
<%= link_to "Edit", [:edit, current_user, exercise] %>
<%= link_to "Destroy", [current_user, exercise], method: :delete, data: { confirm: "Are you sure?" } %>
```

# listing last 7 days' wrokouts on page

listing_exercises_spec.rb

```ruby
require "rails_helper"

RSpec.feature "Listing Exercises" do

	before do
		[...]

		@e3 = @john.exercises.create(duration_in_min: 35,
			workout: "On treadmill",
			workout_date: 8.days.ago)
	end

	scenario "shows user's workout for last" do
		[...]

		expect(page).not_to have_content(@e3.duration_in_min)
		expect(page).not_to have_content(@e3.workout)
		expect(page).not_to have_content(@e3.workout_date)
	end
end
```

# in model

```ruby
class Exercise < ApplicationRecord
  belongs_to :user

  alias_attribute :workout_details, :workout
  alias_attribute :activity_date, :workout_date
  # to make same name as in the form

  validates :duration_in_min, numericality: { greater_than: 0.0 }
  validates :workout_details, presence: true
  validates :activity_date, presence: true

  default_scope { where(
  	'workout_date > ?', 
  	7.days.ago)
  	.order(workout_date: :desc)
  }
end
```

in creating_exercises_spec.rb

```ruby
fill_in "Activity date", with: 3.days.ago
```

# d3

gem "d3-rails"

application.js

```js
//= require d3
```

exercises/index.html.erb

generate div tag programatically

```html
<%= content_tag :div, "", id: "chart", data: { workouts: @exercises } %>
```
you can get the data in javascript files!!!

```js
$(document).ready(function() {
  $("#exercise_workout_date").datepicker({ dateFormat: 'yy-mm-dd' });
  
  // if we are in the right url
  var regex = /\/users\/\d+\/exercises$/i;
  if($(location).attr('pathname').match(regex)) {
    drawChart();
  }
});
var drawChart = function() {
  var margin = { top: 100, right: 20, bottom: 100, left: 50 },
      width  = 600 - margin.left - margin.right,
      height = 450 - margin.top - margin.bottom;
  var JSONData = $("#chart").data("workouts");
 
  if (!JSONData) {
    return;
  }
 
  var data = JSONData.slice()
  
  var parseTime = d3.timeParse("%Y-%m-%d");
  
  var workoutFn = function(d) { return d.duration_in_min }
  var dateFn = function(d) { return parseTime(d.workout_date) }
  
  var x = d3.scaleTime()
    .range([0, width])
    .domain(d3.extent(data, dateFn))
  
  var y = d3.scaleLinear()
    .range([height, 0])
    .domain([0, d3.max(data, workoutFn)])
  
  var workout_line = d3.line()
      .x(function(d) { return x(d.workout_date); })
      .y(function(d) { return y(d.duration_in_min);  });
  
  data.forEach(function(d) {
    d.workout_date = parseTime(d.workout_date);
    d.duration_in_min = +d.duration_in_min;
  });
  
  var svg = d3.select("#chart").append("svg")
  .attr("width", width + margin.left + margin.right)
  .attr("height", height + margin.top + margin.bottom)
  .append("g")
    .attr("transform", "translate(" + margin.left + "," + margin.top + ")");
  
    svg.append("path")
        .attr("class", "line")
        .attr("d", workout_line(data));
  
  svg.append("g")
   .attr("class", "x axis")
   .attr("transform", "translate(0," + height + ")")
   .call(d3.axisBottom(x)
      .ticks(d3.timeDay.every(1))
      .tickFormat(d3.timeFormat('%Y-%m-%d'))
    )
   .selectAll("text")
      .style("text-anchor", "end")
      .attr("dx", "-.8em")
      .attr("dy", ".15em")
      .attr("transform", "rotate(-60)");
  
   // x axis label
   svg.append("text")
      .attr("x", width / 2)
      .attr("y", height + margin.top - 15)
      .style("text-anchor", "middle")
      .text("Date of workout")
  
   svg.append("g")
    .attr("class", "y axis")
    .call(d3.axisLeft(y).ticks(4));
  
  // y axis label
  svg.append("text")
     .attr("transform", "rotate(-90)")
     .attr("y", 0 - margin.left)
     .attr("x", 0 - (height / 2))
     .attr("dy", "1em")
     .style("text-anchor", "middle")
     .text("Workout duration (min)")
  
   // Chat title
   svg.append("text")
      .attr("x", (width / 2))
      .attr("y", 0 - (margin.top / 2))
      .style("text-anchor", "middle")
      .style("font-size", "18px")
      .style("text-decoration", "underline")
      .text("Workout duration vs Workout date")
};
```


# editing exercise feature spec


editing_exercise_spec.rb

```ruby
require "rails_helper"

RSpec.feature "Editing Exercise" do

	before do
		@owner = User.create!(email: "owner@example.com", password: "password")
		@owner_exercise = @owner.exercises.create!(
			duration_in_min: 48, 
			workout: "My body building activity", 
			workout_date: Date.today)

		login_as(@owner)
	end

	scenario "with valid data succeeds" do
		visit "/"

		click_link "My Lounge"

		path = "/useres/#{@owner.id}/exercises/#{@owner_exercise.id}/edit"
		link = "a[href=\'#{path}\']"
		find(link).click

		fill_in "Duration", with: 45
		click_button "Update Exercise"

		expect(page).to have_content("Exercise has been updated")
		expect(page).to have_content(45)
		expect(page).not_to have_content(48)
	end
end
```


# model

class Exercise < ApplicationRecord
  belongs_to :user

  alias_attribute :workout_details, :workout
  alias_attribute :activity_date, :workout_date
  # to make same name as in the form

  validates :duration_in_min, numericality: { greater_than: 0.0 }
  validates :workout_details, presence: true
  validates :activity_date, presence: true

  default_scope { where(
  	'workout_date > ?', 
  	7.days.ago)
  	.order(workout_date: :desc)
  }
end

# deleting an exercise spec

deleting_exercise_spec.rb

```ruby
require "rails_helper"

RSpec.feature "Deleting Exercise" do

	before do
		@owner = User.create!(email: "owner@example.com", password: "password")
		@owner_exercise = @owner.exercises.create!(duration_in_min: 48, workout: "My body building activity", workout_date: Date.today)
		login_as(@owner)
	end

	scenario do
		visit "/"

		click_link "My Lounge"

		path = "/users/#{@owner.id}/exercises/#{@owner_exercise.id}"
		link = "//a[contains(@href,\'#{path}\') and .//text()='Destroy']"

		find(:xpath, link).click

		expect(page).to have_content("Exercise has been deleted")
	end
end
```






































