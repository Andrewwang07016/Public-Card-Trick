# Card-Trick
The program will create a random deck of cards, deal them out, pick them up, and determine the secret card.
#include <iostream>
#include <iomanip>
using namespace std;

// Function prototypes
void BuildDeck(int deck[], const int size);
void PrintDeck(int deck[], const int size);
void PrintCard(int card);
void Deal(int deck[], int play[][3]);
void PickUp(int deck[], int play[][3], int column);
void SecretCard(int deck[]);
string CapitalizedName(string placeholder);

int main()
{

	// Declares and initializes variables
	int column = 0, i = 0;
	int seeDeck = 0;
	int playAgain = 0;
	string name;

	// Declares a 52 element array of integers to be used as the deck of cards
	int deck[52] = { 0 };

	// Declares a 7 by 3 array to receive the cards dealt to play the trick
	int play[7][3] = { 0 };


	// Generate a random seed for the random number generator
	srand(time(0));

	// Openning message. Asks the player for his/her name
	cout << "Hello, I am a really tricky computer program and " << endl
		<< "I can even perform a card trick.  Here's how." << endl
		<< "To begin the card trick type in your name: ";
	cin >> name;

	// Capitalize the first letter of the person's name
	name = CapitalizedName(name);

	do
	{
		// Builds the deck
		BuildDeck(deck, 52);


		// Asks if the player wants to see the entire deck. If so, then it prints it out.
		cout << "Ok " + name + ", first things first.  Do you want to see what " << endl
			<< "the deck of cards looks like (1-yes/0-no)? ";
		cin >> seeDeck;

		if (seeDeck == 1)
		{
			cout << endl;
			PrintDeck(deck, 52);
		}

		cout << endl << name << " pick a card and remember it..." << endl;

		// Begins the card trick loop
		for (i = 0; i < 3; i++)
		{
			// Begins the trick by calling the function to deal out the first 21 cards
			Deal(deck, play);

			// Includes error checking for entering which column
			do
			{
				// Asks the player to pick a card and identify the column where the card is
				cout << endl << "Which column is your card in (0, 1, or 2)?: ";
				cin >> column;
			} while (column < 0 || column > 2);
			
			// Picks up the cards, by column, with the selected column second
			PickUp(deck, play, column);

		}

		// Displays the top ten cards, and then reveal the secret card
		SecretCard(deck);

		// Asks if the player wants to play again
		
		cout << name << ", would you like to play again (1-yes/0-no)? ";
		cin >> playAgain;
	} while (playAgain == 1);
		

	// Thanks the player for playing, end message
	cout << endl << endl << "Thank you for playing the card trick!" << endl;
	return 0;
}

void BuildDeck(int deck[], const int size)
{
	int used[52] = { 0 };
	int card = 0, i = 0;

	// Generates cards until the deck is full with 52 integers
	for (i = 0; i < 52; i++)
	{
		// Generates a random number between 0 and 51
		do
		{
			card = rand() % 52;
		
		// Checks the used array at the position of the card
		// If 0, adds the card and set the used location to 1.  If 1 then generates another number
		} while (used[card] == 1);
		
		used[card]++;
		deck[i] = card;
			
		
	}
	return;
}

void PrintDeck(int deck[], const int size)
{
	int i;

	// Prints out each card in the deck
	for (i = 0; i < 52; i++)
	{


		PrintCard(deck[i]);
		cout << endl;
	}

}

// Assumes a deck and players with 3 cards each (to deal)
void Deal(int deck[], int play[][3])
{
	// Initializes variables
	int row = 0, col = 0, card = 0;
 	// Nested for loop setting number of rows and columns and assigns the deck of cards to them
	for (row = 0; row < 7; row++)
	{
		play[row][col] = deck[card];
		for (col = 0; col < 3; col++)
		{
			play[row][col] = deck[card];
			card++;
		}




	}
	
	
	
	// Deals cards by passing addresses of cardvalues from the deck array to the play array
 	// Formats the output display
	cout << endl;
	cout << ("   Column 0               Column 1               Column 2") << endl;
	cout << "================================================================"
		<< endl;
	for (row = 0; row < 7; row++)
	{
		for (col = 0; col < 3; col++)
		{
			
			PrintCard(play[row][col]);
			cout << "      ";
		}
		cout << endl;
	}


	return;
}

void PrintCard(int card)

{
	// Initializes variables
	int rank = 0;
	int suit = 0;

	// Determines the rank of the card and print it e.g. King, Queen, Jack
	rank = card % 13;

	switch (rank)
	{
	case 0:
		cout << " King";
		break;
	case 1:
		cout << "  Ace";
		break;
	case 10:
		cout << "   10";
		break;
	case 11:
		cout << " Jack";
		break;
	case 12:
		cout << "Queen";
		break;
	default:
		cout << "    " << rank;

	}

	// Determines the suit of the card and print it out e.g. Clubs, Hearts, Diamonds, Spades
	suit = card / 13;

	switch (suit)
	{
	case 0:
		cout << " of Clubs   ";
		break;
	case 1:
		cout << " of Hearts  ";
		break;
	case 2:
		cout << " of Diamonds";
		break;
	case 3:
		cout << " of Spades  ";

	}

	return;
}

// Assumes a deck and players with 3 cards each to pickup
void PickUp(int deck[], int play[][3], int column)
{
	int card = 0, row = 0;

	for (row = 0; row < 3; row++)
	{
 		// Cycles 0, 1, 2, determining which card in the column to pickup
		int pickupColumn = (column + row + 2) % 3;

		for (int cardInColumn = 0; cardInColumn < 7; cardInColumn++)
		{
			deck[card++] = play[cardInColumn][pickupColumn];


		}



	}

	


	return;
}

// Searches for the secret card
void SecretCard(int deck[])
{
	int card = 0;

	cout << endl << "Finding secret card...";
	for (card = 0; card < 10; card++)
	{
	
		PrintCard(deck[card]);
		cout << endl;
	}

	cout << endl << "Your secret card is: ";
	PrintCard(deck[card]);
	cout << endl;
	return;
}

// Placeholder capitalizes the first letter in string
string CapitalizedName(string placeholder)
{
	if (!placeholder.empty())
	{
		placeholder[0] = toupper(placeholder[0]);


	}
	return placeholder;

}
