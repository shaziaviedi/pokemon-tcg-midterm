# Pokémon TCG Midterm Project
## Exploring the Pokémon TCG Market: How rarity, card category, and set era relate to price

## Executive Summary

This project is an exploratory data analysis of Pokémon Trading Card Game cards using public data from the Pokémon TCG API, with optional expansion context from Bulbapedia. The goal of the project is to understand which card characteristics appear most associated with higher market prices. Pokémon cards are a useful subject for this type of analysis because they exist in two overlapping systems of value. They are game objects with features such as HP, type, supertype, and evolution stage, but they are also collectible objects whose prices are shaped by scarcity, demand, nostalgia, and set identity. Because of this dual role, the Pokémon TCG provides a strong example of how structured metadata and market data can be studied together through Python.

The main research question for this project is: **Which Pokémon TCG card characteristics are most associated with higher market prices?** More specifically, this project asks whether collectible features such as rarity, set, and release year appear more strongly related to price than gameplay-oriented features such as HP. It also asks whether broad card categories, such as Pokémon, Trainer, and Energy, show meaningful differences in market value.

The main data source for the project was the Pokémon TCG API. The cards endpoint was used to collect several pages of card data so that the final dataset would comfortably exceed the assignment requirement of at least 300 rows. The sets endpoint was also used to provide set-level information such as set name, series, release date, total cards, and printed total. In addition to the API, Bulbapedia’s list of English Pokémon TCG expansions was collected as an optional contextual source for expansion history. Although the main analysis can stand on the Pokémon TCG API alone, the Bulbapedia page provided useful historical context about the progression of expansion eras.

A major part of the project involved cleaning and restructuring the data before analysis. The API returns nested JSON objects rather than a flat spreadsheet, so the first step was to flatten the relevant fields into a pandas DataFrame. The final card-level dataset included both categorical and numeric variables such as card name, supertype, subtype, type, HP, rarity, artist, regulation mark, set name, set series, release date, set total, set printed total, and price fields from both TCGplayer and Cardmarket. This allowed the project to clearly meet the requirement of having at least 12 mixed features.

One important challenge was how to handle price. The API does not store one universal price field for all cards. Instead, prices are often grouped by finish type, such as normal, holofoil, reverse holofoil, or first edition holofoil. Because of this, the project used a helper function to choose one available TCGplayer market price per card in a consistent order. This was a simplification, but it made it possible to create a usable price variable for exploratory analysis. Numeric fields such as HP, set size, and market price were converted into numeric types using pandas, while release date was converted into datetime format so that a release year feature could be created. Missing values in key categorical fields were filled with placeholder labels such as `Unknown` to make grouping and plotting easier.

After cleaning, the project created an analysis subset that only kept cards with a positive market price and non-missing values for the main variables of interest. This meant the subset was smaller than the full scraped dataset, but it made the statistical summaries and visualizations much easier to interpret. In practical terms, this reflects the fact that not every card in the API has complete market information attached to it.

The first part of the analysis focused on the general price distribution. A histogram of market prices showed that the distribution is highly skewed. Most cards in the usable pricing subset are relatively inexpensive, while a smaller number of cards are much more expensive. This means the market is not evenly spread. Instead, it resembles many collectible markets, where a large number of items sit at low values while a small number of outliers carry much of the prestige and price concentration. Because of this skew, a log-transformed price plot was also created to better show structure in the lower and middle ranges of the distribution.

The next major part of the analysis examined price by rarity. Boxplots and grouped averages both suggested that rarity is one of the strongest variables related to price. Higher-status rarity categories tended to have noticeably higher average and median prices than more common rarity groups. At the same time, the spread within some rarity groups was still quite large, which suggests that rarity matters a lot but does not fully explain card value on its own. Even within the same rarity category, some cards are priced much higher than others, which points to the importance of additional factors like set identity, artwork, nostalgia, and collector demand.

Another part of the analysis looked at price by supertype. Cards were grouped into broad categories such as Pokémon, Trainer, and Energy. There were some visible differences between these categories, but the separation was not as strong as it was for rarity. This suggests that broad card category has some relationship to market value, but it is weaker than the relationship between rarity and price. In other words, knowing that a card is Pokémon or Trainer tells less about its likely value than knowing its rarity and set context.

The project also considered time by grouping cards according to release year. A line plot of average price by release year suggested that older cards often have higher average values, although the pattern is not completely smooth. Some older eras contain cards with especially strong collector demand, but there is also large variation from one set to another. This makes release year helpful for identifying broad historical patterns, but not enough by itself to explain price. Set identity still matters strongly, even within the same era.

A related analysis grouped cards by set and calculated average market price for sets with at least a minimum number of cards in the pricing subset. This showed that some sets stand out clearly above others in average value. This is important because it supports the idea that set identity plays a major role in the Pokémon TCG market. Sets can become valuable not only because of age, but also because of iconic chase cards, cultural memory, or overall reputation in the collecting community.

To test whether gameplay-oriented variables had the same explanatory power, the project compared HP and price. A scatterplot of HP versus price showed no strong direct relationship. High-HP cards were not automatically expensive, and low-HP cards could still have substantial value. This is one of the clearest findings in the project because it shows that collectible value is not mainly determined by battle strength or mechanical usefulness. Instead, the market appears to reward rarity, set prestige, and collectibility much more than raw gameplay attributes.

Taken together, the main conclusion of the project is that Pokémon TCG card prices appear to be shaped more strongly by collectibility variables than by gameplay variables. Rarity, set identity, and release context are much more associated with price than HP or broad card category. This supports the idea that the Pokémon TCG market functions first and foremost as a collectible market, even though the cards also belong to a playable game system. In that sense, this project shows how market value can drift away from functional use and instead cluster around scarcity, nostalgia, and symbolic importance.

There are several limitations that should be kept in mind when interpreting the results. First, not every card has complete market price data, so the analysis subset is smaller than the full card dataset. Second, the price field used in the project is a simplification because cards can have multiple finish-specific prices. Third, the Pokémon TCG market changes over time, and the API captures only a snapshot of the latest available market data rather than a full historical series. Fourth, some older cards have more missing metadata than newer cards, which may affect comparisons across eras. Finally, this is an exploratory project, so it identifies patterns and relationships but does not claim to establish causation.

Even with these limitations, the project demonstrates the usefulness of public APIs for data analysis in Python. It shows how `requests`, pandas, functions, loops, list and dictionary handling, data cleaning, grouping, and seaborn plotting can be combined into a reproducible workflow from raw API response to interpretable visual results. It also shows that a collectible card market can be studied through both statistical and cultural lenses, where metadata such as rarity and set identity reveal patterns of value that are not obvious from gameplay statistics alone.

A useful next step would be to narrow the project to a smaller part of the Pokémon TCG ecosystem, such as one era, one expansion family, or one card type, and explore price behavior at a more detailed level. Another possible extension would be to compare raw card prices with graded-card prices using an additional source. In its current form, however, the project already provides a clear answer to the research question: higher Pokémon TCG prices appear much more connected to rarity, set, and release context than to gameplay-related features like HP.

---

## Research Question

**Which Pokémon TCG card characteristics are most associated with higher market prices?**

This project focuses on the relationship between price and:

- rarity  
- supertype  
- subtype  
- set identity  
- release year  
- HP  

---

## Data Sources

### Main source
- Pokémon TCG API cards endpoint  
- Pokémon TCG API sets endpoint  

### Optional context source
- Bulbapedia list of English Pokémon TCG expansions  

These sources were chosen because they provide both structured metadata and pricing-related information, making them suitable for exploratory analysis in pandas.

---

## Methods

The workflow for the project was:

1. Pull multiple pages of card data from the Pokémon TCG API  
2. Pull set data from the sets endpoint  
3. Flatten nested JSON into a pandas DataFrame  
4. Clean numeric and categorical variables  
5. Parse release dates and create release year  
6. Extract one usable market price per card  
7. Create an analysis subset of cards with valid positive prices  
8. Produce summary statistics and visualizations with seaborn and matplotlib  

The final dataset includes more than 300 rows and more than 12 features, with a mix of numeric and categorical variables.

---

## Features Used

Examples of features included in the final dataset:

- `card_id`
- `name`
- `supertype`
- `subtype_primary`
- `type_primary`
- `hp`
- `evolves_from`
- `rarity`
- `artist`
- `regulation_mark`
- `set_name`
- `set_series`
- `set_release_date`
- `release_year`
- `set_total`
- `set_printed_total`
- `price_usd`
- `price_kind`
- `cardmarket_avg_sell_eur`
- `cardmarket_trend_eur`

---

## Key Findings

- Card prices are highly skewed, with many low-priced cards and a smaller number of expensive outliers.  
- Rarity appears much more associated with price than gameplay features like HP.  
- Some sets have noticeably higher average prices than others.  
- Release year matters, but not evenly, because set identity still shapes value.  
- Broad card category matters somewhat, but less than rarity and set context.  

---

## Limitations

- Not every card has complete price data.  
- Price had to be simplified across multiple finish types.  
- Market values change over time and reflect only the time of collection.  
- Some metadata is missing more often for older cards.  
- This is exploratory analysis, not a predictive or causal model.  

---

## Files

- `Shazia_Python DSD_Midterm.ipynb` — full code, figures, and analysis  
- `data/cards_raw.csv` — raw card pull  
- `data/cards_clean.csv` — cleaned card dataset  
- `data/sets.csv` — set metadata  
- `data/bulbapedia_expansions.csv` — optional scraped expansion context  
- `figures/` — exported plots  

---

## References

- Pokémon TCG API documentation: https://docs.pokemontcg.io/
- Pokémon TCG API cards endpoint: https://docs.pokemontcg.io/api-reference/cards/search-cards/
- Pokémon TCG API sets endpoint: https://docs.pokemontcg.io/api-reference/sets/search-sets/
- Bulbapedia list of English Pokémon TCG expansions: https://bulbapedia.bulbagarden.net/wiki/List_of_Pok%C3%A9mon_Trading_Card_Game_expansions
