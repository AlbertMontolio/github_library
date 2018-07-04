Blackjack

Spec, write out the requirements

- Player and the Dealer (Computer)

- Deck, able to shuffle the deck

- Deal a hand, take a card from the deck deal it to the dealer, then the player etc.

- Card can be face down or up

- Ace can have value of 1 or 11

- Stand, Hit (meaning take card)

- Dealer has no option but to hit if less than 17

- 21, player cannot hit anymore and their turn ends

Classess - Card, Deck, Hand

Card - Suit - Speades, Hearts, Clubs or Diamonds
- Rank - 1 and 11
- Show

SUITS = ["Spades", "Hearts", "Clubs", "Diamonds"]
RANKS = ['2','3','4','5','6','7','8','9','10','Jack','Queen','King','Ace']

card_spec.rb
```ruby
require_relative "card"

RSpec.describe Card do

	before do
		suit = "Diamonds"
		rank = "8"

		@card = Card.new(suit, rank)
	end

	it "responds to suit" do
		expect(@card).to respond_to(:suit)
	end

	it "responds to rank" do
		expect(@card).to respond_to(:rank)
	end

	it "responds to show" do
		expect(@card).to respond_to(:show)
	end

	# we want to be able to see it
	it "'show' method returns 'true'" do
		expect(@card.show).to eq(true)
	end

	it "'suit' method returns 'Diamonds'" do
		expect(@card.suit).to eq('Diamonds')
	end

	it "'rank' method returns '8'" do
		expect(@card.rank).to eq('8')
	end
end
````

card.rb

```ruby
class Card
	attr_accessor :suit, :rank, :show
	SUITS = ["Spades", "Hearts", "Clubs", "Diamonds"]
	RANKS = ['2','3','4','5','6','7','8','9','10','Jack','Queen','King','Ace']

	def initialize(suit, rank)
		@show = true

		if SUITS.include?(suit) and RANKS.include?(rank)
			@suit = suit
			@rank = rank
		else
			@suit = "UNKNOWN"
			@rank = "UNKNOWN"
		end
	end
end
```


Card = Suit, Rank, Display of card and show/hide

# Deck - 52 cards
- shuffle
- suits, ranks
- deck
- deal_card
- replace_with

its gonna respond to this things

shuffled deck

pop method

deck of cards

Suits, Ranks

Loop over the Suits - for each suit, fill in the ranks
4 * 13 = 52


# Hand

Dealer
Gets a hand
Player
Gets a hand

Hand class
- gives us the cards dealt
- dealt cards array - method to add cards to this array
- total value of player's or dealer's dealt cards
- showing the details of each card dealt and the total value




























