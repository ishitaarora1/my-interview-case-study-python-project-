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

*Note- You can click on the links given below to see each of the visuals respectively . You can also view them in the seperate png files uploaded in this repository.*

df['order_purchase_timestamp'].hist(figsize = (15,5), bins = 2*12+10 - 9 + 1)

plt.xlabel('Month')

plt.ylabel('Number of Orders')

plt.title('Number of Orders over time')

<"C:\Users\arora\OneDrive\Desktop\interview case study files\no of orders over time.png">









