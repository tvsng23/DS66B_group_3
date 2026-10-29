# Data

`e-shop_clothing_2008.csv` is the **e-shop clothing 2008** clickstream dataset from the UCI Machine Learning
Repository. It contains 165,474 rows and 14 columns, separated by semicolons (`;`), and records the clicks on
product pages of an online store (clothing for pregnant women) between 1 April and 13 August 2008.
Each row is one click on one product page; there is no information about purchases.

The file in this folder is the original, unmodified download. All cleaning (removing the truncated last day,
correcting the category of product A18) is done inside the notebook `notebooks/DS66B_group_3.ipynb`.

## Variables

The notebook renames the 14 columns to the short names below.

| Name in notebook | Description | Values |
|---|---|---|
| `year`, `month`, `day` | Date of the click | 2008; months 4–8 |
| `order` | Sequence number of the click within the session | 1–195 |
| `country` | Country of origin of the IP address | codes 1–47 (29 = Poland) |
| `session` | Session identifier | 24,026 sessions |
| `category` | Main product category | 1 trousers, 2 skirts, 3 blouses, 4 sale |
| `product` | Product code | 217 codes (e.g. A13) |
| `colour` | Colour of the product | codes 1–14 |
| `location` | Position of the photo on the page | 1–6 (top left ... bottom right) |
| `photo` | Model photography | 1 en face, 2 profile |
| `price` | Price in US dollars | 18–82 |
| `price2` | Whether the price is higher than the average price of the category | 1 yes, 2 no |
| `page` | Page number of the product within the website | 1–5 |

There are no missing values and no duplicated rows. Most variables are codes rather than quantities
(for example, colour 12 is not "larger" than colour 3), so they are one-hot encoded in the notebook.

## References

- M. Lapczynski and S. Bialowas, "Discovering patterns of users' behaviour in an e-shop – comparison of consumer
  buying behaviours in Poland and other European countries", *Studia Ekonomiczne*, no. 151, pp. 144–153, 2013.
- D. Dua and C. Graff, UCI Machine Learning Repository, University of California, Irvine, 2019.
  Check the dataset page there for the licence and citation terms.
