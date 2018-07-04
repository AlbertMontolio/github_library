```
yarn install --ignore-engines
```

install js packages

```
yarn add --ignore-engines select2
```

if you run rails s, downloading stuff from lewagon

<%= f.association :ingredient, input_html: { class: "beautiful-dropdown" }, include_hidden: false %>

https://select2.org/getting-started/basic-usage


https://michalsnik.github.io/aos/

https://github.com/michalsnik/aos

You can't delete an ingredient if it used by at least one cocktail.
rake not wokring students trying to create methods stuff
answer: dont put anyhting, no dependent:destroy

bydefault, rails doesnt allow you delete if the parrent has childs


# ticket
validation of does with unique ingredients

uniqueness scope

# ticket

create models

dose mdoel


rails g model Dose description:text cocktail:references ingredient:references

render @cocktails

# ticket

link to, makes a hrf,
inside a bootstrap button. when hovers, you get the text-decoration

in css, txt decoration: none, for a
does not work

becuase bootstrap wins, you can overwrie it with important


# ticket
picture to big, how to make it smaller

# ticket
form_for, routes, without resources :cocktails and stuff
then you have to url, method

and how to select, collection show name, pass the object, in 
select tag?
in simple form is easy, f.association and thats it

# ticket

stylesheet structure from lewagon.



# ticket
doses_controller.rb

render "cocktails/show"

# ticket
is the assets not bad for the SEO?

# ticket

<%#= f.association :ingredient, input_html: { class: "beautiful-dropdown" }, include_hidden: false %>

"dose"=><ActionController::Parameters {"ingredient_id"=>"1", "description"=>"aaaaaaaa"}

you put :ingredient, and it works


<%= f.input :ingredient, placeholder: "ingredient for the dose", required: false, collection: Ingredient.all, label: false %>

"dose"=><ActionController::Parameters {"ingredient"=>"1", "description"=>"aaaaaaaa"}

you should put manually ingredient_id in the fucking form

collections are not associated to a model

associations yes



<%= f.association :ingredient, input_html: { class: "beautiful-dropdown" }, include_hidden: false, collection: Ingredient.all[0..3] %>


by default the associations put Model.all

you can specifiy which collection you show

# yui not working in heroku! deactivate it! how to make it work?





