# ----> Analysis is Done By,
#    Om Bhut - 200031101611011
#    Vishvas Chauhan - 200031101611014

# **Netflix Dataset**

Data Analysis with Python

This Netflix Dataset has information about the TV Shows and Movies available on
Netflix till 2021.

This dataset is collected from Flixable which is a third-party Netflix search
engine, and available on Kaggle website for free.

```python
import pandas as pd
```

```python
from google.colab import files
uploaded = files.upload()
```

```python
!pip install https://github.com/aaren/notedown/tarball/master
```

```python
!notedown /content/FinalAnalysis.ipynb --to markdown --strip > output.md
```

```python
import io
df = pd.read_csv(io.BytesIO(uploaded['netflix_titles.csv']))
# Dataset is now stored in a Pandas Dataframe
```

```python
df
```

# **Getting some basic information about Dataset**

1.head()

```python
df.head()               #show top-5 records of dataset

```

2.tail()

```python
df.tail()     #shows bottom-5 recors of dataset
```

3.shape

```python
df.shape         # to show the No. of Rows and Columns
```

4.size

```python
df.size           # to show No. of total values(elements) in the dataset
```

5.columns

```python
df.columns
```

6.dtypes

```python
df.dtypes          # to show the data-type of each column
```

7.info()

```python
df.info()              # To show indexes, columns, data-types of each column, memory at once
```

# **-----> Data cleaning and preprocessing**




# ----->> Is there any Duplicate Record in this dataset ? If yes, then remove
the duplicate records.

duplicate()

```python
df.head()
```

```python
df[df.duplicated()]                        # To check row wise and detect the Duplicate rows
```

```python
df.drop_duplicates(inplace = True)                                # To Remove the Duplicate rows permanently
```

```python
df[df.duplicated()]
```

# ----> Is there any Null Value present in any column ? Show with Heat-map.

isnull()

```python
df.isnull()                                               # To show where Null value is present
```

```python
df.isnull().sum()                                               # To show the count of Null values in each column
```

```python
import seaborn as sns                                               # To import Seaborn library
```

```python
sns.heatmap(df.isnull())                                               # Using heat-map to show null values count
```

# ----->Handling Missing Data

# 1.Filling the missing 'Country' value with 'United States'

```python
df['country'].fillna('United States' , inplace=True)
df['country'].isnull().any()
```

```python
df.isnull().sum() 
```

# 2.Filling the missing 'Rating' value with 'TV-MA'.

```python
df['rating'].fillna('TV-MA' , inplace=True)
df['rating'].isnull().any()
```

```python
df.isnull().sum() 
```

# 3.Filling the missing 'duration' time with '150 minute '.

```python
df['duration'].fillna('150 minute' , inplace=True)
df['duration'].isnull().any()
```

```python
df.isnull().sum() 
```

# 4.Filling the missing 'date_added' value with 'september 20,2021'.

```python
df['date_added'].fillna('september 20, 2021' , inplace=True)
df['date_added'].isnull().any()
```

```python
df.isnull().sum() 
```

# 5.Filling the missing 'director' value with 'information is not avilable'.

```python
df['director'].fillna('information is not avilable' , inplace=True)
df['director'].isnull().any()
```

```python
df.isnull().sum()
```

# 6.Filling the missing 'cast' value with 'information is not avilable'.

```python
df['cast'].fillna('Tom criuse,Tom Ellis' , inplace=True)
df['cast'].isnull().any()
```

```python
df.isnull().sum()
```

```python

```

# **----> Questioning and Answering**

# Q.1. For 'The Conjuring', what is the Show Id and Who is the Director of this
show ?

isin()

```python
df.head()
```

```python
df[df['title'].isin(['The Conjuring'])]       #  To show all records of a particular item in any column
```

```python
df[df['title'].str.contains('The Conjuring')]    #To show all records of a particular string in any column
```

# Q.2. In which year highest number of the TV Shows & Movies were released ?
count graph.

```python
df.release_year.value_counts()[:20]
```

```python
import matplotlib.pyplot as plt
```

```python
plt.figure(figsize = (10,6))
sns.countplot(y='release_year' ,order = df['release_year'].value_counts().index[0:20],data = df)
plt.title('Content Release in Years on Netflix Vs Count')
```

# Q.3. How many Movies & TV Shows are in the dataset ? Show with Bar Graph.

groupby()

```python
df.head(2)
```

```python
df.groupby('type').type.count()                 # To group all unique items of a column and show their count
```

countplot()

```python
sns.countplot(df['type'])        # To show the count of all unique values of any column in the form of bar graph
```

# Q.4. Show all the Movies that were released in year 2019.

Filtering

```python
df[ (df['type'] == 'Movie') & (df['release_year']==2019) ]
```

# Q.5. Show only the Titles of all TV Shows that were released in India only.

Filtering

```python
df[ (df['type']=='TV Show') & (df['country']=='India') ] ['title']
```

# Q.6. Show Top 10 Directors, who gave the highest number of TV Shows & Movies
to Netflix ?

```python
df['director'].value_counts().head(10)
```

# Q.7. Show all the Records, where "type is Movie and genres is Comedies" or
"Country is United Kingdom".

```python
df[ (df['type']=='Movie') & (df['listed_in']=='Comedies') ]
```

```python
df[ (df['type']=='Movie') & (df['listed_in']=='Comedies') | (df['country']=='United Kingdom')]
```

# Q.8. In how many movies/shows, Tom Cruise was cast ?

```python
df[df['cast'].str.contains('Tom Cruise')]
```

# Q.9. What are the different Ratings defined by Netflix ?

unique()

```python
df['rating'].nunique()
```

```python
df['rating'].unique()
```

# Q.9.1. How many Movies got the 'TV-14' rating, in Canada ?

```python
df[(df['type']=='Movie') & (df['rating']=='TV-14')].shape
```

```python
df[(df['type']=='Movie') & (df['rating']=='TV-14') & (df['country']=='Canada')].shape
```

# Q.9.2. How many TV Show got the 'R' rating, after year 2018 ?

```python
df[(df['type']=='TV Show') & (df['rating']=='R')]
```

```python
df[(df['type']=='TV Show') & (df['rating']=='R') & (df['release_year'] > 2018)]
```

# Q.10.Show most given rating on netflix Using pie chart.

```python
df['rating'].value_counts().plot.pie(autopct='%1.1f%%',shadow=True,figsize=(50,8))
plt.show()
```

# Q.11. What is the maximum duration of a Movie/Show on Netflix ?

```python
df.duration.unique()
```

```python
df.duration.dtypes
```

```python
df[['Minutes', 'Unit']] = df['duration'].str.split(' ', expand = True)
```

```python
df['Minutes']=df['Minutes'].astype('int')
```

```python
df
```

```python
df['Minutes'].max()
```

```python
df['Minutes'].min()
```

# Q.12. Which individual country has the Highest No. of TV Shows ?

```python
df_tvshow = df[df['type'] == 'TV Show']
```

```python
df_tvshow.country.value_counts()
```

```python
df_tvshow.country.value_counts().head(1)
```

# Q.13.Top 10 Artist present on Netflix.

```python
plt.figure(figsize=(15,5))
sns.barplot(x = df["cast"].value_counts().head(10).index,
            y = df["cast"].value_counts().head(10).values)
plt.xticks(rotation=80)
plt.title("Top10 Artist present on Netflix",fontweight="bold")
plt.show()
```

# Q.14. How can we sort the dataset by Year ?

```python
df.sort_values(by = 'release_year')
```

```python
df.sort_values(by = 'release_year', ascending = False).head(10)
```

# Q.15. Find all the instances where :
#      Category is 'Movie' and Type is 'Dramas'
#      or
#     Category is 'TV Show' & Type is 'Kids' TV'

```python
df [ (df['type']=='Movie') & (df['listed_in']=='Dramas') ].head(2)
```

```python
df [ (df['type']=='TV Show') & (df['listed_in']== "Kids' TV") ]
```

```python
df [ (df['type']=='Movie') & (df['listed_in']=='Dramas') | (df['type']=='TV Show') & (df['listed_in']== "Kids' TV") ]
```

# Q.16. In how many movies/shows, kriti sannon was cast ?

```python
df[df['cast'].str.contains('Kriti Sanon')]
```

# Q.17.Top10 Genre in Movies and TV Shows.

```python
plt.figure(figsize=(15,5))
sns.barplot(x = df["listed_in"].value_counts().head(10).index,
            y = df["listed_in"].value_counts().head(10).values)
plt.xticks(rotation=80)
plt.title("Top10 Genre in Movies",fontweight="bold")
plt.show()
```

# Q.18. How many movies/shows was direct by Rohit Shetty ?

```python
df[df['director'].str.contains('Rohit Shetty')]
```

# Q.19. Tv Shows of Egypt?

```python
df[ (df['type']=='TV Show') & (df['country']=='Egypt') ] ['title']
```

# Q.20.List down title whose genres is Kids'TV and country is Russia.

```python
df [ (df['type']=='TV Show') & (df['listed_in']== "Kids' TV") & (df['country']=='Russia') ]
```

# Q.21.Movie duration by year.

```python
import matplotlib.pyplot as plt
```

```python
# Create a figure and increase the figure size
fig = plt.figure(figsize=(10,20))

# Create a scatter plot of duration versus year
plt.scatter(df['release_year'],df['Minutes'])

# Create a title
plt.title('Movie Duration by Year of Release')

# Show the plot
plt.show()
```


