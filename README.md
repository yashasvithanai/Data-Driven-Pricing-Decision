# Data Driven Pricing Decision

### Business Objective 
To make a data-driven pricing decision for MinuteMaid’s 64oz orange juice. 

## Data Dictionary 
- _store_ A unique store identifier
- _brand_ A brand identifier
- _week_ An identifier for the week
- _isFeature_ A binary variable that indicates whether this brand was featured
- _units_ The total units of this brand sold in this store/week
- _price_ The price, in dollars, for one unit of this brand
- _marginalCost_ A numeric variable that specifies the cost per unit in that store/week 

## Analytical Concepts Used and their Business Application
-	Utilizing 3 causal models to estimate demand models thus estimating profits for different potential prices and obtaining the optimal price against highest profit generating point.
-	Observing omitted variable bias caused due to not considering isFeature, returning an incorrect optimal price estimate which can severely affect a company’s pricing decision.
-	Observing how interaction variables change the optimal prices when a promotional activity is executed.

### Demand Model 1

The following causal regression model records price elasticity i.e. the effect of price on expected demand. We will use this to generate optimal price from the demand model when the product is not featured. 

`Lm1 <- lm(log(units)~ price + isFeature, data = minuteMaidData64)`

Now, we obtain the demand model based on the price elasticity for different potential prices. Here, coefficient of price = -1.013.

<img width="430" alt="r1" src="https://user-images.githubusercontent.com/119455759/211060937-b9168ac0-3568-4299-9a93-7e5a728c5f38.png">

The optimal price is obtained by calculating the point generating highest expected profit.

Optimal Price from this Model when a product is not featured: $2.65.

### Demand Model 2
The following causal regression model records price elasticity i.e. the effect of price on expected demand disregarding whether the product was featured. As we do not consider isFeature, the coefficient of price will be biased, returning an incorrect estimate.

`secondLM <- lm(log(units)~ price, data = minuteMaidData64)`

The coefficient of price obtained = -1.44

<img width="434" alt="r2" src="https://user-images.githubusercontent.com/119455759/211061049-68cf2b29-0893-48e3-8844-9fd75334b3d5.png">

Optimal Price according to the second model: $2.36

### Demand Model 3
The following regression model records the interaction between price and featuring the product.

` thirdLM <- lm(log(units)~ price*isFeature, data = minuteMaidData64)`

The interaction coefficient obtained is -1.04654

<img width="424" alt="r3" src="https://user-images.githubusercontent.com/119455759/211061111-98014646-bb95-4499-a2d2-61390308e1dc.png">

Optimal price when a product is featured: $2.2

## Observations
1.	**Comparing Model 1 and Model 2**

The coefficient of price in Model 2 will be biased as isFeature is omitted in the model. isFeature will have a negative correlation with price and a positive correlation with quantity demanded leading to a negative bias. So, the consumers in model 2 are shown to be more price sensitive. 

2.	Therefore, Model 2 underestimates the optimal price of product, showcasing how omitted variable bias can affect the pricing decision.

3.	**Comparing Model 2 and Model 3**

Model 3 has a lowered featured price as opposed to the standard price of 2. The interaction coefficient of -1.04654 shows that consumers tend to be more price sensitive when a product is featured, and we will thus charge a lower price when it is featured.

4.	We also anticipate **the effect on optimal price because of not considering competitors’ prices**

When a competitor's price decreases, the demand for our product reduces and our prices also reduce as a response. So, omitting competitor's prices will lead to positive bias. We will thus assume that the consumers are less price sensitive than they are, causing us to price the product higher than optimal amount.

### Key Takeaway 
Therefore, while Model 3 records price elasticity when a product is featured and not featured and helps us estimate optimal prices for both cases, the prices are still biased as competitors’ prices are not considered. Causal models require us to creatively think of variables which can affect the dependent variables, omitting which can cause biased results. We may never obtain biased results, however, considering as many as possible can help us reduce the bias and get close to the results.
