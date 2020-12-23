## Importing the `pandas` library

When we import `pandas` in Python, we generally use the `pd` acronym. As
such, instead of writing `pandas.shape` we can just type `pd.shape`. As
you’ve noticed this is just a matter of convenience nonetheless it’s
important to follow the community’s conventions, that’s why I highly
encourage you to always work with the `pd` acronym.

    import pandas as pd

# Reading a `csv` file in pandas

Using the `read_csv()` function we can read either a local `csv` file or
as in the following a remote `csv` file. We’ll work with the
**penguins** data frame which is provided by the
[palmerpenguins](https://allisonhorst.github.io/palmerpenguins/) `R`
package.

> feel free to copy paste the URL below to check out the data frame in a
> raw format

    URL = "https://gist.githubusercontent.com/feddelegrand7/2f32818389d58eb62e8e47ff08751415/raw/9c2debfc2277aba6c227a0b1746f4af78c9798fe/penguins.csv"
    penguins = pd.read_csv(URL)

# Getting an overview of the data frame

Before any analysis, it’s useful to get some basic information about the
structure of a data frame. Here `df.shape()` will print out the number
of rows and columns in a `tuple` format:

    penguins.shape

    ## (344, 8)

OK ! it’s obvious that our `penguins` data frame contains 344 rows and 8
columns. Now, will use `penguins.head(3)` in order to display the first
3 rows:

    penguins.head(3)

    ##   species     island  bill_length_mm  ...  body_mass_g     sex  year
    ## 0  Adelie  Torgersen            39.1  ...       3750.0    male  2007
    ## 1  Adelie  Torgersen            39.5  ...       3800.0  female  2007
    ## 2  Adelie  Torgersen            40.3  ...       3250.0  female  2007
    ## 
    ## [3 rows x 8 columns]

If you’re working with a data frame that is too wide and you need a list
of the columns’ names, you can use the `columns` attribute:

    penguins.columns

    ## Index(['species', 'island', 'bill_length_mm', 'bill_depth_mm',
    ##        'flipper_length_mm', 'body_mass_g', 'sex', 'year'],
    ##       dtype='object')

It’s also possible to print out the row-index of the data frame using
the `index` attribute.

    penguins.index

    ## RangeIndex(start=0, stop=344, step=1)

The `info()` method is a pretty cool tool that helps get an overall
understanding of the structure of our data. It prints out in order:

-   The class of the data frame : `DataFrame`
-   The range of the index: `344` rows
-   The number of **non-missing** values, here spotted as `non-null`
-   The type of each column or `Dtype`. Here we have two types of
    columns:
    1.  `object`: you can think of it as a character string column;
    2.  `float64`: a numeric column
-   Lastly, we get the `memory usage` which provides an idea about the
    memory size of our data frame.

<!-- -->

    penguins.info()

    ## <class 'pandas.core.frame.DataFrame'>
    ## RangeIndex: 344 entries, 0 to 343
    ## Data columns (total 8 columns):
    ##  #   Column             Non-Null Count  Dtype  
    ## ---  ------             --------------  -----  
    ##  0   species            344 non-null    object 
    ##  1   island             344 non-null    object 
    ##  2   bill_length_mm     342 non-null    float64
    ##  3   bill_depth_mm      342 non-null    float64
    ##  4   flipper_length_mm  342 non-null    float64
    ##  5   body_mass_g        342 non-null    float64
    ##  6   sex                333 non-null    object 
    ##  7   year               344 non-null    int64  
    ## dtypes: float64(4), int64(1), object(3)
    ## memory usage: 21.6+ KB

# Selecting a column in pandas

There are two ways of selecting columns in `pandas`, we can do:

1.  `df['column_name']` or:
2.  `df.column_name`.

for many reasons that will become obvious later, you should always use
the first syntax.

In our example, let’s say that we wanted to select the `species` column,
we can do:


    penguins['species']

    ## 0         Adelie
    ## 1         Adelie
    ## 2         Adelie
    ## 3         Adelie
    ## 4         Adelie
    ##          ...    
    ## 339    Chinstrap
    ## 340    Chinstrap
    ## 341    Chinstrap
    ## 342    Chinstrap
    ## 343    Chinstrap
    ## Name: species, Length: 344, dtype: object

**Important** ⚗️ : a column taken individually has a special form and a
special type called `pandas Series`. To summarize a `pandas` data frame
has a `DataFrame` class and is composed of several columns that taken
**individually** have a `Series` class.

    type(penguins['species'])

    ## <class 'pandas.core.series.Series'>

Now let’s select multiple columns, maybe `species`, `island` and
`bill_length_mm`.

    penguins['species', 'island', 'bill_length_mm']

    ## Error in py_call_impl(callable, dots$args, dots$keywords): KeyError: ('species', 'island', 'bill_length_mm')
    ## 
    ## Detailed traceback: 
    ##   File "<string>", line 1, in <module>
    ##   File "C:\Users\Administrateur\AppData\Local\r-miniconda\envs\r-reticulate\lib\site-packages\pandas\core\frame.py", line 2902, in __getitem__
    ##     indexer = self.columns.get_loc(key)
    ##   File "C:\Users\Administrateur\AppData\Local\r-miniconda\envs\r-reticulate\lib\site-packages\pandas\core\indexes\base.py", line 2897, in get_loc
    ##     raise KeyError(key) from err

We got an error 😠 !!! what happened ? in fact there is a dialogue here:

> pandas: you want to select multiple column ? me: Yes! please !!!
> pandas: OK! but I want a **list \[\]** of columns, in other words, the
> columns must be provided inside a list ! otherwise you’ll get an error
> !!!! me: emo::ji(‘unamused’)

So the correct way of subsetting columns is:

    penguins[['species', 'island', 'bill_length_mm']]

    ##        species     island  bill_length_mm
    ## 0       Adelie  Torgersen            39.1
    ## 1       Adelie  Torgersen            39.5
    ## 2       Adelie  Torgersen            40.3
    ## 3       Adelie  Torgersen             NaN
    ## 4       Adelie  Torgersen            36.7
    ## ..         ...        ...             ...
    ## 339  Chinstrap      Dream            55.8
    ## 340  Chinstrap      Dream            43.5
    ## 341  Chinstrap      Dream            49.6
    ## 342  Chinstrap      Dream            50.8
    ## 343  Chinstrap      Dream            50.2
    ## 
    ## [344 rows x 3 columns]

Works like a charm 😒

# Removing a column

We’ve a perfect data frame to play, why would we dare remove a column or
two ??? just for the sake of the example … ok that’s was the joke of the
day, now let’s get into serious stuffs.

The `drop()` method allows us to remove the columns that we specify.
Let’s get rid of `species` and `island`:

    penguins_drop = penguins.drop(columns= ['species', 'island'])
    penguins_drop.head(5)

    ##    bill_length_mm  bill_depth_mm  flipper_length_mm  body_mass_g     sex  year
    ## 0            39.1           18.7              181.0       3750.0    male  2007
    ## 1            39.5           17.4              186.0       3800.0  female  2007
    ## 2            40.3           18.0              195.0       3250.0  female  2007
    ## 3             NaN            NaN                NaN          NaN     NaN  2007
    ## 4            36.7           19.3              193.0       3450.0  female  2007

# Renaming columns

When renaming a column, `pandas` expect that you define a `dictionary`
as follows:

`{'current_name' : 'new_name'}`

So if for example, we wanted to change the name of `species` and
`island` to respectively `penguin_name` and `land`, we can proceed as
follows:

    penguins.rename({'species': 'penguin_name', 'island':'land'}, 
                    axis = 'columns')

    ##     penguin_name       land  bill_length_mm  ...  body_mass_g     sex  year
    ## 0         Adelie  Torgersen            39.1  ...       3750.0    male  2007
    ## 1         Adelie  Torgersen            39.5  ...       3800.0  female  2007
    ## 2         Adelie  Torgersen            40.3  ...       3250.0  female  2007
    ## 3         Adelie  Torgersen             NaN  ...          NaN     NaN  2007
    ## 4         Adelie  Torgersen            36.7  ...       3450.0  female  2007
    ## ..           ...        ...             ...  ...          ...     ...   ...
    ## 339    Chinstrap      Dream            55.8  ...       4000.0    male  2009
    ## 340    Chinstrap      Dream            43.5  ...       3400.0  female  2009
    ## 341    Chinstrap      Dream            49.6  ...       3775.0    male  2009
    ## 342    Chinstrap      Dream            50.8  ...       4100.0    male  2009
    ## 343    Chinstrap      Dream            50.2  ...       3775.0  female  2009
    ## 
    ## [344 rows x 8 columns]

and voila ! here we had to set `axis = 'columns'` to specify that we’re
renaming columns not indexes (which is the default)
