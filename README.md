# python-api-challenge

This challenge had two main parts.  First was getting weather data from a list of roughly 600 cities.  The second section was filtering 
	for desirable weather circumstances for a vacation location.

Both sections have their own folder.  In each folder there is an api_keys.py file that needs to be edited by the user and enter in their API key
	for the appropriate API.  IE:
			# OpenWeatherMap API Key
			weather_api_key = "YOUR KEY HERE!"	<<<

			# Google API Key
			g_key = "YOUR KEY HERE!"	<<<

1) WeatherPy:

	Random lists of both latitude and longitude are generated within a range of -90 to 90 latitude, and -180 to 180 longitude.
	
	The latitude and longitude are used in the citipy api to generate a list of unique cities.  This city list is then used with the open weather api to
		get weather data for each of these cities.  Any city not found in api is noted and then ignored.

	A function is built to ask the user what unit of measure they would wish to view the temperature.  This also helps determine the metric for wind speed
		even though that is not explicitly asked.

	A for loop goes through each city in the list of cities to create the specific url.  Each city with the index and the total count of cities is printed to 
		see what city the code is working on.  All the desired data is appended to a dictionary.  If the API reads a 404 code, that city is appended to a 
		cities_notlisted list, and the for loop will continue to next element.  After each city the program pauses for 1 second to stay within the 
		constraint of 60 calls per minute.  This pause results in this loop taking roughly 10 minutes to complete (600/60 = 10).

	When the loop is complete.  A new DataFrame is created by the appended dictionary.  The dataframe is output to the cities.csv file within the output_data folder.
		Then the list of cities not listed is printed at the end.
	
	In order to avoid having to pull the data each time the notebook is opened; a read_csv is used on the cities.csv file that was created just before.

	Maximum temperature, Humidity, Cloudiness and Wind speed all individually made into a scatter plot versus latitude.  A function is created to receive the
		desired x and y axis and then print the scatter plot using matplotlib.

	The same scatter plots are done again, but broken up by North and South hemispheres, and with a linear regression line put in the plot.  Two other functions
		were created just as before.  One for the north and one for the south.

2) VacationPy:

	The Vacation portion also reads in the cities.csv file.

	A Heatmap is created by configuring gmaps with a google api key.  Then concatinating latitude and longitude to get the location and using humidity as the weight.
		zoom level of 1.75 was used to display a little bit more than 1 earth.  Default displayed the earth 4 times.

	Criteria was assigned for the users prefered weather to determine the best cities to vaction at.

	This updated dataframe of cities was looped through the google places API to find the first hotel found within a 5000 radius.  The first hotel found is then added to a new
		column in the new hotel dataframe.

	Using the original heatmap.  Markers are now added to the map at the location of these desired hotels.  This final image is saved as hotel_heatmap.jpg.