# Blackjack Game

## Imports and Global Variables

### Step 1: Import the random module to shuffle the deck. Then, declare variables to store suits, ranks, and values.

```python

import random

suits = ('Hearts', 'Diamonds', 'Spades', 'Clubs')
ranks = ('Two', 'Three', 'Four', 'Five', 'Six', 'Seven', 'Eight', 'Nine', 'Ten', 'Jack', 'Queen', 'King', 'Ace')
values = {'Two': 2, 'Three': 3, 'Four': 4, 'Five': 5, 'Six': 6, 'Seven': 7, 'Eight': 8, 'Nine': 9, 'Ten': 10, 'Jack': 10,
          'Queen': 10, 'King': 10, 'Ace': 11}

playing = True
```


## Class Definitions

### Step 2: Create a Card Class

A `Card` object has two attributes: suit and rank. Optionally, add a method to return a string representation of the card.

```python
class Card:
    def __init__(self, suit, rank):
        self.suit = suit
        self.rank = rank

    def __str__(self):
        return self.rank + ' of ' + self.suit

```

### Step 3: Create a Deck Class

A `Deck` class holds all 52 card objects and can shuffle and deal cards.

```python
class Deck:
    def __init__(self):
        self.deck = []  # Start with an empty list
        for suit in suits:
            for rank in ranks:
                self.deck.append(Card(suit, rank))  # Build Card objects and add them to the list

    def shuffle(self):
        random.shuffle(self.deck)

    def deal(self):
        single_card = self.deck.pop()
        return single_card

    def __str__(self):
        deck_comp = ''  # Start with an empty string
        for card in self.deck:
            deck_comp += '\n ' + card.__str__()  # Add each Card object's print string
        return 'The deck has:' + deck_comp

```

### Testing the Deck

Let's see what our Deck looks like!

```typescript
const test_deck = new Deck();
console.log(test_deck);
```

The deck has:

Two of Hearts  
Three of Hearts  
Four of Hearts  
Five of Hearts  
Six of Hearts  
Seven of Hearts  
Eight of Hearts  
Nine of Hearts  
Ten of Hearts  
Jack of Hearts  
Queen of Hearts  
King of Hearts  
Ace of Hearts  
Two of Diamonds  
Three of Diamonds  
Four of Diamonds  
Five of Diamonds  
Six of Diamonds  
Seven of Diamonds  
Eight of Diamonds  
Nine of Diamonds  
Ten of Diamonds  
Jack of Diamonds  
Queen of Diamonds  
King of Diamonds  
Ace of Diamonds  
Two of Spades  
Three of Spades  
Four of Spades  
Five of Spades  
Six of Spades  
Seven of Spades  
Eight of Spades  
Nine of Spades  
Ten of Spades  
Jack of Spades  
Queen of Spades  
King of Spades  
Ace of Spades  
Two of Clubs  
Three of Clubs  
Four of Clubs  
Five of Clubs  
Six of Clubs  
Seven of Clubs  
Eight of Clubs  
Nine of Clubs  
Ten of Clubs  
Jack of Clubs  
Queen of Clubs  
King of Clubs  
Ace of Clubs  
### Step 4: Create a Hand Class

In addition to holding `Card` objects dealt from the `Deck`, the `Hand` class may be used to calculate the value of those cards using the `values` dictionary defined above. It may also need to adjust for the value of Aces when appropriate.


```python
class Hand:
    def __init__(self):
        self.cards = []  # Start with an empty list as we did in the Deck class
        self.value = 0   # Start with zero value
        self.aces = 0    # Add an attribute to keep track of aces

    def add_card(self, card):
        self.cards.append(card)
        self.value += values[card.rank]
    
    def adjust_for_ace(self):
        pass
```
### Testing the Hand Class

Before we tackle the issue of changing Aces, let's make sure we can add two cards to a player's hand and obtain their value:

```python
test_deck = Deck()
test_deck.shuffle()
test_player = Hand()
test_player.add_card(test_deck.deal())
test_player.add_card(test_deck.deal())
test_player.value
```
### Output
`14`

Lets see what these two cards are:

```python
for card in test_player.cards:
    print(card)
```
### Output

Nine of Hearts Five of Hearts
### Step 5: Create a Chips Class

In addition to decks of cards and hands, we need to keep track of a Player's starting chips, bets, and ongoing winnings. This could be done using global variables, but in the spirit of object-oriented programming, let's make a Chips class instead!

```python
class Chips:
    def __init__(self):
        self.total = 100  # This can be set to a default value or supplied by user input
        self.bet = 0
        
    def win_bet(self):
        self.total += self.bet
    
    def lose_bet(self):
        self.total -= self.bet
```
### A Note About Our Default Total Value

Alternatively, we could have passed a default total value as a parameter in the `__init__`. This would have let us pass in an override value at the time the object was created rather than wait until later to change it. The code would have looked like this:

```python
def __init__(self, total=100):
    self.total = total
    self.bet = 0
```
## Function Definitions

A lot of steps are going to be repetitive. That's where functions come in! The following steps are guidelines - add or remove functions as needed in your own program.

### Step 6: Write a Function for Taking Bets

Since we're asking the user for an integer value, this would be a good place to use `try/except`. Remember to check that a Player's bet can be covered by their available chips.

```python
def take_bet(chips):
    while True:
        try:
            chips.bet = int(input('How many chips would you like to bet? '))
        except ValueError:
            print('Sorry, a bet must be an integer!')
        else:
            if chips.bet > chips.total:
                print("Sorry, your bet can't exceed", chips.total)
            else:
                break
```
### Step 7: Write a Function for Taking Hits

Either player can take hits until they bust. This function will be called during gameplay anytime a Player requests a hit, or a Dealer's hand is less than 17. It should take in Deck and Hand objects as arguments, and deal one card off the deck and add it to the Hand. You may want it to check for Aces in the event that a player's hand exceeds 21.

```python
def hit(deck, hand):
    hand.add_card(deck.deal())
    hand.adjust_for_ace()
```
### Step 8: Write a Function Prompting the Player to Hit or Stand

This function should accept the deck and the player's hand as arguments and assign `playing` as a global variable. If the Player Hits, employ the `hit()` function above. If the Player Stands, set the `playing` variable to `False` - this will control the behavior of a while loop later on in our code.

```python
def hit_or_stand(deck, hand):
    global playing  # to control an upcoming while loop

    while True:
        x = input("Would you like to Hit or Stand? Enter 'h' or 's' ")

        if x[0].lower() == 'h':
            hit(deck, hand)  # hit() function defined above

        elif x[0].lower() == 's':
            print("Player stands. Dealer is playing.")
            playing = False

        else:
            print("Sorry, please try again.")
            continue
        break
```
### Step 9: Write Functions to Display Cards

When the game starts, and after each time the Player takes a card, the dealer's first card is hidden while all of the Player's cards are visible. At the end of the hand, all cards are shown along with each hand's total value. Below are the functions to handle these scenarios:

### Show Some Cards

This function displays the dealer's hand with one card hidden and shows the player's hand.

```python
def show_some(player, dealer):
    print("\nDealer's Hand:")
    print(" <card hidden>")
    print('', dealer.cards[1])
    print("\nPlayer's Hand:", *player.cards, sep='\n ')
```
### Step 10: Write Functions to Handle End of Game Scenarios

In this step, we will implement functions to handle the various outcomes of the game. Each function will take the player's hand, dealer's hand, and chips as needed.

### Functions to Handle Game Outcomes

```python
def player_busts(player, dealer, chips):
    print("Player busts!")
    chips.lose_bet()

def player_wins(player, dealer, chips):
    print("Player wins!")
    chips.win_bet()

def dealer_busts(player, dealer, chips):
    print("Dealer busts!")
    chips.win_bet()

def dealer_wins(player, dealer, chips):
    print("Dealer wins!")
    chips.lose_bet()

def push(player, dealer):
    print("Dealer and Player tie! It's a push.")
```
## Game Loop

After defining the functions, the following code sets up the main game loop:

```python
while True:
    # Print an opening statement
    print('Welcome to BlackJack! Get as close to 21 as you can without going over!\n\
    Dealer hits until she reaches 17. Aces count as 1 or 11.')

    # Create & shuffle the deck, deal two cards to each player
    deck = Deck()
    deck.shuffle()

    player_hand = Hand()
    player_hand.add_card(deck.deal())
    player_hand.add_card(deck.deal())
    
    dealer_hand = Hand()
    dealer_hand.add_card(deck.deal())
    dealer_hand.add_card(deck.deal())

    # Set up the Player's chips
    player_chips = Chips()  # remember the default value is 100    

    # Prompt the Player for their bet
    take_bet(player_chips)

    # Show cards (but keep one dealer card hidden)
    show_some(player_hand, dealer_hand)

    while playing:  # recall this variable from our hit_or_stand function
        
        # Prompt for Player to Hit or Stand
        hit_or_stand(deck, player_hand) 
        
        # Show cards (but keep one dealer card hidden)
        show_some(player_hand, dealer_hand)  
        
        # If player's hand exceeds 21, run player_busts() and break out of loop
        if player_hand.value > 21:
            player_busts(player_hand, dealer_hand, player_chips)
            break        

    # If Player hasn't busted, play Dealer's hand until Dealer reaches 17 
    if player_hand.value <= 21:
        
        while dealer_hand.value < 17:
            hit(deck, dealer_hand)    
    
        # Show all cards
        show_all(player_hand, dealer_hand)
        
        # Run different winning scenarios
        if dealer_hand.value > 21:
            dealer_busts(player_hand, dealer_hand, player_chips)

        elif dealer_hand.value > player_hand.value:
            dealer_wins(player_hand, dealer_hand, player_chips)

        elif dealer_hand.value < player_hand.value:
            player_wins(player_hand, dealer_hand, player_chips)

        else:
            push(player_hand, dealer_hand)        

    # Inform Player of their chips total 
    print("\nPlayer's winnings stand at", player_chips.total)
    
    # Ask to play again
    new_game = input("Would you like to play another hand? Enter 'y' or 'n' ")
    
    if new_game[0].lower() == 'y':
        playing = True
        continue
    else:
        print("Thanks for playing!")
        break
```
### Example Game Output

Welcome to BlackJack! Get as close to 21 as you can without going over!
    Dealer hits until she reaches 17. Aces count as 1 or 11.
How many chips would you like to bet? 5

Dealer's Hand:
 <card hidden>
 Seven of Spades

Player's Hand:
 Queen of Spades
 Nine of Hearts

Dealer's Hand:
 Ace of Clubs
 Seven of Spades
Dealer's Hand = 18

Player's Hand:
 Queen of Spades
 Nine of Hearts
Player's Hand = 19
Player wins!

Player's winnings stand at 105
Would you like to play another hand? Enter 'y' or 'n' y
Welcome to BlackJack! Get as close to 21 as you can without going over!
    Dealer hits until she reaches 17. Aces count as 1 or 11.
How many chips would you like to bet? 5

Dealer's Hand:
 <card hidden>
 Nine of Clubs

Player's Hand:
 Queen of Diamonds
 Ace of Hearts
Would you like to Hit or Stand? Enter 'h' or 's' s
Player stands. Dealer is playing.

Dealer's Hand:
 <card hidden>
 Nine of Clubs

Player's Hand:
 Queen of Diamonds
 Ace of Hearts

Dealer's Hand:
 Eight of Clubs
 Nine of Clubs
Dealer's Hand = 17

Player's Hand:
 Queen of Diamonds
 Ace of Hearts
Player's Hand = 21
Player wins!

Player's winnings stand at 105
Would you like to play another hand? Enter 'y' or 'n' y
Welcome to BlackJack! Get as close to 21 as you can without going over!
    Dealer hits until she reaches 17. Aces count as 1 or 11.
How many chips would you like to bet? 65

Dealer's Hand:
 <card hidden>
 Four of Spades

Player's Hand:
 Queen of Clubs
 Four of Diamonds
Would you like to Hit or Stand? Enter 'h' or 's' h

Dealer's Hand:
 <card hidden>
 Four of Spades

Player's Hand:
 Queen of Clubs
 Four of Diamonds
 Eight of Diamonds
Player busts!

Player's winnings stand at 35
Would you like to play another hand? Enter 'y' or 'n' n
Thanks for playing!




















