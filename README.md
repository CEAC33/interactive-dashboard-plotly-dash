# interactive-dashboard-plotly-dash

## Covid-19 Dashboard

### Import Datasets links from GitHub

index.py

```python
import dash
import dash_html_components as html
import dash_core_components as dcc
from dash.dependencies import Input, Output
import plotly.graph_objs as go
import pandas as pd

url_confirmed = 'https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_confirmed_global.csv'
url_deaths = 'https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_deaths_global.csv'
url_recovered = 'https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_recovered_global.csv'

confirmed = pd.read_csv(url_confirmed)
deaths = pd.read_csv(url_deaths)
recovered = pd.read_csv(url_recovered)
```

### Unpivot the Columns of Datasets in Pandas

![image](https://user-images.githubusercontent.com/51218415/159982312-5ea16cc6-8013-48f1-a997-a215cda9eff0.png)


```python
# Unpivot data frames
date1 = confirmed.columns[4:]
total_confirmed = confirmed.melt(id_vars=['Province/State', 'Country/Region', 'Lat', 'Long'], value_vars=date1, var_name='date', value_name='confirmed')
date2 = deaths.columns[4:]
total_deaths = deaths.melt(id_vars=['Province/State', 'Country/Region', 'Lat', 'Long'], value_vars=date2, var_name='date', value_name='death')
date3 = recovered.columns[4:]
total_recovered = recovered.melt(id_vars=['Province/State', 'Country/Region', 'Lat', 'Long'], value_vars=date3, var_name='date', value_name='recovered')
```

![image](https://user-images.githubusercontent.com/51218415/159985460-5d9d1772-c12b-4760-bc0d-9b62711f79af.png)

![image](https://user-images.githubusercontent.com/51218415/159985845-079e1d0d-61cd-4827-8b9d-6ecd6caa93b3.png)

![image](https://user-images.githubusercontent.com/51218415/159986200-38ad28b2-38b1-4cc4-8fdf-7b1ba1eb4491.png)

### Merging the Data Frames in Pandas

```python
# Merging data frames
covid_data = total_confirmed.merge(right = total_deaths, how = 'left', on = ['Province/State', 'Country/Region', 'date', 'Lat', 'Long'])
covid_data = covid_data.merge(right = total_recovered, how = 'left', on = ['Province/State', 'Country/Region', 'date', 'Lat', 'Long'])
```



