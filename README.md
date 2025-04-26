# SQL-PROJECTS-
A collection of my SQL files for my LinkedIn Learning SLQ for Data Analysis Final Data Visualization Project.

##INTERACTIVE DASHBOARD BY STATE FOR SALES

### We will create a View from Order Summary 
 CREATE VIEW V_CustomerOrderSummary as
SELECT State,
  AVG(TotalDue) as Avg_Due,
  MIN(TotalDue) as Min_Due,
  MAX(TotalDue) as Max_Due,
  SUM(TotalDue) as Total_Sales
FROM Customer C
  JOIN Orders O ON O.CustomerID = C.CustomerID
GROUP BY State

### The view of our V_Orders
CREATE OR REPLACE VIEW V_Orders AS
SELECT O.CreationDate AS OrderDate,
  CONCAT(S.FirstName, ' ', S.LastName) AS Salesperson,
  COUNT(1) AS 'Amount of Orders',
  SUM(TotalDue) AS 'Total Due'
FROM Orders O
  join Salesperson S ON(O.SalespersonID = S.SalespersonID)
GROUP BY O.CreationDate,
  concat(S.FirstName, ' ', S.LastName)
  
# Creating a Web-based Dashboard using Dash
import dash
from dash import dcc
from dash import html

fig = go.Figure()

### I wanted to add four bar charts, each for a different metric Total Sales, Average Sales, Minimum Sales and Maximum Sales so I added each bar chart as a trace

fig.add_trace(
    go.Bar(x=list(df.State),
        y=list(df.Total_Sales),
        name='Total Sales',
        text=list(df.Total_Sales.round(1)),
           textposition="auto"))

fig.add_trace(
    go.Bar(x=list(df.State),
           y=list(df.Avg_Due),
           name='Average Sales',
           text=list(df.Avg_Due.round(1)),
           textposition="auto"))

fig.add_trace(
    go.Bar(x=list(df.State),
           y=list(df.Min_Due),
           name='Min Sales',
           text=list(df.Min_Due.round(1)),
           textposition="auto"))

fig.add_trace(
    go.Bar(x=list(df.State),
           y=list(df.Max_Due),
           name='Max Sales',
           text=list(df.Max_Due.round(1)),
           textposition="auto"))

![image](https://github.com/anjami-a/SQL-SALES-PROJECT-/blob/main/Screenshot%202025-04-26%20at%207.44.56%20AM.png)
![image](https://github.com/anjami-a/SQL-SALES-PROJECT-/blob/main/Screenshot%202025-04-26%20at%207.44.41%20AM.png)
![image](https://github.com/anjami-a/SQL-SALES-PROJECT-/blob/main/Screenshot%202025-04-26%20at%207.44.48%20AM.png)
![image](https://github.com/anjami-a/SQL-SALES-PROJECT-/blob/main/Screenshot%202025-04-26%20at%207.44.52%20AM.png)

### We need only once trace to be visible at a time (controlled via buttons), below is how I defined the interactive buttons for the chart

label_list = [
    dict(label='Total Sales',
          method="update",
        args=[{"visible": [True, False, False, False]},
               {"title": "H Plus Sport Order Summary",
                "annotations": []}]),
    dict(label='Average Sales',
         method="update",
         args=[{"visible": [False, True, False, False]},
               {"title": "H Plus Sport Order Summary",
               "annotations": []}]),
    dict(label='Min Sales',
          method="update",
        args=[{"visible": [False, False, True, False]},
               {"title": "H Plus Sport Order Summary",
                "annotations": []}]),
    dict(label='Max Sales',
         method="update",
         args=[{"visible": [False, False, False, True]},
               {"title": "H Plus Sport Order Summary",
               "annotations": []}])
]

### To add dropdown menus feature using the label list above I use fig.update_layout function  
  
fig.update_layout(
    updatemenus=[
        dict(
            active=0,
            buttons=label_list,
        )
    ])
app = dash.Dash()
app.layout = html.Div([
    dcc.Graph(figure=fig)
])

app.run(debug=True, use_reloader=False)

## SIMPLE BAR PLOT FOR SALES

### Import the libraries needed to run the code
import pandas as pd
import sqlalchemy as sa
import plotly.express as px
import plotly.graph_objects as go
from sqlalchemy.engine import create_engine

### Connecting to the HPlusSport database
engine = create_engine("mysql+pymysql://mariadb:mariadb@localhost/hplussport?charset=utf8mb4")
connection = engine.connect()

### Let's take a peak at our data. This code shows the first 5 records
df = pd.read_sql_query("SELECT * FROM V_CustomerOrderSummary",
                       connection)
df.head()

![Alt image](https://github.com/anjami-a/SQL-SALES-PROJECT-/blob/823f8e32f45525767516f0cf4c5dc87e3246e9ab/Screenshot%202025-04-20%20at%206.08.41%20AM.png)   

### Below is a simple bar chart of sales by state
fig = px.bar(df, x='State', y='Total_Sales')
fig.show()

![image](https://github.com/user-attachments/assets/f71ebb60-dc32-4f9a-a6e5-fa87de914e54)



  

