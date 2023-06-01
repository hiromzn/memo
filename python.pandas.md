
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
list2 = [["P001", "iPhone 8 64GB", 85000],
         ["P002", "iPhone X 256GB", 130000],
         ["P003", "iPhone SE 32GB", 37000]]
columns2 = ["Product ID", "Product Name", "Price (JPY)"]
df1 = pd.DataFrame(data=list2, columns=columns2)

print( df1 )

##### COLUMN / LINE #####
# add line
df2 = pd.DataFrame(data = [["P004", "iPhone 7 32GB", 10000]], columns = columns2)
#ERROR# df1=df1.append(df2, ignore_index = True)

# add column
df1["Screen Size (in.)"] = [4.7, 5.8, 4, 4.7]

# delete "Screen Size (in.)" column
df1.drop("Screen Size (in.)",axis=1,inplace=True)

# delete line 2 and 3
df1.drop([2, 3], inplace = True)

##### INDEX #######
# create index
df1.set_index("Product ID",inplace=True)

# delete index
df1.reset_index(inplace=True)

##### get data #####
# get head 3
df1.head(3)

```

