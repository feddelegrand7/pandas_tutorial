## Importing the `pandas` library

When we import `pandas` in Python, we generally use the `pd` acronym. As
such, instead of writing `pandas.shape` we can just type `pd.shape`. As
you’ve noticed this is just a matter of convenience nonetheless it’s
important to follow the community’s conventions, that’s why I highly
encourage you to always work with the `pd` acronym.

    import pandas as pd

When we import `pandas` in Python, we generally use the `pd` acronym. As
such, instead of writing `pandas.shape` we can just type `pd.shape`. As
you’ve noticed this is just a matter of convenience nonetheless it’s
important to follow the community’s conventions, that’s why I highly
encourage you to always work with the `pd` acronym.

    import pandas as pd

# Reading a `csv` file in pandas

# Reading a `csv` file in pandas

Using the `read_csv()` function we can read either a local `csv` file or
as in the following a remote `csv` file. We’ll work with the
**penguins** data frame which is provided by the
[palmerpenguins](https://allisonhorst.github.io/palmerpenguins/) `R`
package.

> feel free to copy paste the URL below to check out the data frame in a
> raw format

    URL = "https://raw.githubusercontent.com/allisonhorst/palmerpenguins/master/inst/extdata/penguins.csv"
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

Did you notice the `...` ? it just means that when the data frame is too
wide, pandas truncates (hide) some columns in order to print nicely. It
does the same when printing many rows. In order to deal with this
situation we can print the column names using `columns` attribute:

    penguins.columns

    ## Index(['species', 'island', 'bill_length_mm', 'bill_depth_mm',
    ##        'flipper_length_mm', 'body_mass_g', 'sex', 'year'],
    ##       dtype='object')

We can also use `pd.set_option`:

    # 20 is the maximum number of column that we want to see
    # obviously we have less than 20 columns
    pd.set_option('max_columns', 100) 
    penguins.head()

    ##   species     island  bill_length_mm  bill_depth_mm  flipper_length_mm  \
    ## 0  Adelie  Torgersen            39.1           18.7              181.0   
    ## 1  Adelie  Torgersen            39.5           17.4              186.0   
    ## 2  Adelie  Torgersen            40.3           18.0              195.0   
    ## 3  Adelie  Torgersen             NaN            NaN                NaN   
    ## 4  Adelie  Torgersen            36.7           19.3              193.0   
    ## 
    ##    body_mass_g     sex  year  
    ## 0       3750.0    male  2007  
    ## 1       3800.0  female  2007  
    ## 2       3250.0  female  2007  
    ## 3          NaN     NaN  2007  
    ## 4       3450.0  female  2007

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
    ##   File "C:\Users\ADMINI~1\AppData\Local\R-MINI~1\envs\R-RETI~1\lib\site-packages\pandas\core\frame.py", line 2902, in __getitem__
    ##     indexer = self.columns.get_loc(key)
    ##   File "C:\Users\ADMINI~1\AppData\Local\R-MINI~1\envs\R-RETI~1\lib\site-packages\pandas\core\indexes\base.py", line 2897, in get_loc
    ##     raise KeyError(key) from err

We got an error ! a `KeyError` 😠 !!! what happened ? in fact there is a
dialogue here:

-   pandas: you want to select multiple column ?
-   me: Yes! please !!!
-   pandas: OK! but I want a **list \[\]** of columns, in other words,
    the columns must be provided inside a list ! otherwise you’ll get an
    error !!!!
-   me: 😒

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

**Important** ⚗️: many `pandas` method have the `inplace` parameter that
takes a boolean as argument. By defaut it is set to `False`, so that we
need to create a new variable in order to keep the outcome of the
methods. Setting `inplace = True` would apply **directly** the changes
to the original data frame without the need to create a variable.

# Renaming columns

When renaming a column, `pandas` expect that you define a `dictionary`
as follows:

`{'current_name' : 'new_name'}`

So if for example, we wanted to change the name of `species` and
`island` to respectively `penguin_name` and `land`, we can proceed as
follows:

    penguins.rename({'species': 'penguin_name', 'island':'land'}, 
                    axis = 'columns', 
                    inplace = True)
    penguins.head()

    ##   penguin_name       land  bill_length_mm  bill_depth_mm  flipper_length_mm  \
    ## 0       Adelie  Torgersen            39.1           18.7              181.0   
    ## 1       Adelie  Torgersen            39.5           17.4              186.0   
    ## 2       Adelie  Torgersen            40.3           18.0              195.0   
    ## 3       Adelie  Torgersen             NaN            NaN                NaN   
    ## 4       Adelie  Torgersen            36.7           19.3              193.0   
    ## 
    ##    body_mass_g     sex  year  
    ## 0       3750.0    male  2007  
    ## 1       3800.0  female  2007  
    ## 2       3250.0  female  2007  
    ## 3          NaN     NaN  2007  
    ## 4       3450.0  female  2007

and voila ! here we had to set `axis = 'columns'` to specify that we’re
renaming columns not indexes (which is the default). Note that we didn’t
have to create a variable to keep the outcome of the `rename` method
thanks to `inplace` parameter.

It is also easy to add `prefix` and `suffix` characters to column names.
It’s a bit silly but let us add a `<<<` in front of all column names:

    penguins.add_prefix('<<<')

    ##     <<<penguin_name    <<<land  <<<bill_length_mm  <<<bill_depth_mm  \
    ## 0            Adelie  Torgersen               39.1              18.7   
    ## 1            Adelie  Torgersen               39.5              17.4   
    ## 2            Adelie  Torgersen               40.3              18.0   
    ## 3            Adelie  Torgersen                NaN               NaN   
    ## 4            Adelie  Torgersen               36.7              19.3   
    ## ..              ...        ...                ...               ...   
    ## 339       Chinstrap      Dream               55.8              19.8   
    ## 340       Chinstrap      Dream               43.5              18.1   
    ## 341       Chinstrap      Dream               49.6              18.2   
    ## 342       Chinstrap      Dream               50.8              19.0   
    ## 343       Chinstrap      Dream               50.2              18.7   
    ## 
    ##      <<<flipper_length_mm  <<<body_mass_g  <<<sex  <<<year  
    ## 0                   181.0          3750.0    male     2007  
    ## 1                   186.0          3800.0  female     2007  
    ## 2                   195.0          3250.0  female     2007  
    ## 3                     NaN             NaN     NaN     2007  
    ## 4                   193.0          3450.0  female     2007  
    ## ..                    ...             ...     ...      ...  
    ## 339                 207.0          4000.0    male     2009  
    ## 340                 202.0          3400.0  female     2009  
    ## 341                 193.0          3775.0    male     2009  
    ## 342                 210.0          4100.0    male     2009  
    ## 343                 198.0          3775.0  female     2009  
    ## 
    ## [344 rows x 8 columns]

Same thing with `add_suffix`:

    penguins.add_suffix('>>>')

    ##     penguin_name>>>    land>>>  bill_length_mm>>>  bill_depth_mm>>>  \
    ## 0            Adelie  Torgersen               39.1              18.7   
    ## 1            Adelie  Torgersen               39.5              17.4   
    ## 2            Adelie  Torgersen               40.3              18.0   
    ## 3            Adelie  Torgersen                NaN               NaN   
    ## 4            Adelie  Torgersen               36.7              19.3   
    ## ..              ...        ...                ...               ...   
    ## 339       Chinstrap      Dream               55.8              19.8   
    ## 340       Chinstrap      Dream               43.5              18.1   
    ## 341       Chinstrap      Dream               49.6              18.2   
    ## 342       Chinstrap      Dream               50.8              19.0   
    ## 343       Chinstrap      Dream               50.2              18.7   
    ## 
    ##      flipper_length_mm>>>  body_mass_g>>>  sex>>>  year>>>  
    ## 0                   181.0          3750.0    male     2007  
    ## 1                   186.0          3800.0  female     2007  
    ## 2                   195.0          3250.0  female     2007  
    ## 3                     NaN             NaN     NaN     2007  
    ## 4                   193.0          3450.0  female     2007  
    ## ..                    ...             ...     ...      ...  
    ## 339                 207.0          4000.0    male     2009  
    ## 340                 202.0          3400.0  female     2009  
    ## 341                 193.0          3775.0    male     2009  
    ## 342                 210.0          4100.0    male     2009  
    ## 343                 198.0          3775.0  female     2009  
    ## 
    ## [344 rows x 8 columns]

and finally, we can do both:

    print(penguins.add_prefix('<<<').add_suffix('>>>'))

    ##     <<<penguin_name>>> <<<land>>>  <<<bill_length_mm>>>  <<<bill_depth_mm>>>  \
    ## 0               Adelie  Torgersen                  39.1                 18.7   
    ## 1               Adelie  Torgersen                  39.5                 17.4   
    ## 2               Adelie  Torgersen                  40.3                 18.0   
    ## 3               Adelie  Torgersen                   NaN                  NaN   
    ## 4               Adelie  Torgersen                  36.7                 19.3   
    ## ..                 ...        ...                   ...                  ...   
    ## 339          Chinstrap      Dream                  55.8                 19.8   
    ## 340          Chinstrap      Dream                  43.5                 18.1   
    ## 341          Chinstrap      Dream                  49.6                 18.2   
    ## 342          Chinstrap      Dream                  50.8                 19.0   
    ## 343          Chinstrap      Dream                  50.2                 18.7   
    ## 
    ##      <<<flipper_length_mm>>>  <<<body_mass_g>>> <<<sex>>>  <<<year>>>  
    ## 0                      181.0             3750.0      male        2007  
    ## 1                      186.0             3800.0    female        2007  
    ## 2                      195.0             3250.0    female        2007  
    ## 3                        NaN                NaN       NaN        2007  
    ## 4                      193.0             3450.0    female        2007  
    ## ..                       ...                ...       ...         ...  
    ## 339                    207.0             4000.0      male        2009  
    ## 340                    202.0             3400.0    female        2009  
    ## 341                    193.0             3775.0      male        2009  
    ## 342                    210.0             4100.0      male        2009  
    ## 343                    198.0             3775.0    female        2009  
    ## 
    ## [344 rows x 8 columns]

What we did here is called `method chaining`. We applied the
`add_suffix` method on the result of `penguins.add_prefix('<<<')`.

# Rows slicing

There are two methods available in the `pandas` library to slice rows:
`loc` and `iloc`. There is a subtle difference between the two:

-   `loc` allows you to slice rows using the index **label**;
-   `iloc` instead is integer position based and allows row slicing the
    same way we slice `lists` objects.

It’s a bit confusing I know, let’s dive into an example to illustrate
those differences. Let’s say we want to get the rows 0, 11 and 31 from
our data frame:


    penguins.iloc[[0, 11, 31]]

    ##    penguin_name       land  bill_length_mm  bill_depth_mm  flipper_length_mm  \
    ## 0        Adelie  Torgersen            39.1           18.7              181.0   
    ## 11       Adelie  Torgersen            37.8           17.3              180.0   
    ## 31       Adelie      Dream            37.2           18.1              178.0   
    ## 
    ##     body_mass_g   sex  year  
    ## 0        3750.0  male  2007  
    ## 11       3700.0   NaN  2007  
    ## 31       3900.0  male  2007

Pretty simple right ? now what if wanted to subset all the rows from 250
to the last row (343):

    penguins.iloc[250:]

    ##     penguin_name    land  bill_length_mm  bill_depth_mm  flipper_length_mm  \
    ## 250       Gentoo  Biscoe            48.4           14.4              203.0   
    ## 251       Gentoo  Biscoe            51.1           16.5              225.0   
    ## 252       Gentoo  Biscoe            48.5           15.0              219.0   
    ## 253       Gentoo  Biscoe            55.9           17.0              228.0   
    ## 254       Gentoo  Biscoe            47.2           15.5              215.0   
    ## ..           ...     ...             ...            ...                ...   
    ## 339    Chinstrap   Dream            55.8           19.8              207.0   
    ## 340    Chinstrap   Dream            43.5           18.1              202.0   
    ## 341    Chinstrap   Dream            49.6           18.2              193.0   
    ## 342    Chinstrap   Dream            50.8           19.0              210.0   
    ## 343    Chinstrap   Dream            50.2           18.7              198.0   
    ## 
    ##      body_mass_g     sex  year  
    ## 250       4625.0  female  2009  
    ## 251       5250.0    male  2009  
    ## 252       4850.0  female  2009  
    ## 253       5600.0    male  2009  
    ## 254       4975.0  female  2009  
    ## ..           ...     ...   ...  
    ## 339       4000.0    male  2009  
    ## 340       3400.0  female  2009  
    ## 341       3775.0    male  2009  
    ## 342       4100.0    male  2009  
    ## 343       3775.0  female  2009  
    ## 
    ## [94 rows x 8 columns]

We can also easily subset the last 10 rows:

    penguins.iloc[-10:]

    ##     penguin_name   land  bill_length_mm  bill_depth_mm  flipper_length_mm  \
    ## 334    Chinstrap  Dream            50.2           18.8              202.0   
    ## 335    Chinstrap  Dream            45.6           19.4              194.0   
    ## 336    Chinstrap  Dream            51.9           19.5              206.0   
    ## 337    Chinstrap  Dream            46.8           16.5              189.0   
    ## 338    Chinstrap  Dream            45.7           17.0              195.0   
    ## 339    Chinstrap  Dream            55.8           19.8              207.0   
    ## 340    Chinstrap  Dream            43.5           18.1              202.0   
    ## 341    Chinstrap  Dream            49.6           18.2              193.0   
    ## 342    Chinstrap  Dream            50.8           19.0              210.0   
    ## 343    Chinstrap  Dream            50.2           18.7              198.0   
    ## 
    ##      body_mass_g     sex  year  
    ## 334       3800.0    male  2009  
    ## 335       3525.0  female  2009  
    ## 336       3950.0    male  2009  
    ## 337       3650.0  female  2009  
    ## 338       3650.0  female  2009  
    ## 339       4000.0    male  2009  
    ## 340       3400.0  female  2009  
    ## 341       3775.0    male  2009  
    ## 342       4100.0    male  2009  
    ## 343       3775.0  female  2009

Now let’s play a little bit with `loc`. If we run the same code as
previously we get the same results:


    penguins.loc[[0, 11, 31]]

    ##    penguin_name       land  bill_length_mm  bill_depth_mm  flipper_length_mm  \
    ## 0        Adelie  Torgersen            39.1           18.7              181.0   
    ## 11       Adelie  Torgersen            37.8           17.3              180.0   
    ## 31       Adelie      Dream            37.2           18.1              178.0   
    ## 
    ##     body_mass_g   sex  year  
    ## 0        3750.0  male  2007  
    ## 11       3700.0   NaN  2007  
    ## 31       3900.0  male  2007

Why is that ? simply because in our case index **labels** and **values**
are exactly the same. Now let’s change and see what happens, we’ll set
the `penguin_name` column as our new data frame index:


    indexed_penguins = penguins.set_index('penguin_name')

    indexed_penguins.head()

    ##                    land  bill_length_mm  bill_depth_mm  flipper_length_mm  \
    ## penguin_name                                                                
    ## Adelie        Torgersen            39.1           18.7              181.0   
    ## Adelie        Torgersen            39.5           17.4              186.0   
    ## Adelie        Torgersen            40.3           18.0              195.0   
    ## Adelie        Torgersen             NaN            NaN                NaN   
    ## Adelie        Torgersen            36.7           19.3              193.0   
    ## 
    ##               body_mass_g     sex  year  
    ## penguin_name                             
    ## Adelie             3750.0    male  2007  
    ## Adelie             3800.0  female  2007  
    ## Adelie             3250.0  female  2007  
    ## Adelie                NaN     NaN  2007  
    ## Adelie             3450.0  female  2007

The modifications are visible but we can double check:

Here the indexes of `penguins`:

    penguins.index

    ## RangeIndex(start=0, stop=344, step=1)

And below the indexes of `indexed_penguins`:

    indexed_penguins.index

    ## Index(['Adelie', 'Adelie', 'Adelie', 'Adelie', 'Adelie', 'Adelie', 'Adelie',
    ##        'Adelie', 'Adelie', 'Adelie',
    ##        ...
    ##        'Chinstrap', 'Chinstrap', 'Chinstrap', 'Chinstrap', 'Chinstrap',
    ##        'Chinstrap', 'Chinstrap', 'Chinstrap', 'Chinstrap', 'Chinstrap'],
    ##       dtype='object', name='penguin_name', length=344)

Now if we use `loc`, we’ll get a `KeyError`:

    indexed_penguins.loc[[0, 11, 31]]

    ## Error in py_call_impl(callable, dots$args, dots$keywords): KeyError: "None of [Int64Index([0, 11, 31], dtype='int64', name='penguin_name')] are in the [index]"
    ## 
    ## Detailed traceback: 
    ##   File "<string>", line 1, in <module>
    ##   File "C:\Users\ADMINI~1\AppData\Local\R-MINI~1\envs\R-RETI~1\lib\site-packages\pandas\core\indexing.py", line 879, in __getitem__
    ##     return self._getitem_axis(maybe_callable, axis=axis)
    ##   File "C:\Users\ADMINI~1\AppData\Local\R-MINI~1\envs\R-RETI~1\lib\site-packages\pandas\core\indexing.py", line 1099, in _getitem_axis
    ##     return self._getitem_iterable(key, axis=axis)
    ##   File "C:\Users\ADMINI~1\AppData\Local\R-MINI~1\envs\R-RETI~1\lib\site-packages\pandas\core\indexing.py", line 1037, in _getitem_iterable
    ##     keyarr, indexer = self._get_listlike_indexer(key, axis, raise_missing=False)
    ##   File "C:\Users\ADMINI~1\AppData\Local\R-MINI~1\envs\R-RETI~1\lib\site-packages\pandas\core\indexing.py", line 1254, in _get_listlike_indexer
    ##     self._validate_read_indexer(keyarr, indexer, axis, raise_missing=raise_missing)
    ##   File "C:\Users\ADMINI~1\AppData\Local\R-MINI~1\envs\R-RETI~1\lib\site-packages\pandas\core\indexing.py", line 1298, in _validate_read_indexer
    ##     raise KeyError(f"None of [{key}] are in the [{axis_name}]")

To work with `loc` we need to provide the index **label**:

    indexed_penguins.loc["Chinstrap"]

    ##                land  bill_length_mm  bill_depth_mm  flipper_length_mm  \
    ## penguin_name                                                            
    ## Chinstrap     Dream            46.5           17.9              192.0   
    ## Chinstrap     Dream            50.0           19.5              196.0   
    ## Chinstrap     Dream            51.3           19.2              193.0   
    ## Chinstrap     Dream            45.4           18.7              188.0   
    ## Chinstrap     Dream            52.7           19.8              197.0   
    ## ...             ...             ...            ...                ...   
    ## Chinstrap     Dream            55.8           19.8              207.0   
    ## Chinstrap     Dream            43.5           18.1              202.0   
    ## Chinstrap     Dream            49.6           18.2              193.0   
    ## Chinstrap     Dream            50.8           19.0              210.0   
    ## Chinstrap     Dream            50.2           18.7              198.0   
    ## 
    ##               body_mass_g     sex  year  
    ## penguin_name                             
    ## Chinstrap          3500.0  female  2007  
    ## Chinstrap          3900.0    male  2007  
    ## Chinstrap          3650.0    male  2007  
    ## Chinstrap          3525.0  female  2007  
    ## Chinstrap          3725.0    male  2007  
    ## ...                   ...     ...   ...  
    ## Chinstrap          4000.0    male  2009  
    ## Chinstrap          3400.0  female  2009  
    ## Chinstrap          3775.0    male  2009  
    ## Chinstrap          4100.0    male  2009  
    ## Chinstrap          3775.0  female  2009  
    ## 
    ## [68 rows x 7 columns]

Nevertheless, `iloc` works perfectly. It doesn’t care about the index
label, it only focuses on the index position (from 0 to n-1 rows):

    indexed_penguins.iloc[[0, 11, 31]]

    ##                    land  bill_length_mm  bill_depth_mm  flipper_length_mm  \
    ## penguin_name                                                                
    ## Adelie        Torgersen            39.1           18.7              181.0   
    ## Adelie        Torgersen            37.8           17.3              180.0   
    ## Adelie            Dream            37.2           18.1              178.0   
    ## 
    ##               body_mass_g   sex  year  
    ## penguin_name                           
    ## Adelie             3750.0  male  2007  
    ## Adelie             3700.0   NaN  2007  
    ## Adelie             3900.0  male  2007

Note that both `loc` and `iloc` allow you to subset rows and columns. To
get the first 10 rows and the last two column, we can do:

    # remember that -1 is the integer index of last column
    penguins.iloc[10:, [-2, -1]]

    ##         sex  year
    ## 10      NaN  2007
    ## 11      NaN  2007
    ## 12   female  2007
    ## 13     male  2007
    ## 14     male  2007
    ## ..      ...   ...
    ## 339    male  2009
    ## 340  female  2009
    ## 341    male  2009
    ## 342    male  2009
    ## 343  female  2009
    ## 
    ## [334 rows x 2 columns]

A quick drawback for `iloc` is that it’s not possible to use column
names to subset a data frame, so the following code will throw an error:


    penguins.iloc[10:, ['sex', 'year']]

    ## Error in py_call_impl(callable, dots$args, dots$keywords): IndexError: .iloc requires numeric indexers, got ['sex' 'year']
    ## 
    ## Detailed traceback: 
    ##   File "<string>", line 1, in <module>
    ##   File "C:\Users\ADMINI~1\AppData\Local\R-MINI~1\envs\R-RETI~1\lib\site-packages\pandas\core\indexing.py", line 873, in __getitem__
    ##     return self._getitem_tuple(key)
    ##   File "C:\Users\ADMINI~1\AppData\Local\R-MINI~1\envs\R-RETI~1\lib\site-packages\pandas\core\indexing.py", line 1443, in _getitem_tuple
    ##     self._has_valid_tuple(tup)
    ##   File "C:\Users\ADMINI~1\AppData\Local\R-MINI~1\envs\R-RETI~1\lib\site-packages\pandas\core\indexing.py", line 702, in _has_valid_tuple
    ##     self._validate_key(k, i)
    ##   File "C:\Users\ADMINI~1\AppData\Local\R-MINI~1\envs\R-RETI~1\lib\site-packages\pandas\core\indexing.py", line 1363, in _validate_key
    ##     raise IndexError(f".iloc requires numeric indexers, got {arr}")

On the other hand, `loc` does permit column subsetting by names:

    penguins.loc[10:, ['sex', 'year']]

    ##         sex  year
    ## 10      NaN  2007
    ## 11      NaN  2007
    ## 12   female  2007
    ## 13     male  2007
    ## 14     male  2007
    ## ..      ...   ...
    ## 339    male  2009
    ## 340  female  2009
    ## 341    male  2009
    ## 342    male  2009
    ## 343  female  2009
    ## 
    ## [334 rows x 2 columns]
