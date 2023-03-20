# importing the necessary libraries

import numpy as np

import pandas as pd

import matplotlib.pyplot as plt

import matplotlib.image as mpimg

%matplotlib inline

import seaborn as sns

import plotly.express as px


# importing the datasets

order_payments=pd.read_csv('C:\\Users\\arora\\OneDrive\\Desktop\\interview case study files\\Order Payments Dataset.csv')

order_reviews =pd.read_csv('C:\\Users\\arora\\OneDrive\\Desktop\\interview case study files\\Order Reviews Dataset.csv')

orders       = pd.read_csv('C:\\Users\\arora\\OneDrive\\Desktop\\interview case study files\\Orders Dataset.csv')

order_items  = pd.read_csv('C:\\Users\\arora\\OneDrive\\Desktop\\interview case study files\\Order items Dataset.csv')

customers    = pd.read_csv('C:\\Users\\arora\\OneDrive\\Desktop\\interview case study files\\Customers Dataset.csv')

products     = pd.read_csv('C:\\Users\\arora\\OneDrive\\Desktop\\interview case study files\\Product Dataset.csv')

# merging all the datasets

#### # merging all the orders table


orders_data1= pd.merge(orders,order_items,on='order_id')

#### #merging 'order_payments' and 'order_reviews' data 

orders_data2= pd.merge(order_payments,order_reviews,on='order_id')


#### #merging orders_data1 and orders_data2 to make final orders data


orders = pd.merge(orders_data1,orders_data2, on='order_id',how='outer')


#### orders #(final orders table)

#### #merging orders table with customers table on 'customer_id'

orders_customers = pd.merge(orders,customers, on='customer_id',how='outer')


orders_customers

#### # viewing orders and customers merged table.

#### #finally , merging the customers and orders data with the products data

final_data = pd.merge(orders_customers,products,on='product_id')

df=final_data

# exploring the dataset  


df.columns

df.shape

df.duplicated().sum()

df.isnull().sum()

#### #we can see that there are a few null values in 3 of the columns .

# cleaning the data

orders['order_purchase_timestamp'] = pd.to_datetime(orders['order_purchase_timestamp'], format='%d-%m-%Y %H:%M')

orders['order_delivered_customer_date'] = pd.to_datetime(orders['order_delivered_customer_date'], format='%d-%m-%Y %H:%M')

orders['order_estimated_delivery_date'] = pd.to_datetime(orders['order_estimated_delivery_date'], format='%d-%m-%Y %H:%M')

delivery_time = orders['order_delivered_customer_date'] - orders['order_purchase_timestamp']

est_delivery_time = orders['order_estimated_delivery_date'] - orders['order_purchase_timestamp']

est_real_delivery_time = est_delivery_time - delivery_time


#### #creating the column to calculate the price of a product 

df['price_of_product']=df['payment_installments']*df['payment_value']

df['price_of_product'].head()


# EDA

df['payment_type'].value_counts() #exploring what all payment types are used by the customers 

df['product_name_in_english'].value_counts() # these are all the products available

df['review_score'].value_counts()

#### #5 is the highest score any product gets and 2 is the lowest score

#### #adjusting some display settings of decimal points

pd.set_option('display.precision',2)

# visualisation of data



df['order_purchase_timestamp'].hist(figsize = (15,5), bins = 2*12+10 - 9 + 1)

plt.xlabel('Month')

plt.ylabel('Number of Orders')

plt.title('Number of Orders over time')

<"![no of orders over time](https://user-images.githubusercontent.com/108074039/226286052-8a7a742f-82b0-40a5-9e7c-08525bbc4a52.png)
">


delivery_time.astype('timedelta64[D]').plot.hist(figsize = (15,5), bins = 10)
plt.xlabel('Days')
plt.ylabel('Number of Orders')
plt.title('Delivery time in days')

<"![delivery time in days](https://user-images.githubusercontent.com/108074039/226286420-26d44a47-c6b4-4cac-906f-49c9b441568f.png)
">


est_delivery_time.astype('timedelta64[D]').plot.hist(figsize = (15,5), bins = 50)
plt.xlabel('Days')
plt.ylabel('Number of Orders')
plt.title('Estimated delivery time in days')

<"![estimated delivery time in days](https://user-images.githubusercontent.com/108074039/226286610-f6211ac7-725b-4798-a1a5-ec5c8cd0ea77.png)
">

est_real_delivery_time.astype('timedelta64[D]').plot.hist(figsize = (15,5), bins = 50)
plt.xlabel('Days')
plt.ylabel('Number of Orders')
plt.title('Difference between estimated and actual delivery time in days')

<"![difference between estimated and actual delivery time](https://user-images.githubusercontent.com/108074039/226286748-fa746e42-6e58-4281-b9be-fc4765bcea27.png)
">


import datetime as dt
order_weekdays = (orders['order_purchase_timestamp'].
    apply(lambda x: x.day_name()).
    value_counts())
plt.pie(
    order_weekdays,
    labels=order_weekdays.index,
    autopct='%1.1f%%',
    shadow=True,
    startangle=90
)
plt.title('Orders on different weekdays')

<"![number of orders by weekdays](https://user-images.githubusercontent.com/108074039/226287140-007b8192-c604-4f32-8b51-e09c40bf6164.png)
">

rare_products = df[['product_name_in_english']].value_counts()[-10:].plot.bar()
rare_products.set_xlabel('Product Category')
rare_products.set_ylabel('Ordered quantity')
rare_products.set_title('Ordered quantities for 10 least commonly ordered product categories')

<"![least 10 products](https://user-images.githubusercontent.com/108074039/226287275-c30e1edb-de88-41ab-b855-1cc05494d997.png)
">

rare_products = df[['product_name_in_english']].value_counts()[1:11].plot.bar()
rare_products.set_xlabel('Product Category')
rare_products.set_ylabel('Ordered quantity')
rare_products.set_title('Ordered quantities for 10 most commonly ordered product categories')
add Codeadd Markdown

<"![top 10 most ordered](https://user-images.githubusercontent.com/108074039/226287717-81eb081b-b8f4-4271-893f-a051637d53e1.png)

df['review_score'].value_counts(normalize=True).sort_index().plot()

#as we can see from the plot , most no of products have got good review scores.

<"![review scores](https://user-images.githubusercontent.com/108074039/226287859-3f10d1c9-7470-4f93-a950-739bc5d9fc36.png)



labels = 'once', 'more than once'
sizes = multiple_orders[1], sum(multiple_orders.values()) - multiple_orders[1]
explode = (0,0.1)
plt.pie(
    sizes,
    explode=explode,
    labels=labels,
    autopct='%1.1f%%',
    shadow=True,
    startangle=90
)
plt.title('How many times did a customer order?')

<"![order frequency](https://user-images.githubusercontent.com/108074039/226288213-acffed6a-9f62-4fc9-886e-18e11e386439.png)
">











