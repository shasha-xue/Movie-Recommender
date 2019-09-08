# Generating An Executive Summary Of S&P 500 Index

All packages I applied can be download with pip and conda.
Please find the stock_analysis.py which can finish the whole precedure automatically.Below show more details about the code.

Input_1: Stock name: type: string, eg. '^GSPC'.
Input_2: Time period for the data to download. type: string, eg. '5y', valid periods: 1d,5d,1mo,3mo,6mo,1y,2y,5y,10y,ytd,max

Running code with below:
from stock_analysis import *
obj = stock_analysis()
obj.pdf_report('^GSPC', '5y')

Output_1: One page PDF report including plots and random text description. Only one figure, but three subfigures obtained with this script. The first subfigure is the candlestrick chart of 5 years. The second subfigure is the moving averages plots. The third subfigure is the 5 years data with predictions in the following 90 days.
Output_2: Figure is also saved in separate file for future use (maybe use).

## Work Flow

### Step 1. Download Data 

Automatically download recent 5 years stock data from Yahoo Finance by using yfinance package. 5 years counts from the day run the stock_report.py file.

Since the data can be download automatically, I didn't include how to use Pandas to load and read the csv file. Loading csv data with pandas can be done with following command:
import pandas as pd
df = pd.read_csv("path/data.csv")

### Step 2: PLot the candlestick chart of the data             

mpl_finance has the function to plot the candlestick chart of the data. The candlestick data only include the Data, Open, High, Low, Close. used quotes list in the code to include all data columns.

### Step 3: Plot the 30, 60, 120 day moving average            

Used Pandas rolling means to get the 30, 60, 120 days moving average and plot orginial data and all obtained moving averages plots in one subfigure.

### Step 4: Use ARIMA model to make predictions.          

Stock price is highly related with the prices in the last few day. So I chose to use Time Series to make the prediction. ARIMA model is applied in this problem.

In order to save time, the parameters of (p, d, q) are only chosen up to 3 and used intertools to generate the combinations of those three paramters. The Akaike Information Critera(AIC) values are used to compare the performance the models. AIC is widely used in measuring the model performance. It not only measures how goodness of the fit, but also tests the simplicity of the model.

After obtained the best model parameter, the last 20% of the original data(5 years data) are used to make in sample predictions. The results can be found in subfigure 3 (labeled as in sample prediction). In sample prediction worked very well.Then I predicted the next 90 days stock price using the trained model which you can find it in subfigure 3 with label Future 90 Days Prediction.

### Step 5: Automate to generate 1 page PDF including figure and random text          

ReportLab is used to exprot everything in one page PDF.
loremipsum is used to generate random text.
~                                                      
