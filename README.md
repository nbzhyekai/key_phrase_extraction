# Glossary extraction documentation

this reposotary contains code for glossary extraction from books 
- 2013_Bodie.Kane.Marcus_Investments (McGraw-Hill, 10th Ed)
- CFA2019-L1V1
- Hull, John C. - Options, futures and other derivatives, 9th ed. (2013)
- Finance Analysis - Business Ratios And Formulas - A Comprehensive Guide - S M Bragg (John Wiley _ Sons) - 2007 [0470055170]
- Internet.Trading.Course.The.complete.course.in.online.investment.(Financial.Times.Series)(ISBN.0273656309)
- JAMES R HEDGES Hedges On Hedge Funds How To Successfully Analyze AND SELECT AN INVESTMENT
- Finance - The Options Course - High Profit And Low Stress Trading Methods
- Derivatives Demystified-A Step-by-Step Guide to Forwards, Futures, Swaps _ Options-sAyYiEd

The extraction code for each book and extraction result is put in separate folders. 

## Extraction method overview
1. Read in pdf files as html tags (same for all books)
2. Based on the html tags format, extract key phrases and corresponding explanations; separate acronyms from key phrases (different from one book to another)
3. Clean extracted text, e.g. remove special characters, remove extra spacing, 


## Extraction method description by books
### 2013_Bodie.Kane.Marcus_Investments (McGraw-Hill, 10th Ed)
sample text looks like
```` 
   arbitrage pricing theory    An asset pricing theory that is derived from a factor model, using diversification and arbitrage arguments. The theory describes the relationship between expected returns on securities, given that there are no opportunities to create wealth through risk-free arbitrage investments.
````
extration rule
- More spaces between key phrases and explanation


### CFA2019-L1V1
sample text looks like
```` 
Bond equivalent yield  A calculation of yield that is annualized using the ratio of 365 to the number of days to maturity.
````
extration rule
- More spaces between key phrases and explanation


### Hull, John C. - Options, futures and other derivatives, 9th ed. (2013)
sample text looks like
```` 
Compound Correlation Correlation implied from the market price of a CDO tranche.
```` 
extration rule
- All word in key phrases starts with a capital letter. The first word of explanation start with a captial letter. 
- Tokenize text. 
- Find the first word start with a lower case letter, which is the second word of explanation. Denote the position of the first lower case letter in tokenized text as `p`
- Position index for key phrases `[0, p-2]`
- Position index for explanations `[p-1, len(text)]`


### Finance Analysis - Business Ratios And Formulas - A Comprehensive Guide - S M Bragg (John Wiley _ Sons) - 2007 [0470055170]
sample text looks like
````
Accounts payable. Current liability on the balance sheet, representing short-term obligations to pay suppliers.
````
extration rule
- Split by full stop. key phrase is the first item, and explanation is the second item onwards.


### Internet.Trading.Course.The.complete.course.in.online.investment.(Financial.Times.Series)(ISBN.0273656309)
````
Abandoned option Where an option is neither sold nor exercised but allowed to lapse at expiry.

American Stock Exchange (AMEX) Located in New York, this is the third-largest US stock exchange. Shares trade in the same ‘auction’ manner used by the larger New York Stock Exchange, unlike the Nasdaq’s ‘market-making’ methods.
````
extration rule
- Check the first letter of the second word is captial case or lower case
- Tokenize text. 
- If capital, find the first word starting with lower case letter. Denote its position as `p`. The position of key phrases `[0, p-2]`. The position of explanation `[p-1, len(text)]`
- If lowercase, find the word starting with next captial letter. Denote its position as `p`. The position of key phrases `[0, p-1]`. The position of explanation `[p, len(text)]`


### JAMES R HEDGES Hedges On Hedge Funds How To Successfully Analyze AND SELECT AN INVESTMENT
sample text looks like
````
Risk premium The extra rate of return required to attract investors to an asset due to the incremental risk incurred from investing in it. 
````
extraction rule
- Find the postion of the second word start with captial letter


### Finance - The Options Course - High Profit And Low Stress Trading Methods
sample text looks like
````
ABC wave Elliott wave terminology for a three-wave countertrend price movement. Wave A is the first price wave against the trend of the market. Wave B is a corrective wave to Wave A. Wave C is the final price move to complete the countertrend price action.

All Ordinaries Index The major index of Australian stocks. This index represents 280 of the most active listed companies or the majority of the equity capitalization (excluding foreign companies) listed on the Australian Stock Exchange (ASX).

annual rate of return The simple rate of return earned by an investor for each year.
````
extraction rule
- Tokenize text
- If the first word start with lower case letter, find the next word starting with capital case letter. Denote its position as `p`. The position of key phrase `[0, p-1]`
- If the first word start with captical letter, find the next word starting with captial letter (Denote its position as `p1`) and next word starting with lower case letter (Denote its position as `p2`). If `p1 < p2`, key phrase position `[0, p2-2]`. Otherwise, key phrase position `[0, p1-1]`


### Derivatives Demystified-A Step-by-Step Guide to Forwards, Futures, Swaps _ Options-sAyYiEd
sample text looks like
````
Call feature A feature that allows the issuer of a bond to redeem the bond before maturity.

Chicago Board Options Exchange (CBOE) The major options exchange founded in 1973.
````
extraction rule
- Tokenize text
- If the second word start with a capital letter, find the nearest word start with lower case letter, denote its position `p`. key phrase position `[0, p-2]`
- If the second word start with a lower case letter, find the next word start with capital letter, denote its position `p`. key phrase position `[0, p-1]`

## Dependencies
- tika
- bs4
