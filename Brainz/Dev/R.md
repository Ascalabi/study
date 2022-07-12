# R
use  table() to get a count of everythig in tables (basically lahko countas true false in ti jih da v dva table)

## Data types

### Vectors
When you want to create vector with more than one element, you should use c() function which means to combine the elements into a vector.
```r
# Create a vector.
apple <- c('red','green',"yellow")
print(apple)

# Get the class of the vector.
print(class(apple))

# Creating a sequence from 5 to 13.
v <- 5:13

# Create vector with elements from 5 to 9 incrementing by 0.4.
print(seq(5, 9, by = 0.4))

# Accessing vector elements using position.
t <- c("Sun","Mon","Tue","Wed","Thurs","Fri","Sat")
u <- t[c(2,3,6)]

# Accessing vector elements using logical indexing.
v <- t[c(TRUE,FALSE,FALSE,FALSE,FALSE,TRUE,FALSE)]
print(v)
```
You have a lot of vector arithmetics so check them out. Same as relational operators ki ti returnajo vectors false true..


### Factors
Factors are used to categorize the data and store it as levels. Useful in the columns which have a limited number of unique values. Useful in data analysis for statistical modeling.

Factors are created using a vector. It stores the vector along with the **distinct values** of the elements in the vector as labels. Useful ins statistical modeling.
```r
# Create a vector.
apple_colors <- c('green','green','yellow','red','red','red','green')

# Create a factor object.
factor_apple <- factor(apple_colors)

# Print the factor.
print(factor_apple)
# nlevels functions gives the count of levels
print(nlevels(factor_apple))
```
When we execute the above code:
```
[1] green  green  yellow red    red    red    green 
Levels: green red yellow
[1] 3
```

## Graphical representation:

```r
#se uporablja:
barplot(tab, ylab="Stevilo filmov", main="Razmerje med stevilom filmov glede na dolzino")

#za mean vrednosti:
boxplot(dokumentarci, komedije, names=c("Dokumentarci", "Komedije"), ylab="Dolzina", main="Dolzina dokumentarcev in 
komedij")

#in pol za dobit average crto: 
abline(h=mean(dokumentarci), col="blue")

#za razmerje uporabljaj pie
pie(table(players$position), main="Razmerje stevila igralcev po igralnih polozajih")

#histogram groups the values into continous ranges:
hist(md$rating[md$Comedy == "1"], xlab="Ocena filma", ylab="Stevilo filmov", main="Histogram ocen za komedije")

#cool scatterplot:
plot(x,y, type = "l", xlab="leto", ylab="st dokumentarcev", main="stevilo animirank po letih")

```


dt <- rpart(insurance ~ ., train)

rpart.plot(dt)

poskusimo ta model na test data:

predicted ≤- predict(dt, test, type="class")

observed ← test$insurance #basically class iz test

q ← observed == predicted