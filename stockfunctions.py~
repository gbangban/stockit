from datetime import datetime, date, time, timedelta
import random
import urllib


def readsymlist():
	#open the file containing the stock symbols
	stock_sym = open('/home/gautam/Dropbox/programming_shit/stockprogrampython/tmp/stocksym', 'r')

	#create the list of stock symbols
	tmplist1 = [stock_sym.readline() for i in xrange(50)]
	tmplist2 = [i for i in tmplist1 if i != ""]
	symbol_list = [i[0:-1] for i in tmplist2]
	return symbol_list

def readstockdata(path, symlist, dt, debug=False):

	if debug:
		print "**Reading stock data. Date = "+str(dt)

	stockdata = {}

	#open the data file for date dt
	daily_stock_data_file = open(path+str(dt), 'r')
	#write each line of data to stockdata
	line = daily_stock_data_file.readline()
	j = 0
	while line != "":
		lineliststr = line[0:-2].split(",")
		linelistfl = [float(i) for i in lineliststr if i != "N/A"]
		stockdata[symlist[j]] = linelistfl
		line = daily_stock_data_file.readline()
		j+=1

	return stockdata


def writestockdata(symbol_list, debug=False):

	if debug:
		print "**Writing stock data. Date = "+ str(date.today())+" Number of companies = " + str(len(symbol_list))

	#create and open the file to write to (named by date)
	daily_stock_data_file = open('/home/gautam/Dropbox/programming_shit/stockprogrampython/tmp/stockdata'+str(date.today()), 'w')

	#prepare the url
	url = "http://finance.yahoo.com/d/quotes.csv?s="
	st = "".join([i + "+" for i in symbol_list])
	url+=st[0:-1]+"&f=l1j5k4r"

	#read from the url and write to file
	yahoo_stock_data = urllib.urlopen(url)
	daily_stock_data_file.write(yahoo_stock_data.read())


#Read company names
def read_companynames():
	#open the file containing the company names
	comp_names = open('/home/gautam/Dropbox/programming_shit/stockprogrampython/tmp/companynames', 'r')

	#create the list of company names
	tmplist1 = [comp_names.readline() for i in xrange(50)]
	return [i[1:-3] for i in tmplist1 if i != ""]


#Bad days
def badday(dt):
	return dt.weekday() == 5 or dt.weekday() == 6 or dt == date(2012, 12, 25) or dt == date(2013, 1, 1) or dt == date(2013, 1, 21) or dt == date(2013, 2, 18) or dt == date(2013, 3, 20) or dt == date(2013, 3, 25) or dt == date(2013, 3, 26) or dt == date(2013, 3, 27) or dt == date(2013, 4, 1)


#algorithm functions
def getprices(stockdata, sym):
	return {i:stockdata[i][sym][0] for i in stockdata.keys()}

def getpercentchanges(prices):
	latest = max(prices.keys())
	return {i:100*(prices[latest]-prices[i])/prices[i] for i in prices.keys() if i != latest}


def getdailypercentchanges(prices):
	return {i:100*(prices[i]-prices[max([k for k in prices.keys() if k < i])])/prices[max([k for k in prices.keys() if k < i])] for i in prices.keys() if i != min(prices.keys())}


