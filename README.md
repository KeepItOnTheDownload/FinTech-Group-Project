## CEO Compensation & Stock Prices Analysis

# 1. Intro
In this project we will evaluate CEO compensation, tenure, acceptance rate by board and shares owned by executives in order to predict stock prices for stocks belonging to the tech or financial sector 
   
In the vast landscape of the US equity market there were many sectors we could have focused on but for the purpose of this class we focused on finance and technology. We were able to look up the holdings of two of the biggest ETFs that ecapsulate these two sectors. XLF is a finance ETF that has holdings of 68 of the largest finance stocks in the U.S. economy. From this sector we were able to analyze the stock prices of banks like JP Morgan, Bank of America & Wells Fargo among others.

From the XLK etf we were able to determine the 68 biggest technology companies in the U.S. market. These companies were led by names like Microsoft, Apple, Adobe and Intel. Along with this information we were able to find data on CEO compensation going back to 2007. The benefit of choosing between these two sectors is that since 2007, Technology has been the leader in the recovery of the economy since the Great Financial Crisis. Meanwhile Financials has been a laggard, with multiple big banks coming under scrutiny for recieving bailouts from the U.S. taxpayers and CEO compensation becoming a big talking point. Should these banks that "failed" be rewarding their CEOs with gigantic salaries? Is there a correlation between these large salaries and the companies doing better? We try to answer these questions in the report.

![Sector](C:Users\boitz\CU-NYC-FIN-PT-08-2019-U-C\Project1\Pictures\sector_etfs.png)
  
We initally ran into some problems obtaining data from online APIs. Although we were able to access IEX Cloud for financial data, it limited us to the last 5 years. To solve this problem we gained access to a Bloomberg Terminal and were able to pull data going back 12 years to 2007. From this data source we were able to pull:
 - Stock prices of all the stocks in XLF and XLK
- The market cap of the stock
- CEO compensation dating back to 2007
- Shares owned by the CEO
- Approval of the board over the timeframe

#  Requirements: 

*  Use Pandas to clean and format your dataset(s).

* Create a Jupyter Notebook describing the **data exploration and cleanup** process.

* Create a Jupyter Notebook illustrating the **final data analysis**.

* Use PyViz, Panel, Plotly Express, and Hvplot to create six to eight visualizations of your data (ideally, at least two per question you ask of your data), and then aggregate these visualizations into a dashboard.

* Save PNG images of your visualizations to distribute to the class and instructional team and for inclusion in your presentation and your repo's README.md file.

* Use one new Python library that hasn't been covered in class.

* Optionally, use at least one API, if you can find an API with data pertinent to your primary research questions.

* Create a README.md in your repo with a write-up summarizing your major findings. This should include a heading for each question you asked of your data and under each heading a short description of what you found and any relevant plots.

# Process, Cleaning & Renaming:
### Relevant Libraries 
    import pandas as pd
    import numpy as np
    import datetime as dt
    from pathlib import Path
    import os
    import matplotlib.pyplot as plt
    import hvplot.pandas
    import plotly.express as px
    import matplotlib
    import panel as pn
    from panel.interact import interact
    from panel import widgets
    import matplotlib
    import missingno as msno
    get_ipython().run_line_magic('matplotlib', 'inline')

### CSV Files: 
    approval_fin.csv
    approval_tech.csv
    comp_fin.csv
    comp_tech.csv
    ktcap_fin.csv
    mktcap_tech.csv
    price_fin.csv
    price_tech.csv
    sector_prices.csv
    sharesceo_fin.csv
    sharesceo_tech.csv
    tenure_fin.csv
    tenure_tech.csv

## Process
Inorder to gather the necessary data to perfome meaningful analysis and adhear to the requirement we used the CSV files listed below and created them into DataFrames. As we initally ran into some problems obtaining data from online APIs. Although we were able to access IEX Cloud for financial data, it limited us to the last 5 years. To solve this problem we gained access to a Bloomberg Terminal and were able to pull data going back 12 years to 2007. From this data source we were able to pull:
* Stock prices of all the stocks in XLF and XLK
* The market cap of the stock
* CEO compensation dating back to 2007
* Shares owned by the CEO
* Approval of the board over the timeframe
  

After obtaining the data, the first step was cleaning the data. Loading in the csv documents, we initially changed the date to the index and later dropped the column for "Date". By doing this for all the datasets we could use the DateTimeIndex as the unviersal index throughout the data. Please note that the dataframes are not from the same lenghth.

*Picture of Setting DateTimeIndex

 Using a new package called Missingno, we were able to visualize what data we were missing. This new package will read through our dataframe and let us know what data was NaN or unavailable, allowing us to see where our data was most reliable and where it was not.

*Picture of Missingno

Using this new package, we were able to clean our data by removing all the rows with NaN values

*Picture of DropNa(how='all')

On top of checking the holdings of XLF and XLK for their performance over the timeframe, we were able to check the performance of each sector of the US economy

### 2. Reading approval rate of CEO for finanacial stocks
    temp_csv = Path("Resources/approval_fin.csv")
    data1_fin=pd.read_csv(temp_csv)
    data1_fin.set_index(pd.to_datetime(data1_fin['Date'], infer_datetime_format=True),inplace=True)
    data1_fin.drop(columns=['Date'], inplace=True)} 


### 3. Using Missingno to inspect the data and see how much missing data do we have.

Black represents there is a value, while white means that the data is missing 

    msno.matrix(data1_fin)


#Removing rows with all NaN values 
data1_fin = data1_fin.dropna(how='all')
data1_fin.head(3)


#%%
# Reading approval rate of CEO for technology stocks
temp_csv = Path("Resources/approval_tech.csv")
data1_tech=pd.read_csv(temp_csv)
data1_tech.set_index(pd.to_datetime(data1_tech['Date'], infer_datetime_format=True), inplace=True)
data1_tech.drop(columns=['Date'], inplace=True)


#%%
#Using new package to inspect the data and see how much missing data do we have.
msno.matrix(data1_tech)


#%%
#Removing rows with all NaN values 
data1_tech = data1_tech.dropna(how='all')
data1_tech.head(3)


#%%
# Reading compensation rate of CEO for financial stocks
temp_csv = Path("Resources/comp_fin.csv")
data2_fin=pd.read_csv(temp_csv)
data2_fin.set_index(pd.to_datetime(data2_fin['Date'], infer_datetime_format=True), inplace=True)
data2_fin.drop(columns=['Date'], inplace=True)
#Scaling by million 
data2_fin=data2_fin/1000000
data2_fin.head(3)


#%%
# Reading compensation rate of CEO for technology stocks
temp_csv = Path("Resources/comp_tech.csv")
data2_tech=pd.read_csv(temp_csv)
data2_tech.set_index(pd.to_datetime(data2_tech['Date'], infer_datetime_format=True), inplace=True)
data2_tech.drop(columns=['Date'], inplace=True)
#Scaling by million 
data2_tech=data2_tech/1000000
data2_tech.head(3)


#%%
# Reading tenure of CEO for financials stocks - measure in years 
temp_csv = Path("Resources/tenure_fin.csv")
data3_fin=pd.read_csv(temp_csv)
data3_fin.set_index(pd.to_datetime(data3_fin['Date'], infer_datetime_format=True), inplace=True)
data3_fin.drop(columns=['Date'], inplace=True)
data3_fin.head(3)


#%%
# Reading tenure of CEO for technology stocks - measure in years 
temp_csv = Path("Resources/tenure_tech.csv")
data3_tech=pd.read_csv(temp_csv)
data3_tech.set_index(pd.to_datetime(data3_tech['Date'], infer_datetime_format=True), inplace=True)
data3_tech.drop(columns=['Date'], inplace=True)
data3_tech.head(3)


#%%
# Reading shares owned by ceo as % of shares outstanding for financial stocks
temp_csv = Path("Resources/sharesceo_fin.csv")
data4_fin=pd.read_csv(temp_csv)
data4_fin.set_index(pd.to_datetime(data4_fin['Date'], infer_datetime_format=True), inplace=True)
data4_fin.drop(columns=['Date'], inplace=True)


#%%
#Using new package to inspect the data and see how much missing data do we have.
msno.matrix(data4_fin)
##only 4 years worth of data .. we need to analyze this data by year 


#%%
#Removing rows with all NaN values 
data4_fin = data4_fin.dropna(how='all')
data4_fin.head()


#%%
# Reading shares owned by ceo as % of shares outstanding for technology stocks
temp_csv = Path("Resources/sharesceo_tech.csv")
data4_tech=pd.read_csv(temp_csv)
data4_tech.set_index(pd.to_datetime(data4_tech['Date'], infer_datetime_format=True), inplace=True)
data4_tech.drop(columns=['Date'], inplace=True)


#%%
#Using new package to inspect the data and see how much missing data do we have.
msno.matrix(data4_tech)
##only 4 years worth of data .. we need to analyze this data by year 


#%%
#Removing rows with all NaN values
data4_tech = data4_tech.dropna(how='all')
data4_tech.head()


#%%
# Reading price data for financial stocks
temp_csv = Path("Resources/price_fin.csv")
price_fin=pd.read_csv(temp_csv)
price_fin.set_index(pd.to_datetime(price_fin['Date'], infer_datetime_format=True), inplace=True)
price_fin.drop(columns=['Date'], inplace=True)
price_fin.head(3)


#%%
# Removing non trading dates from the price data 
price_fin = price_fin.dropna(how='all')
price_fin.head(3)


#%%
# Reading price data for technology stocks 
temp_csv = Path("Resources/price_tech.csv")
price_tech=pd.read_csv(temp_csv)
price_tech.set_index(pd.to_datetime(price_tech['Date'], infer_datetime_format=True), inplace=True)
price_tech.drop(columns=['Date'], inplace=True)
price_tech.head(3)


#%%
# Removing non trading dates from the price data 
price_tech = price_tech.dropna(how='all')
price_tech.head(3)


#%%
# Reading market cap - financial stocks
temp_csv = Path("Resources/mktcap_fin.csv")
mkt_cap_fin=pd.read_csv(temp_csv)
mkt_cap_fin.set_index(pd.to_datetime(mkt_cap_fin['Date'], infer_datetime_format=True), inplace=True)
mkt_cap_fin.drop(columns=['Date'], inplace=True)
mkt_cap_fin.head()


#%%
# Reading market cap - technology stocks 
temp_csv = Path("Resources/mktcap_tech.csv")
mkt_cap_tech=pd.read_csv(temp_csv)
mkt_cap_tech.set_index(pd.to_datetime(mkt_cap_tech['Date'], infer_datetime_format=True), inplace=True)
mkt_cap_tech.drop(columns=['Date'], inplace=True)
mkt_cap_tech.head()


#%%
# Reading prices for sector ETFs
temp_csv = Path("Resources/sector_prices.csv")
price_sector=pd.read_csv(temp_csv)
price_sector.set_index(pd.to_datetime(price_sector['Date'], infer_datetime_format=True), inplace=True)
price_sector.drop(columns=['Date'], inplace=True)
price_sector.head(3)


#%%
# Removing non trading dates from the price data 
price_sector = price_sector.dropna(how='all')
price_sector.head(3)


#%%
#Calculating sector returns 
ret_sector = price_sector.pct_change()
ret_sector = ret_sector.dropna(how='all')
ret_sector.head(3)


#%%
#Calculating cumulative returns -
cumulative_returns_sector = (1 + ret_sector).cumprod()
cumulative_returns_sector.head()


#%%
#Using new package to inspect the data and see how much missing data do we have.
msno.matrix(ret_sector) 
#XLRE missing the majority of the data -- this classification for real state is a new convention


#%%
#Calculating cumulative returns by sector ETFs 
cumulative_returns_sector.hvplot.line( title='Sector ETFs and SP500', width=900, height=400)


#%%
#check dispersion on compensation metric 
# Box plot to visually see range of values per name
data2_fin.hvplot.box(figsize=(25,15),rot=90,group_label='Financial Stocks', legend=False, value_label='Compensation', title='Dispersion on compensation metric')


#%%
#check dispersion on compensation metric 
# Box plot to visually see range of values per name
data2_tech.hvplot.box(figsize=(25,15),rot=90,group_label='Tech Stocks', legend=False, value_label='Compensation', title='Dispersion on compensation metric')
##Given that the distribution is tighter for tech we should expect finantial metrics to have more predictability


#%%
#check dispersion on tenure metric 
# Box plot to visually see range of values per name
data3_fin.hvplot.box(figsize=(25,15),rot=90,group_label='Financial Stocks', legend=False, value_label='Tenure', title='Dispersion on tenure metric')


#%%
#check dispersion on tenure metric 
# Box plot to visually see range of values per name
data3_tech.hvplot.box(figsize=(25,15),rot=90,group_label='Tech Stocks', legend=False, value_label='Tenure', title='Dispersion on tenure metric')


#%%
#check dispersion on approval metric 
# Box plot to visually see range of values per name
data1_fin.hvplot.box(figsize=(25,15),rot=90,group_label='Financial Stocks', legend=False, value_label='Approval', title='Dispersion on approval metric')


#%%
#check dispersion on approval metric 
# Box plot to visually see range of values per name
data1_tech.hvplot.box(figsize=(25,15),rot=90,group_label='Tech Stocks', legend=False, value_label='Approval', title='Dispersion on approval metric')


#%%
#check dispersion on share ownership metric 
# Box plot to visually see range of values per name
data4_fin.hvplot.box(figsize=(25,15),rot=90,group_label='Financial Stocks', legend=False, value_label='Ownership', title='Dispersion on Ownership metric')


#%%
#check dispersion on share ownership metric 
# Box plot to visually see range of values per name
data4_tech.hvplot.box(figsize=(25,15),rot=90,group_label='Tech Stocks', legend=False, value_label='Ownership', title='Dispersion on Ownership metric')


#%%
#creating data frame with yearly prices 
#Finacial stocks 
price_fin.reset_index(inplace=True)
price_fin.Date = pd.to_datetime(price_fin.Date)
yearly_price_fin = price_fin.resample('Y', on='Date').last()
yearly_price_fin.drop(columns=['Date'], inplace=True)
yearly_price_fin.head()
#Tech stocks 
price_tech.reset_index(inplace=True)
price_tech.Date = pd.to_datetime(price_tech.Date)
yearly_price_tech = price_tech.resample('Y', on='Date').last()
yearly_price_tech.drop(columns=['Date'], inplace=True)
yearly_price_tech.head()


#%%
#Calculating yearly returns 
#Finanacial stocks 
ret_yearly_fin = yearly_price_fin.pct_change()
ret_yearly_fin = ret_yearly_fin.dropna(how='all')
ret_yearly_fin.head(3)
#Tech Stocks 
ret_yearly_tech = yearly_price_tech.pct_change()
ret_yearly_tech = ret_yearly_tech.dropna(how='all')
ret_yearly_tech.head(3)


#%%
# Plotting the correlation between CEO compensation and future yearly return
#correlation analysis between compensation and yearly return - forward looking --  Financial Stocks

df1=ret_yearly_fin.reset_index()
df1.drop(columns=['Date'], inplace=True)
df2=data2_fin.reset_index()
df2.drop(columns=['Date'], inplace=True)
data_combined=df1.corrwith(df2, axis = 1, method= 'spearman' )
mean_fin=data_combined.mean()
mean_fin=pd.Series(mean_fin)
mean_fin=pd.concat([mean_fin]*11, ignore_index=True)
mean_fin.hvplot()
new_df = pd.Series(range(2008,2019))

mean_fin=pd.concat([new_df,mean_fin],axis="columns", join="inner")
mean_fin.columns=['year','mean correlation']
mean_fin.set_index('year', inplace=True)

data_combined=pd.concat([new_df,data_combined],axis="columns", join="inner")
data_combined.columns=['year','correlation']
data_combined.set_index('year', inplace=True)
data_combined.hvplot(x='year', y='correlation',kind='scatter',title='correlation between CEO compensation and future yearly return')* mean_fin.hvplot(line_color='red', line_width=0.5,ylim=(-0.5, 0.5))

#horizontal line is the mean value over the period analyzed
#WE can see that for financial stocks the correlation is almost 0 over time, it fluctuates some years positive, others negative


#%%
# Plotting the correlation between CEO compensation and future yearly return
#correlation analysis between compensation and yearly return - forward looking  Tech Stocks

df1=ret_yearly_tech.reset_index()
df1.drop(columns=['Date'], inplace=True)
df2=data2_tech.reset_index()
df2.drop(columns=['Date'], inplace=True)
data_combined=df1.corrwith(df2, axis = 1, method= 'spearman' )
mean_fin=data_combined.mean()
mean_fin=pd.Series(mean_fin)
mean_fin=pd.concat([mean_fin]*11, ignore_index=True)
mean_fin.hvplot()
new_df = pd.Series(range(2008,2019))

mean_fin=pd.concat([new_df,mean_fin],axis="columns", join="inner")
mean_fin.columns=['year','mean correlation']
mean_fin.set_index('year', inplace=True)

data_combined=pd.concat([new_df,data_combined],axis="columns", join="inner")
data_combined.columns=['year','correlation']
data_combined.set_index('year', inplace=True)
data_combined.hvplot(x='year', y='correlation',kind='scatter',title='correlation between CEO compensation and future yearly return')* mean_fin.hvplot(line_color='red', line_width=0.5,ylim=(-0.5, 0.5))

#horizontal line is the mean value over the period analyzed
#WE can see that for tech stocks the correlation is negative over time, the more compensation the CEO gets the worst the stock performs in later years 
#Correlation is subtle .. not that high though. overall CEO comp is negatively correlated with future performance 


#%%
#correlation analysis between tenure and yearly return - forward looking -- Financial Stocks
df1=ret_yearly_fin.reset_index()
df1.drop(columns=['Date'], inplace=True)
df2=data3_fin.reset_index()
df2.drop(columns=['Date'], inplace=True)

data_combined=df1.corrwith(df2, axis = 1, method= 'spearman' )
mean_fin=data_combined.mean()
mean_fin=pd.Series(mean_fin)
mean_fin=pd.concat([mean_fin]*11, ignore_index=True)
mean_fin.hvplot()
new_df = pd.Series(range(2008,2019))

mean_fin=pd.concat([new_df,mean_fin],axis="columns", join="inner")
mean_fin.columns=['year','mean correlation']
mean_fin.set_index('year', inplace=True)

data_combined=pd.concat([new_df,data_combined],axis="columns", join="inner")
data_combined.columns=['year','correlation']
data_combined.set_index('year', inplace=True)
data_combined.hvplot(x='year', y='correlation',kind='scatter',title='correlation between CEO tenure and future yearly return')* mean_fin.hvplot(line_color='red', line_width=0.5,ylim=(-0.5, 0.5))

#horizontal line is the mean value over the period analyzed
#WE can see that for financial stocks the correlation is slightly positive over time, very small though, we need more data to validate 


#%%
#correlation analysis between tenure and yearly return - forward looking -- need to add dates 
df1=ret_yearly_tech.reset_index()
df1.drop(columns=['Date'], inplace=True)
df2=data3_tech.reset_index()
df2.drop(columns=['Date'], inplace=True)

data_combined=df1.corrwith(df2, axis = 1, method= 'spearman' )
mean_fin=data_combined.mean()
mean_fin=pd.Series(mean_fin)
mean_fin=pd.concat([mean_fin]*11, ignore_index=True)
mean_fin.hvplot()
new_df = pd.Series(range(2008,2019))

mean_fin=pd.concat([new_df,mean_fin],axis="columns", join="inner")
mean_fin.columns=['year','mean correlation']
mean_fin.set_index('year', inplace=True)

data_combined=pd.concat([new_df,data_combined],axis="columns", join="inner")
data_combined.columns=['year','correlation']
data_combined.set_index('year', inplace=True)
data_combined.hvplot(x='year', y='correlation',kind='scatter',title='correlation between CEO tenure and future yearly return')* mean_fin.hvplot(line_color='red', line_width=0.5,ylim=(-0.5, 0.5))

#horizontal line is the mean value over the period analyzed
#WE can see that for tech stocks the correlation is slightly positive over time. which means that if the CEO remains in the company 
#for a long period of time that is a good indication of future stock performance, while if there is a lot of turnover that is an 
#indication of stock underperformance in the future
#Tech stocks exhibit clearer pattern than financial stocks 


#%%
# YOUR CODE HERE!
#Creating scatter plots for shares owned by ceo as % of shares outstanding for financial stocks and yearly return
df1=ret_yearly_fin.reset_index()
df1.drop(columns=['Date'], inplace=True)
pn.extension()

new_index= ['2015', '2016', '2017','2018']

def rel_ownership_year(year):
    if year=='2015':
        
        datay2=pd.concat([df1.iloc[8],data4_fin.iloc[0]],axis="columns", join="inner")
        datay2.columns=['yearly_ret','shares']
  
    elif year=='2016':
        datay2=pd.concat([df1.iloc[9],data4_fin.iloc[1]],axis="columns", join="inner")
        datay2.columns=['yearly_ret','shares']
        
    elif year=='2017':
        datay2=pd.concat([df1.iloc[10],data4_fin.iloc[2]],axis="columns", join="inner")
        datay2.columns=['yearly_ret','shares']
    
    else:
        datay2=pd.concat([df1.iloc[11],data4_fin.iloc[3]],axis="columns", join="inner")
        datay2.columns=['yearly_ret','shares']

    return datay2.hvplot( kind='scatter',x='yearly_ret', y='shares',title='Relationship between shares owned by CEO and Future yearly return',xlim=(-1, 1),ylim=(0, 10))

interact(rel_ownership_year, year=new_index)

#The hypothesis we are trying to solve with this data is that we think that when there is more at stake for the CEO -financialy- 
#the more he/she be concern on managing well the company. it is midly confirm with the data. There is very little dispersion on the 
#metric.. CEOs own similar % for the majority of the stocks 


#%%

#Creating scatter plots for shares owned by ceo as % of shares outstanding for technology stocks and yearly return
df1=ret_yearly_tech.reset_index()
df1.drop(columns=['Date'], inplace=True)
pn.extension()

new_index= ['2015', '2016', '2017','2018']

def rel_ownership_year_tech(year):
    if year=='2015':
        
        datay2=pd.concat([df1.iloc[8],data4_tech.iloc[0]],axis="columns", join="inner")
        datay2.columns=['yearly_ret','shares']
  
    elif year=='2016':
        datay2=pd.concat([df1.iloc[9],data4_tech.iloc[1]],axis="columns", join="inner")
        datay2.columns=['yearly_ret','shares']
        
    elif year=='2017':
        datay2=pd.concat([df1.iloc[10],data4_tech.iloc[2]],axis="columns", join="inner")
        datay2.columns=['yearly_ret','shares']
    
    else:
        datay2=pd.concat([df1.iloc[11],data4_tech.iloc[3]],axis="columns", join="inner")
        datay2.columns=['yearly_ret','shares']

    return datay2.hvplot( kind='scatter',x='yearly_ret', y='shares',title='Relationship between shares owned by CEO and Future yearly return',xlim=(-1, 1),ylim=(0, 10))

interact(rel_ownership_year_tech, year=new_index)

#The hypothesis we are trying to solve with this data is that we think that when there is more at stake for the CEO -financialy- 
#the more he/she be concern on managing well the company. it is midly confirm with the data. There is very little dispersion on the 
#metric.. CEOs own similar % for the majority of the stocks 


#%%
#correlation analysis between approval and yearly return - forward looking 
df1=ret_yearly_fin.reset_index()
df1.drop(columns=['Date'], inplace=True)
df1.drop(df1.index[[0,1,2]],inplace=True)
df1=df1.reset_index()
df1.drop(columns=['index'], inplace=True)
df2=data1_fin.reset_index()
df2.drop(columns=['Date'], inplace=True)
df2.drop(df2.index[[0]],inplace=True)
df2=df2.reset_index()
df2.drop(columns=['index'], inplace=True)


data_combined=df1.corrwith(df2, axis = 1, method= 'spearman' )
mean_fin=data_combined.mean()
mean_fin=pd.Series(mean_fin)
mean_fin=pd.concat([mean_fin]*11, ignore_index=True)
mean_fin.hvplot()
new_df = pd.Series(range(2010,2020))

mean_fin=pd.concat([new_df,mean_fin],axis="columns", join="inner")
mean_fin.columns=['year','mean correlation']
mean_fin.set_index('year', inplace=True)

data_combined=pd.concat([new_df,data_combined],axis="columns", join="inner")
data_combined.columns=['year','correlation']
data_combined.set_index('year', inplace=True)
data_combined.hvplot(x='year', y='correlation',kind='scatter',title='correlation between CEO approval and future yearly return')* mean_fin.hvplot(line_color='red', line_width=0.5,ylim=(-0.5, 0.5))

#horizontal line is the mean value over the period analyzed
#WE can see that for financial stocks it does not appear that there is a relationship between approval and future performance 
#Correlation 0 over time. 


#%%
#correlation analysis between approval and yearly return - forward looking -- need to add dates 
df1=ret_yearly_tech.reset_index()
df1.drop(columns=['Date'], inplace=True)
df1.drop(df1.index[[0,1,2]],inplace=True)
df1=df1.reset_index()
df1.drop(columns=['index'], inplace=True)
df2=data1_tech.reset_index()
df2.drop(columns=['Date'], inplace=True)
df2.drop(df2.index[[0]],inplace=True)
df2=df2.reset_index()
df2.drop(columns=['index'], inplace=True)

data_combined=df1.corrwith(df2, axis = 1, method= 'spearman' )
mean_fin=data_combined.mean()
mean_fin=pd.Series(mean_fin)
mean_fin=pd.concat([mean_fin]*11, ignore_index=True)
mean_fin.hvplot()
new_df = pd.Series(range(2010,2020))

mean_fin=pd.concat([new_df,mean_fin],axis="columns", join="inner")
mean_fin.columns=['year','mean correlation']
mean_fin.set_index('year', inplace=True)

data_combined=pd.concat([new_df,data_combined],axis="columns", join="inner")
data_combined.columns=['year','correlation']
data_combined.set_index('year', inplace=True)
data_combined.hvplot(x='year', y='correlation',kind='scatter',title='correlation between CEO approval and future yearly return')* mean_fin.hvplot(line_color='red', line_width=0.5,ylim=(-0.5, 0.5))

#horizontal line is the mean value over the period analyzed
#WE can see that for tech stocks the correlation is positive over time. which means that if the CEO is widely approved by the board
#that is a good indication of stock outperfomance in the future 
#Tech pattern is more clear than financial stocks but confirms intuition where the more approval the better 


#%%
##Extract selecting year 2015 for tech stocks and visually see interactions 
#Compensation, approval, tenure
#We are calculating rank on each metric and we want to see the extreme values relationship given that 
#the correlation are low on the 3 metrics 

data_combined3=pd.concat([ret_yearly_tech.iloc[8],data2_tech.iloc[8],data1_tech.iloc[6],data3_tech.iloc[8]],axis="columns", join="inner")
data_combined3.columns=['Future_ret','compensation','approval','tenure']
data_combined3.dropna(inplace=True)
data_combined3=data_combined3.sort_values(by=['Future_ret'],ascending=False)
data_combined3["Rank_FutRet"] = data_combined3["Future_ret"].rank(ascending=False)
data_combined3["Rank_Compompensation"] = data_combined3["compensation"].rank(ascending=False) 
data_combined3["Rank_Approval"] = data_combined3["approval"].rank(ascending=False) 
data_combined3["Rank_Tenure"] = data_combined3["tenure"].rank(ascending=False) 
data_combined3.drop(columns=['Future_ret','compensation','approval','tenure'], inplace=True)
data_combined3=data_combined3.drop(data_combined3.index[15:46])


fig=px.parallel_coordinates(data_combined3, color='Rank_FutRet')
fig.show()

#very hard to see from the plot itself, but this plot was a requirement on the project
