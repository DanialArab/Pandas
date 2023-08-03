# Pandas

This repository documents my understanding of Pandas and contains <a href="https://github.com/DanialArab/Pandas/blob/master/pandas.ipynb">my solutions presented as a Jupyter Notebook</a> to 200 detailed exercises (50 main exercises along with 150 beyond exercises) from the book written by Reuven Lerner <a href="https://www.manning.com/books/pandas-workout">Pandas Workouts</a>.

Below is the summary of my notes from the book:

# Table of content

1. [Pandas Series](#1)
   1. [Series Fundamentals](#2)
   2. [Series methods](#3)
      1. [sort_values()](#4)
      2. [argmax()](#5)
      3. [idxmax()](#6)
      4. [round()](#7)
      5. [astype()](#8)
      6. [pd.read_csv()](#9)
      7. [value_counts()](#10)
      8. [pd.cut()](#11)
   3. [Retrieving a series individual elements using textual vs. positional indexes](#12)
   4. [Selecting values using mask/boolean index in pandas](#13)
   5. [Repeated values for index in pandas series](#14)
   6. [Fancy indexing](#15)
   7. [Summary](#16)
2. [Pandas DataFrames](#17)
   1. [DataFrames Fundamentals](#18)
   2. [Bracket vs. dot notation](#19)
   3. [Adding columns to a dataframe](#20)
   4. [Retrieving and Assigning with loc](#21)
   5. [DataFrames methods/functions](#22)
      1. [pd.concat()](#23)
      2. [query()](#24)
      3. [fillna() and dropna()](#25)
      4. [count()](#26)
      5. [interpolate()](#27)
      6. [memory_usage()](#28)
   6. [Three ways to optimize your Pandas data frame's memory usage](#28)
3. [Importing and exporting data](#29)
   1. []
   2. []

5. [Indexes](#4)

6. [Cleaning data](#5)

7. [Grouping, joining, and sorting](#6)

8. [Strings](#7)

9. [Dates and times](#8)

10. [Visualization](#9)

11. [Performance](#10)

Pandas is all about analyzing data. And a major part of the analysis that we do in Pandas can be phrased as, **"Where this is the case, show me that"** (Reuven Lerner).


<a name="1"></a>
## Pandas Series

<a name="2"></a>
### Series Fundamentals

+ If I don't specify an index when creating a pandas series, then the series index will be the default, integers starting from 0
+ Given the flexibility and power pandas gives me, I can assign a list, NumPy array, or pandas series as an index. However, the data structure you pass must be of the same length as the series. If it isn’t, you’ll get a ValueError exception.
+ I can set, or even change, the index even after the series has already been created using s.index = ...
+ Vectorization in Pandas: One of the most important ideas in pandas (and in NumPy) is that of vectorized operations. When you perform an operation on two different series, the indexes are matched, and the operation is performed via the indexes.

<a name="3"></a>
### Series methods

<a name="4"></a>
#### sort_values()
    
+ It is a pandas method that sorts the values of a one-dimensional array or a pandas Series. It returns a new Series or DataFrame (depending on the input) with the values sorted in ascending or descending order. By default, the sorting is in **ascending order**, but you can sort in descending order by passing the parameter ascending=False

<a name="5"></a>
#### argmax() 

+ It is a method that returns the index of the first occurrence of the maximum value in a one-dimensional array or a pandas Series. It is similar to the idxmax() method, but it returns the index position of the maximum value rather than the index label.


<a name="6"></a>
#### idxmax() 

+ It is a pandas method that returns the label of the first occurrence of the maximum value in a one-dimensional array or a pandas Series. It is similar to the argmax() function, but it returns the index label of the maximum value rather than the index position.

<a name="7"></a>
#### round()

+ The "round" method, when given a positive integer argument, rounds numbers after the decimal point. When given a negative integer argument, it rounds numbers *before* the decimal point!

<a name="8"></a>
#### astype ()

Data types in Pandas: 

Every series has a dtype attribute, and you can always read from that to know the type of data it contains. Every value in a series is of that type; unlike a Python list or tuple, you cannot have different types mixed together in a series. 

Normally, pandas guesses the dtype based on the data you pass it at creation. You can specify the type of data in a series by passing a value to the dtype parameter when you create a series like:

      s = pd.Series ([1, 2, 3], dtype = np.float64)

What if you want to change the dtype of a series once you’ve already created it? You can’t set the dtype attribute; it’s read only. Instead, you will need to create a new series based on the existing one by invoking the **astype method.**

<a name="9"></a>
#### pd.read_csv()

It is more typically used to create a data frame, but if we provide it with a file that contains only one data point per line, and pass a True value to the **squeeze parameter**, then we’ll get a series back. 

      s = pd.read_csv('file.csv', squeeze = True, header = None)

I also set the header parameter to be None, indicating that the first line in the file should not be taken as a column name, but rather is data to be included in our calculations. This will result in the series having a name value of 0, which we can safely ignore.

<a name="10"></a>
#### value_counts ()

It is a series method that is one of the favorite ones. If you apply value_counts to the series s, you get back a new series whose keys are the distinct values in s, and whose values are integers indicating how often each value appeared. 

Notice that the values are automatically sorted from most common to least common.

Because we get a series back from value_counts, we can use all of our series tricks on it. For example, we can invoke head on it, to get the five most common elements. We can also use fancy indexing, in order to retrieve the counts for specific values. 

If we’re interested in the percentages, not in the raw values, value_counts has an optional normalize parameter, that if set to True returns the fraction, like:

      pass_count.value_counts(normalize = True)[['1', '6']]


<a name="11"></a>
#### pd.cut()

The pd.cut method allows us to **set numeric boundaries, and then to cut a series into parts (known as "bins") based on those boundaries.** Moreover, it can assign labels to each of the bins.

Notice that pd.cut is not a series method, but rather a function in the top-level pd namespace. We’ll pass it a few arguments:

      pd.cut(s, bins = [s.min(), 2, 10, s.max()], labels = ['short', 'medium', 'long'])

* our series, s
* a list of four integers representing the boundaries of our three bins, assigned to the bins parameter
* a list of three strings, the labels for our three bins, assigned to the labels parameter

Note that the bin boundaries are **exclusive on the left, and inclusive on the right.** In other words, by specifying that the "medium" bin is between 2 and 10, that means >2 but <=10. The result of this call to pd.cut is a series of the same length as s, but with the labels replacing the values

**A good point:**

we can include the lower bound with passig **include_lowest = True** like

      df_nyc['trip_distance_group'] = pd.cut(df_nyc['trip_distance'],

                                                bins = [0, 2, 10, df_nyc['trip_distance'].max()],

                                                labels = ['short', 'medium', 'long'],

                                                include_lowest = True)


<a name="12"></a>
### Retrieving a series individual elements using textual vs. positional indexes

+ Use .iloc for the numeric index, and .loc for the custom textual index. You don’t need to use .iloc or .loc when you **retrieve slices**, however.
+ Some notes on .iloc and .loc accessor to retreive values:

   + Instead of using s[i] it is generally better to use s.iloc[i] to retrieve positional numeric index i from series s. This is necessary when working with dataframes, but not when working with series. However, do it to get more comfortable with, since it is a must when working with dataframes.
   + However, the need for .iloc and .loc goes away when slicing. Even when we’re working with data frames, pandas assumes that if you’re using a slice, then you want to work with the index (rather than the columns). As an example on slicing, go with s['September': 'January'], although I can use s.loc['September': 'January']. Also go with s[0:5] although you can try s.iloc[0:5]
   + When slicing using a textual index like s['September': 'January'], unlike using the positional numeric indexes here the slice end is no longer "up to and not including," but is rather "up to and including."
   +  Most of the time, I prefer to use textual indexes in pandas, because they’re easier to understand, and make the code more readable. But there is a cost: In some simple benchmarking that I performed, I found that it took Pandas twice as long to get the text-based slice as the number-based slice. If you find that your Pandas analysis is taking a long time, it might be worthwhile to try using positional indexes.


<a name="13"></a>
###  Selecting values using mask/boolean index in pandas

In Python and other traditional programming languages, we can select elements from a sequence using a combination of for loops and if statements (THIS IS EXACTLY WHAT I DID TO SOLVE THIS PROBLEM). While you could do that in pandas, you almost certainly don’t want to. Instead, you want to select items using a combination of techniques known as a **"boolean index"** or a **"mask index"**.

Mask indexes are useful and powerful, but their syntax can take some time to get used to.

It is called "mask index," because we’re using the list of booleans as a type of sieve, or mask, to select only certain elements.

An explicitly defined list of booleans isn’t very useful or common. But we can also use a series of booleans and those are easy to create. All we need to do is use a comparison operator (e.g., ==) which returns a boolean value. Then we can broadcast the operation, and get a series back. 

We also can use a mask index for assignments like 

      s[s <= s.mean()] = 999 
      
where we replaced the elements less than or equal to the mean with 999.

This technique is worth learning and internalizing because it’s both powerful and efficient. It’s useful when working with individual series, as in this chapter, but it’s also applicable to entire data frames, as we’ll see later in the book.

<a name="14"></a>
###  Repeated values for index in pandas series

Yes, unlike the index in a Python string, list, or tuple which are unique, as are the keys in a Python dictionary, the series index can have repeated values—not just integers, but also strings (as in this example) and even other data structures, such as times and dates (as we’ll see in chapter 9). Normally, when we retrieve a value from a series via loc, we expect to get a single value back. But if the index is repeated, then we will get back multiple values. And in pandas, multiple values will be returned as a series.

**notes**:

+ This means that when you retrieve s.loc[i], for a given index value, you can’t know in advance whether you will get a single, scalar value (if the index occurs only once) or a series (if the index occurs multiple times). This is another case in which you need to know your data, to know what type of value you’ll get back.
+ In pandas, the diff() function can be used to calculate the difference between consecutive elements in a Series. This function can also take an optional argument, periods, which specifies the number of periods to shift for the differences to be taken. By default, periods is set to 1.

<a name="15"></a>
###  Fancy indexing

We’ve seen that I can retrieve the item at index 2 with s.loc[2], or the item at index 4 with s.loc[4]. But I can actually retrieve both of them at the same time with what’s known as "fancy indexing" **passing a list, series, or similar iterable inside of the square brackets**. For example:

      s.loc [[2, 4]]

The outer square brackets indicate that we want to retrieve from s using loc. And the inner square brackets indicate that we want to retrieve more than one item. Pandas returns a series, keeping the original indexes and values.

<a name="16"></a>
### Summary

In this chapter, we saw that a pandas series provides us with some powerful tools to analyze data. Whether it’s the index, reading data from files, calculating descriptive statistics, retrieving values via fancy indexing, or even categorizing our data via numeric boundaries, we were able to do quite a lot.

In the next chapter, we’ll expand our reach to look at data frames, the two-dimensional data strucures that most people think of when they work with pandas.

<a name="17"></a>
## Pandas DataFrames

<a name="18"></a>
### DataFrames Fundamentals

+ Data frames are two-dimensional tables that look and work similarly to an Excel spreadsheet. The rows are accessible via an index—yes, the same index that we have been using so far with our series! **So long as you use .loc and .iloc to retrieve elements via the index, you’ll be fine.**
+ But of course, data frames also have columns, each of which has a name. Each column is effectively its own series, which means that it has an independent dtype from other columns.
+ In a typical data frame, each column represents a feature, or attribute, of our data, while each row represents one sample.
+ Side note: the term "outliers" doesn’t have a precise, standard definition. one definition is: 

      inter-quartile range (IQR) = quantile(0.75) - quantile(0.25)

Outliers would then be values **below the quantile(0.25) - 1.5 * IQR**, or any values **above the quantile(0.75) + 1.5 * IQR**.

We’ll use this definition here, but you might find that a different definition—say, anything below the mean - two standard deviations, or above the mean + two standard deviations, might be a better fit for your data.

<a name="19"></a>
### Bracket vs. dot notation 

Bracket:

When we’re working with a series, we can retrieve values in several different ways: Using the index (and loc), using the position (and iloc), and also using plain ol' square brackets ("[" and "]"), which is essentially equivalent to loc.

When we work with data frames, we **must use loc or iloc.** That’s because square brackets refer to the columns. If you try to retrieve a row, via the index, using square brackets, you’ll get an error message saying that no such column exists.

Square brackets always refer to columns, and never to rows. **Except, that is, when you pass them a slice, in which they look at the rows.** If you want to retrieve multiple columns, then you must use **fancy indexing.** You cannot use a slice.

dot notation: 

If you want to retrieve the column colname from data frame df, you can say df.colname. **THIS DOES NOT WOTK** when columns names include spaces and other illegal-in-Python-identifer characters. 

<a name="20"></a>
### Adding columns to a dataframe

We just assign to the data frame, using the name of the column that we want to spring into being. It’s typical to assign a series, but you can also assign a NumPy array or list, so long as it is of the same length as the other, existing columns.

There is another way to add a column to a Pandas data frame, namely the assign method. Preferably use just add a new column directly, as described here. But assign returns a new data frame, rather than modifying an existing one, which can come in handy.

Column names are unique—so just as with a dictionary, assigning to an existing column will replace it with the new one. That said, if your data frame’s columns are not of the same dtype, you might find yourself with a SettingWithCopyWarning when assigning for replacement. 

<a name="21"></a>
### Retrieving and Assigning with loc

It’s pretty straightforward to retrieve an entire row from a data frame, or even replace a row’s values with new ones. For example, I can grab the values in the row with index abcd with df.loc['abcd']. If I prefer to use the numeric (positional) index, then I can instead use df.iloc[5]. In both cases, I get back a series. (Yes, even though the columns of a data frame are a series, Pandas uses a series whenever it returns multiple, one-dimensional data.)


Retrieving a whole row isn’t that tough. But what if want to retrieve only part of a row? More significantly, how could we set values on only part of a row?

Pandas actually provides us with a number of techniques for setting values. My preferred method is to use loc, providing two arguments in the square brackets. The first describes the row(s) that we want to retrieve, while the second describes the column(s) we want to retrieve. This technique also lends itself to changing and updating values in a data frame.

If I want to retrieve row a and c and columns x and y: 

    df.loc[['a', 'c'], ['x', 'y']]

I can describe our rows using a boolean index. That is, we can create a boolean series using a conditional operator (e.g., < or ==), and apply it to the rows and/or the columns. For example, I can find all of the rows in which x is greater than 200:

    df.loc[df['x']>200]

I can then add a second value boolean index, after the comma, indicating which columns we want: 

    df.loc[df['x']>200, df.loc['c'] > 400]

The above expression will return all of those rows from df in which column x was greater than 200, and all those columns from df in which row c was greater than 400.

Notice that because the first boolean index is choosing rows, it is based on a column. And because the second boolean index is choosing columns, it is based on a row—which means that it should be using loc.

Of course, our conditions can be far more complex than these. But as long as you keep in mind that you want to select based on rows before the comma, and based on columns after the comma, you should be fine.

If I want to set all of the values in row b, where column c is even, to new values, then I can assign a list (or NumPy array, or pandas series) of three items, matching the three I get back from the query:

    df.loc['b', df.loc['c'] % 2 == 0] = [123, 456, 789]

I can also assign a data frame or 2-dimensional NumPy array to any two-dimensional selection. For example, here I’ll assign the float 1 to 12 different elements of df:

    df.loc[df['v'] > 400, df.loc['d'] > 400] = np.ones(12).reshape(4,3)

We can broadcast a scalar values to any of the above. For example:

    df.loc[df['v'] > 400, df.loc['d'] > 400] = 987

Finally, if your data frame’s columns are not of the same dtype, then you might encounter a SettingWithCopyWarning when you replace an existing column with a new set of values. You can avoid trouble by using loc to assign to all rows and a particular column using :, as if we were working with a slice:

    df.loc[:, 'd'] = df['d'].astype(np.float16)

The above code will replace the current values of column d with new values, all of them having a dtype of float16.

<a name="22"></a>
### DataFrames methods/functions 

We’ll also see how just about every **series method** will also work on a data frame, returning one value per data frame column.

<a name="23"></a>
#### pd.concat()

The pd.concat function concatenates dataframes with each other. It’s a top-level Pandas function, and takes a list of data frames you would like to concatenate. By default, pd.concat assumes that you want to join them top-to-bottom, but you can do it side-to-side if you want by setting the axis parameter.

The result of pd.concat is a new data frame.

**note on the final dataframe after the concatenation**: 

Next, I asked you to ensure that the new data frame (the one after concatenation)’s index doesn’t contain duplicate values—something that is almost certainly the case at this point, given that we created df from two previous data frames. You can actually check to see if a data frame’s index contains repeated values with the code

    df_ny_taxi_2020.index.is_unique

If this returns True, then the values are already unique. If not, then some Seaborn plots will give you errors. We could renumber the index on our own, but why work so hard, when Pandas includes this functionality? We can just say:

    df_ny_taxi_2020 = df_ny_taxi_2020.reset_index(drop=True)

By passing drop=True, we tell reset_index not to make the just-ousted (means just-removed) index column a regular column in the data frame, but rather to drop it entirely.

<a name="24"></a>
#### query()

It is used to retrieve rows.

The traditional way to select rows from a data frame, as we have seen, is via a boolean index. But there is another way to do it, namely the query method. This method might feel especially familiar if you have previously used SQL and relational databases.

The basic idea behind query is simple: We provide a **string** that Pandas turns into a full-fledged query. We get back a filtered set of **rows** from the original data frame. For example, let’s say that I want all of the rows in which the column v is greater than 300. Using a traditional boolean index, I would write:

    df[df['v'] >300]

Using query, I can instead write:

    df.query('v > 300')


These two techniques return the same results. When using query, though, we can name columns without the clunky square brackets, or even the dot notation. It becomes easier to understand.

What if I want to have a more complex query, such as where column v is greater than 300 and column w is odd? We can write it as follows:

    df.query('v > 300 & w % 2 ==1') 

It’s not necessary, but I still like to use parentheses to make the query a bit more readable:

    df.query('(v > 300) & (w % 2 == 1)')

Note that query cannot be used on the left side of an assignment.

Also note that in some simple benchmarks that I ran, using query took about twice as long to execute as loc. It might be more convenient, but it isn’t necessarily a good idea if you’re worried about performance.

Is pandas query for just rows?

Re:

Yes, the Pandas query method is used to **filter rows** in a DataFrame based on a Boolean expression. It returns a **new DataFrame** containing only the rows that satisfy the specified condition.

<a name="25"></a>
#### fillna() and dropna() 

How to deal with NaN and missing data:

Pandas uses something known as NaN, aka "not a number." NaN is the Pandas style for writing nan, a value that’s also available in NumPy. Both names are aliases to the same strange value, a float that cannot be converted into an integer, and that is not equal to itself.

In NumPy, we typically search for NaN values with the **isnan** function. Pandas has a different approach, though: We can **replace** the NaN values in a series (or data frame) with the **fillna** method. And we can **drop any row with NaN values with the dropna** method.

Both of these methods return a **new series or data frame**, rather than modifying the original object. However, the new object you get back might not have copied the data, which means that assigning to it might produce the famous, dreaded SettingWithCopyWarning. If you plan to modify the series or data frame that you get back from df.dropna, you should probably invoke the copy method, just to be sure:

    df = df.dropna().copy()

This ensures that you can then modify df without having to suffer from that warning.

As you can imagine, it might be a bit extreme to remove any row containing even a single NaN value. For that reason, the dropna method has a **thresh** parameter, to which we can pass an integer—the number of **good, non-NaN values** that a row must contain in order for it to be kept. You might need to give some serious thought to how strictly you want to filter your data.

<a name="26"></a>
#### count()

The **count** method on a series returns the number of **non-NaN values**. If there are no NaN values at all, then the result will be the same as the size of the series.

The count method on a data frame returns a series, with the columns' names as the index. Any columns with lower numbers, Reuven should have meant "compared to the len of dataframe", contain NaN values.

<a name="27"></a>
####  interpolate()

When your data contains missing values, you have a few possible ways to handle this. You can remove rows with missing values, but that might remove a large number of otherwise useful rows. A standard alternative is interpolation, in which you replace NaN with values that are likely to be close to the original ones. The values might be wrong, but they will be roughly in the right ballpark.

When we call df.interpolate, it returns a new data frame. In theory, all of the columns will be interpolated—but if there is only missing data in one specific column that column will be interpolated. 

Reuven's tip on urgency to use .loc: 

If you’re like many Pandas users, then you might have thought about things like this:

  	  df_temps[df_temps['temp'] < 0]['temp'] = 0

Logically, this makes perfect sense. There’s just one problem: You cannot know in advance if it will work. That’s because Pandas does a lot of internal analysis and optimization when it’s putting together our queries. You thus cannot know if your assignment will actually change the temp column on df, or—and this is the important thing—if Pandas has decided to cache the results of your first query, applying ['temp'] to that cached, internal value rather than to the original one.

As a result, it’s common—and maddening!—to get a SettingWithCopyWarning from Pandas. It looks like this:

<ipython-input-2-acedf13a3438>:1: SettingWithCopyWarning:
A value is trying to be set on a copy of a slice from a DataFrame.
Try using .loc[row_indexer,col_indexer] = value instead
    
When you get this warning, it’s because Pandas is trying to be helpful and nice, telling you that your assignment might have no effect. The warning, by the way, isn’t telling you that the assignment won’t work, because it might. It all depends on the amount of data you have, and how Pandas thinks it can or should optimize things.

The telltale sign that you might get this warning is the use of double square brackets—not nested, with one pair inside of the other, but with one right after the other. Whenever you see **][** in pandas queries, you should try hard to avoid it, because it might spell trouble when you assign to it. And truthfully, retrieving with this syntax, while something that all of us have done over the years, is something that you can avoid **using loc** and the "rows, columns" selection syntax that we’ve seen and discussed.

So, how should we actually set these values? It’s actually pretty straightforward:

    df_temps.loc[df_temps['temps'] < 0, 'temp']  = 0
                                     
If you use this syntax for all of your assignments, you won’t ever see that dreaded SettingWithCopyWarning message. You’ll be able to use the **same syntax for retrieval and assignment**. And you can even be sure that things are running pretty efficiently.

<a name="28"></a>
#### memory_usage()

We’ll talk more about this in future chapters, but the memory_usage method allows you to see how much memory is being used by each column in a data frame. It returns a series of integers, in which the index lists the columns and the values represent the memory used by each column. 

      df.memory_usage()       

to get the dataframe's details:

      df.info(memory_usage = 'deep')

<a name="29"></a>
### Three ways to optimize your Pandas data frame's memory usage

+ choose your columns wisely using "usecols", df.columns gives me the name of columns 
+ choose dtypes appropriately 
+ remove rows that you don't need through cleaning
                              
<a name="29"></a>
## Importing and exporting data

**CSV, the non-standard standard**

At its heart, CSV assumes that your data can be described as a two-dimensional table. The rows are represented as rows in the file, and the columns are separated by… well, they’re separated by commas, at least by default. CSV files are text files, which means that you can read (and edit) them without special tools.

Rather than take a stand on how CSV files should be formatted, pandas tries to be open and flexible. When we read from a CSV file (with pd.read_csv) or write a data frame to CSV (with df.to_csv), you can choose from many, many parameters, each of which can affect the way in which it is written. Among the most common are:

* sep, the field separator, which is (perhaps obviously) a comma by default, but can often be a tab ('\t')
* header, whether there are headers describing column names, and on which line of the file they appear, which can be controlled by the header parameter
* indexcol, which column, if any, should be set to be the index of our data frame
* **usecols**, which columns from the file should be included in the data frame

Why do we read CSV files with the pd.read_csv function, rather than with a method on an existing data frame? Because the goal of read_csv is to create (and return) a new data frame based on the contents of the CSV file, not to modify or update the contents of an existing one.

Most of the time, and especially when a CSV file has headers indicating the column names, I like to use those names in my call to read_csv. That makes the function call easier to read and debug. But when you want to rename the columns with the **names parameter**, you need to describe them numerically. Moreover, in order to avoid having the header row read as data, we need to indicate which row contains the header (0, in our case), effectively causing it to be ignored. like 

    oil_filename = '../data/wti-daily.csv'

    oil_df = pd.read_csv(oil_filename,

                         parse_dates=[0],

                         header=0,

                         index_col=0,

                         names=['date', 'oil'])
                     
my implementation which worked just fine is below, i think since i first used names parameter then parse_dates and index_col i am good:

    df_oil = pd.read_csv(directory + 'wti-daily.csv',

                        header = 0,

                        names = ['date', 'oil'],

                        parse_dates = ['date'],

                        index_col = ['date'])


**a side note from reuven on columns's names:**

There is nothing inherently wrong with loading the data in this way, meaning keeping the original columns'names. However, when we use pd.query and pd.eval, it’s often annoying to have column names that includes spaces in them. Yes, we can use **backticks**, but it’s more convenient to give them names that’ll allow us to treat them as variables inside of the query string. So while there’s nothing technically wrong with loading the data as I’ve done here, I’ll then want to set the headers to be single-word names. I can do that by assigning a list of strings to df.columns:

    df.columns = ['pid', 'state', 'ptype', 'make', 'color', 'feet'] 

Now, you might be thinking that it would be more effective to set these names as part of the call to read_csv. After all, read_csv has a names parameter, which takes a list of strings that are assigned to the newly created data frame. However, things get tricky if we want to rename the columns (with names) and also load a subset of the columns (with usecols). That’s because passing a value to names means that you need to use those names, rather than the original ones from the file, when choosing columns in usecols. And you can only do that if you name all of the columns, which is rather annoying.

Actually, there is another way to do it: You can specify which columns you want by passing a list of integers to usecols. pandas will select the columns at those indexes. You can then assign them names by passing a value to the names parameter. Here’s how I would do that:

    df = pd.read_csv(filename,

                    usecols=[1, 2, 3, 7, 33, 37],

                    names=['pid', 'state', 'ptype', 'make', 'color', 'feet'])

Will this work? Yes, it will, and in many cases, it might be the preferred way to go. However, I have two problems with it: First, I find it somewhat annoying to find the integer positions for the columns we want to load. And secondly, when I ran this code on my computer, I got the "low memory" warning that we’ve sometimes seen in previous examples. I thus decided to avoid the annoyance of finding the desired columns' numeric locations and the low-memory warning, and to use the two-step column renaming that appears in the solution.

**compressed files can be directly impoted into pandas**

You might have noticed that the files I’ve provided both have a .csv.gz suffix. This means that they are compressed with "gzip"—but you don’t need to uncompress them before loading, because **Pandas is smart enough to automatically do so when we run read_csv**.

    institutions_filename = '../data/Most-Recent-Cohorts-Institution.csv.gz'

    institutions_df = pd.read_csv(institutions_filename,

                    usecols=['OPEID6'])


# useful tip on data science

When teaching data science, I often use the phrase "know your data." That’s because it’s important to really know as much about your data as you can before willy-nilly reading it into memory. You probably don’t want to load all of the columns into pandas. And you might want to specify the type of data that’s in each column, rather than let pandas just guess.

Most data sets come with a "data dictionary," a file that describes the columns, their types, their meanings, and their ranges. It’s almost always worth your while to read a data dictionary when starting to analyze the data. In many cases, the dictionary will give you insights into the data.
                                 
#### usecols:

The usecols parameter to pd.read_csv allows us to select which columns from the CSV file will be kept around. The parameter takes a list as an argument, and that list can either contain integers (indicating the numeric index of each column) or strings representing the column names. I generally prefer to use strings, since they’re more readable, and that’s what I did here.

### url

You can pass a URL to read_csv, and assuming that the URL returns a CSV file, pandas will return a new data frame. The rest of the parameters are the same as any other call to read_csv. The only difference is that you’re reading from a URL, rather than from a file on a filesystem.

Why is this important and useful? Because there are numerous systems that produce hourly or hourly reports, publishing in CSV format to a URL that doesn’t change. If you retrieve data from that URL, then you’re guaranteed to get a CSV file reflecting the latest and greatest data. Thanks to the URL provisions of read_csv, you can include pandas in your daily reporting routine, summarizing and extracting the most important data from this report.

In many cases, CSV files published to a URL will require authentication via a username and password. In some cases, sites allow you to include such authentication details in the URL. For those that don’t, you won’t be able to retrieve directly via read_csv. Rather, you’ll need to retrieve the data separately, perhaps using the excellent third-party **requests package**, and then create a StringIO with the contents of the retrieved data.

What always amazes me about using pd.read_csv is how easy it is to read CSV data from a URL. Other than the fact that the data comes from the network, it works the same as reading from a file. Among other things, we can select which columns we want to read using the usecols parameter.

                                       
### tips on reading text file through pd.read_csv

First just open it to see what is the field separator. Then see if you have any comments and if so see how the start of a comment line is mared, then you need to do header = None b/c it is not a csv file and so definitely doesn’t have headers. then you may want to pass names including your desired names for the dataframe you will be creating. 


#### 1. sep
CSV files are named for the default field separator, the comma. By default, pandas assumes that we have comma-separated values. It’s fine if we want to use another character, but then we’ll need to specify that in the sep keyword argument. If our separator is :, so we’ll pass sep=':' to read_csv.

#### 2. comment
read_csv lets us specify the string that marks the start of a comment line. By passing it comment=' ', we indicate that the parser should simply ignore such lines. if the comment starts with # then i need to pass comment='#'

#### 3. header
By default, read_csv assumes that the first line of the file is a header, containing column names. It also uses that first line to figure out how many fields will be on each line. **If a file contains headers, but not on the file’s first line, then you can set header to an integer value, indicating on which line read_csv should look for them**. But if your file is not really a CSV file then it definitely doesn’t have headers. Fortunately, you can tell read_csv that there is no header with header=None.

#### 4. blank lines
What about the blank lines? We actually got off pretty easy here, in that **read_csv ignores blank lines by default**. If you want to treat blank lines as NaN values, then you can pass skip_blank_lines=False, rather than accepting the default value of True.

#### 5. names
if your file is not a csv file and like it is a text file. In this case, if we don’t give any names, then the data frame’s columns will be labeled with integers, starting with 0. There’s nothing technically wrong with this, but it’s harder to work with data in this way. Besides, it’s easy enough to pass the names we want to give our columns, as a list of strings. 

### How to specify more than one separator

I’m often asked if we can specify more than one separator. For example, what if fields can be separated by either : or by ,? What do we do then?

pandas actually has a great solution: If sep contains more than one character, then it is treated as a **regular expression**. So if you want to allow for either colons or commas, you could pass a separator of [:,]. If that looks reasonable to you, then congratulations: You probably know about regular expressions. If you don’t know them, then I strongly encourage you to learn them! Regular expressions are extremely useful to anyone working with text, which is nearly every programmer. I have a free tutorial on regular expressions using PPython at RegexpCrashCourse.com/, if you’re interested.

The big downside to using regular expressions to handle field separators is that it requires the use of a Python-based CSV parser. By default, pandas uses a C-based parser, which runs faster and uses less memory. Consider whether you really need this functionality, and thus the performance hit that the Python-based parser creates.
                                      
### Reuven crash course on regular expressions 

This is an E-mail course: Regexps crash course meaning in 14 days I am supposed to receive one email teaching me regexps along with some exercises.

* day_1

 -- Regexps have a reputation for being impossible to understand, even among experienced programmers; my goal is to show you that this doesn't have to be the case.
 
 -- What we want, then, is a way to describe the patterns for which we're looking in the text, in a way that is compact and precise. And that's the job of regular expressions.

 -- Regexps allow us to search for patterns within text.  Once we have found that pattern, we can do any number of things with it:
* Print it out;
* Use it as the basis for a search-and-replace operation;
* Or, dig deeper into the match, extracting portions of it.

 -- Regexps have been around for many years, but as the quantity of text produced becomes ever larger, the need for finding ways to search for patterns has become ever more urgent. 

### html

The **pd.read_html** function, like pd.read_csv, takes a file-like object or a URL. It assumes that it’ll encounter HTML-formatted text containing at least one table. It turns each table into a data frame, then returns a list of those data frames.
      
### JSON

There’s no doubt that CSV is an important, useful, and popular format. But in some ways, it has been eclipsed by another format: JSON, aka "JavaScript Object Notation." **JSON allows us to store numbers, text, lists, and dictionaries in a text format that’s both readable and writable with a wide variety of programming languages**. Because it’s both easier to work with and smaller than XML, while also more expressive than CSV, it’s no surprise that JSON has become a common format for both storing and exchanging data. JSON has also become the standard format for Internet APIs, allowing us to access a variety of services in a cross-platform manner.

Just as we can retrieve CSV-formatted data with pd.read_csv, we can retrieve JSON-formatted data with **pd.read_json**.
      
### excel file

Also like read_csv, the read_excel method has index_col, usecols, and names parameters, allowing you to specify which columns should be used for the data frame, what they should be called, and whether one or more should be used as the data frame’s index.
      
### Dataframes and dtype

In Chapter 1, we saw that every series has a dtype describing the type of data that it contains. We can retrieve this data using the dtype attribute, and we can tell pandas what dtype to use when creating a series using the dtype parameter when we invoke the Series class.

In a data frame, each column is a separate pandas series, and thus has its own dtype. By invoking the **dtypes (notice the plural!)** method on our data frame, we can find out what the dtype is of each column. This information, along with additional details about the data frame, is also available by invoking the **info method** on our data frame.

When we read data from a CSV file, pandas tries its best to infer the dtype of each column. Remember that CSV files are really text files, so pandas has to examine the data to choose the best dtype. It will basically choose between three types:

* If the values can all be turned into integers, then it chooses int64.
* If the values can all be turned into **floats—which includes NaN**—then it chooses float64.
* Otherwise, it chooses object, meaning core Python objects.

However, there are several problems with letting pandas analyze and choose the data in this way.

First, while these default choices aren’t bad, they can be overly large for many values. We often don’t need 64-bit numbers, so choosing int64 or float64 will waste disk space unnecessarily.

The second problem is much more subtle: If pandas is to correctly guess the dtype for a column, then it needs to examine all of the values in that column. But if you have millions of rows in a column, then that process can use a huge amount of memory. For this reason, read_csv reads the file into memory in pieces, examining each piece in turn and then creating a single data frame from all of them. You normally won’t know that this is happening; pandas does this in order to save memory.

This can potentially lead to a problem, if it finds (for example) values that look like integers at the top of the file, and values that look like strings at the bottom of the file. In such a case, you end up with a dtype of object, and with values of different types. This is almost certainly a bad thing, and pandas warns you about this with a DtypeWarning. If you load the New York City taxi data from January 2020 into pandas without specifying usecols, then you might well get this warning—I often did, on my computer.

One way to avoid this mixed-dtype problem is to tell pandas not to skimp on memory, and that it’s OK to examine all of the data. You can do that by passing a False values to the **low_memory parameter in read_csv.** By default, low_memory is set to True, resulting in the behavior that I’ve described here. But remember that setting low_memory to False might indeed use lots of memory, a potentially big problem if your dataset is large.

Notice that I passing the low_memory=False keyword argument tells pandas that I had enough RAM on my computer that it could look through all of the rows in the data set, when trying to determine what dtype to assign to each column.

**from chatGPT:**

The low_memory parameter in pandas is used to control the memory usage when reading a large file. When set to True, pandas will attempt to read the file **in chunks**, which can **reduce the memory usage but may also slow down the read operation**. When set to False, pandas will read the entire file into memory at once, which can **speed up the read operation but may also consume a lot of memory.**

By default, the low_memory parameter is set to True. If you have enough memory available and want to speed up the read operation, you can set low_memory to False. 

**A better solution is to tell pandas that you don’t want it to guess the dtype, and that you would rather tell it explicitly. You can do that by passing a dtype parameter to read_csv, with a Python dictionary as its argument. The dict’s keys will be strings, the names of the columns being read from disk, and the values will be the data types you want to use.** It’s typical to use data types from pandas and NumPy, but if you specify int or float, then pandas will simply use np.int64 or np.float64. And if you specify str, then pandas will store the data as Python strings, assigning a dtype of object.

Finally: It’s often tempting to set a dtype to be an integer value. But remember that **if the column contains NaN, then it cannot be defined as an integer dtype. Instead, you’ll need to read the column as floating-point data, remove or interpolate the NaN values, and then convert the column (using astype) to the integer type you want.**
      
### How to wisely choose the wisest dtype

* Like for example for trip_distance, total_amount, and tip_amount it seems we do need float but how to choose its type like float64 is more appropriate or float16? you can take a look at the following table and see if the maximum number in your column is below the max value that can be represented by each dtype then that dtype is safe to use:
      
|int | |   |
| -- | --|  -- |
| dtype | range of values|  **max value**|
| int8 | (-128 to 127)| 127 |
| int16 | (-32768 to 32767)| 32_767 |
| int32 | (-2147483648 to 2147483647)| 2_147_483_647 |
| int64 | (-9223372036854775808 to 9223372036854775807)| 9223372036854775807 |


|float | |   |
|-- | --|  -- |
|dtype | range of values|  **max value**|
| float16 (half-precision floating-point) | (-65504 to 65504) | 65_504 |
| float32 (single-precision floating-point) |(-3.4028235 x 10^38 to 3.4028235 x 10^38) | 3.4028235 x 10^38 |
| float64 (double-precision floating-point) | (-1.79 x 10^308 to 1.79 x 10^308) | 1.79 x 10^308 |

<a name="4"></a>
## 4. Indexes

Every data frame has an index (describing the rows) and a list of columns. Indexes in Pandas are extremely flexible and powerful; an index can even be hierarchical, allowing us to query our data in sophisticated ways. Understanding how we can create, replace, and use indexes is a crucial part of working with Pandas. In this chapter, we’ll practice working with indexes in a variety of ways. We’ll also see how we can change a data frame’s index, and how we can use it to summarize our data in a **"pivot table."**

We have already seen numerous examples of how to retrieve one or more rows from a data frame **using its index, along with the loc atttribute.** We don’t necessarily need to use the index to select rows from a data frame, but it does make things easier to understand and for clearer code. For this reason, we often want to use one of a **data frame’s existing columns as an index**. Sometimes, we’ll want to do this permanently, while at other times, we’ll want to do it briefly, just to make our queries clearer.

You’ll likely end up doing a **great deal of setting and resetting the index as you work with pandas with real-life data sets.**

We have already seen that if we want to retrieve rows from a data frame that meet a particular condition, we can use a boolean index. **Oftentimes, especially if we are looking for specific values from a column, it makes more sense to turn that column into the data frame’s index.** pandas makes it easy to do this, with the **set_index** method.

**If we want to perform several queries based on the specific column, it makes sense to set the index to that particular column.** like if we are going to perform several queries based on the parking tickets' issue date, it makes sense to set the index to the Issue Date column. Notice that set_index returns a **new data frame**, based on the original one, which we assign back to df. As of this point, if we make queries that involve the index (typically using loc), it’ll be based on the value of issue date. Also: As far as the data frame is concerned, **there is no longer an Issue Date column! Its identity as a named column is gone**, at least for now.

As of this writing, the set_index method (along with many others in pandas) supports the **inplace parameter**. If you call set_index and pass inplace=True, then the method will return None, and will modify the dataframe. The core pandas developers have warned that this is a bad idea, because it makes incorrect assumptions about memory and performance. There is no benefit to using inplace=True. As a result, the inplace parameer is likely to go away in a future version of pandas.

Thus while it might seem wasteful to call set_index and then assign its result back to df, this is the preferred, idiomatic way that we are to do things in pandas.

### Working with multi-indexes

Every data frame has an index, giving labels to the rows. We have already seen that we can use the loc accessor to retrieve one or more rows using the index. For example, I can say

    df.loc['a']

to retrieve all of the rows with the index value a. Remember that the **index doesn’t necessarily contain unique values**; retrieving loc['a'] might return a series of values representing a single row, but it also might return a data frame whose rows all have the index value a.

This sort of index ofen serves us quite well. But there are many cases in which it’s not quite enough. That’s beacuse the world is full of hierarchical information, or information that is easier to process if we make it hierarchical.

For example, every business wants to know their sales figures. But just getting a single number doesn’t let you analyze the information in a truly useful way. So you might want to break it down by product, in order to know how well each product is selling well, and which is contributing the most to your bottom line. (We saw a version of this in Exercise 8.) However, even that isn’t quite enough; you probably want to know how well each product is selling per month. If your store has been around for a while, you might want to break it down even further than that, finding the quantity of each product you’ve sold, per month, per year. A **multi-index** will let you do precisely that.

I can create a multi-index by passing a **list of columns to set_index**:

    df = df.set_index(['year', 'month'])

Remember that when you’re creating a multi-index, you want **the most general part to be on the outside, and thus be mentioned first**. If you were to create a multi-index with dates, you would do it using year, month, and day, in that order. If you were to create a multi-index for your company’s sales data, you might use region, country, department, and product, in that order.

With this in place, we can now retrieve in a variety of different ways. For example, I can get all of the sales data, for all products, in 2018:

    df.loc[2018]

I can get all sales data for just products A and C in 2018:

df.loc[2018, ['A', 'C']]

**Notice that I’m still applying the same rule as we’ve always used with loc—the first argument describes the row(s) we want, and the second argument describes the column(s) we want. Without a second argument, we get all of the columns.**

I’ve got a multi-index on this data frame, which means that I can break the data down not just by year, but also by month. For example, what did it look like for all three products in June, 2019?

    df.loc[(2019, 'Jun')]

Notice that I’m still using square brackets with loc. However, the first (and only) argument is a tuple (i.e., round parentheses). **Tuples are typically used in a multi-index situation when you want to specify a specific combination of index levels and values**. For example, I’m looking for 2019 and Jun—the outermost level and the inner level—so I use the tuple (2019, 'Jun'). I can, of course, retrieve the sales data just for producs A and C here, too:

    df.loc[(2019, 'Jun'), ['A', 'C']]

What if I want to see more than one year at a time? For example, let’s say that I want to see all data for 2019 and 2020. I can say:

    df.loc[[2019, 2020]]

And if I want to see all data for 2019 and 2020, but only products B and C?

    df.loc[[2019, 2020], ['B', 'C']]

Another way to write this is by using a "slice":

    df.loc[2019:2020, ['B', 'C']]

Equivalently, you can use the slice builtin function for the same effect:

    df.loc[slice(2019,2020), ['B', 'C']]

What if I want to get all of the data from June in both 2019 and 2020? It’s going to be a bit complicated:

* I use square brackets with loc
* The first argument in the square brackets describes the rows I want—and I want all columns, so there won’t be a second argument
* I want to select multiple combinations from the multi-index, **so I’ll need a list**
* Each year-month combination will be a separate tuple in the list.
The result is:

    df.loc[[(2019, 'Jun'), (2020, 'Jun')]]

What if I want to look at all of the values that took place in June, July, or August, across all three years? We could, of course, do it manually:

    df.loc[[(2018, 'Jun'), (2018, 'Jul'), (2018, 'Aug'),
            (2019, 'Jun'), (2019, 'Jul'), (2019, 'Aug'),
            (2020, 'Jun'), (2020, 'Jul'), (2020, 'Aug')]]

This worked well, but it seems a bit wordy. Isn’t there another way that we could do this? The answer is "yes." Intuitively, we might guess that we can tell pandas we want all of the years (2018, 2019, and 2020), and only three months (Jun, Jul, and Aug). We could, thus, write the following:

    df.loc[([2018, 2019, 2020], ['Jun', 'Jul', 'Aug'])]

**But this won’t work!** And it’s rather surprising and confusing to find that it doesn’t work, when it seems so obvious and intuitive, given everything else we know about pandas. So, what’s missing? **An indicator of which columns we want,** what’s what:

    df.loc[([2018, 2019, 2020], ['Jun', 'Jul', 'Aug']), ['A', 'B', 'C']]

**While the second argument (i.e., a selection of columns) is generally optional when using loc, here it isn’t: You need to indicate which column, or columns, you want, along with the rows. You can do it explicitly, as I did above, or you can use Python’s "slice" syntax:**

    df.loc[([2018, 2019, 2020], ['Jun', 'Jul', 'Aug']), 'A':'B']

If you want all of the columns, you can use a colon all by itself:

    df.loc[([2018, 2019, 2020], ['Jun', 'Jul', 'Aug']), :]

Assuming that the **index is sorted**, you can even select the years using a slice:

    df.loc[(:, ['Jun', 'Jul', 'Aug']), 'A':'B']

Oh, **wait—actually, you can’t do that here.** That’s because **Python only allows the colon within square brackets**. And we tried to use the colon within a tuple, which uses regular, round parentheses. However, we can use the builtin slice function with None as an argument for the same result:

    df.loc[(slice(None), ['Jun', 'Jul', 'Aug']), 'A':'B']

And sure enough, that works. You can think of slice(None) as a way of indicating to pandas that we are willing to have all values, as a wildcard.

As you can see, loc is an extremely versatile tool, allowing us to retrieve from a multi-index in a variety of ways.

As we have seen, setting the index can make it easier for us to create queries about our data. But sometimes our data is hierarchical in nature. That’s where the pandas concept of a "multi-index" comes into play. With a multi-index, you can set the index not just to be a single column, but multiple columns. Imagine, for example, a data frame containing sales data: You might want to have sales broken down by year, and then further broken down by region. Once you use the phrase "further broken down by," a multi-index is almost certainly a good idea.

read_csv also has a **index_col** parameter. If we pass an argument to that parameter, then we can tell read_csv to do it all in one step—reading in the data frame, and setting the index to be the column that we request. However, it turns out that we can pass a **list of columns as the argument to index_col**, thus creating the multi-index as the data frame is collected. For example:


    df = pd.read_csv(filename,
                    usecols=['Year', 'State.Code', 'Total.Math', 'Total.Test-takers', 'Total.Verbal'],
                    index_col=['Year', 'State.Code'])

### Sorting by index

When we talk about sorting data in pandas, we’re usually referring to sorting the **data**. For example, I might want to have the rows in my data frame sorted by price or by regional sales code. We’ll talk more about that kind of sorting in Chapter 6.

But pandas lets us sort our data frames in an additional way: **Based on the index values**. We can do that with the method **sort_index**, which like so many other methods returns **a new data frame with the same content, but whose rows are sorted based on index values**. We can thus say:

    df = df.sort_index()

**If your data frame contains a multi-index, then the sorting will be done primarily along the first level, then along the second level, and so forth.**

In addition to having some aesthetic benefits, **sorting a data frame by index can make certain tasks easier, or even possible.** For example, **if you try to select a slice of rows, pandas insists that the index will be sorted, in order to avoid the ambiguity.**

If your data frame is unsorted and has a multi-index, then performing some operations might result in a warning:

**PerformanceWarning: indexing past lexsort depth may impact performance**

This is pandas trying to tell you that the combination of large size, multi-index, and an unsorted index are likely to cause you trouble. You can avoid the warning by sorting your data frame via its index.

If you want to check whether a data frame is sorted, you can check this attribute:

    df.index.is_monotonic_increasing

Note that this is not a method, but is rather a boolean value. Also note that some older documentation and blogs mentions the method is_lexsorted, which has been deprecated in recent versions of pandas; you should instead be using is_monotonic_increasing.

**point on inplace**
You can invoke set_index with inplace=True. If you do this, then set_index will modify the existing data frame object, and will return None. But as with all other uses of inplace=True in pandas, the core developers strongly recommend against doing this. Instead, you should invoke it regularly (i.e., with a default value of inplace=False), and then assign the result to a variable—which could be the variable already referring to the data frame, as I’ve done here: df = df.sort_index()

### critically important tip on sorting by index
While we don’t necessarily need to sort our data frame by its index, certain pandas operations will work better if we do. Moreover, if we don’t sort the data frame, we might get the PerformanceWarning that I mentioned earlier in this chapter. So especially when we’re going to be doing operations with a **multi-index**, it’s a good idea to sort by the index at the get-go.

### CRITICALLY important point i learned on my own and not mention explicitly in the book 

when I have multi-indexes and want to specify let's say index at level 3 i cannot ignore the first two levels like in the following that does not work:

    df_game.loc['Archery', ['Team', 'Medal']].value_counts()

b/c here sport is the 3rd level index specified by 'Archery', I do need to specify indexes at levels 1 and 2 through slice(None), but i can safely ignore any other indexes after the one i specified (here the 3rd index level which was sport and specified with 'Archery'). the correct query is as follows: 

    df_game.loc[(slice(None), slice(None), 'Archery'), ['Team', 'Medal']].value_counts()
    
### xs and IndexSlice methods

#### xs

As we have already seen, loc makes it pretty straightforward to retrieve data from our multi-indexed data frames. However, there are times when we might want to use a multi-index in a different kind of way. pandas provides us with a few other methods for doing so, one being xs and the other IndexSlice.

Because multi-indexed data frames are both common and important, pandas provides a number of ways to retrieve data from them.

Let’s start with xs, which lets us accomplish what we did in Exercise 23, namely find matches for certain levels within a multi-index. For example, one question in the previous exercise asked you to find the mean height of participants in the "Table Tennis Women’s Team" event from all years of the Olympics. **Using loc, we had to tell pandas to accept all values for year, all values for season, and all values for sport—in other words, we were only checking the fourth level of the multi-index, namely the event**. Our query looked like this:

    df.loc[(slice(None), 'Summer' , slice(None), "Table Tennis Women's Team"), 'Height'].mean()

Using xs, we could shorten that query to:

    df.xs("Table Tennis Women's Team", level='Event').mean()

You might have noticed that I actually lied a bit, when I said that we didn’t search by season. As you can see in the loc-based query, we actually did include that in our search. Fortunately, I can handle that by passing a list of levels to the level parameter, and a tuple of values as the first argument:

    df.xs(('Summer', "Table Tennis Women's Team"), level=['Season', 'Event']).mean()

Notice that **xs is a method, and is thus invoked with round parentheses. By contrast, loc is an accessor attribute, and is invoked with square brackets.** And yes, it’s often hard to keep track of these things.

You can, by the way, use integers as the arguments to level, rather than names. I find column names to be far easier to understand, though, and encourage you to do the same.

#### IndexSlice

A more general way to retrieve from a multi-index is known as IndexSlice. Remember when I mentioned earlier that we cannot use : inside of round parentheses, and thus need to say slice(None)? Well, **IndexSlice solves that problem: It uses square brackets, and can use slice syntax for any set of values.**

For example, I can say:

    from pandas import IndexSlice as idx

    df.loc[idx[1980:2020, :, 'Swimming':'Table tennis'], :]

The above code allows us to select a range of values for each of the levels of the multi-index. No longer do we need to call the slice function. Now we can use the standard Python : syntax for slicing within each level. The result of calling IndexSlice (or idx, as I aliased it here) is a tuple of Python slice objects:

**I cannot understand this:**

    (slice(1980, 2020, None),
     slice(None, None, None),
     slice('Swimming', 'Table tennis', None))

In other words, IndexSlice is syntactic sugar, allowing pandas to look and feel more like a standard Python data structure, even when the index is far more complex.

### Great tip: 

When you cannot get what you want with playing with loc, xs, and idx is a good sign of a need to consider changing the existing indexes and maybe setting new index using **set_index** and **reset_index** methods.

### Pivot tables

So far, we have seen how to use indexes to restructure our data, making it easier to retrieve different slices of the information that it contains, and thus answer particular questions more easily. But the questions we have been asking have all had a single answer. In many cases, we want to apply a particular aggregate function to many different combinations of columns and rows. One of the most common and powerful ways to accomplish this is with a "pivot table."

A pivot table allows us to create a new table (data frame) from a subset of an existing data frame. Here’s the basic idea:

* Our data frame contains **two columns that have categorical, repeating, non-hierarchical data**.
* Our data frame has a third column that is numeric.
* We then create a new data frame from those three columns, as follows:

    * All of the unique values from the first categorical column become the **index, or row labels.**
    * All of the unique values from the second categorical column become the **column labels.**
    * Wherever the two categories match up, we get either **the single value where those two intersect, or the mean of all values where they intersect.**
    
It takes a while to understand how a pivot table works. But once you get it, it’s hard to un-see: You start to find uses for it everywhere.

For example, remember the table we created earlier? I’m going to re-create it here (i reran the code in the cells below):

    np.random.seed(0)
    df = pd.DataFrame(np.random.randint(0, 100, [36, 3]),
                  columns=list('ABC'))
    df['year'] = [2018] * 12 + [2019] * 12 + [2020] * 12
    df['month'] = 'Jan Feb Mar Apr May Jun Jul Aug Sep Oct Nov Dec'.split() * 3
    df

This table shows the sales of each product per year and month. And you can certainly understand the data, if you look at it in a certain way. But what if we were interested in seeing sales figures for product A? It might make more sense, and be easier to parse, if we were to use the months (a categorical, repeating value) as the rows, the years (again, a categorical, repeating value) as the columns, and then the figures for product A as the values. We can create such a pivot table as follows:

    df.pivot_table(index='month', columns='year', values='A')

You might notice that the months in our resulting table are sorted in alphabetical order, which is unlikely to be the most useful way to present htem. We can fix this by telling the pivot_table method not to sort the rows, passing **sort=False** in our method call:

    df.pivot_table(index='month', columns='year', values='A', sort=False)

What if more than one row has the same values for year and month? **By default, pivot_table will then run the mean aggregation method on all of the values**. But if you prefer to use a different aggregation function, you can pass it as an argument to **aggfunc** in your call to pivot_table. For example, you can count the values in each intersection box by passing the np.size function:

    df.pivot_table(index='month', columns='year', values='A', sort=False, aggfunc=np.size)

Remember that a pivot table will have one row for each unique value in your first chosen column, and a column for each unique value in your second chosen column. If there are hundreds of unique values in either (or even worse, in both), then you could end up with a gargantuan (enormous) pivot table. This will not only be hard to understand and analyze, but will also consume large amounts of memory. Moreover, if your data isn’t very lean (see Chapter 5), then you might well find all sorts of junk values in your pivot table’s index and columns.

The pivot tables are constructed based on **actual columns, and not the index**, and so when reading csv file to crfaete a df we’ll stick with the default, numeric index that pandas assigns to every data frame and I don't need to set index.

**isin**

we can use the isin method, which allows us to pass a list of possibilities, and get a True value whenever the 'Team' column is equal to one of those possible strings. In my experience, the isin method is one of those things that seems so obvious when you start to use it, but that is far from obvious until you know to look for it.

I can thus keep only those countries in this way:

    df = df[df['Team'].isin(['Great Britain', 'France', 'United States', 'Switzerland', 'China', 'India'])]

**tips to crfeate a pivot table**

With our data frame in place, we can now start to create some pivot tables, to examine our data from a new perspective. For example, here I was first asked to compare the average age of players for each team (across all sports) all years. As usual, when we’re creating pivot tables, we need to consider **what will be the rows, the columns, and the values**:

* The rows (index) will be the unique values from the Year column
* The columns will be the unique values from the Team column
* The values themselves will be from the Age column

Sure enough, we can then create our pivot table as follows:

    pd.pivot_table(df, index='Year', columns='Team', values='Age')

<a name="5"></a>
## 5. Cleaning data

I’ve often heard data scientists say that **80 percent of their job involves cleaning data**. What does it mean to "clean data"? Here is a partial list:

    * rename columns
    * rename the index
    * remove irrelevant columns
    * split one column into two
    * combine two or more columns into one
    * remove non-data rows
    * remove repeated rows
    * remove rows with missing data (aka NaN)
    * replace NaN data with a single value
    * replace NaN data via interpolation
    * standardize strings
    * fix typos in strings
    * remove whitespace from strings
    * correct the types used for columns
    * identify and remove outliers

We have already discussed some of these techniques in previous chapters. But the importance of cleaning your data, and thus ensuring that your analysis is as accuraet is possible, cannot be overstated.

### HOW MUCH IS MISSING?

We’ve already seen, on a number of occasions, that data frames (and series) can contain NaN values. One question we often want to answer is: How many NaN values are there in a given column? Or, for that matter, in a data frame?

**isnull**

If you call isnull on a column, it returns a boolean series, one that has True where there is a NaN value, and False in other places. **You can then apply the sum method to the series, which will return the number of True values**, thanks to the fact that Python’s boolean values inherit from integers, and can be in place of 1 (True) and 0 (False) if you need:

    s.isnull().sum()


If you run isnull on a data frame, then you will get a new data frame back, with True and False values indicating whether there is a null value in that particular row-column combination. And of course, then **you can run sum on the resulting data frame, finding how many NaN values there are in each column**:

    df.isnull().sum()

    any and all methods

Instead of summing the results of a call to isnull, you can also use the any and all methods, both of which return boolean values. any will return True for each row in which at least one of the values is True, and all will return True for each row in which all of the values are True. You can thus do the following:

    df[df.isnull().all()]

**info**

Finally, the df.info method returns a wealth of information about the data frame on which it’s run, including

* the name and type of each column, 
* a summary of how many columns there are of each type, 
* and the estimated memory usage. (We’ll talk more about this memory usage in Chapter 11.) 
* If the data frame is small enough, then it’ll also show you how many null values there are in each column. However, this calculation can take some time. Thus, the df.info will only count null values below a certain threshold. If you’re above that threshold (the **pd.options.display.max_info_columns** option), then you’ll need to tell pandas explicitly to count, by passing **show_counts=True**:


    df.info(show_counts = True)

**isnull and isna**

pandas defines both isna and isnull for **both series and data frames**. What’s the difference between them? **Actually, there is no difference**. If you look at the pandas documentation, you’ll find that they’re identical except for the name of the method being called. In this book, I’ll use isnull, but if you prefer to go with isna, then be my guest.

Note that both of these are different from np.isnan, a method defined in NumPy, on top of which pandas is defined. I try to stick with the methods that pandas defines, which integrate better into the rest of the system, in my experience.

Rather than using **~**, which **pandas uses to invert boolean series and data frames**, you can often use the **notnull** methods, for both series and data frame.

    df.dropna()

Normally, as we just saw, dropna removes **any row that contains any NaN value**. But we can tell it to look only in a subset of the columns, ignoring NaN values in any other columns. The result is a much cleaner query:

    semi_good_df = df.dropna(subset=['Plate ID', 'Registration State',
                                     'Vehicle Make', 'Street Name'])
                                 
note from chatGPT: The "dropna" method is used with the "subset" parameter set to a list of these column names, so the rows with NaN values in **any of these columns will be dropped**. The resulting "semi_good_df" DataFrame will only contain the rows where the values in all of these columns are not NaN.     

    df.replace

Replace values in one or more columns with other values like

    no_blankplate_df = df.replace({'Plate ID':'BLANKPLATE'}, NaN)

through the above code the Plate ID of the cars (in the column named 'Plate ID') with BLANKPLATE as a Plate ID were truned into NaN values. 

    df_tit['home.dest'] = df_tit['home.dest'].replace(most_common_destinations) note: most_common_destinations is a pd.Series

the above code replaces the values of the column named 'home.dest' in the df_tit dataframe: the values equivalent to index of the most_common_destinations series will be replaced with the corresponding values from the series's values.

    df['Vehicle Color'] = df['Vehicle Color'].replace(colormap) note: colormap is a python dictionary 

The call to replace returns a new series in which any value in df['Vehicle Color'] that matches a key in colormap is changed to be the corresponding value in colormap. 

### shape vs. count

If we want to see how many rows do we have in our df, we may use count or shape:

**count ignores any NaN values**—so especially if we know that the data has missing values, we’ll get an unhelpful answer from count. Moreover, count will return a separate value for each column in the data frame. This can be useful if we want to compare the number of non-NaN values in each column. But if all we want to do is know the number of rows, regardless of content, retrieving shape is the way to go.

Also note that **shape is an attribute that returns a tuple, not a method, so it doesn’t need parentheses after the word shape.**

side note:

Python’s f-strings have a special format code that, when put a **comma after : on an integer**, puts commas before every three digits:

**print(f"NYC loses {(df_ticket.shape[0] - df_ticket_new.shape[0]) * 100:,} $")**

### what to do with NaN

when we have NaN values, we have a few options:

* remove them
* leave them
* replace them with something else

What is the right choice? The answer, of course, is "it depends." **If you’re getting your data ready to feed into a machine-learning model, then you’ll likely need to get rid of the NaN values, either by removing those rows or by replacing them with something else.** If you’re calculating basic sales information, then you might be OK with null values, since they aren’t going to affect your numbers too much. And of course, there are many variations on these.

If you choose option 3, namely "replace them with something else," then that raises another question: What do you want to replace the NaN values with? A value that you have chosen? Something calculated from the data frame itself? Something calculated on a per-column basis? Any and all of these are appropriate under different circumstances.

Deciding what we should do with each NaN-containing column depends on a variety of factors, i**ncluding the type of data that the column contains. Another factor is just how many rows have null values**. In two cases—fare and embarked we have one and two null rows, respectively. Given that our data frame has more than 1,300 rows, missing 1 or 2 of them won’t make any significant difference. I thus suggest that we remove those rows from the data frame.

When it comes to the age column, though, we might want to consider our steps carefully. I’m inclined to use the mean here. But you could use the mode. You could also use a more sophisticated technique, using the mean from within a particular cabin. You could even try to get the complete set of ages on the Titanic, and choose from a random distribution built from that.

**Using the mean age has some advantages: It won’t affect the mean age, although it will reduce the standard deviation (can be proved by std formula). It’s not necessarily wrong, even though we know that it’s not totally right, either**. In another context, such as sales of a particular product in an online store, replacing missing values with the mean can sometimes work, especially if you have similar products with a similar sales history.

**some insights on wise replacement:**

I want to set the home.dest column similarly to what I did with the age column—but instead of using the mean, I’ll use the mode (i.e., the most common value). I’ll do this for two reasons: First, because you can only calculate the mean from a numeric value, and the destination is a categorical/textual value. Secondly, because this means that given no other information, we might be able to assume that a passenger is going where most others are going. We might be wrong, but this is the least wrong choice that we can make. We could, of course, be a bit more sophisticated than this, choosing the mode of home.dest for all passengers who embarked at the same place, but we’ll ignore that for now.


### Combining and splitting columns

One common aspect of data cleaning involves creating one new column from several existing columns, as well as the reverse—creating multiple columns from a single existing column.

Perhaps even more frequently, though, cleaning data involves taking one complex column, and turning it into one or more simpler columns. For example, you can imagine taking a column with a float64 dtype, and turning it into two int64 columns, one with the integer portion and one with the floating-point portion.

**str accessor**:

If s isn’t a single string, but rather a series that contains strings and if we want to retrieve the slice 3:5 from each of those strings, then we can use the str accessor on the series, followed by the slice method. The syntax is a bit different than what we used with Python strings, but it should still feel somewhat familiar:

**s.str.slice(3,5)**

The result of the above code is a new series of string objects, of the same length as s, containing two-element strings taken from indexes 3 and 4 of each row in s.

It’s common to slice and dice the columns of a data frame in this way, retrieving only those parts that are of interest to us. This not only makes the problem easier to see, understand, and solve, but it also allows us to remove the original (larger) column, saving memory and improving computation speed.


point:

if some of the values of a dataframe's column contain characters other than digits or some of the values are NaN, **which as floating-point values, cannot be coerced into integers**, i cannot change the dtype of that column through the following code:

df['age'] = df['age'].astype(np.int64)

So here first i need to take care of the above two issues (using isdigit, isnull, dropna etc.) then run the above code.

note on df = df.dropna(subset=['age']): 

Notice that I’m once again using the subset parameter. Not that there are any rows in the index with NaN values, **but it’s always a good idea to be specific, just in case.**

### Inconsistent data

Missing data is a common issue that you’ll need to deal with when importing data sets. But equally common is inconsistent data, when the same value is represented by a number of different values.

Thus, a big part of cleaning real-world data involves making it more consistent—or to use a term from the world of databases, **"normalizing"** it.

### Summary

Cleaning data is one of the most important parts of data analysis, although it’s not very glamorous. In this chapter, we saw that effective cleaning of data **requires not just knowing the techniques, but also applying judgment**—knowing when you can allow null or duplicate values, and then what you should do with them. pandas comes with a wide variety of tools that we can use in cleaning our data, from removing NaN values to replacing them, to replacing existing values, to running custom functions on each row in a series or data frame. The techniques that we explored in this chapter, along with the interpolate method that we saw before, are important tools in your data-cleaning toolbox, and will likely come up in many of the projects you work on.


<a name="6"></a>
## 6. Grouping, joining, and sorting

We often need to break our input data apart, to zoom in on particularly interesting subsets, to combine data from different sources, to transform the data into a new format or value, and then to sort it according to a variety of criteria. This combination of techniques is collectively known in the pandas world as **"split-apply-combine."** It’s common to use one or more of these when analyzing data. In this chapter, we’ll explore these techniques.

-- Much like the GROUP BY clause in an SQL query, we can use **grouping in panda**

-- Another common technique, also encountered when working with SQL databases, is that of **"joining."**

-- A third technique, one which you have likely seen in other languages and frameworks, is that of **sorting**. In Chapter 5, we already saw how to use **sort_index** to order a data frame’s rows by the values in the index. In this chapter, we’ll look at **sort_values**, which **reorders the rows based on the values in one or more columns.**

**some useful methods**:

    df.transpose() or df.T: Returns a new data frame with the same values as df, but with the columns and index exchanged

The transpose method is invoked like any other method in pandas, using parentheses:

    df.transpose()

HOWEVER, its convenient alias, **T, is not a method, and thus should not be invoked with parentheses:**

    df.T

    df.pct_change: For a given data frame, indicates the percentage difference between each cell and the corresponding cell in the previous row.

    df.groupby: Allows us to invoke one or more aggregate methods for each value in a particular column.

    df.join: Join two data frames together based on their **indexes**

    df.merge: Join two data frames together based on **any columns**

    df.corr: Show the correlation between the numeric columns of a data frame

A score of 1 indicates that it’s 100% positively correlated, meaning that when one column goes up, the other column goes up by the same degree. A score of -1 indicates that it’s 100% negatively correlated, meaning that when one column goes up, the other goes down by the same degree. A score of 0 indicates that there is no correlation at all. Generally speaking, we say that the closer to 1 (or -1) the score, the more highly correlated two columns will be. By default, corr uses what’s known as the "Pearson correlation," about which you can read more here: https://en.wikipedia.org/wiki/Pearson_correlation_coefficient

### sorting

Data in relational (SQL) databases isn’t stored in any particular order. There are several reasons for this:

* The order in which the rows are stored doesn’t affect many queries,
* It’s more efficient for the database itself to figure out the order in which rows should be stored, and
* There are so many ways in which we might want to sort the data that the database shouldn’t guess. Rather, it should allow us to choose how we want to sort and extract the information.

Now pandas does keep the rows of our data frame ordered, so **it’s not exactly like a relational database**. But it’s true that for many types of analysis, the order of the rows doesn’t matter. After all, if you’re calculating a column’s mean, then it doesn’t matter where you start or end.

But if you want to display data—say, sales records, network statistics, or inflation projections—then you’ll likely want to order them. But how you want to order them depends on the context. Sales records might need to be ordered by department, network statistics might need to be ordered by subnets, and inflation projects might need to be ordered chronologically.

Another reason to sort is to get the highest or lowest values from a particular column in the data frame.

**point on sort_index vs. sort_values:**

You could argue that there isn’t really much difference between the two; we could take a column, temporarily make it the index, sort by the index, and then return the column back to the data frame. But the difference between sort_index and sort_values isn’t just technical. We’re thinking about our data, and how we want to access it, in different ways.

sort_values is also different from sort_index in another way, namely that we can sort **by any number of columns**. Imagine, once again, that your data frame contains sales data. You might want to sort it by price, by region, or by salesperson—or even by a combination of these. When we sort by the index, by contrast, we’re effectively sorting by a single column.

**point on using iloc vs loc on the sorted dataframe**

    df.sort_values('trip_distance', ascending=False)['total_amount'].iloc[:20]:

Notice that we have to use iloc here, and not loc. That’s because **loc works with the actual index values**—which, now that we’ve sorted the data frame by trip_distance, will be unordered. Asking for loc[:20] will return many more than 20 rows.

So we need to use iloc or head/tail to retrieve the first/last 20 rows, because the index was all scrambled (means disordered) after our **sort operation**. But you can pass **ignore_index=True to sort_values**, and then the resulting data frame will have a numeric index, starting at 0.

### grouping

Sometimes we want to run an aggregate function on each piece of our data. 

This functionality, known as "grouping," should also be familiar to you if you’ve worked with relational databases. 

    df.groupby('department')
    
Notice that the argument to groupby needs to be the **name of a column**. And the result of running the groupby method is a **DataFrameGroupBy object**, which is useful to us because of the aggregate methods we can invoke on it. For example, I can call count, and thus find out how many items we have in each department:

    df.groupby('department')['product_id'].count()

While I’ve used count in my examples here, you can use **any aggregation method when grouping, such as mean, std, min, max, and sum**. So we could get the average product price, per department, in our store as follows:

    df.groupby('department')['retail price'].mean()

What if we want to know both the mean and the standard deviation of prices in our store, grouped by department? You can actually do that, by altering the syntax somewhat: Instead of calling an aggregation method directly, we can **apply the agg method to our DataFrameGroupBy object. That method then takes a list of methods, each of which will be applied to**

    df.groupby('department')['retail price'].agg([np.mean, np.std])

What if we want to run mulitple aggregations on separate columns? 

In such a case, we don’t need to filter columns via square brackets. Rather, we can pass the entire DataFrameGroupBy object to agg. We then pass multiple keyword arguments to agg:

* The key to each keyword argument will be the name of an output column
* The value to each keyword argument is a two-element tuple:

    * The first element in the tuple is a string, the name of the column in the original data frame we want to analyze
    * The second element in the tuple is also a string, the name (yes, as a string) of an aggregation method we wish to run on that column.

For example, we can get the mean and standard deviation of retail_price per department, as well as find the max sales for each department:

    df.groupby('department').agg(mean_price=('retail_price', 'mean'),
                                 std_price=('retail_price', 'std'),
                                max_sales=('sales', 'max'))


side note: 
Normally, groupby sorts the group keys. If you don’t want to see this, or if you are concerned that it’s making your query too slow, you can pass sort=False to groupby:

    df.groupby('department', sort=False)['retail_price'].agg([np.mean, np.std])


### When we’re using groupby, we have to keep several things in mind:

* On what data frame are we operating?
* Which column will supply the groups? This column will almost always be categorical in nature, either with a limited number of string values or with a limited set of integers (as is the case here). The distinct values from this column will be the rows in the output from our aggregation method.
* Which column(s) do we want to analyze? That is, on which columns will we run our aggregation methods?
* Finally, which aggregation method(s) will we be running?


### joining 

Like grouping, joining is a concept that you might have encountered previously, when working with relational databases. The joining functionality in pandas is quite similar to that sort of database, although the syntax is quite different.

how does pandas know which rows on the left should be joined with which rows on the right? The answer, at least by default, is that it uses the **index**. Wherever the index of the left side matches the index of the right side, it’ll join them together, giving them a new row that contains all columns from both left and right.

This means that we’ll want to change our data frames, such that both are using the same values for their indexes. So I need to set_index of the two dataframes to the common column in both then now that our data frames have a **common reference point in the index**, we can create a new data frame combining the two:

    products_df.join(sales_df)

note: both products_df and sales_df dataframes have a column named 'product_id':

    * products_df = products_df.set_index('product_id') 

    * sales_df = sales_df.set_index('product_id')

another example:

    products_df.join(sales_df).groupby(['date','name'])[
            'retail_price'].sum().sort_index()
        
**point on left vs. right join**

When we are performing a left join it means that the left side (i.e., the data frame on  which we're running the join, df_tour in the below example) **determines which rows will be included**. If there is no match on the right, then we get back a null value.

    df_tour.join(df_coun_name)

**interesting point**:

we can run join on a df whose index is not the same as the other df, in this case the caller df (the one on the left) should have a column which should be matched with the index of the other df, in the example below the index of df_coun_name is numeric and not the same as the index of df_tour but df_coun_name has a column named 'abbr' which can be used to join with the other df whose index is matched with 'abbr' column of the caller df, df_coun_name I mean. However the point is that the data frame passed as an argument to join will always be joined on **its index**.

    df_coun_name.join(df_tour, on = 'abbr')

point from reuven:
I always like to create **separate data frames, and then join them together.** This not only lets me do things step by step, but also ensures that I can debug, improve, and rerun my steps more easily.


### normalization

Separating your data into two or more pieces, so that **each piece of information appears only a single time, is known as "normalization."** There are all sorts of formal theories and descriptions of normalization, but it all boils down to **keeping the information in separate places**, and joining data frames when necessary.

Sometimes, you’ll normalize your own data. But sometimes, you’ll receive data that has been normalized, and then separated into separate pices. For example, many data sets are distributed in separate CSV file, which almost always means that you’ll need to join two or more data frames together in order to analyze the information. Other times, you might want to normalize the data yourself, in order to gain flexibility or performance.

One final point: The join that I’ve shown you here is known as a **"left join"** in that values of product_id on the left (i.e., in products_df) drive which rows will be selected on the right (i.e., sales_df). More advanced joins, known as **"outer joins"** allow us to tell pandas that even if there isn’t a corresponding row on the left or the right, we will want to have a row in the result, albeit (meaning although) one filled with null values. We’ll explore those in Exercise 35, at the end of this chapter.


**.to_frame() method**

You can only invoke join on a **data frame** and **NOT on a series**. (You can pass a series as the argument to join, though—so a series can be **the right side, but not the left side, of a pandas join.)**

Fortunately, we can call the to_frame method on our series, and get back a single-column data frame with the same index as we had in the series:

    country_points.to_frame().join(tourism_spending)

Once again, it’s important to remember that a join links the left data frame with the right one, connecting them along their indexes. 

**What happens if the left and right data frames have identically named columns?**

After all, while pandas indexes don’t need to have unique elements, column names must be unique. If you try to join frames such that you’ll end up with more than one column with the same name, you’ll get a ValueError exception, saying, "columns overlap but no suffix specified." And indeed, pandas allows you to specify what the suffixes should be for the left side **(lsuffix)** and right side **(rsuffix)** when you invoke join. For example, we can join oecd_df with itself (already a wild idea known as a "self join," for which there are actually practical uses) with

    oecd_df.join(oecd_df, lsuffix='_l', rsuffix='_r')

The data frame we get back has two identical columns, named country_l and country_r (country is the name of the column in oecd_df).


We may have NaN values in some rows after left join. That’s because the **index of left data frame** (in this case, country_points.to_frame()) **dictates the index of the resulting data frame. As a result, this is knowon as a "left join." In a left join, columns from the right frame will be missing values (and thus have NaN) wherever there was no corresponding row for the left’s index.**

**right join**

If we want to use the right data frame’s index in the result, then we can use a "right join." You can accomplish that in pandas by passing **how='right' to the join method. (By default, the method assumes how='left'.)** In such a case, you’ll get NaN values on columns from the left frame wherever it has no index entry corresponding to the right.

**outer join**

We can also be fancy, and do an "outer join," in which case the **output frame’s index is the combination of the left’s index and the right’s index**. You might thus end up with NaN values in columns from both the left and right, depending on which index value was missing. 

    country_points.to_frame().join(tourism_spending, how='outer')

Outer joins ensure that you don’t lose any data when combining data sources, but they don’t automatically interpolate values, either—so you will almost certainly end up with some null values, which (as we’ve seen in Chapter 5) need cleaning in various ways.

**inner join**

The "inner" join assures that the index in the final df will only contain values that exist in all of the data frames to be joined. I can do this by passing the keyword argument **how='inner'** to call to join:

df_oil.join(df_ice_cream, how = 'inner').join(df_mile_traveled, how = 'inner')


#### Great point on merge vs. join:

My understanding: i think merge works with column names and not index so the difference between merge and join is that
join works with index but merge works with column names:

**chatGPT comment on my above understanding;**

Yes, that's correct! The merge method in pandas works by merging two DataFrames based on the values in specified columns, which can be different from the index. The join method, on the other hand, works by merging two DataFrames based on their indexes.

**How is the default merge different from the default join, when it comes to NaN values?**

By default, join performs a "left join," meaning that there might be NaN values in the column(s) from the right side. However, merge performs an "inner join" by default, meaning that it uses the intersection of the indexes from the right and left. As a result, NaN values won't occur thanks to the join (but they might come in thanks to NaN values in the input frames.

### Window functions

let's say i have:

    df = DataFrame({'sales':[100, 150, 200, 250,
                    200, 150, 300, 400,
                    500, 100, 300, 200],
                  'quarters':'Q1 Q2 Q3 Q4'.split() * 3})
              
If we want to find out how much we sold, total, through the current quarter? That is, I want to know how much we sold in Q1. Then in Q1+Q2. Then Q1+Q2+Q3. And so on, until the final result will be df['sales'].sum().

pandas provides us with "window functions." There are several different types of window functions, but the basic idea is that they allow us to **run an aggregate function, such as mean, on subsections of our data frame.**

### 1. expanding window function: 

What I described earlier, that we would like to know, for each quarter, how much we revenue we had through that quarter, is a classic example of a window function. This is known as an **"expanding window,**" because we run the function with an ever-expanding number of lines—first one line, then two, then three… all the way up to the entire data frame.

    df['sales'].expanding().sum()

Perhaps we don’t want to get a cumulative total, but rather want to get a **running average** of how much we’ve sold per quarter. We can run mean, or any other aggregation method:

    df['sales'].expanding().mean()

In this case, the output from expanding will be 100, 125, 150, and 175.


### 2. rolling window function:

With rolling window functions we determine **how many rows** will be considered to be part of the window. For example, if the window size is 3, then we’ll run the aggregation function on row index 0-2, then 1-3, then 2-4, etc., until we get to the end of the data frame. For example, if you want to find out the mean of rows that are close to one another, you can do it as follows:

    df['sales'].rolling(3).mean()

In the above code, rolling is how I indicate that I want to run a rolling window function, and the argument 3 indicates that I want to have three rows in each window. We’ll thus invoke mean on rows 0-2, then 1-3, then 2-4, then 3-5, etc. T**he series that we get back from this call will put the result of mean in the same location as the third (and final) row in our rolling window. This means that row indexes 0 and 1 will have NaN values.**

### 3. pct_change window function:

A third type of window function is pct_change. When we run this on a series, we get back a new series, with NaN at row index 0. The remaining rows indicate the percentage change from the previous row to the current one:

    df['sales'].pct_change()

The result is calculated as **(later_row - earlier_row) / earlier_row**:

    * index 0 is always NaN
 
pct_change is great for finding **how much your values have gone up, or down, from row to row.**

### Filtering and transforming

#### filter method

* filter takes a function as an argument.

* The function we pass will be invoked **once per group**. It receives a data frame—a subset of df—as its argument.

* The function must return True or False. to indicate whether rows from that group should be included or excluded in the resulting data frame.

* The function can either be a full-fledged Python function (i.e., one defined with def), or it can be lambda for an inline, anonymous function.

Here’s an example of such a function, as well as how I could invoke it:

    def year_average_is_at_least_90(df):
        return df['score'].mean() > 90

    df.groupby('year').filter(year_average_is_at_least_90)

OR

    df.groupby('year').filter(lambda df: df['score'].mean() > 90)

**Whereas filter on a data frame works on a row-by-row basis, filter on a GroupBy object works on a group-by-group basis.**

#### transform method

Another, related method that you can use on a **GroupBy object** is transform. In this case, the point is not to remove rows from the original data frame, but rather to transform them in some way. For example, let’s say that we want to turn the score into a percentage, expressed as a float. We could say:
 
    df.groupby('year')['score'].transform(lambda x: x/100)

another example: 
In the resulting series of **df.groupby('year')['score'].transform(np.max)**, the value in each row is the highest value of score from that particular year. In other words, we have replaced every score by the maximum score for that year. 

**As you can see, the grouped version of transform is useful when we want to transform values in a data frame on a group-by-group basis, much as the grouped version of filter is useful when we want to filter values on a group-by-group basis.**

The filter method for GroupBy is very similar to Python’s builtin filter function, and that the transform method for GroupBy is very similar to Python’s builtin map function. They work a bit differently, since they’re acting on data frames rather than simple iterables, but the usage is similar.

**encouraging point:**

The DataFrameGroupBy versions of filter and transform are, in my experience, among the most complex pieces of functionality that pandas provides. It might take you a while to think through what calculation you want to perform, and then to find the right way to express it in pandas.

<a name="7"></a>
## 7. Strings

It turns out that pandas is also well equipped to handle text. It does this not by storing string data in NumPy, but rather by using full-fledged string objects, either those that come with Python or (more recently) a pandas-specific string class that reduces both ambiguity and errors. (I’ll have more to say about these two string types, and when to use each one, later in the chapter.) In either case, we have access to a wide variety of methods that we can apply to these strings, normally via the **str accessor.**

In this chapter, you’ll work through exercises that will help you to identify and understand how to work with textual data and the str accessor in pandas. After going through these exercises, you’ll know which string methods are available, feel more comfortable using them, and **even know how to apply your own custom functions to string columns.**


### str accessor

Traditional Python strings support a large number of methods and operators, ranging from search (str.index) to replacement (str.replace) to substrings (slices) to checks of the string’s content (e.g., str.isdigit and str.isspace). But if we have a series containing strings, how can we invoke such a method on every element?

Experienced Python developers would normally expect to use a for loop, or perhaps a list comprehension. But in pandas, we’ll do whatever we can to avoid such loops. We do have the option of the **apply** method, which we can use to apply a **string method to every element**. And indeed, **apply is needed if you want to use a custom function, rather than a method that comes with pandas.**

pandas encourages us to use the **str accessor, which gives us access to a variety of string methods—including, but not limited to, standard Python string methods.** The method will be applied to **every element in the series**, and **will return a new series of the same length and with the same index, whose values are the results of invoking the method on each element.**

The str accessor supports methods beyond those available in Python’s str class. For example, you can search in a string using **contains**. However, contains allows you to use a regular expression. I can thus find all of the words with either a or e:

    s.str.contains('[ae]')

Note that while str.contains currently (as of this writing) defaults to treating its argument as a regular expression, there are plans for that default value to change. It’s thus a good idea to be explicit about your intentions by passing regex=True, so that the string isn’t taken literally:

    s[s.str.contains('[ae]', regex=True)]


**read() method**

The read method returns a string containing the contents of the file:

    s = pd.Series(open(filename).read().split())

What if the file contains several terabytes of data? Then unless the IT department at your company is unusually generous, you’ll find yourself running out of memory. Normally, I suggest that people **not read an entire file into memory at once, instead iterating over its lines**. 

**split method**

    s.str.split(): 

In this case, we did not specify a delimiter, which means that split will use any **whitespace—space, tab, newline, carriage return, and vertical tab**—to break them apart. 

**isin method**

isin method allows us to perform an "or" search—one that we could certainly do with pandas boolean operators and a mask index, but which becomes shorter and more readable with isin.

**good point**

Because pandas index objects are similar to series, I could run the isin method on them like: 

top_ten_used = df_so['LanguageHaveWorkedWith'].str.split(';').explode().value_counts().head(10).index

top_ten_want_to_use = df_so['LanguageWantToWorkWith'].str.split(';').explode().value_counts().head(10).index

top_ten_used[top_ten_used.isin(top_ten_want_to_use)]

**intersection method**

But it turns out that pandas makes it easy to do this, with the "intersection" method. Note that this method works on **index objects, and not on series**:

top_ten_want_to_use.intersection(top_ten_used)

**explode method**

s.explode() returns a new series with each element on its **own line** meaning in its row

**cool point on explode method**

the series that we get back from explode has the **same index as the original series on which it was run.** Meaning that if the original column had an index of 0 and mentioned both Python and JavaScript in one row like seperated with ; but in one row, then the resulting series will have two rows, both with an index of 0, one with Python and the other with JavaScript.

This means that while we cannot assign the exploded series as a column, we can use join to merge the series onto the data frame.

**get_dummies method** 

It allows us creating "dummy **columns**." Instead of a column containing the string 'JavaScript;Python', we’ll create one column called JavaScript and another called Python, putting 1s where the person marked themselves as using JavaScript, and 0s where they indicated they did not.

I can create a new data frame of dummy values based on LanguageHaveWorkedWith using the str.get_dummies method:

df['LanguageHaveWorkedWith'].str.get_dummies(sep=';')

* i belive it is kind of like one hot encoding in ML. chatGPT thoughts on my understanding:  Yes, that's correct. get_dummies is a method in the pandas library in Python that is used to perform one-hot encoding. One-hot encoding is a technique used to represent categorical variables as numerical data, so that they can be used as input to machine learning algorithms.

<a name="8"></a>
## 8. Dates and times

Both the Python language and pandas handle time data with two different data structures: A **"timestamp" data type (also known as a "datetime"** in many languages and systems) handles **specific points in time, one that you can point to using a calendar**. A timestamp happens once, and only once—when you were born, when your plane will be taking off, when you and your date will meet at a restaurant, or when the meeting was scheduled to end. You can describe a timestamp with a particular year, month, day, hour, minute, and second.

A second, complementary data type is that of the **"timedelta," known in some systems as an "interval."** A time delta represents the distance between two timestamp objects. So the meeting’s scheduled start and end can be represented as timestamps, but the time that the meeting takes is a timedelta.

It’s good to know that pandas can handle dates and times quite flexibly. We can read data in from files, turning columns into timestamps. We can also convert existing values—both individual values and series objects—into timestamps. We can perform calculations with timedeltas, and perform comparisons with them. But pandas goes further than that, allowing us to use date and time information in our **indexes.** This makes it even easier to **search** for data that took place during specific periods. Even better, we can perform **"resampling,"** which is most easily described as **"grouping by time periods."**

some usefule methods:

**pd.to_datetime**: If passed a series of strings, returns a series of Timestamp objects

**pd.to_timedelta**: If passed a series of strings, returns a series of Timedelta objects

**df.to_csv** : Writes a CSV file based on a data frame

**df.resample**: Performs a **time-based** groupby operation, on a specified period of time

**s.diff**: s.diff() returns a new series with the same index as s, but whose values indicate the difference between that value and the previous value

### Creating DATETIME and TIMEDELTA objects

#### datetime

As we’ve repeatedly seen, pandas largely avoids built-in Python data structures in favor of its own types, or those defined by NumPy. This is also the case when it comes to dates and times: To represent a specific point in time, we use the **Timestamp class**, instead of either the datetime.datetime class that comes with Python or the np.datetime64 class that comes with NumPy.

The standard way to create Timestamp objects is with the module-level function **to_datetime**, which takes a variety of argument types. If passed a single argument, it returns one Timestamp. For example, we can get the current date and time by passing the string ’now’:

    pd.to_datetime('now')

But it's far more common, and useful, for us to call **pd.to_datetime** on an existing series of strings containing date-and-time information. For example:

    s = pd.Series(['1970-07-14', '1972-03-01', '2000-12-16', '2002-12-17', '2005-10-31'])

    pd.to_datetime(s)

Don’t be confused by the indication that the dtype is datetime64, a type from NumPy; the values are all of type Timestamp, a pandas type.

In this example, the strings that we fed to to_datetime were pretty unambiguous, and easy to parse. But what if we have slightly different strings, using month names instead of numbers?

    s = pd.Series(['1970-Jul-14', '1972-Mar-01', '2000-Dec-16', '2002-Dec-17', '2005-Oct-31'])

    pd.to_datetime(s)

Actually, it’ll work just fine: That’s because pd.to_datetime is fairly smart and flexible, and can parse a number of different date formats. So the above format will work, as will this one:

    s = pd.Series(['14-Jul-1970', '01-Mar-1972', '16-Dec-2000', '17-Dec-2002', '31-Oct-2005'])

    pd.to_datetime(s)


But what if we pass dates that are a bit more ambiguous? For example, what if the months are all numbers:

    s = pd.Series(['14-07-1970', '01-03-1972', '16-12-2000', '17-12-2002', '31-10-2005'])

    pd.to_datetime(s)

Once again, it works just fine. However, there are times when it’s less obvious, and where human culture and tradition plays a role. For example, take the following:

    s = pd.Series(['01/03/1972', '05/12/1995'])

    pd.to_datetime(s)

Should pandas interpret these dates as the 1st of March, or the 3rd of January, and as the 5th of December, or the 12th of May? By default, ambiguous date formats are assumed to have **the month first** (i.e., dayfirst is defaulted to False). However, you can override that by passing dayfirst=True to pd.to_datetime:

    s = pd.Series(['01/03/1972', '05/12/1995'])

    pd.to_datetime(s, dayfirst = True)

All of the above examples have only included dates, but we could include time information, as well:

    s = pd.Series(['1970-07-14 8:00', '1972-03-01 10:00 pm', '2000-12-16 12:15:28', '2002-12-17 18:17', '2005-10-31 23:51'])

    pd.to_datetime(s)

Notice that I sometimes included seconds, and in one case indicated am/pm rather than using a 24-hour clock. pandas tries hard to understand all of these formats, and to interpret them as best as possible.

What if you have several series representing the year, month, and date? You can use pd.to_datetime to get a new Timestamp series based on those inputs. This is especially useful if you’re trying to create a Timestamp column from a data frame:

    df = pd.DataFrame([s.split('-') for s in ['14-07-1970', '01-03-1971', '16-12-2000', '17-12-2002', '31-10-2005']],
                      columns='day month year'.split())
                  
    pd.to_datetime(df[['year', 'month', 'day']])

**reading csv files**:

All of this is fine, but it ignores a common use case: We are **loading a CSV file,** and one or more columns in the file are datetime information. **How can we ensure that these columns are interpreted as Timestamp data, and not as strings? We need to tell pandas to do this, using the parse_dates** parameter in the read_csv function. We can pass a **list of columns**, either as names (strings) or as integers (indexes). For example:

    pd.read_csv(filename, parse_dates=['birthday', 'anniversary'])

We could, of course, have imported them as text (i.e., the default) and then run pd.to_datetime on them, but this makes the process a bit easier and cleaner. 

There are a variety of different parameters that you can pass to influence the parsing process. One of them is **dayfirst**, which works just as we saw above—to indicate that the dates being read start with days (as done in Europe) rather than with months (as done in the United States).

Generally speaking, we can pass values to the parse_dates keyword argument, indicating which columns should be passed to pd.to_datetime, and we don’t have to think about it any more. But in the real world, we’re often forced to deal with non-standard formats for date and time information. We might be asked to write data using a particular format, or (even more commonly) to read data that doesn’t conform to a standard that pandas recognizes.

Fortunately, we can **customize the ways in which datetime information is written to disk, as well as how it is parsed when we read it into pandas**.

We can use a custom date-parsing function to handle these odd formats. We can also tell pandas to write datetime columns in a format of our choosing, using the specifiers for **time.strptime.** 

**writing to csv file**:

In theory, we could create a new column based on one of the existing column, but with the format we want, and then export that new column to the CSV file. But it turns out that pandas is one step ahead of us here, allowing us to specify the format in which datetime columns will be written by passing a value to the **date_format parameter.**

The format is specified using **% signs**, using the format specifiers from time.strftime, and described here: <a href="https://docs.python.org/3/library/time.html#time.strftime">string format time</a>. Our output can contain any combination of hours, minutes, months, days, time zones, and other elements that we might like. The format that I described wanting in the output is a bit unusual, in that dates are specified as %d/%m/%Y, meaning two digits for the day, two digits for the month, and four digits for the year, followed by a space character, and then the time in 24-hour format, but with h after the hours, m after the minutes, and s after the seconds. We can specify that as follows:

    df.to_csv('july_2019.csv',

             sep='\t',

             columns=['tpep_pickup_datetime', 'passenger_count',

             'trip_distance', 'total_amount'],

             date_format='%d/%m/%Y %Hh:%Mm:%Ss')

In the above code, I wrote to the file named july_2019.csv, and specified (with the **sep keyword argument**) that we will use tabs to separate the fields. pandas uses the **date_format parameter** to indicate how **all datetime columns** (only tpep_pickup_datetime, in our case) should be written.

when reading our file back into pandas,given the odd format we used to write the data as above, we cannot expect that pandas will interpret the column correctly. For that reason, we’ll need to use a custom **date_parser**. Such a function should expect to get a single string value, and will be called once for each row in the incoming data frame. The function should return a time.time (or datetime.datetime, if you prefer) object, which pandas then puts into the appropriate column of the data frame it creates. In this case, I wrote the function as follows:

    import time

    dt_format = '%d/%m/%Y %Hh:%Mm:%Ss'

    def parse_weird_format(s):

        return time.strptime(s, dt_format)

Finally, we call df.read_csv, specifying not only the filename, separator, and columns, but also (**most importantly) the function we want to use to parse the dates**:

    df = pd.read_csv('july_2019.csv',

               sep='\t',

               usecols=['tpep_pickup_datetime', 'passenger_count', 'trip_distance', 'total_amount'],

               date_parser=parse_weird_format)
           
Notice that the value we pass to date_parser is a function; we’re not invoking parse_weird_format, but rather pd.read_csv is doing so on our behalf, once for each row in the incoming file.

Another alternative approach would be to use **lambda**, which creates an anonymous function. That would be particularly useful in this case, given that our custom date-parsing function is itself a one-liner, invoking time.stptime:

    df = pd.read_csv('july_2019.csv',

               sep='\t',

               usecols=['tpep_pickup_datetime', 'passenger_count', 'trip_distance', 'total_amount'],

               date_parser=lambda s: time.strptime(s, dt_format))
          
Once we have a Timestamp series, we can use the **dt accessor** to retrieve a variety of different parts of each object. For example:

    s.dt.month         # month number
    s.dt.month_name    # month name
    s.dt.hour          # hour
    s.dt.day_of_week   # day of week
    s.dt.is_leap_year  # is it a leap year?

Some of these attributes return numbers, whereas others return boolean values. The full list of attributes you can retrieve via the dt accessor starts at pandas.pydata.org/docs/reference/api/pandas.Series.dt.date.html.

#### timedelta

we can generally say:

datetime - datetime = interval

datetime + interval = datetime

datetime - interval = datetime

For example, if we have two timestamp series, subtracting one from the other gives us a timedelta series. For example:

    s = pd.Series(['1970-07-14 8:00', '1972-03-01 10:00 pm', '2000-12-16 12:15:28', '2002-12-17 18:17', '2005-10-31 23:51'])

    s = pd.to_datetime(s)

    pd.to_datetime('2021-July-01') - s

Here the subtraction operation is broadcast to every element in s, returning a series of timedelta64 objects.

**pd.to_timedelta**: 
 
If you want to create a timedelta object or series, you can also call **pd.to_timedelta**, much as you can call **pd.to_datetime**. The function’s argument is typically going to be a string or series of strings, each describing a time span, such as '1 hour' or '2 days'.

The pieces of a timedelta can be retrieved using the components attribute. For example:

    pd.to_timedelta('2 days 3:20:10').components

If you have a series of timedelta objects, you can retrieve each individual component, as well as the components attribute, via the **dt accessor, much as you can do with Timestamp objects**.

**time series**:

We have already seen how a data frame’s index can be an integer or string. But things get really exciting when you set a **timestamp column to be your index**. In the pandas world, we call that a **"time series."** And when you create a time series, you can take advantage of a number of useful pandas features.

**resampling**

Perhaps the most interesting and powerful feature of a **time series** is **"resampling."** Resampling is similar to a "groupby" query, except that instead of producing one result per value of a categorical column, we get **one result per chunk of time, starting at the earliest point in time and ending with the latest one**. For example, resampling allows us to retrieve the mean value for every day, or every two weeks, or every six months, or every year of a data frame. For example, I can find out how many missions there were in every six-month period covered by our data set:

    df.resample('6M').count()

If the data is **numeric, then you can run other aggregation methods, such as mean and std,** as well.

point: Finally, I asked you to find the mean price of oil for each year in our data set. This is most easily accomplished by using resample, which is a kind of groupby but for time series: It lets run an aggregation method (e.g., mean) for all of the values in a given time period. If the time period doesn’t exist in the data frame, or is cut off, it’ll still show up, to ensure that we have all of the periods from start to finish.

When we run resample, we tell it what time-period granularity we’ll want, giving a number and a letter representing the measurement. For example, we can run our aggregation method on a weekly basis with 1W, or on a bimonthly basis with 2M.

**is_quarter_end attribute**:

    df_wti[df_wti.index.is_quarter_end]
    
<a name="9"></a>
## 9. Visualization

The 900-pound gorilla in the world of Python data visualization is Matplotlib. There’s no doubt that Matplotlib is powerful—but it’s also overwhelming to many people. Fortunately, pandas provides a visualization API that allows us to create plots from our data without having to use Matplotlib explicitly. We thus get the best of both worlds—the ability to plot information in our data frame, without having to learn too much about Matplotlib’s API. However, if and when you need more power, Matplotlib is there, under the hood, available to anyone who wants to use it.

This chapter will also provide you with the opportunity to explore one of the nicest features of the Jupyter notebook, the fact that it **keps images inline**. Whether it’s on your own or by exploring the notebooks that I’ve prepared while writing this book, I strongly encourage you to experiment with Jupyter’s plotting capabilities. The ability to have data, code, and plots in the same document is a game changer for many projects, making it possible for data scientists to both share information and get input from less technical colleagues.

**good to knows**

**df.plot is entry to the plotting system** like df.plot.box(), df.plot.bar() etc.  

**stacked=True**  

s.quantile : Get the value at a particular percentage of the values like s.quantile(0.25)

pandas.plotting.scatter_matrix : Create scatter plots comparing every pair of numeric columns

### Box and Whisker plots 

One of the phrases I often use when teach data analytics is that you need to **"know your data."** And one of the best ways to know your data is with a **box plot.**

We frequently use the describe method to describe our data. The describe method includes the "Tukey five-number summary"—minimum, 0.25 quartile, median, 0.75 quartile, and maximum—along with the mean and standard deviation, which together help us to understand our data.

The "Tukey" in this name refers to John Tukey, a famous mathematician and statistician. It turns out that Tukey didn’t only develop the five-number summary, but also a **graphical depiction of that summary—the boxplot.** (He also invented the words "bit," for "binary digit," and "software," which … well, if you’re reading this book, then you probably know what software is.)

**A boxplot shows us this five-figure summary, but in graphical form:**

(And yes, I’ll have an image and label it!)

The central "box" in the boxplot has three parts:

    -- The top of the box indicates the 75% value (75% quantile)
    
    -- The middle line, **often highlighted in a different color**, indicates the median, the 50% value
    
    -- The bottom of the box indicates the 25% value (25% quantile)
    
Extending above and below the box are two lines, sometimes known as **"whiskers."** In a standard box plot, the **top whisker** typically extends from the upper quartile (75th percentile) to the largest value that is within 1.5 times the interquartile range (IQR). The **bottom whisker** extends from the lower quartile (25th percentile) to the smallest value that is within 1.5 times the IQR. Any data points that fall outside of the whiskers are considered outliers and are typically represented as individual points.

Thus, at a glance, we get a graphical depiction of the five-figure summary.

However, a boxplot will often have **circles above and below the whiskers**. These represent the **outliers**, defined in the case of our boxplots to be **1.5 * IQR below the first quartile (25% mark) or 1.5 * IQR above the third quartile (75% mark).**

Boxplots allow us, at a glance, to better understand our data. They can be especially useful when it comes to comparing data sets; **we can quickly see if they’re on the same scale**, and whether (and where) they overlap.

We can create boxplots in pandas using plot.box on a data frame. In the simplest case, we can say:

**df.plot.box()**

This will create a **separate plot for each of the columns in the data frame df (no this is not true!!!!). This can be particularly useful when creating machine-learning models, when having all of the data in the same range increases the model’s accuracy.**

Note that nowhere in the boxplot do we see the mean value. I personally think that’s a bit of a shame, because the mean can also be a useful measure, imperfect though it might be.

**My question for chatGPT:**

can i conclude about data being skewed or not through seeing a box plot? 

answer:

yes, you can get an idea about whether the data is skewed or not by looking at a box plot.

**If the box in the box plot is approximately symmetric, meaning the median is close to the center of the box, and the whiskers are roughly equal in length, then the data is likely to be approximately symmetric. In this case, the mean and median will be close to each other.**

However, if the box is elongated and the whisker on one side is longer than the other, then the data is likely to be skewed. If the longer whisker is on the right side, the data is said to be positively skewed, which means that there are more low values than high values. If the longer whisker is on the left side, the data is said to be negatively skewed, which means that there are more high values than low values.

In a skewed distribution, the median is usually a more representative measure of central tendency than the mean. So, if the box plot shows evidence of skewness, you should consider reporting the median instead of the mean.

Keep in mind that a box plot only gives a rough idea of the skewness of the data and may not provide a precise description of its shape. It's always a good idea to look at other measures of central tendency, such as the mean and the median, and to assess the shape of the distribution using a histogram or a density plot.

#### Correlation is NOT Causation. BUT what is it then? 

Loosely speaking, two measurements are correlated when movement in one is generally accompanied by movement in another. If the measurements rise and fall together, then they’re considered **"positively correlated."** If one goes up when the other goes down (and vice versa), then they’re said to be **negatively correlated.**

In addition to be positive or negative, correlation can be **weak or strong.** There’s probably a strong correlation between your annual income and the size of your house. There’s probably a weak correlation between your annual income and your shoe size. (Although to be fair, higher income correlates with better nutrition and better health, so the correlation might be stronger than you’d expect.)

If the two variables are strongly correlated, then a large change in one will be accompanied by a large change in the other. If they’re weakly correlated, then a large change in one will by accompanied by a small change in the other.

It’s very tempting, when we see data that is correlated, to say that one causes another. And in **some cases**, that’s certainly true: We can safely say that your higher electric bill was caused by greater consumption.

**But just because two data points are correlated doesn’t mean that one causes the other. And even if one does, you have to be careful to determine just what causes what**. For example, if there is a causal relationship between private-jet ownership and billionaire status, then perhaps I should buy a private jet. That’ll raise the likelihood of my becoming a billionaire, right?

Fortunately, **in the world of data analytics, we’re often less interested in causation than in finding correlations**. If I find that my online store gets more sales between 12 noon and 1 p.m., I don’t really care what’s causing it—but I just do want to know about it, and take advantage of it.

The most common measurement for correlation, and what we’ll use in this book, is called **"Pearson’s correlation coefficient,"** and is often abbreviated as "r". It’s a number between -1 (indicating the strongest possible negative correlation) and 1 (indicating the strongest possible positive correlation), with 0 indicating no correlation. A correlation is always calculated between two data sets, which in the case of pandas, means **two different columns.**

We can also use correlations to hint at underlying similarities and relationships in our data. If two things are correlated, then perhaps there’s some behavior that explains the connection between the two. If that behavior or relationship isn’t obvious, then it can point to a topic worth investigating or understanding better.

**scatter matrix**

The scatter matrix is a great way to get a **quick look at all of the correlations in your data set**. The diagonal, which always contains 1.00 values in the call to df.corr(), is a histogram in the scatter matrix, indicating the distribution of values in each column.

from pandas.plotting import scatter_matrix

scatter_matrix(df_final)

### SEABORN

Matplotlib is, without a doubt, the leading plotting system for Python. Many people have found it hard to learn and use, however, which has led to the creation of several alternatives to Matplotlib. One of the best-known alternatives, Seaborn (http://seaborn.pydata.org/), was written by data scientist Michael Waskom, and **acts as an API on top of Matplotlib.**

**import seaborn as sns**

Whereas pandas visualization is all done via the **plot** attribute, followed by the type of plot we want to create, Seaborn is organized more conceptually, around the different types of insights we might be trying to draw from our plots. We can choose from **four different functions defined within sns**:

    -- To visualize relationships among numeric columns, use sns.relplot. The relplot function is there to show us relationships among numeric columns, and **the default way to do that is with a scatter plot**. 
    -- To visualize relationships that include categorical columns, use sns.catplot.
    -- To understand the distribution of data, use sns.displot.
    -- To visualize regression models, use sns.regplot.

like

    sns.relplot(x = 'max_temp',

               y = 'min_temp',

               data = df.loc[df['city'] == 'Chicago'])
           
Our call to sns.relplot includes **three mandatory keyword arguments**:

* x indicates which column from our data frame will be used for the x axis
* y indicates which column from our data frame will be used for the y axis
* data is a data frame containing both of those columns

In this case, I decided to provide only a subset of the data from df, so that we would only see Chicago weather. But what if I want to see all of the data, from all of the cities?

    sns.relplot(x='max_temp',

                y='min_temp',

                data=df)
            
The good news is that this is much easier to write. But the bad news is that it’s not nearly as useful. Now we’ve mixed together all of the weather reports from all of the cities! Fortunately, Seaborn provides us with a number of different ways to make the data more useful and interesting.

For example, we can ask Seaborn to use a different color for each city by passing a column name to the **hue keyword argument**:

    sns.relplot(x='max_temp',

                y='min_temp',

                data=df,

                hue='city')

We can have each city’s dots use a different marker, as well, by giving the same city argument to the **style parameter**:

    sns.relplot(x='max_temp',

                y='min_temp',

                data=df,

                hue='city', style='city')

            
We don’t have to use the same categorical data for hue and style. For example, we can set the hue per state

    sns.relplot(x='max_temp',

                y='min_temp',

                data=df,

                hue='state', style='city')
            
However, it’s a bit messy to see all of these plots on the same axes. We can ask Seaborn to do the visual equivalent of a groupby, with one plot per value of city. There are two different ways to do this, actually, by setting row (i.e., each row is a different value for the named column) or col (i.e., each column is a different value for the named column). For example:

    sns.relplot(x='max_temp',

                y='min_temp',

                data=df,

                hue='state',

                row='city')
            
While scatter plots are extremely useful, we can also see the relationship between two numeric columns with line plots. The most obvious difference between the two kinds of plots is that Seaborn draws a line between the dots. For example:

    sns.relplot(x='max_temp',

                y='min_temp',

                data=df,

                hue='state', kind='line')
            
The above call is fine, except that it won’t work. In my case, I got both a warning from pandas and an error message from Seaborn. Both of them told me that they cannot handle my data frame as it stands, because its index contains non-unique values.

I can fix this easily with reset_index:

df.reset_index(drop=True)

Note that I passed **drop=True to avoid having the old index added as a column to the data frame**. I’m happy to throw out the old index and replace it with a new one, so I pass drop=True.

    sns.relplot(x='max_temp',

                y='min_temp',

                data=df,

                hue='state',

                kind='line',

                row='city')
            

In some line plots (like exercise 10.6 Exercise 47: Seaborn taxi plots), there are some gray lines around each of our plots. Those indicate the **"confidence interval"** for each calculation. Confidence intervals are a statistical tool to indicate how likely a value is to fall within a certain range. We can disable the confidence intervals by passing **ci='None' on a relplot**.

Seaborn supports a wide variety of other plots, as well. For example, what if we want to see all of the values of max_temp for a given city? You can think of this as a set of one-dimensional scatter plot, and we can see it as follows:

sns.catplot(x='city', y='max_temp', data=df)

Notice how the x axis is for the categories, while the y axis describes which value we’ll be seeing. This plots each of the values in the data set. If we instead want to summarize our data, we can ask to have a box plot, instead:

**sns.catplot(x='city', y='max_temp', data=df, kind='box')**

This shows a box plot for each of the cities' values of max_temp, all side-by-side on the same y axis.

**a note on box plots:**

Box plots are, in the world of Seaborn, **categorical plots**, because they allow us to compare the distribution of values across multiple categories. We thus need to use the catplot function:

-- The x axis will be the categories we’re comparing

-- The y axis will be the values we want to see graphically

-- We’re looking at data from df

-- We want to see a box plot, and thus specify kind='box'

Finally, Seaborn also offers us the chance to create histograms. Because histograms allow us to understand the distribution of our data, we use the sns.displot function. For example, we can get a histogram of all maximum temperatures:

sns.displot(x='max_temp', data=df)

This, of course, shows the distribution of all values of max_temp. We can, once again, give each city its own colored bars by setting hue:

sns.displot(x='max_temp', data=df, hue='city')

And we can see them in a single column, with only one city per row, by saying:

sns.displot(x='max_temp', data=df, hue='city', row='city')

These are just some of Seaborn’s many capabilities. If you’re interested in seeing everything that Seaborn can do, I strongly recommend you check out the documentation at http://seaborn.pydata.org/. I’ve grown to really like the Seaborn approach to visualization—not only does it produce very nice-looking plots, but I find the API easier to understand and work with than many others.


<a name="10"></a>
## 10. Performance

**good to knows**

df.info()

The df.info method returns a summary of information about the data frame, including the total memory usage. By default, it **doesn’t do a "deep" memory check**; in such cases, and if there are **object columns**, the memory will be returned with a **+ sign following the number**. You can avoid the +, and get a precise calculation, by passing memory_usage='deep' as a keyword argument to info:

df.info(memory_usage = 'deep') : Get information about a data frame, including its memory usage

df.memory_usage(deep=True) : Get information about a data frame’s memory usage

df_olympic.memory_usage(deep = True).sum()

df['a'].astype('category') : convert into categorical data type 

df.to_feather('mydata.feather') : Write a data frame to feather format

df = pd.read_feather('mydata.feather') : Create a data frame, based on a feather-formatted file on disk

time.perf_counter() : Get the number of seconds, useful for timing programs

df.eval('v + 300'): Perform actions and queries on a data frame

%timeit 3+2 : Python module for benchmarking code speed, and a Jupyter "magic command" for invoking it

### Memory usage 

How much memory does this data set consume? That’s an important question when working with pandas, because all of our data needs to fit into memory. We can find out by running the memory_usage method on our data frame:

    df.memory_usage()

This returns a series telling us how many bytes are consumed by each column. (The column names from df constitute the index of the returned series.) We can get the total memory usage by summing the values:

    df.memory_usage().sum()

This number is completely **wrong**. That’s because pandas, **by default, ignores the size of any Python objects contained in our data frame**. Given that these objects are generally strings, and that they can be of any length, the difference between the actual memory usage and what is reported here can be quite big.

We can tell pandas to **include all of the objects in its size calculation by passing the deep=True keyword argument**:

**df.memory_usage(deep=True).sum()**

**I want to remind you that it’s important to always use deep=True if you truly want to know the size of your data frame.**

**point** How can I cut down the size of the data set, thus allowing me to potentially work with more data? We’ve already talked about several of them in past chapters:

* Limit which columns are imported, by passing a value to usecols

* Explicitly specify the dtype for each column, allowing you to choose types with fewer bits while simultaneously speeding up the loading of data

* (also the third strategy is to remove rows that you don't need through cleaning, mentioned in reuven's youtube video on memorry optimization)


## Saving Memory with categories

The majority of the memory is being used by **strings**. This means that we need to somehow reduce the size or number of the text strings in our data frame.

One way to do this is with a special pandas data type known as a **"category."** In the case of a category, **each distinct string value is stored a single time, and then referred to multiple times**. However, this replacement is completely transparent to us, as users of the data frame: **We can continue to pretend that the column contains strings, including use of the str accessor to apply string methods to every element of the column.**

We’ve often used **astype** to create a new series based on an existing one. We can do the same thing to create a new categorical column based on one containing text strings:

    df['Games'].astype('category')

Which columns should we attack first? Well, we want those in which the same strings are often repeated. Consider this code:

    (df.count() / df.nunique()).sort_values(ascending=False)

Here, we divide the number of non-null rows in each column by the number of distinct values in that column. The higher the number, the more times the same string is repeated, and thus the greater the memory savings we can achieve by switching the column to a category. I then used sort_values(ascending=false) to sort them in order of priority.

good point:  **df_olympic.nunique()** is a method call to find the number of unique values for each column in a pandas DataFrame df_olympic. The output will be a pandas Series object where the index is the column names and the values are the number of unique values for each column.

But wait a second: This method creates the category based on the data that’s **already** in the series. What if I know that the series might include other values in the future, even if they’re not in the orignal data set? Here’s a simple example:

we can achieve this by creating the category **before creating the series (or column of the data frame), including all of the possible values it might contain**. Then we can ask pandas not to create the category with astype, but rather to assign the specific category type that we’ve defined, with all of its values. we do this by:

    my_defined_category = pd.CategoricalDtype(['Gold', 'Bronze', 'Silver'])

    df_olympic ['Medal'] = df_olympic ['Medal'].astype(my_defined_category)

In the above code, we created a new category, with all of its values, by calling **pd.CategoricalDtype**. Then, **when we called astype, we passed the category that we had created, rather than asking pandas to create a new, anonymous category**.

Another potential benefit of creating this category type is that if we need the same category for multiple columns, we can save even more memory by sharing the category.

**point**. We would benefit most by first standardizing the spellings, and only after creating the category. The more times a string is repeated, the greater the benefit from using a category for that string. When we standardize the spellings, we reduce the number of different strings in a column, and increase the number of times it repeats -- thus strengthening the argument for using a category.

### APACHE ARROW

There’s no doubt that CSV files are convenient to work with. Not only does every programming language and data-analysis system knows how to read from and write to them, but they’re readable by people, too. Heck, we can even go in and edit CSV files by hand, when we need to. The same could be said for JSON, which has also become popular in the last few years. JSON can handle more complex data structures than CSV, but is still a **text-based format.**

The fact that CSV and JSON are human readable almost inherently makes them less efficient for computers: They take up more space on disk than binary formats, and also take longer to read and write. A number of binary formats exist, but none has pushed out the others as a standard.

Fortunately, we have an increasingly viable **binary option—Apache Arrow**, along with its **file format, known as Feather.** Apache Arrow is meant to be a new backend in-memory storage system not just for pandas, but also for other data-analysis libraries and languages, including the popular R language and the **Spark distributed library**. Arrow is designed to handle the data types, and data-storage needs, associated with data analysis. It can handle the data types that we’re used to, from integers and floats to strings and dictionaries. It can even handle categories, of the sort that we saw in Exercise 48. It uses a number of tricks to use less memory than the standard NumPy back end.

Beyond the work being done to make Apache Arrow a fast and useful in-memory database, there is also an on-disk serialization format for that database, known as "feather." Feather files take advantage of the data types in Arrow, along with compression and binary storage. **They are thus smaller in size, and faster to both read and write, than either CSV or JSON.**

To **write a pandas data frame to a feather-formatted file**, you can use the **to_feather method**, which works similarly to to_csv and to_json:

    df.to_feather('mydata.feather')

You can similarly read from a feather-formatted file into a data frame using the **pd.from_feather method**, which works similarly to from_csv and from_json:

    df = pd.from_feather('mydata.feather')

**i think i will use pd.read_feather() since i am used to pd.read_csv() and pd.read_json()**

Because pandas can read from and write to feather files faster than CSV or JSON, your projects would likely benefit from turning files from CSV and JSON into feather at the first opportunity. The larger the data set, the greater the benefit you’ll have from working in this way.

Feather provides another potential way to speed up your work in pandas, **even if you don’t use Arrow or the feather format directly**: When **reading from a CSV file, you can traditionally choose from two different parsing engines, one written in Python and the other written in C**. The Python engine is more flexible and offers more features, but it **slower than the C-language parser**. By **default, pandas tries to use the C parser**, unless you specify an option that it cannot handle, in which case it switches to Python. You can explicitly specify the engine you want when reading the CSV file:

    df = pd.read_csv('mydata.csv', engine='python')

While it is still considered experimental as of this writing, you can now pass a third value to the engine keyword argument, namely the string **'pyarrow'**. If you do that, then pandas will use the Arrow architecture and library to read the CSV file, potentially speeding up its reading into memory. If you find that the data is loading slowly, you might want to try this option. In my experience, this can significantly improve the rate at which CSV data is read into a data frame.

Over the coming years, I expect that the Arrow backend, as well as the feather format, will become increasingly common among users of pandas and other open-source data analysis tools. CSV and JSON aren’t going away, but as data sets grow in size, feather’s faster speed and cross-platform compatibility will become more dominant, and should assume a greater role in your arsenal.

### keep track of time

We’ll need some way to keep track of time. Python’s time module, part of the standard library, provides a number of different methods that could theoretically be used, but it’s generally considered best to use **time.perf_counter()**. This function uses the highest-resolution clock available, and returns a float indicating a number of seconds. The number returned by perf_counter should not be relied upon for calculating the current date and time—but if used within the same program, it can be used to measure the passage of time, which is precisely what we’ll want to do. The reference point of the returned value is **undefined**, so that **only the difference between the results of two calls is valid.**

Python’s standard library also includes the timeit module (http://docs.python.org/3/library/timeit.html?#module-timeit), which includes a number of utilities for benchmarking. I decided not to use timeit, in no small part because that module runs code several times. Each of our runs will take long enough that this didn’t seem like a good fit. But timeit is a good tool to know about for benchmarking your code, and we’ll use it in Exercise 50.

**%timeit**

Python provides the timeit module, which you can use in standard programs, but Jupyter provides a special **%timeit magic method that can be used inline, inside of Jupyter cells**. You can say:

%timeit myfunc(2, 3, 4)

In this example, timeit will run myfunc(2,3,4) **a number of times, reporting the mean execution time along with the variation that it detected**. Just how many loops timeit runs is determined by the code speed; something that takes a fraction of a second might run hundreds or even thousands of times, whereas something that takes more than a few seconds might be run only a handful of times. another example si as follows: 

%timeit df_ticket.loc[df_ticket.eval("state == 'NY' & color == 'WHITE' & feet > 1 & make == 'TOYOT' & ptype == 'PAS'")]

When using the **%timeit magic command in Jupyter, don’t forget:**

    -- Your code must be written on a single line, just after the %timeit magic command. If you have more than one line, then wrap it into a function, and invoke that function.
    -- If you’re timing a function, don’t forget to put () after the function’s name.


### Speeding things up with eval and query

Over the course of this book, I’ve emphasized a number of techniques that you should use to speed up your pandas performance:

        Never use standard Python iterations (for loops and comprehensions) on a series or data frame
        Take advantage of broadcasting
        Use the str accessor for anything string related
        Use the smallest dtype you can, without sacrificing accuracy
        Avoid double square brackets when setting and retrieving values
        Load only those columns that you really need for your analysis
        Columns with repeated values should be turned into categories
        Use a binary format, such as feather, for data you’ll repeatedly save or load

Even after using all of these techniques, you might find that your queries are still running slowly, or using lots of memory. This often occurs when performing an arithmetic operation on two columns, each of which contains many rows. A related problem is when you’re broadcasting an operation on a scalar and a series. While pandas takes advantage of the high-speed calculations in NumPy, much of the work is still being done within the Python language, which is slower to execute than C.

Another problem occurs when you’re creating a boolean series, for use as a mask index, based on several conditions. It’s certainly convenient to use & and | to combine your conditions with logical "and" and "or", but **behind the scenes, pandas has to create multiple boolean series, which are then combined**. If you have 1 million rows in your original column, then combining three conditions will end up creating at least 3 million rows in temporary series, before combining and applying them together.

We can avoid these problems, as well as make our queries more readable, using the **query method** that I introduced back in chapter 2, as well as two versions of the more general eval method. These **reduce the memory needed in queries using | and &**, and can often execute expressions in a **library known as numexpr** (i don't need to do anything like pip installing this library and it just worked fine). The combination of reduced memory and increased speed can sometimes give dramatically faster results, while also using fewer resources.

However, it’s important to understand that these methods are not **cure-alls for your performance problems**:

        Using them on small data frames, with fewer than 10,000 rows, will often result in slower performance, not in faster performance.
        Often, the bottleneck in your performance is in assignment or retrieval of elements, not in the calculation. Which means that there won’t be a speed boost in such cases.
        You’ll need to install the numexpr package from PyPI, and then explicitly tell pandas to use it. If you don’t make this explicit, then pandas will use its default Python-based engine for parsing the query string, resulting in no speedup (but in the exercise reuven did not bring this up and just showed us how faster our queries would be using query and eval and I managed to finish all the exercises without needing to pip install numexpr!).
        Anything which doesn’t involve calculations, comparisons, and boolean operators will either raise an exception or run at the standard (non-enhanced) speed.
Let’s start by looking at the query for data frames. We’ll then talk about two versions of eval, which are part of the same family.

Given a data frame df, the method df.query allows you to describe which rows you want to get back from df. The description is passed as an SQL-like string in which the columns can be named as if they were variables. The result of the query will be a data frame, a subset of df, with **all of the columns from df and those rows for which the comparison returned a True value** (so i think i don't have an option of selecting columns using query mehtod). for example:

    df.loc[((df['a'] > 100) & (df['b'] < 700))] 

    df.query('a > 100 & b < 700')

The version using query might run faster. But in almost all cases, **it’ll use less memory**, because it won’t have to create two separate, temporary boolean series: One for a > 100 and another for b < 700. We might not see these boolean series when running a traditional query, but they’re there, and can use a great deal of memory without you realizing it.

I should add that some people prefer to use df.query for all of their pandas work, because of its readability and reduced memory use (but reuven made a comment in the end that this is not a great idea b/c query method does not allow to select columns)

A related data frame method is **df.eval**, which allows us not only to retrieve from a data frame (as in df.query), but also to perform **other actions, including broadcasting and assigning**. For example:

    df.eval('(a + b) * 3')

The above code adds columns a and b together, then multiplies the new series by 3, via broadcasting. The returned value is a series. What if we were to pass the same code as we used before, with df.query?

    df.eval('a > 100 & b < 700')

The above returns a boolean series. **Whereas df.query applies that boolean series to df, df.eval returns the boolean series itself**, and allows us to apply it if and when we want to do so.We can even add a new column (or update an existing one) by assigning to a column name:

    df.eval('f = d + e - c')

Using a triple-quoted string, you can perform multiple assignments (not conditions) df.eval:

    df.eval('''

    f = d + e - c

    g = a * 2

    h = a * b

    ''')

The third, and final method that allows you to use less memory, speed up computation, and write more readable queries is **pd.eval** (in the exercises i did not need to use it and just used df.eval). Notice that this is a top-level function in the pd namespace, rather than a method we run on a specific data frame. We can use pd.eval instead of df.eval, although we’ll need to explicitly state the name of the data frame we’re working on. For example, we can say:

    pd.eval('df[df.a > 100 & df.b < 700]')

Notice that when using pd.eval, **you’ll almost certainly need to use the dot syntax with columns, rather than the square-bracket syntax** that I have generally used in this book—so to retrieve column a from data frame df, you’ll need to say df.a, rather than df['a']. As a result, this also means that your column names cannot contain spaces in them.

The above code will return all of the rows of df in which a is greater than 100 and b is less than 700, as before. However, we have written the query as a string, which is passed to numexpr. That package will, as we’ve seen, use less memory and (usually) result in better performance. Note that a call to df.eval is translated into a call to pd.eval, which means that you can probably get better performance if you just call pd.eval. **That said, the convenience of the syntax in df.eval is hard to beat** (that should be why in the exercises we used df.eval rather than pd.eval).

As with df.eval, you can assign to one or more columns in the string you pass to pd.eval. But because we’re invoking pd.eval, the data frame on which the assignment should take place isn’t known to the system. You must set it by passing the **target keyword argument**. The assignment is reflected in the data frame that is returned:

    pd.eval('f = df.d + df.e - df.c', target=df)

So, when should you use each of these? Again, the biggest wins are likely to be with compound queries (using & and |), on large data frames. The larger the data frame and the more complex the query, the bigger the speed boost that you might see—but even if you don’t, you’ll almost certainly be using less memory.

Meanwhile, here’s a quick recap on each of these three functions:

* To retrieve selected rows from a data frame, use df.query

* To assign multiple columns, or to perform either queries or assignments on a data frame, use df.eval.

* To work on multiple data frames, use pd.eval. It doesn’t handle multiline assignments, and the syntax makes it a bit uglier, though.

From the comparisons we made, we see that optimiziation of queries is rarely a matter of always using one particular technique. It requires a bit of thinking about what you’re doing, considering what pandas is doing behind the scenes, and then performing some tests to check your assumptions. That said, we can conclude at least two things from these queries: **First, that if you’re combining queries with | or &, you’ll likely get a decent improvement by using query rather than loc, thanks to the speedups provided by numexpr. Second, using isin will almost always be faster than combining multiple queries, because we’re making a single comparison per row, rather than more.**

point:

it would seem that selecting via df.loc and a mask index will have better performance, faster execution time, than just df and a mask index—something that I’ve seen elsewhere, too. here is what i mean:

    %timeit df[df.eval('state == "NY"')] >> 758 ms

compared to

    %timeit df.loc[df.eval('state == "NY"')] >> 729 ms


**Is it always worth using df.query or df.eval instead of .loc?**

I know that there are pandas users who would say "yes," given that even in the simplest of cases, we saw a speedup. And in the most complex cases, the speedup was quite dramatic. So you could argue that since it doesn’t matter much for simple queries on a short data set, but it matters a lot for complex queries on large ones, you should always use these techniques.

However, focusing on speed before you’ve really thought hard about the problem, and where the bottlenecks are, can be misleading. Remember that df.query returns all of the columns from a data frame—so if your data frame contains more columns than you’ll want to get back, it might end up using lots of memory unnecessarily. By contrast, df.loc provides you not only with a row selector, but also with a **column selector**, for more flexibility. **I thus tend to use df.loc for my queries while I’m still putting them together. When I’m done, I can then experiment with these techniques to see how to reduce memory and speed things up.**

**point**

In df.query, we can use the words **and** and **or**, rather than the **symbols & and |**, thanks to the **numexpr library**. After rewriting the queries in exercise 50b using these words instead of symbols i observed improvement in the speed!

