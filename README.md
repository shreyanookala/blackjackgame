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

Just to see that everything works so far, let's see what our Deck looks like!

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
```python
14
```
Lets see what these two cards are:

```python
for card in test_player.cards:
    print(card)
```
### Output

Nine of Hearts Five of Hearts










