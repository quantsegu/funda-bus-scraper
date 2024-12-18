# funda-scraper
Scraper of the Dutch real estate website [Fundainbusinesss.nl]([http://www.funda.nl](https://fundainbusiness.nl/)/), written in Python Scrapy.

## Basic usage
There are two spiders: 

1. `funda_spider` scrapes all properties for sale in a certain city, such as [http://www.funda.nl/koop/amsterdam/](http://www.funda.nl/koop/amsterdam/),
2. `funda_spider_sold` scrapes data on properties which have recently been sold, such as those listed on [http://www.funda.nl/koop/verkocht/amsterdam/](http://www.funda.nl/koop/verkocht/amsterdam/).

After installing [Scrapy](www.scrapy.org), in the project directory simply run the command

`scrapy crawl funda_spider -a place=amsterdam -o amsterdam_for_sale.json`

to generate a JSON file `amsterdam_for_sale.json` with all houses for sale listed on [http://www.funda.nl/koop/amsterdam/](http://www.funda.nl/koop/amsterdam/) and its subpages. The keyword argument `place` can be used to scrape data from other cities; for example `place=rotterdam` will scrape data from [http://www.funda.nl/koop/rotterdam/](http://www.funda.nl/koop/rotterdam/).

For recently sold homes, run

`scrapy crawl funda_spider_sold -a place=amsterdam -o amsterdam_sold.json`

to generate an `amsterdam_sold.json` with data from [http://www.funda.nl/koop/verkocht/amsterdam/](http://www.funda.nl/koop/verkocht/amsterdam/). Alternatively, CSV output can be generated by typing `amsterdam_sold.csv` extension instead of `amsterdam_sold.json`.

## Analysis
The scraped data contains the following fields: `address`, `postal_code`, `year_built`, `area`, `rooms`, `bedrooms`, and `price`, the asking price. For sold homes, the JSON will include `posting_date` and `sale_date`. These properties can be further analyzed using Python Pandas, for example. A couple of applications are shown below.

## Mapping of real estate attributes
By applying geolocation to the addresses, attributes such as price per unit area can be mapped (Figure 1). Such attributes can be used for 'bargain hunting' by identifying outliers.

![Heat map of property price per unit area](/Results/Images/OpenHeatMap_map_only.png)
![Legend](/Results/Images/OpenHeatMap_legend_only.png)  
**Figure 1.** Price per unit area (EUR/m<sup>2</sup>) of houses for sale in Amsterdam on 18 July 2016, plotted using [OpenHeatMap](www.openheatmap.com). (Due to a quotum on the number of geolocation requests per individual address, geolocation was performed by grouping properties by the first 4 digits of their postal codes and using a downloaded [database of their coordinates](https://github.com/bobdenotter/4pp); this is why the 'blobs' are  unevenly distributed). 

An interesting observation from Figure 1 is the clear price difference between Amsterdam Centrum and Amsterdam Noord across the Ij river. (This will probably be reduced once the North-South metro line is completed).

## Determining trends
The data can also be visualized in time, and used as a gauge of market sentiment. Figure 2 illustrates the development of (most recent) asking prices and the time it takes for properties to sell.

![Development of the real estate market in Amsterdam](/Results/Images/Amsterdam_property_sales_with_trend_line_big.png)  
**Figure 2.** Asking prices before sale (above) and days the property was offered on Funda (below) for over 11,000 properties in the period 1 April 2015 - 18 July 2016. The blue dots represent individual properties, the red curves weekly averages, and the green curves (weighted) exponential fits of the weekly averages. 

As seen from Figure 2, over the period observed, house prices have increased by 15% per year on average. Despite that, the average time it takes for a property to sell has more than halved. (It remains to investigate whether these results are biased by how long Funda keeps pages of sold properties online). In short, the data seems to [confirm](http://www.dutchnews.nl/news/archives/2016/04/amsterdam-housing-market-is-overheating-prices-soar-20/) that the Amsterdam housing market is heating up!
