# Pandas_portfolio

This repository documents my understanding of Pandas and contains my solutions (the jupyter notebook file named as pandas.ipynb attached to this repo) to 200 detailed exercises (50 main exercises along with 150 beyond exercises) from the book written by Reuven Lerner (Pandas Workouts -- https://www.manning.com/books/pandas-workout)

Pandas is all about analyzing data. And a major part of the analysis that we do in Pandas can be phrased as, **"Where this is the case, show me that."**

Below is the summary of my notes from the book:

## 1. Pandas Series

**Points:** 

* If I don't specify index when creating a pandas series, then the series index will be the default, integers starting from 0


* Given the flexibility and power pandas gives me, I can assign a list, NumPy array, or pandas series as an index. However, the data structure you pass must be of the same length as the series. If it isn’t, you’ll get a ValueError exception. 


* I can set, or even change, the index even after the series has already been created using s.index = ...

  
* **sort_values() method**:
    
    it is a pandas method that sorts the values of a one-dimensional array or a pandas Series. It returns a new Series or DataFrame (depending on the input) with the values sorted in ascending or descending order. By default, the sorting is in **ascending order**, but you can sort in descending order by passing the parameter ascending=False


* argmax() is a function that returns the index of the first occurrence of the maximum value in a one-dimensional array or a pandas Series. It is similar to the idxmax() method, but it returns the index position of the maximum value rather than the index label.


* idxmax() is a pandas method that returns the label of the first occurrence of the maximum value in a one-dimensional array or a pandas Series. It is similar to the argmax() function, but it returns the index label of the maximum value rather than the index position.


*  The "round" method, when given a positive integer argument, rounds numbers after the decimal point. When given a negative integer argument, it rounds numbers *before* the decimal point!

# **Retrieving** a series individual elements using textual vs. positional indexes:

  -- Use .iloc for the numeric index, and .loc for the custom textual index. You don’t need to use .iloc or .loc when you **retrieve slices**, however.
  
  
* Some notes on .iloc and .loc accessor to retreive values:

  -- Instead of using s[i] it is generally better to use s.iloc[i] to retrieve positional numeric index i from series s. This is necessary when working with dataframes, but not when working with series. However, do it to get more comfortable with, since it is a must when working with dataframes. 
  
  -- However, the need for .iloc and .loc goes away when slicing. Even when we’re working with data frames, pandas assumes that if you’re using a slice, then you want to work with the index (rather than the columns). As an example on slicing, go with s['September': 'January'], although I can use s.loc['September': 'January']. Also go with s[0:5] although you can try s.iloc[0:5]  
  
  -- When slicing using a textual index like s['September': 'January']: unlike using the positional numeric indexes here the slice end is no longer "up to and not including," but is rather "up to and including."
  
  -- Most of the time, I prefer to use textual indexes in pandas, because they’re easier to understand, and make the code more readable. But there is a cost: In some simple benchmarking that I performed, I found that it took pandas twice as long to get the text-based slice as the number-based slice. If you find that your pandas analysis is taking a long time, it might be worthwhile to try using positional indexes.
  
**side note:**

In a normal distribution, used for many statistical assumptions, we expect that 68% of a data set’s values will be within 1 standard distribution of the mean, that 95% will be within two standard distributions, and 99.7 will be within three standard distributions.

# Data types in Pandas 

Every series has a dtype attribute, and you can always read from that to know the type of data it contains. Every value in a series is of that type; unlike a Python list or tuple, you cannot have different types mixed together in a series. 

Normally, pandas guesses the dtype based on the data you pass it at creation:

You can specify the type of data in a series by passing a value to the dtype parameter when you create a series like s = pd.Series ([1, 2, 3], dtype = np.float64)

# astype

What if you want to change the dtype of a series once you’ve already created it? You can’t set the dtype attribute; it’s read only. Instead, you will need to create a new series based on the existing one by invoking the astype method:

# Vectorization in Pandas 

One of the most important ideas in pandas (and in NumPy) is that of vectorized operations. When you perform an operation on two different series, the indexes are matched, and the operation is performed via the indexes.

# Selecting values using mask/boolean index in pandas


In Python and other traditional programming languages, we can select elements from a sequence using a combination of for loops and if statements (**THIS IS EXACTLY WHAT I DID TO SOLVE THE ABOVE PROBLEMS**). While you could do that in pandas, you almost certainly don’t want to. Instead, you want to select items using a combination of techniques known as a **"boolean index"** or a **"mask index"**.

Mask indexes are useful and powerful, but their syntax can take some getting used to.

It is called "mask index," because we’re using the list of booleans as a type of sieve, or mask, to select only certain elements.

An explicitly defined list of booleans isn’t very useful or common. But we can also use a series of booleans and those are easy to create. All we need to do is use a comparison operator (e.g., ==) which returns a boolean value. Then we can broadcast the operation, and get a series back. 

We also can use a mask index for assignment like s[s <= s.mean()] = 999 where we replaced the elements less than or equal to the mean with 999.

This technique is worth learning and internalizing, because it’s both powerful and efficient. It’s useful when working with individual series, as in this chapter—but it’s also applicable to entire data frames, as we’ll see later in the book.

# Repeated values for index in pandas series

Yes, unlike the index in a Python string, list, or tuple which are unique, as are the keys in a Python dictionary, the series index can have repeated values—not just integers, but also strings (as in this example) and even other data structures, such as times and dates (as we’ll see in chapter 9). Normally, when we retrieve a value from a series via loc, we expect to get a single value back. But if the index is repeated, then we will get back multiple values. And in pandas, multiple values will be returned as a series.

**note**:

This means that when you retrieve s.loc[i], for a given index value, you can’t know in advance whether you will get a single, scalar value (if the index occurs only once) or a series (if the index occurs multiple times). This is another case in which you need to know your data, to know what type of value you’ll get back.


from chatGPT:

In pandas, the diff() function can be used to calculate the difference between consecutive elements in a Series. This function can also take an optional argument, periods, which specifies the number of periods to shift for the differences to be taken. By default, periods is set to 1.


# FANCY INDEXING

We’ve seen that I can retrieve the item at index 2 with s.loc[2], or the item at index 4 with s.loc[4]. But I can actually retrieve both of them at the same time with what’s known as "fancy indexing" **—passing a list, series, or similar iterable inside of the square brackets**. For example:

s.loc [[2, 4]]

The outer square brackets indicate that we want to retrieve from s using loc. And the inner square brackets indicate that we want to retrieve more than one item. pandas returns a series, keeping the orignal indexes and values.


# pd.read_csv

read_csv is more typically used to create a data frame—but if we provide it with a file that contains only one data point per line, and pass a True value to the squeeze parameter, then we’ll get a series back. 

**s = pd.read_csv('file.csv', squeeze = True, header = None)**

I also set the header parameter to be None, indicating that the first line in the file should not be taken as a column name, but rather is data to be included in our calculations. This will result in the series having a name value of 0, which we can safely ignore.

# value_counts method

It is a series method that is one of my favorites. If you apply value_counts to the series s, you get back a new series whose keys are the distinct values in s, and whose values are integers indicating how often each value appeared. 

Notice that the values are automatically sorted from most common to least common.

Because we get a series back from value_counts, we can use all of our series tricks on it. For example, we can invoke head on it, to get the five most common elements. We can also use fancy indexing, in order to retrieve the counts for specific values. 

If we’re interested in the percentages, not in the raw values, value_counts has an optional normalize parameter, that if set to True returns the fraction. 

so the solution to the above problem would be much soimpler like pass_count.value_counts(normalize = True)[['1', '6']]


# pd.cut

The pd.cut method allows us to **set numeric boundaries, and then to cut a series into parts (known as "bins") based on those boundaries.** Moreover, it can assign labels to each of the bins.

Notice that pd.cut is not a series method, but rather a function in the top-level pd namespace. We’ll pass it a few arguments:

pd.cut(s, bins = [s.min(), 2, 10, s.max()], labels = ['short', 'medium', 'long'])

* our series, s
* a list of four integers representing the boundaries of our three bins, assigned to the bins parameter
* a list of three strings, the labels for our three bins, assigned to the labels parameter

Note that the bin boundaries are **exclusive on the left, and inclusive on the right.** In other words, by specifying that the "medium" bin is between 2 and 10, that means >2 but <=10. The result of this call to pd.cut is a series of the same length as s, but with the labels replacing the values

**good point:**

we can include the lower bound with passig **include_lowest = True** like

df_nyc['trip_distance_group'] = pd.cut(df_nyc['trip_distance'],

                                      bins = [0, 2, 10, df_nyc['trip_distance'].max()],
                                      
                                      labels = ['short', 'medium', 'long'],
                                      
                                      include_lowest = True)

# 1.9 Summary

In this chapter, we saw that a pandas series provides us with some powerful tools to analyze data. Whether it’s the index, reading data from files, calculating descriptive statistics, retrieving values via fancy indexing, or even categorizing our data via numeric boundaries, we were able to do quite a lot.

In the next chapter, we’ll expand our reach to look at data frames, the two-dimensional data strucures that most people think of when they work with pandas.
