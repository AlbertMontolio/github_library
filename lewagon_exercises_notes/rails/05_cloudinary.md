class Cocktail < ApplicationRecord
  has_many :doses, dependent: :destroy
  has_many :ingredients, through: :doses


don't destroy ingredients of course
so in the seeds,

dose.destroy_all
cocktail.destory_all
ingredient.destroy_all

f.associations :ingredient, collection: Ingredient.all

resources :doses, only: [:destroy]

you dont put the method destroy in the nested

# ticket

sort an array of objects, per attributes


