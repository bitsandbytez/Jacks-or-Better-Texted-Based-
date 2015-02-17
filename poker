from random import randint

# Jacks or Better

balance = 1000
payoff = (0,1,2,3,4,6,9,25,90,800)


# Functions
def createDeck(cards, suits):
	deck = []
	for card in suits:
		for x in range(0,13):
			deck.append(cards[x] + card)

	return deck

def dealCards(players_hand, deck):
	for i in range(0, (5 - len(players_hand))):
		players_hand.append(deck.pop(randint(1,len(deck) - 1)))

	return players_hand

def printCards(players_hand, suit_chars):
	for i, card in enumerate(players_hand):
		#print card[1]
		print "%i: %s%s" % (i+1,card[0],chr(suit_chars[card[1]]))
	print "\n"	

def chooseCards(players_hand, suit_chars):
	decision = 'y'
	try:
		while(decision[0].lower() != 'n'):
			card_removal = raw_input("What card do you want to get rid of? 0 to exit \n1 through 5 \n")
			if(int(card_removal) == 0):
				return players_hand
			players_hand.pop(int(card_removal) - 1)
			print "\n"
			printCards(players_hand,suit_chars)

		return players_hand
	except ValueError:
		print "Not an option. Try again."
		return chooseCards(players_hand)		

def setHandValues(players_hand, card_values_list):
	cardValue = []
	for i in range(0,5):
		cardValue.append(card_values_list[players_hand[i][0]])

	return cardValue			

def setHandSuits(players_hand):	
	cardSuit = []
	for i in range(0,5):
		cardSuit.append(players_hand[i][1])

	return cardSuit

def straight(players_hand_values):
	players_hand_values.sort()
	if(players_hand_values[0] == 2 and \
		players_hand_values[1] == 3 and \
		players_hand_values[2] == 4 and \
		players_hand_values[3] == 5 and \
		players_hand_values[4] == 15):
		return True
	elif(players_hand_values[0] == players_hand_values[1] -1  and \
		players_hand_values[1] == players_hand_values[2] -1  and \
		players_hand_values[2] == players_hand_values[3] -1  and \
		players_hand_values[3] == players_hand_values[4] -1):
		return True	

	return False	

def flush(players_hand_suits):
	if(players_hand_suits[0] == players_hand_suits[1] and \
		players_hand_suits[0] == players_hand_suits[2] and \
		players_hand_suits[0] == players_hand_suits[3] and \
		players_hand_suits[0] == players_hand_suits[4]):
		return True
	return False

def pairs(balance,bet,players_hand_values, cards):
	pairs = {}
	for x in range(0,5):
		if(pairs.has_key(players_hand_values[x])):
			pairs[players_hand_values[x]] = pairs[players_hand_values[x]] + 1
		else:
			pairs[players_hand_values[x]] = 1
	 
	list_of_pairs = []
	number_of_pairs = 0
	three_of_a_kind = False
	four_of_a_kind = False
	two_of_a_kind = False
	for x in pairs:
		if(pairs[x] == 4):
			four_of_a_kind = True
		elif(pairs[x] == 3):
			three_of_a_kind = True
		elif(pairs[x] == 2):
			number_of_pairs = number_of_pairs + 1
			two_of_a_kind = True
			list_of_pairs.append(x)

	if(four_of_a_kind):
		print 'Four of a kind'
		balance = payout(balance,bet,7,payoff)
		return balance
	elif(three_of_a_kind and two_of_a_kind):
		print 'Full House'
		balance = payout(balance,bet,6,payoff)
		return balance
	elif(three_of_a_kind):
		print 'Three of a kind'
		balance = payout(balance,bet,3,payoff)
		return balance
	elif(two_of_a_kind):
		if(list_of_pairs[0] >= 11): # jack or better pair
			pair_output = 'Pair of '
			for x in list_of_pairs:
				pair_output = pair_output + str(cards[x - 2]) + 's'

			print pair_output
			balance = payout(balance,bet,1,payoff)
			return balance
		elif(number_of_pairs == 2): # two pair
			pair_output = 'Pair of '
			for x in list_of_pairs:
				pair_output = pair_output + str(cards[x - 2]) + 's'

			print pair_output
			balance = payout(balance,bet,2,payoff)
			return balance	
			

	print 'You Lose!'
	balance = payout(balance,bet,0,payoff)
	return balance								

# Money Section

def resetMoney():
	return 1000

def wager(balance):
	try:
		bet = int(raw_input("How much do you want to bet?"))
		if(bet > balance):
			print "You don't have that much money. try again"
			return wager(balance)
		return bet	
	except:
		print "Yea buddy, not a bet."
		return wager(balance)

def payout(balance,bet, hand, payout):
	balance += bet * payout[hand]
	return balance			

# Main Code
keep_playing = True
while(keep_playing):

	if(balance <= 0):
		get_balance = raw_input("You're broke! Want more money?")
		if(get_balance[0].lower() == 'y'):
			balance = resetMoney()
		else:
			break	

	# Lists
	suits = ['C','H','S','D']
	suit_chars = {'C':005, 'D':004,'S':006,'H':003}
	cards = ['2','3','4','5','6','7','8','9','T','J','Q','K','A']
	card_values_list = {}
	deck = []
	players_hand = []
	players_hand_values = []
	players_hand_suits = []

	# betting
	print "Current balance is $%s" % balance
	bet = wager(balance)

	for card in suits:
		for x in range(0,13):
			card_values_list[cards[x]] = x + 2

	deck = createDeck(cards, suits)
	players_hand = dealCards(players_hand, deck)

	# dealing a hand and selecting new cards
	printCards(players_hand, suit_chars)
	players_hand = chooseCards(players_hand, suit_chars)
	players_hand = dealCards(players_hand, deck)
	printCards(players_hand, suit_chars)

	players_hand_values = setHandValues(players_hand, card_values_list)
	players_hand_suits = setHandSuits(players_hand)

	if(flush(players_hand_suits) and straight(players_hand_values) and players_hand_values[0] ==  10):
		print "Royal Flush"
		balance = payout(balance,bet,9,payoff)
	elif(flush(players_hand_suits) and straight(players_hand_values) and players_hand_values[0] !=  10):
		print "Straight Flush"
		balance = payout(balance,bet,8,payoff)
	elif(flush(players_hand_suits) and not straight(players_hand_values)):
		print "Flush"
		balance = payout(balance,bet,5,payoff)
	elif(not flush(players_hand_suits) and straight(players_hand_values)):
		print "Straight"
		balance = payout(balance,bet,4,payoff)
	else:
		balance = pairs(balance,bet,players_hand_values, cards)

	answer = raw_input("Keep playing? y or n\n")
	
	if(answer[0].lower() != 'y'):
		keep_playing = False
