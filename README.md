# SM_Data_Analysis(https://github.com/SADANANDA1989/SM_Data_Analysis.git)

##title: "Amazon_book"
author: ---
"SMeher"
date: "2025-07-11"
output:
  pdf_document: default
  html_document: default
---
-- *AMAZON BOOK CASE STUDY*
#Pacage installation
```{r}
library(tidyverse)

```

--upload the .csv file
```{r upload file}
read_csv("Amazon_bestseller_book.csv")

```

--assign csv file to a vector named as amazon_book
```{r create a vector}
amazon_book <- read.csv("Amazon_bestseller_book.csv")

```

# View the first few rows
```{r view rows}
head(amazon_book)

```
# Get summary statistics
```{r summary}
summary(amazon_book)
```
# Check the structure of the data
```{r structure}
str(amazon_book)
```
# Count books by genre
```{r Count books by genre}
table(amazon_book$Genre)

```
# Count books by rating
```{r Count books by rating}
table(amazon_book$User.Rating)
```
# Average rating by genre

```{r  Average rating by genre}
aggregate(`User.Rating` ~ Genre, data = amazon_book, mean)

```
# Most frequent authors
```{r Most frequent authors}
sort(table(amazon_book$Author), decreasing = TRUE) [1:10]
```
# Top 5 frequent book name
```{r Top 5 frequent book}
sort(table(amazon_book$Name), decreasing = TRUE) [1:5]
```
# Plot price distribution
```{r Plot price distribution}
hist(amazon_book$Price, main = "Price Distribution of Bestsellers")
```
# Plot ratings over time
```{r Plot ratings over time}
plot(`User.Rating` ~ Year, data = amazon_book)
```
# Rename column to remove space
```{r}
  colnames(amazon_book)[3] <- "User_Rating"
```
```{r}
view(amazon_book)
```
--Method 1: Using dplyr
```{r}
library(dplyr)

top_10_expensive <- amazon_book %>%
  arrange(desc(Price)) %>%  
  head(10) %>%             
  select(Name, Author, Price, `User_Rating`, Reviews, Year, Genre)  

# View the result
print(top_10_expensive)
```
--Method-2: Base R Approach
# Top 10 higehest price book
# Sort the data by Price in descending order and extract top 10
```{r Top 10 higehest price book}
top_10_expensive <- amazon_book[order(-amazon_book$Price), ][1:10, ]
```
# Select relevant columns
```{r}
top_10_expensive <- top_10_expensive[, c("Price","Name", "Author",  "User_Rating", "Reviews", "Year", "Genre")]
```
# View the result
```{r}
print(top_10_expensive)
```
--Visualization (Bar Plot of Top 10 Most Expensive Books)
```{r}
library(ggplot2)

ggplot(top_10_expensive, aes(x = reorder(Name, Price), y = Price, fill = Genre)) +
  geom_bar(stat = "identity") +
  coord_flip() +  # Horizontal bars for better readability
  labs(
    title = "Top 10 Most Expensive Amazon Bestseller Books",
    x = "Book Title",
    y = "Price (USD)"
  ) +
  theme_minimal()
```
--Top 10 highest-rated books
#Method 1: Using dplyr
```{r}
library(dplyr)

top_10_rated <- amazon_book %>%
  arrange(desc(`User_Rating`)) %>%  # Sort by User Rating (descending)
  head(10) %>%                     # Select top 10
  select(Name, Author, `User_Rating`, Reviews, Price, Year, Genre)  # Keep key columns

# View the result
print(top_10_rated)
```
--Method 2: Base R Approach
# Sort by User Rating (descending) and take top 10
# Select relevant columns (optional)
# View the result
```{r}
top_10_rated <- amazon_book[order(-amazon_book$`User_Rating`), ][1:10, ]


top_10_rated <- top_10_rated[, c("Name", "Author", "User_Rating", "Reviews", "Price", "Year", "Genre")]


print(top_10_rated)
```
-- *Visualization (Top 10 Rated Books)*
```{r}
library(ggplot2)

ggplot(top_10_rated, aes(x = reorder(Name, `User_Rating`), y = `User_Rating`, fill = Genre)) +
  geom_bar(stat = "identity") +
  coord_flip() +  # Horizontal bars for readability
  labs(
    title = "Top 10 Highest-Rated Amazon Bestseller Books",
    x = "Book Title",
    y = "User Rating (out of 5)"
  ) +
  theme_minimal() +
  scale_y_continuous(limits = c(0, 5))  # Adjust if ratings exceed 5
```

# Key Findings from the Amazon Bestseller Books Case Study
## Genre Distribution

Non-Fiction (310 books) slightly outnumbers Fiction (240 books) in the dataset, suggesting a strong market for informative/self-help content.

## Pricing Insights

The most expensive book (*Diagnostic and Statistical Manual of Mental Disorders, 5th Edition: DSM-5*) costs $105, while the average price is $13.1.

Top 10 expensive books are dominated by Non-Fiction (7 out of 10), including academic/reference books.

## User Ratings

Average rating: 4.62/5, with Fiction books having a slightly higher average (4.65) than Non-Fiction (4.60).

Top-rated books (4.9/5) are primarily children’s books (e.g., Brown Bear, Brown Bear, What Do You See?) and Dog Man series.

## Trends Over Time

Higher-rated books tend to have lower prices (e.g., top-rated books average $6.2 vs. dataset average of $13.1).

Books with 5,000–20,000 reviews typically have ratings of 4.7+, indicating popularity correlates with quality perception.

## Author and Title Patterns

Most frequent authors: Jeff Kinney (12 books), Gary Chapman (11), and Rick Riordan (11).

Top recurring titles: Publication Manual of the American Psychological Association (10 editions) and StrengthsFinder 2.0 (9 editions).

# Recommendations

## For Publishers & Authors

Leverage Non-Fiction Demand: Invest in high-quality reference/self-help books, which dominate the high-price segment.

Children’s Books: Expand series like Dog Man, as they achieve both high ratings and moderate pricing.

## For Pricing Strategy

Competitive Pricing: Top-rated books are often priced below $10; avoid overpricing to maintain ratings.

Bundle Offers: For expensive titles (e.g., *DSM-5*), offer discounts or digital versions to increase accessibility.

## For Amazon Sellers

Promote High-Rated, Affordable Books: Use algorithms to highlight books with 4.7+ ratings and <$15 prices, as they likely drive customer satisfaction.

Genre-Based Campaigns: Target Fiction readers with highly rated novels and Non-Fiction readers with practical guides.

## Future Research

Analyze profit margins (price vs. production cost) to identify the most lucrative genres.

Study seasonal trends (e.g., academic books selling more in fall).

# *Conclusion*
The analysis reveals that high ratings don’t require high prices, as evidenced by top-performing children’s books. Non-Fiction dominates in volume and premium pricing, while Fiction excels in user satisfaction. Strategic pricing, genre focus, and series expansion are key to leveraging Amazon’s bestseller trends.

## Final Takeaway:
Balance quality (ratings), affordability, and genre to maximize sales and customer loyalty in the competitive bookselling market.





