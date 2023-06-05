
# urls
- https://ai-inter1.com/python-pandas/
- https://ai-inter1.com/pandas-dataframe_basic/

# install

```
pip install pandas
```

# basic

- Series : 1次元のデータ
- DataFrame : 2次元のデータ

- basic code

```
import pandas as pd

# create data frame data
list2 = [["P001", "iPhone 8 64GB", 1000],
         ["P002", "iPhone X 25GB", 2000],
         ["P003", "iPhone SE 3GB", 3000],
         ["P004", "iPhone SE 4GB", 4000],
         ["P005", "iPhone SE 5GB", 5000]]
columns2 = ["A", "B", "C"]
df01 = pd.DataFrame(data=list2, columns=columns2)

print( df01 )

##### COLUMN / LINE #####

print( df01.columns.values )			# show all columns name
print( df01.columns.to_list() )		# show all columns name
print( df01.columns[1:4].values )	# show columns name (1<= number < 4 : first column number is 0)
# [ 'col2', 'col3', 'col4' ]

# XXXX.loc : specify by name of column and line
print( df01.loc[:,['A']] )
print( df01.loc[:,['A','C']] ) # A and C
print( df01.loc[:,'A':'C'] )   # from A to C

# XXXX.iloc : specify by number of column and line (number:[0..n])
print( "# x: 0:3" )
print( df01.iloc[0:3,[0,1]] )
print( "# x: 0:2" )

print( "##### INDEX" )
print( df01.index )
print( "# add index to A" )
df01.set_index("A", inplace=True)
print( df01.index )
print( df01.index.values )

print( "# index (key, row)" )
print( "# index (iterrows(): iterate rows(Series))" )
for idx, row in df01.iterrows():
    print( idx )
    print( row )

print( "# index (itertuples(): iterate named tuple)" )
for row in df01.itertuples():
    print( row )

print( "##### COLUMNS" )
print( df01.columns )
print( "# columns name list" )
for col in df01:
    print( col )

print( "# columns name/item list" )
for name, item in df01.items():
    print( name )
    print( item )

# add line
df2 = pd.DataFrame(data = [["P004", "iPhone 7 32GB", 10000]], columns = columns2)
#ERROR# df01=df01.append(df2, ignore_index = True)

# add column
df01["Screen Size (in.)"] = [4.7, 5.8, 4, 4.7]

# delete "Screen Size (in.)" column
df01.drop("Screen Size (in.)",axis=1,inplace=True)

# delete line 2 and 3
df01.drop([2, 3], inplace = True)

##### INDEX #######
# create index
df01.set_index("A",inplace=True)

# get index line
for 
# delete index
df01.reset_index(inplace=True)

##### get data #####
# get head 3
df01.head(3)

```

