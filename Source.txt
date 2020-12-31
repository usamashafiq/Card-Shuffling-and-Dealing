#include <iostream>
#include <cstdlib>
#include <ctime>
#include <string>
#include <cassert>
#include <cstdlib>
#include <string>
#include <algorithm>
using namespace std;
class Card {
public:
	Card();
	Card(int card);
	string getSuit() const;
	string getFace() const;

private:
	int _card;  // range 0..51

	static const string CARD_FACES[];
	static const string CARD_SUITS[];
};
//================================================= static constants
const string Card::CARD_FACES[] = { "A", "2", "3", "4", "5", "6", "7"
, "8", "9", "10", "J", "Q", "K" };
const string Card::CARD_SUITS[] = { "S", "H", "C", "D" };



//================================================= Constructor
Card::Card() {
	_card = 0;  // TODO: Should initialize to Joker.
}


//================================================= Constructor
Card::Card(int card) {
	_card = card;
}


//================================================== getFace
//  Action    : Returns face value of card.
//  Returns   : a string representing card face: "A", "2", ...

string Card::getFace() const {
	return CARD_FACES[_card % 13];
}//end getFace


 //================================================== getSuit
 //  Action    : Returns suit of a card value.
 //  Returns   : a string "S" (Spades), "H", (Hearts),
 //                       "C" (Clubs), or "D" (Diamonds).

string Card::getSuit() const {
	return CARD_SUITS[_card / 13];
}//end getSuit
class Deck {
public:
	Deck();
	Card dealOneCard();
	void shuffle();

private:
	Card _cards[52];
	int  _nextCardIndex;
};
//=========================================== Constructor
Deck::Deck() {
	// Initialize the array to the ints 0-51
	for (int i = 0; i<52; i++) {
		_cards[i] = Card(i);
	}
	shuffle();
	_nextCardIndex = 0;  // index of next available card
}


//================================================== deal
//  Action    : Returns random Card.

Card Deck::dealOneCard() {
	assert(_nextCardIndex >= 0 && _nextCardIndex<52);
	return _cards[_nextCardIndex++];
}//end dealOneCard


 //================================================ shuffle
 //  Action    : Shuffles Deck.
 //  Returns   : none.
void Deck::shuffle() {
	// Shuffle by exchanging each element randomly
	for (int i = 0; i<52; i++) {
		int randnum = rand() % 52;
		swap(_cards[i], _cards[randnum]);
	}
	_nextCardIndex = 0;
}//end shuffle
int main() {
	int numOfCards; // Input number for how many cards to deal.
	srand(time(0)); // Initializes random "seed".
	Deck deck;

	while (cin >> numOfCards) {
		deck.shuffle();

		cout << "Your hand is: ";
		for (int cardNum = 0; cardNum<numOfCards; cardNum++) {
			Card c = deck.dealOneCard();
			string suit = c.getSuit();
			string face = c.getFace();
			cout << face << suit << " ";
		}
		cout << endl;
	}

	return 0;
}//end main
