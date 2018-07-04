rails destroy controller lalala
rails destroy model yadayada
rails destroy scaffold hohoho

```ruby
Rails.application.routes.draw do
  get "restaurants/:restaurant_id/reviews",     to: "reviews#index"
  get "restaurants/:restaurant_id/reviews/new", to: "reviews#new"
  post "restaurants/:restaurant_id/reviews",    to: "reviews#create"
  get "reviews/:id",                            to: "reviews#show"
  get "reviews/:id/edit",                       to: "reviews#edit"
  patch "reviews/:id",                          to: "reviews#update"
  delete "reviews/:id",                         to: "reviews#destroy"
end
```

you don't need to nest routes all the time

in the index new and create yes, because you need the restaurant.

but in the others, you don't need such a long url!

shallow nesting

```ruby
Rails.application.routes.draw do
  resources :restaurants do
    resources :reviews, only: [ :index, :new, :create ]
  end
  resources :reviews, only: [ :show, :edit, :update, :destroy ]
end
```


Namespaced routing

validation

>>  @restaurant.errors.messages

show the errors

```html
<!-- app/views/restaurants/_form.html.erb -->
<%= simple_form_for(@restaurant) do |f| %>
  <% if @restaurant.errors.any? %>
    <div class="errors-container">
      <ul>
        <% @restaurant.errors.full_messages.each do |message| %>
          <li><%= message %></li>
        <% end %>
      </ul>
    </div>
  <% end %>

  <!-- [...] all the fields -->
<% end %>
```



If you have trouble running rake, you may need to run bin/rake. It means that your $PATH does not contain the ./bin folder, something you can fix in your dotfiles' zshrc (see our default conf)


https://github.com/lewagon/dotfiles/blob/master/zshrc#L16-L18


Flat.destroy_all

google map api key



Maps Static API v2

https://developers.google.com/maps/documentation/maps-static/intro

https://maps.googleapis.com/maps/api/staticmap?center=Brooklyn+Bridge,New+York,NY&zoom=13&size=600x300&maptype=roadmap
&markers=color:blue%7Clabel:S%7C40.702147,-74.015794&markers=color:green%7Clabel:G%7C40.711614,-74.012318
&markers=color:red%7Clabel:C%7C40.718217,-73.998284
&key=YOUR_API_KEY


<div class="map" style="background-image: url('<%= @flat.picture_url %>')"></div>

students are using digital ocean to host some projects.

its a box, you own your server.

make work simple form without resources :restaurants

simple form call action controller custom

```
simple form, url: _path, verb: "post"
```

https://stackoverflow.com/questions/7507633/how-to-define-action-with-simple-form-for

check numericality

validates


img tag, with src, how to access the image.png in assets

i used image_path



















