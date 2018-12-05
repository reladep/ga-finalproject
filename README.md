TITLE: California (Carbon) Love
SUBJECT: A Scenario Analysis Model for CA Carbon Allowances and Emissions
AUTHOR: ALEX CHOWN 

INTRODUCTION

California Carbon Allowances (CCAs) Overview:
	 CCAs are carbon credits issued under California’s Cap-and-Trade program. California’s Cap and Trade program was launched in 2012 to reduce CO2 emissions back to 1990 levels by 2020. The latest regulation (Senate Bill 32 in 2016) extends the reduction goal to 40% of 1990 levels by 2030. California’s program, overseen and enforced by the California Air and Resource Board (CARB), sets limits on CO2 emissions and sells allowances for the right to emit. The program has been extended to 2030 (AB 398 in 2017). CCAs essentially function as a carbon tax for carbon-intensive businesses, with the proceeds from the auctions supporting state wide investments in climate change mitigation such as smart transportation, energy efficiency and access, land rehabilitation, and affordable housing.  Since inception, the program has generated $8B in state revenues and currently has wide poltical support. 

Investment Opportunity Highlights:
	- 85% of market participants are compliance-related entities - industrials and utilities who are mandated to cover their CO2 emissions from their California operations through - however, the program also allows investors to participate and purchase CCAs.
	- Investors who hold CCAs, by purchasing them at auctions and in the secondary market, gain exposure to a CPI+5% gross IRR return, which corresponds to the price floor for CCAs maintained by the California Air Pollution Board (CARB).
	- In additiona to the natural price escalation, California’s aggressive greenhouse gas reduction policies will require eventual tightening of the supply of CCAs by CARB, thereby creating a shortage in the market and potentially doubling CCA prices by the middle of the next decade.
	- On the downside, a US recession could hit the Californian economy and reduce economic activity and power consumption. California’s emissions dropped by more than 5% from 2008 to 2009 (CARB, 2017, California GHG Emissions Inventory). Lower emissions would create CCA surpluses, however, the continuation of the cap-and-trade program and the escalating price floor limits the long-term downside from this scenario. Faster technological progress, particularly in energy efficiency and smart transportation, could also limit the use of CCAs given the lower fossil-fuel based energy demand (and therefore emissions). 

California (Carbon) Love Overview:
  1. Imports historical California emissions data from 1980 to 2015 via the US Energy Information Administration (EIA) API, stores this data into a CSV and returns it via a GET route in flask and Jsonify. Locally hosted on http://127.0.0.1:5000. 
		- http://api.eia.gov/series/?api_key=YOUR_API_KEY_HERE&series_id=EMISS.CO2-TOTV-TT-TO-CA.A
  2. Calculates the future price floor and expected cumulative return via a GET route in flask and Jsonify, with user defined URL inputs of end year and expected inflation rate. Locally hosted on http://127.0.0.1:5000.
  3. Creates a scenario analysis calculator via a POST route in flask and Jsonify, where the 'Postman' values of end year, inflation rate, emissions decline rate, and a binary recession value ('yes/no') returns: future allowances total, future emissions total, future price, projected cumulative return, and the allowances-emissions supply-demand gap.  Locally hosted on http://127.0.0.1:5000. Stores this data into a CSV and uses a dictionary '.update' overide to replace these values.  The inputs of this CSV file are currently used as base inputs for other CSV-based models. 

INSTALLATION

Runs on Python 3 with 'pip install' requests, Flask, pytime.

EXAMPLE REQUESTS

	1. Historic Emissions Data flask GET @app.route returns:

	http://127.0.0.1:5000/emissions

	[
		[
			"2015",
			363.548701
		],
		[
			"2014",
			356.676568
		],
		[
		"2013",
		359.763576
		],
	etc.]

	2. Price Floor and Return Calculator flask GET @app.route requires inputs:

	http://127.0.0.1:5000/future_price/2022/12/31/3

	and returns:
		{
			current price: 15.66,
			future price floor: 21.43,
			projected cumulative return: 36.85
		}

	3. Scenario Analysis Calculator flask POST @app.route posts:

	{
		"end_year":"2027/6/01",
		"inflation":2.2,
		"emissions_decline":-5,
		"recession":"yes"
	}

	and returns:

	http://127.0.0.1:5000/scenario_analysis

	{
    	"scenario_analysis": 
    	{
	        "allowances_total": 102138.48,
	        "current_price": 15.66,
	        "emissions_decline": -5,
	        "emissions_total": 211615.41,
	        "end_year": "2027/6/01",
	        "future_price": 22.26,
	        "inflation": 2.2,
	        "projected_cumulative_return": 42.14,
	        "recession": "yes",
	        "start_date": "Tue, 04 Dec 2018 00:00:00 GMT",
	        "supply_demand_gap": -109476.93
   		}
	}

END
