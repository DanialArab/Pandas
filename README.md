# Pandas_portfolio

This repository documents my understanding of Pandas and contains my solutions (the **jupyter notebook file named pandas.ipynb attached to this repo**) to 200 detailed exercises (50 main exercises along with 150 beyond exercises) from the book written by Reuven Lerner (Pandas Workouts -- https://www.manning.com/books/pandas-workout)

Pandas is all about analyzing data. And a major part of the analysis that we do in Pandas can be phrased as, **"Where this is the case, show me that."**

Below is the summary of my notes from the book:

# Table of content

1. [Pandas Series](#1)

2. [Pandas DataFrames](#2)

3. [Importing and exporting data](#3)

4. [Indexes](#4)

5. [Cleaning data](#5)

6. [Grouping, joining, and sorting](#6)

7. [Midway project](#7)

8. [Strings](#8)

9. [Dates and times](#9)

10. [Visualization](#10)

11. [Performance](#11)

12. [Final project](#12)



<a name="1"></a>
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

### **Retrieving** a series individual elements using textual vs. positional indexes:

  -- Use .iloc for the numeric index, and .loc for the custom textual index. You don’t need to use .iloc or .loc when you **retrieve slices**, however.
  
  
* Some notes on .iloc and .loc accessor to retreive values:

  -- Instead of using s[i] it is generally better to use s.iloc[i] to retrieve positional numeric index i from series s. This is necessary when working with dataframes, but not when working with series. However, do it to get more comfortable with, since it is a must when working with dataframes. 
  
  -- However, the need for .iloc and .loc goes away when slicing. Even when we’re working with data frames, pandas assumes that if you’re using a slice, then you want to work with the index (rather than the columns). As an example on slicing, go with s['September': 'January'], although I can use s.loc['September': 'January']. Also go with s[0:5] although you can try s.iloc[0:5]  
  
  -- When slicing using a textual index like s['September': 'January']: unlike using the positional numeric indexes here the slice end is no longer "up to and not including," but is rather "up to and including."
  
  -- Most of the time, I prefer to use textual indexes in pandas, because they’re easier to understand, and make the code more readable. But there is a cost: In some simple benchmarking that I performed, I found that it took pandas twice as long to get the text-based slice as the number-based slice. If you find that your pandas analysis is taking a long time, it might be worthwhile to try using positional indexes.
  
**side note:**

In a normal distribution, used for many statistical assumptions, we expect that 68% of a data set’s values will be within 1 standard distribution of the mean, that 95% will be within two standard distributions, and 99.7 will be within three standard distributions.

### Data types in Pandas 

Every series has a dtype attribute, and you can always read from that to know the type of data it contains. Every value in a series is of that type; unlike a Python list or tuple, you cannot have different types mixed together in a series. 

Normally, pandas guesses the dtype based on the data you pass it at creation:

You can specify the type of data in a series by passing a value to the dtype parameter when you create a series like s = pd.Series ([1, 2, 3], dtype = np.float64)

### astype

What if you want to change the dtype of a series once you’ve already created it? You can’t set the dtype attribute; it’s read only. Instead, you will need to create a new series based on the existing one by invoking the astype method:

### Vectorization in Pandas 

One of the most important ideas in pandas (and in NumPy) is that of vectorized operations. When you perform an operation on two different series, the indexes are matched, and the operation is performed via the indexes.

### Selecting values using mask/boolean index in pandas

In Python and other traditional programming languages, we can select elements from a sequence using a combination of for loops and if statements (**THIS IS EXACTLY WHAT I DID TO SOLVE THE ABOVE PROBLEMS**). While you could do that in pandas, you almost certainly don’t want to. Instead, you want to select items using a combination of techniques known as a **"boolean index"** or a **"mask index"**.

Mask indexes are useful and powerful, but their syntax can take some getting used to.

It is called "mask index," because we’re using the list of booleans as a type of sieve, or mask, to select only certain elements.

An explicitly defined list of booleans isn’t very useful or common. But we can also use a series of booleans and those are easy to create. All we need to do is use a comparison operator (e.g., ==) which returns a boolean value. Then we can broadcast the operation, and get a series back. 

We also can use a mask index for assignment like s[s <= s.mean()] = 999 where we replaced the elements less than or equal to the mean with 999.

This technique is worth learning and internalizing, because it’s both powerful and efficient. It’s useful when working with individual series, as in this chapter—but it’s also applicable to entire data frames, as we’ll see later in the book.

### Repeated values for index in pandas series

Yes, unlike the index in a Python string, list, or tuple which are unique, as are the keys in a Python dictionary, the series index can have repeated values—not just integers, but also strings (as in this example) and even other data structures, such as times and dates (as we’ll see in chapter 9). Normally, when we retrieve a value from a series via loc, we expect to get a single value back. But if the index is repeated, then we will get back multiple values. And in pandas, multiple values will be returned as a series.

**note**:

This means that when you retrieve s.loc[i], for a given index value, you can’t know in advance whether you will get a single, scalar value (if the index occurs only once) or a series (if the index occurs multiple times). This is another case in which you need to know your data, to know what type of value you’ll get back.


from chatGPT:

In pandas, the diff() function can be used to calculate the difference between consecutive elements in a Series. This function can also take an optional argument, periods, which specifies the number of periods to shift for the differences to be taken. By default, periods is set to 1.


### FANCY INDEXING

We’ve seen that I can retrieve the item at index 2 with s.loc[2], or the item at index 4 with s.loc[4]. But I can actually retrieve both of them at the same time with what’s known as "fancy indexing" **—passing a list, series, or similar iterable inside of the square brackets**. For example:

s.loc [[2, 4]]

The outer square brackets indicate that we want to retrieve from s using loc. And the inner square brackets indicate that we want to retrieve more than one item. pandas returns a series, keeping the orignal indexes and values.


### pd.read_csv

read_csv is more typically used to create a data frame—but if we provide it with a file that contains only one data point per line, and pass a True value to the squeeze parameter, then we’ll get a series back. 

**s = pd.read_csv('file.csv', squeeze = True, header = None)**

I also set the header parameter to be None, indicating that the first line in the file should not be taken as a column name, but rather is data to be included in our calculations. This will result in the series having a name value of 0, which we can safely ignore.

### value_counts method

It is a series method that is one of my favorites. If you apply value_counts to the series s, you get back a new series whose keys are the distinct values in s, and whose values are integers indicating how often each value appeared. 

Notice that the values are automatically sorted from most common to least common.

Because we get a series back from value_counts, we can use all of our series tricks on it. For example, we can invoke head on it, to get the five most common elements. We can also use fancy indexing, in order to retrieve the counts for specific values. 

If we’re interested in the percentages, not in the raw values, value_counts has an optional normalize parameter, that if set to True returns the fraction. 

so the solution to the above problem would be much soimpler like pass_count.value_counts(normalize = True)[['1', '6']]


### pd.cut

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

### Summary

In this chapter, we saw that a pandas series provides us with some powerful tools to analyze data. Whether it’s the index, reading data from files, calculating descriptive statistics, retrieving values via fancy indexing, or even categorizing our data via numeric boundaries, we were able to do quite a lot.

In the next chapter, we’ll expand our reach to look at data frames, the two-dimensional data strucures that most people think of when they work with pandas.

<a name="2"></a>
## 2. Pandas DataFrames

Data frames are two-dimensional tables that look and work similar to an Excel spreadsheet. The rows are accessible via an index—yes, the same index that we have been using so far with our series! **So long as you use .loc and .iloc to retrieve elements via the index, you’ll be fine.**

But of course, data frames also have columns, each of which has a name. Each column is effectively its own series, which means that it has an independent dtype from other columns.

In a typical data frame, each column represents a feature, or attribute, of our data, while each row represents one sample.

We’ll also see how just about every series method will also work on a data frame, returning one value per data frame column.

### BRACKETS OR DOTS?

When we’re working with a series, we can retrieve values in several different ways: Using the index (and loc), using the position (and iloc), and also using plain ol' square brackets, which is essentially equivalent to loc.

When we work with data frames, we **must use loc or iloc.** That’s because square brackets refer to the columns. If you try to retrieve a row, via the index, using square brackets, you’ll get an error message saying that no such column exists.

Square brackets always refer to columns, and never to rows. **Except, that is, when you pass them a slice, in which they look at the rows.** If you want to retrieve multiple columns, then you must use **fancy indexing.** You cannot use a slice.

**dot notation**

If you want to retrieve the column colname from data frame df, you can say df.colname. **THIS DOES NOT WOTK** when columns names include spaces and other illegal-in-Python-identifer characters. 

### Adding columns to a dataframe

We just assign to the data frame, using the name of the column that we want to spring into being. It’s typical to assign a series, but you can also assign a NumPy array or list, so long as it is of the same length as the other, existing columns.

There is another way to add a column to a pandas data frame, namely the assign method. I generally prefer to just add a new column directly, as described here. But assign returns a new data frame, rather than modifying an existing one, which can come in handy.

Column names are unique—so just as with a dictionary, assigning to an existing column will replace it with the new one. That said, if your data frame’s columns are not of the same dtype, you might find yourself with a SettingWithCopyWarning when assigning for replacement. 

### Retrieving and Assigning with loc

It’s pretty straightforward to retrieve an entire row from a data frame, or even replace a row’s values with new ones. For example, I can grab the values in the row with index abcd with df.loc['abcd']. If I prefer to use the numeric (positional) index, then I can instead use df.iloc[5]. In both cases, I get back a series. (Yes, even though the columns of a data frame are a series, pandas uses a series whenever it returns multiple, one-dimensional data.)


Retrieving a whole row isn’t that tough. But what if want to retrieve only part of a row? More significantly, how could we set values on only part of a row?

pandas actually provides us with a number of techniques for setting values. My preferred method is to use loc, providing two arguments in the square brackets. The first describes the row(s) that we want to retrieve, while the second describes the column(s) we want to retrieve. This technique also lends itself to changing and updating values in a data frame.

If I want to retrieve row a and c and columns v and y: 

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

### pd.concat

The pd.concat function concatenates dataframes with each other. It’s a top-level pandas function, and takes a list of data frames you would like to concatenate. By default, pd.concat assumes that you want to join them top-to-bottom, but you can do it side-to-side if you want by setting the axis parameter.

The result of pd.concat is a new data frame.

### note on the final dataframe after the concatenation: 

Next, I asked you to ensure that the new data frame (the one after concatenation)’s index doesn’t contain duplicate values—something that is almost certainly the case at this point, given that we created df from two previous data frames. You can actually check to see if a data frame’s index contains repeated values with the code

    df_ny_taxi_2020.index.is_unique

If this returns True, then the values are already unique. If not, then some Seaborn plots will give you errors. We could renumber the index on our own, but why work so hard, when pandas includes this functionality? We can just say:

    df_ny_taxi_2020 = df_ny_taxi_2020.reset_index(drop=True)

By passing drop=True, we tell reset_index not to make the just-ousted index column a regular column in the data frame, but rather to drop it entirely.

### THE Query method --  to retrieve rows
The traditional way to select rows from a data frame, as we have seen, is via a boolean index. But there is another way to do it, namely the query method. This mehod might feel especially familiar if you have previously used SQL and relational databases.

The basic idea behind query is simple: We provide a **string** that pandas turns into a full-fledged query. We get back a filtered set of **rows** from the original data frame. For example, let’s say that I want all of the rows in which the column v is greater than 300. Using a traditional boolean index, I would write:

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

**question for chatGPT:** is pandas query for just rows?

Re:

Yes, the pandas query method is used to **filter rows** in a DataFrame based on a Boolean expression. It returns a **new DataFrame** containing only the rows that satisfy the specified condition.

### **Outliers**

The term "outliers" doesn’t have a precise, standard definition. one definition is: 

**"inter-quartile range," or "IQR" = quantile(0.75) - quantile(0.25)**

Outliers would then be values **below the quantile(0.25) - 1.5 * IQR**, or any values **above the quantile(0.75) + 1.5 * IQR**.

We’ll use this definition here, but you might find that a different definition—say, anything below the mean - two standard deviations, or above the mean + two standard deviations, might be a better fit for your data.


**side note, i asked chatGPT is quantile the same as quartile?**

### NaN and missing data

pandas uses something known as NaN, aka "not a number." NaN is the pandas style for writing nan, a value that’s also available in NumPy. Both names are aliases to the same strange value, a float that cannot be converted into an integer, and that is not equal to itself.

In NumPy, we typically search for NaN values with the **isnan** function. pandas has a different approach, though: We can **replace** the NaN values in a series (or data frame) with the **fillna** method. And we can **drop any row with NaN values with the dropna** method.


Both of these methods return a **new series or data frame**, rather than modifying the original object. However, the new object you get back might not have copied the data, which means that assigning to it might produce the famous, dreaded SettingWithCopyWarning. If you plan to modify the series or data frame that you get back from df.dropna, you should probably invoke the copy method, just to be sure:

    df = df.dropna().copy()

This ensures that you can then modify df without having to suffer from that warning.

As you can imagine, it might be a bit extreme to remove any row containing even a single NaN value. For that reason, the dropna method has a **thresh** parameter, to which we can pass an integer—the number of **good, non-NaN values** that a row must contain in order for it to be kept. You might need to give some serious thought to how strictly you want to filter your data.

The **count** method on a series returns the number of **non-NaN values**. If there are no NaN values at all, then the result will be the same as the size of the series.

The count method on a data frame returns a series, with the columns' names as the index. Any columns with lower numbers, Reuven should have meant "compared to the len of dataframe", contain NaN values.


Quantiles and quartiles are related concepts, but they are not exactly the same thing.

A quartile is a specific type of quantile that divides a dataset into four equal parts. The first quartile, denoted Q1, represents the 25th percentile of the data, which means that 25% of the data falls below this value. The second quartile, denoted Q2 or the median, represents the 50th percentile of the data. The third quartile, denoted Q3, represents the 75th percentile of the data.

Quantiles, on the other hand, are a more general concept that divides a dataset into equal parts, but not necessarily into four equal parts. For example, the median is a quantile that divides the data into two equal parts (i.e., the 50th percentile). Other common quantiles include the decile (which divides the data into ten equal parts) and the percentile (which divides the data into 100 equal parts).

So, while quartiles are a type of quantile, the term "quantile" is a more general term that includes other types of divisions of the data into equal parts.

### interpolation

When your data contains missing values, you have a few possible ways to handle this. You can remove rows with missing values, but that might remove a large number of otherwise useful rows. A standard alternative is interpolation, in which you replace NaN with values that are likely to be close to the orignal ones. The values might be wrong, but but they will be roughly in the right ballpark.

When we call df.interpolate, it returns a new data frame. In theory, all of the columns will be interpolated—but if there is only missing data in one specific column that column will be interpolated. 

### Reuven's tip on urgency to use .loc

If you’re like many pandas users, then you might have thought about things like this:

  	  df_temps[df_temps['temp'] < 0]['temp'] = 0

Logically, this makes perfec sense. There’s just one problem: You cannot know in advance if it will work. That’s because pandas does a lot of internal analysis and optimization when it’s putting together our queries. You thus cannot know if your assignment will actually change the temp column on df, or—and this is the important thing—if pandas has decided to cache the results of your first query, applying ['temp'] to that cached, internal value rather than to the original one.

As a result, it’s common—and maddening!—to get a SettingWithCopyWarning from pandas. It looks like this:

<ipython-input-2-acedf13a3438>:1: SettingWithCopyWarning:
A value is trying to be set on a copy of a slice from a DataFrame.
Try using .loc[row_indexer,col_indexer] = value instead
    
When you get this warning, it’s because pandas is trying to be helpful and nice, telling you that your assignment might have no effect. The warning, by the way, isn’t telling you that the assignment won’t work, because it might. It all depends on the amount of data you have, and how pandas thinks it can or should optimize things.

The telltale sign that you might get this warning is the use of double square brackets—not nested, with one pair inside of the other, but with one right after the other. Whenever you see **][** in pandas queries, you should try hard to avoid it, because it might spell trouble when you assign to it. And truthfully, retrieving with this syntax, while something that all of us have done over the years, is something that you can avoid **using loc** and the "rows, columns" selection syntax that we’ve seen and discussed.

So, how should we actually set these values? It’s actually pretty straightforward:

    df_temps.loc[df_temps['temps'] < 0, 'temp']  = 0
                                     
If you use this syntax for all of your assignments, you won’t ever see that dreaded SettingWithCopyWarning message. You’ll be able to use the **same syntax for retrieval and assignment**. And you can even be sure that things are running pretty efficiently.

<a name="3"></a>
## 3. Importing and exporting data

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
                                 
### Three ways to optimize your Pandas data frame's memory usage

df.info(memory_usage = 'deep') gives detailes
                                       
    1. choose your columns wisely using "usecols", df.columns gives me the name of columns 
    2. choose dtypes appropriately 
    3. remove rows that you don't need through cleaning 

reference: reuven's youtube channel: https://www.youtube.com/watch?v=q3k3UrunY3M

### memory_usage method

We’ll talk more about this in future chapters, but the memory_usage method allows you to see how much memory is being used by each column in a data frame. It returns a series of integers, in which the index lists the columns and the values represent the memory used by each column. 

    df.memory_usage()       

### usecols

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



<a name="5"></a>
## 5. Cleaning data

<a name="6"></a>
## 6. Grouping, joining, and sorting

<a name="7"></a>
## 7. Midway project

<a name="8"></a>
## 8. Strings


<a name="9"></a>
## 9. Dates and times

<a name="10"></a>
## 10. Visualization

<a name="11"></a>
## 11. Performance

<a name="12"></a>
## 12. Final project
