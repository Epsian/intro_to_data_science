---
pre: <b>9/14. </b>
title: "R/R studio"
weight: 4
summary: "Practise some R fundamentals."
format:
    hugo:
      toc: true
      output-file: "_index.en.md"
      reference-links: true
      code-link: true
      
---



-   [Overview][]
-   [Problem Sets][]
    -   [1. Data Types][]
    -   [2. Vectors][]
    -   [3. Objects][]
    -   [4. NAs][]
    -   [5. Dataframes][]
    -   [6. Conditionals][]

## Overview

Learning to code is like learning a language, meaning you can only get better by practicing. Today we're going to practice some R fundamentals. You are encouraged to work with your neighbors on these problems. If you ever get stuck, call over myself or the data assistant for help.

## Problem Sets

### 1. Data Types

There are five main data **types** in R, they are:

-   `logical` - `TRUE` or `FALSE`
-   `integer` - whole numbers like 1, 5, 100
-   `numeric` - numbers with decimal places like 5.25
-   `character` - anything with letters
-   `factors` - for categorical data, numbers with descriptive labels
-   `NA` - `NA` are the absence of data, or something missing.

Depending on the type of data, you can only perform certain actions on them. For example, in the console, type `1 + 4` and press enter. Works fine! Now try `1 + "pie"`. No good. In the later example, we tried to add a **numeric** and a **character**, which R can't understand.

For each of the following, try using the `class()` function to discover the data type. Try to guess what each one is before you run the function.

-   `5`
-   `6.4`
-   `TRUE`
-   `"FALSE"`

### 2. Vectors

Everything in R is a **vector**, which is an ordered arrangement of data of the *same type*. Even single bit of data, like if you enter `"a"` into the console, returns `[1] "a"`, which is showing a vector of length 1, with our data `"a"` in it. You can test it out yourself using the `length()` function.

In the console, type `length("a")` and press enter. The `length()` function will look at a vector, and tell you how many **elements** it has, or how many pieces of data there are inside it. As an example, try the following; in the console type `c("a", "b", "c")`, it returns a vector with three elements. Now try `length(c("a", "b", "c"))`, what did it return? Notice that is only returned a single number describing the vector it was given.

You can also run `class()` on a **vector**, which is an ordered arrangement of data. you can make vectors using the `c()`, or **combine**, function. Try to guess the **type** of the following vectors before using `class()`. Remember, a vector can only hold one type of data!

-   `c(1, 2, 3, 4)`
-   `c("five", "six", "seven")`
-   `c(8, "9", 10)`
-   `c(TRUE, FALSE)`

Vectors take some getting used to, but as you come to understand how they work, you will be able to take advantage of how powerful R really is. One important property of a vector is that you can apply actions to all elements of a vector at once. For example, once again run `1 + 4` in the console. You will get back a vector of length 1, with the result of `[1] 5`, meaning the element in position 1 of our vector is `5`. Now try `1 + c(4, 5, 6, 7)`. What do you think will happen?

This **vectorized** mode of thinking takes some time to get used to. Run the following after trying to anticipate the results:

-   `5 + c(10, 20, 30)`
-   `c(10, 50, 100) / 5`
-   `c(1, 5, 10) + c(2, 4, 8)`

Really take the time to think over these results, especially the third one.

<div class="question">

Explain the results of `5 + c(10, 20, 30)` and `c(1, 5, 10) + c(2, 4, 8)`. What is being done to each vector that would cause the results we are seeing?

</div>

<div class="answer">

In the first example (`5 + c(10, 20, 30)`) five is being added to each element of the vector.

In the second example (`c(1, 5, 10) + c(2, 4, 8)`) each element of the vector is being added with the element in the same position of the other vector.

</div>

Some functionalities of R only make sense on a vector, for example, taking the `mean()` or average. It would be pointless to take the average of one number!

Try this:

`mean(c(1, 5, 10))`

### 3. Objects

You don't have to type out your vector every time you want to use it, you can save it to an **object** using an **assignment**. Try typing `letter_vec <-  c("a", "b", "c")` and pressing enter. You should see `letter_vec` appear in your **environment** tab on the upper right. Type `letter_vec` into the console and press enter. We see the same data as if we had just entered `c("a", "b", "c")` again, because that is what we saved inside the `letter_vec` **object**.

<div class="question">

Create a vector called `number_vec` of 10 numbers. The numbers can be any you like.

</div>

<div class="answer">

number_vec \<- c(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)

</div>

We can now start to do things to the object we created. Try using `class()` and `length()` on our `letter_vec` and `number_vec` objects.

You can use vectors with each other as well. Try the following: `number_vec + number_vec`

You can ask for only part of a vector by using the square brackets `[ ]`. Say we wanted the letters in the first, third, and fifth positions of our `letter_vec` object. I could ask R for `letter_vec[c(1, 3)]`, and get back `c("a", "c")`.

<div class="question">

Create a vector called `example_vector` containing `c(1, 3, 5, 7, 9, 11)`, and then get the values in the fourth, fifth, and sixth position.

</div>

<div class="answer">

example_vector \<- c(1, 3, 5, 7, 9, 11)

example_vector\[c(4, 5, 6)\]

</div>

### 4. NAs

`NA`s are missing values. They cause a lot of problems, and are very common. A good portion of the work of data scientists is know how to work around missing values. Let's re-do some of our earlier examples to to get a handle on `NA`s.

First, what happens if we try and add to a `NA`?

`NA + 1`

It will return another `NA`. Why? Because we don't know what the value of 1 plus an unknown would be, so it is safest to just remain unknown. The same is true with `NA`s in a vector.

`1 + c(NA, 1, 2)`

We can get the results for known values, but the unknown will stay unknown. This is true if something uses all elements of a vector, like `mean()`. Try: `mean(c(NA, 5, 10))`. Everything turns into an `NA`, because without knowing all the numbers, we can't be certain what the mean would be.

`NA`s do count as an element in a vector though, as we can see if we run `length(c(1, 2, 3))` and `length(c(NA, 2, 3))`. Put simply, `NA`s exist, we just don't know what they are.

<div class="question">

Imagine you are collecting data by asking people questions on a survey. When would you want to write down `NA`? Is there a difference between `NA` and someone responding "I don't know"?

</div>

<div class="answer">

An `NA` is meant to represent *missing* data. For example, if you handed out a survey, and someone returned it to you without an answer. If someone wrote 'I don't know' that *is* an answer in itself, and thus not an `NA`.

</div>

### 5. Dataframes

Recall that **dataframes** are aggregations of vectors into rows and columns. They must be square, meaning that all the columns must be of equal length, and all rows must have values in every column. There can be `NA`s though, so watch out! For this section, we're going to be looking at the results of our class survey.

To get started, copy the following into the R console and press enter to run it. `read.csv` is a function that reads tabular (square) data into R and creates a dataframe so we can work with it. We are giving it the argument of the URL for our class data survey to load that data from. We are then asking it to put the results into an object called `survey`. We will be coming back to this data later on in the problem set.

You can learn more about `read.csv` by opening the help file using `?read.csv`.

``` r
survey = read.csv("https://raw.githubusercontent.com/Intro-to-Data-Science-Template/intro_to_data_science_reader/main/content/class_worksheets/4_r_rstudio/data/survey_data.csv")
```

#### Exploration

Let's start by looking at the survey. You can do this by going over to the **Environment** tab in the upper right pane in R Studio. Click on the **survey** object you see there. This will open a viewer for you to look at the data. Can you find yourself?

We can also use some functions to get a summary. Start by entering `str(survey)` into the console. `str()` stands for *Structure*, and gives us an overview of dataframe objects. We can see how many **obs** or observations there are (rows) as well as how many **variables** there are (columns). It then lists all the variables, what type they are, and a preview of the contents.

<div class="question">

How many variables are there of each type in the survey dataframe?

</div>

<div class="answer">

-   Logical: 6
-   Integer: 3
-   Numeric: 0
-   Character: 14

</div>

Another good tool for starting to understand a dataframe is the `head()` function. `head()` will display the first handful of rows for you in the console so you can see what they look like. There is also the companion function of `tail()`, but that is used less often. Try both on our `survey` data before moving on.

#### Subsetting

One of the most important skills you will develop in R is how to work with dataframes, and how to **subset** them, or select only the content from them that you want. The two basic ways to do this is with the dollar sign `$` and with square brackets `[ ]`. The `$` lets you ask for specific columns from a dataframe. For example, if I wanted the column of just the average hours of sleep from our class, I could call ask for the `hours_sleep` column from the `survey` dataframe by entering `survey$hours_sleep`. Try that now. You should get back a vector of that single column. The same format will work to call any column by name.

<div class="question">

Using the `$` and the `mean()`, find the average hours of sleep our class gets per night.

</div>

<div class="answer">

`mean(survey$hours_sleep)`

</div>

Another way to subset dataframes with with square brackets `[ ]`. Imaging a dataframe is like a map with a grid. You can find any spot on that map by finding the intersection of the grid coordinates. The same is true of a dataframe. For example, if I wanted the value in the third row and fourth column in our survey dataframe (it contains the value 27), I could ask for it by entering `survey[*row*, *column*]` or `survey[3, 4]`.

<div class="question">

Get the values of each of the following:

-   Row 5, column 5
-   Row 2, column 20
-   Row 13, column 11

</div>

<div class="answer">

-   survey\[5,5\] = `r survey[5,5]`
-   survey\[2,20\] = `r survey[2,20]`
-   survey\[13,11\] = `r survey[13,11]`

</div>

You can also use the square brackets to ask for full columns, like `$`. Simply put the name of the column (in quotes!) in place of the column number, for example `survey[ , "hours_sleep"]`. Whenever you want everything, you can just leave a blank space. You can even get whole rows this way, but that isn't used as often; like this `survey[1, ]`.

A good way to work out how to use square brackets is the phrase "such that." For example, if I wanted "`survey` data *such that* it included rows 1, 2, and 5, and column 'hours sleep,'" that would translate to `survey[c(1, 2, 5), "hours_sleep"]`.

<div class="best">

Always use the names of columns when using square brackets if possible. Columns may move what spot they are in, so calling them by number position can be dangerous. R will return whatever is in that position, regardless of what you want! But it will always find the column with the same name, no matter what spot it is in.

</div>

#### Adding to Dataframes

You can add to dataframes using the same tools to subset from them. Rather than describing where to take data from, you'll now be describing where the new data goes.

Say we wanted to add a new column to our `survey` dataframe. First, we would need some new data to add that is of equal length to the number of cases (rows) in the dataframe. We have 15 rows, so we need a vector of data of the same length, so one value will fit into each row of the dataframe. Let's make a new vector of letters of the proper length.

`letters` is a pre-built object in R, meaning it is always there in the background; you can see it by typing `letters` into the console. Let's use our new sub-setting skills to get enough letters to fit our dataframe. We can do this by creating a new object, `new_column_vector`, and assigning the appropriate number of letters, in our case 15; we can even let R pick the right number using the `nrow()` function! Try this: `new_column_vector <- letters[1:nrow(survey)]`. You may have noticed a shortcut I used here. If I want some numbers that are all next to each other, like 1-2-3 or 9-10-11, I can use a colon `:` to say "from this number to this number." So `1:10` is the same thing as `c(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)`; handy!

Now that we've got our vector, we can add it to our dataframe! All you have to do is tell R what you want the new column to be called, and what to put in it. It looks like this: `survey$COLUMN_NAME <- STUFF_TO_PUT_IN_COLUMN`.

<div class="question">

Put our `new_column_vector` in our `survey` dataframe, calling the new column `alphabet`.

</div>

<div class="answer">

survey\$alphabet \<- new_column_vector

</div>

Now if we inspect out dataframe by clicking on it in the environment pane, we should be able to scroll all the way to the right and see our new column!

### 6. Conditionals

Another way to subset is by making comparisons. Say we wanted to see the rows in our data *only* for people whose birthday is in May. If we go look at our `survey` data in the viewer by clicking on it in the **environment** pane again, we can see the variable `b_month` has months in it. Let's test the type of that column using `class(survey$b_month)`

Now that we know the type of the `b_month` column, we can use that to subset the data. Where before we were just taking whole sections of the dataframe, now we are going to be asking for specific parts that match certain **conditions**, thus this is called **conditional sub-setting**. We define these conditions using **comparison operators**.

#### Comparison

For now, we want to get the data only for those rows *such that* `b_month` is `May`. We can do that using the double equal sign **comparison operator** in R, `==`. **Comparison operators** all compare one thing against another, and tell you if that comparison is `TRUE` or `FALSE`. The **Equal operator**, `==`, will test if one thing, or vector of things, is equal to another. For example, try executing the following in the console:

-   `1 == 1`
-   `1 == 2`
-   `1 == c(1, 2, 1)`

Now, let's apply that same idea to our `b_month` column. Try executing `survey$b_month == "May"`. This will take our `b_month` column, and test the condition that the values in `b_month` are equal to "May." Note that capitalization matters! This get's us halfway to our goal of seeing the data only for rows where `b_month` is `May`, but it's not quite what we want. We now have a vector of `TRUE` and `FALSE` for `b_month`, now we want to use that to actually subset our data.

We are going to ask R for our `survey` data, *such that* we only see the rows there `b_month` is equal to `May`, including all columns. We can do that using `survey[survey$b_month == "May", ]`.

There are several other common **comparison operators**, one of the most important being `!=`, which stands for **not equal to**. Try running `survey[survey$b_month != "May", ]`, what does it return? Do you understand why?

The most common conditionals include:

-   `==` - Equal to
-   `!=` - Not equal to
-   `>` - Greater than
-   `>=` - Greater than or equal to
-   `<` - Less than
-   `<=` - Less than or equal to

  [Overview]: #overview
  [Problem Sets]: #problem-sets
  [1. Data Types]: #data-types
  [2. Vectors]: #vectors
  [3. Objects]: #objects
  [4. NAs]: #nas
  [5. Dataframes]: #dataframes
  [6. Conditionals]: #conditionals
