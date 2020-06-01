# Eclair-GUI
Temporary GUI to replace ACINQ's depreciated Eclair GUI

HOW TO USE			
			
This file reads API data from 3 separate text documents containing the output from Eclair API calls:			
	channels		
	channelstats		
	AllNodes		
I'm assuming this file will not work with other implementations unless the API calls are standardised.			
If you know VBA, you could modify the code to correctly read alternate API data to suit your needs.			
			
Run the above mentioned API calls and save the outputs in 3 text documents.			
	You will need to re-run "channels" each time you want updated info		
	You only need to re-run "channelstats" if you want to see updated relay data		
	You rarely need to re-run "AllNodes" simply to update the alias for new nodes or nodes who have changed alias		
			
Enable macros in this workbook.			
			
Modify the direct path in the VBA code to point to the 3 text files.			
	In Excel, click on Developer/View Code		
	In Modules folder, click on READAPI		
	Modify filepaths on 3 lines containing: myFile = "C:\Users....		
	Alternatively, use the adjacent "commented out" lines instead, to open them via dialog box each time		
			
On Dashboard Sheet, click on READ API DATA button. Everything should update, with aliases populating last.			
Not sure how well it scales, but for 100 channels runtime is approx 4 seconds on a modern PC.			
			
How to read the output:			
			
	Dashboard		
		Columns B thru G: Payment channel data from API call "channels"	
		Column A: aliases, cross-referenced from node ID using API call "AllNodes"	
		Columns H thru J: Relay and fee data from API call "channelstats"	
		Columns K thru Q (hidden): Supporting data for Bandwidth chart	
		CHANNEL BALANCE chart: Each bar represents a payment channel, and the RELATIVE funds on both sides, to show how balanced the channel is.	
			The range will need to be manually adjusted by you if you have far more or far fewer channels. I couldn't get it to be a dynamic range.
		BANDWIDTH chart: The number of channels (Y-axis) that have enough capacity to route a payment of size X (X-axis), and in which direction.	
			A well-balanced node (not necessarily well balanced channels) would have roughly overlapping curves for incoming and outgoing transactions.
			Only channels in NORMAL state are included - OFFLINE channels can't relay transactions.
		Stats below Bandwidth chart: Your largest capacity in each direction, and current price of BTC in USD.	
			
	Channel Watchlist		
		A list of "problem" channels that are in any state other than NORMAL	
		SINCE: How long the channel has been in the current state.	
			This is only the earliest date as seen by the spreadsheet. It isn't reading network data to get this figure.
		LOCKED FUNDS: The amount of funds (in USD) on your side of this problem channel.	
			If a channel has been offline for a while but no funds are on your side, you may not wish to close it, for example. (Could be someone's mobile wallet.)
		NOTES: Freeform column for you to add your own notes. (i.e. "This is u/SuperNode from reddit, the guy who is troubleshooting his RasPi. Don't close this channel.")	
			
		*** If you have a mobile wallet connected to your own Lightning Node, your mobile wallet will often show up on this list if it's OFFLINE,	
		   and you'll likely see a large balance of LOCKED FUNDS. (i.e. you're likely to accidentally force close your own channel if you're not paying attention.)	
			
	Rebalance		
		A listing of all of your payment channels, ranked by the amount of imbalance (in true BTC value, not by percentage of channel capacity)	
		OUT / IN : RemoteBalance/LocalBalance   Will read from 0% to 100%, with 50% being a perfectly balanced channel (of any capacity).	
		SURPLUS : The amount of excess BTC on your local side beyond a perfect 50% balance. A nagetive value means you need more local funds to achieve balance.	
			
		*** This page is very helpful for rebalancing channels if you have many channels. Because they are ranked by absolute value of imbalance, you can quickly identify	
		   a channel with excess local funds and a complementary channel with nearly equal deficit (opposite sign). Assuming a route can be found for the total amount of the surplus,	
		   these 2 channels could be fully balanced in a single transaction. If not, a smaller payment could still be used and each channel would simply fall down in the rankings of imbalance.	
			
			
For help,			
GITHUB user: STAWKEYE			
