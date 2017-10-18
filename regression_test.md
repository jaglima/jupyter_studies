
# Time serie regression analysis

In this post we will see a regression analysis over weather station data and reanalysis data. We downloaded NOAA NCEP data from here and station from here. The data bases from the station can be downloaded here.

Regression analysis goal is to test if there are a linear relationship between two variables. In this case we want to test if simulated data in the reanalysis process represents in a acceptable manner the observed data over a geographical point. 

Normally regression data is composed by three stepes: regression, errors analysis and hipoteses tests. 


1. Adjusting data

We want to compute the ETCCDI index for minimuns of monthly maximum daily temperatures. According with ETCCDI, this index is defined as TXn. 

Using a regression, we want adjust a line between the observed data and simulated data. In a perfect simulation we would have an 45º degrees fitted line, what 


```python
import pandas as pd
path = 'data/'
ini = '1996-01-01'
end = '2016-11-30'
locations = ['BARBALHA']

stations = pd.read_csv(path + 'station_tmax_BARBALHA.csv', index_col=0, parse_dates=True)
reanalysis = pd.read_csv(path + 'noaa_reanalysis_tmax_BARBALHA.csv', index_col=0, parse_dates=True)

```

Taking a look in the data loaded


```python
stations
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BARBALHA</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1996-01-01</th>
      <td>33.2</td>
    </tr>
    <tr>
      <th>1996-01-02</th>
      <td>34.4</td>
    </tr>
    <tr>
      <th>1996-01-03</th>
      <td>34.3</td>
    </tr>
    <tr>
      <th>1996-01-04</th>
      <td>33.9</td>
    </tr>
    <tr>
      <th>1996-01-05</th>
      <td>34.9</td>
    </tr>
    <tr>
      <th>1996-01-06</th>
      <td>33.8</td>
    </tr>
    <tr>
      <th>1996-01-07</th>
      <td>32.5</td>
    </tr>
    <tr>
      <th>1996-01-08</th>
      <td>32.7</td>
    </tr>
    <tr>
      <th>1996-01-09</th>
      <td>33.1</td>
    </tr>
    <tr>
      <th>1996-01-10</th>
      <td>34.7</td>
    </tr>
    <tr>
      <th>1996-01-11</th>
      <td>33.3</td>
    </tr>
    <tr>
      <th>1996-01-12</th>
      <td>34.0</td>
    </tr>
    <tr>
      <th>1996-01-13</th>
      <td>32.4</td>
    </tr>
    <tr>
      <th>1996-01-14</th>
      <td>35.0</td>
    </tr>
    <tr>
      <th>1996-01-15</th>
      <td>27.3</td>
    </tr>
    <tr>
      <th>1996-01-16</th>
      <td>30.4</td>
    </tr>
    <tr>
      <th>1996-01-17</th>
      <td>25.3</td>
    </tr>
    <tr>
      <th>1996-01-18</th>
      <td>30.4</td>
    </tr>
    <tr>
      <th>1996-01-19</th>
      <td>29.4</td>
    </tr>
    <tr>
      <th>1996-01-20</th>
      <td>30.5</td>
    </tr>
    <tr>
      <th>1996-01-21</th>
      <td>28.1</td>
    </tr>
    <tr>
      <th>1996-01-22</th>
      <td>30.7</td>
    </tr>
    <tr>
      <th>1996-01-23</th>
      <td>31.7</td>
    </tr>
    <tr>
      <th>1996-01-24</th>
      <td>30.9</td>
    </tr>
    <tr>
      <th>1996-01-25</th>
      <td>32.5</td>
    </tr>
    <tr>
      <th>1996-01-26</th>
      <td>34.5</td>
    </tr>
    <tr>
      <th>1996-01-27</th>
      <td>32.7</td>
    </tr>
    <tr>
      <th>1996-01-28</th>
      <td>31.3</td>
    </tr>
    <tr>
      <th>1996-01-29</th>
      <td>31.9</td>
    </tr>
    <tr>
      <th>1996-01-30</th>
      <td>35.9</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>2016-11-01</th>
      <td>36.7</td>
    </tr>
    <tr>
      <th>2016-11-02</th>
      <td>36.1</td>
    </tr>
    <tr>
      <th>2016-11-03</th>
      <td>36.0</td>
    </tr>
    <tr>
      <th>2016-11-04</th>
      <td>36.4</td>
    </tr>
    <tr>
      <th>2016-11-05</th>
      <td>36.3</td>
    </tr>
    <tr>
      <th>2016-11-06</th>
      <td>36.8</td>
    </tr>
    <tr>
      <th>2016-11-07</th>
      <td>36.7</td>
    </tr>
    <tr>
      <th>2016-11-08</th>
      <td>35.9</td>
    </tr>
    <tr>
      <th>2016-11-09</th>
      <td>35.1</td>
    </tr>
    <tr>
      <th>2016-11-10</th>
      <td>36.4</td>
    </tr>
    <tr>
      <th>2016-11-11</th>
      <td>36.5</td>
    </tr>
    <tr>
      <th>2016-11-12</th>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2016-11-13</th>
      <td>36.8</td>
    </tr>
    <tr>
      <th>2016-11-14</th>
      <td>37.0</td>
    </tr>
    <tr>
      <th>2016-11-15</th>
      <td>36.1</td>
    </tr>
    <tr>
      <th>2016-11-16</th>
      <td>35.7</td>
    </tr>
    <tr>
      <th>2016-11-17</th>
      <td>36.4</td>
    </tr>
    <tr>
      <th>2016-11-18</th>
      <td>37.5</td>
    </tr>
    <tr>
      <th>2016-11-19</th>
      <td>33.9</td>
    </tr>
    <tr>
      <th>2016-11-20</th>
      <td>34.1</td>
    </tr>
    <tr>
      <th>2016-11-21</th>
      <td>37.0</td>
    </tr>
    <tr>
      <th>2016-11-22</th>
      <td>37.5</td>
    </tr>
    <tr>
      <th>2016-11-23</th>
      <td>37.3</td>
    </tr>
    <tr>
      <th>2016-11-24</th>
      <td>38.3</td>
    </tr>
    <tr>
      <th>2016-11-25</th>
      <td>38.1</td>
    </tr>
    <tr>
      <th>2016-11-26</th>
      <td>37.5</td>
    </tr>
    <tr>
      <th>2016-11-27</th>
      <td>37.4</td>
    </tr>
    <tr>
      <th>2016-11-28</th>
      <td>37.4</td>
    </tr>
    <tr>
      <th>2016-11-29</th>
      <td>38.4</td>
    </tr>
    <tr>
      <th>2016-11-30</th>
      <td>37.7</td>
    </tr>
  </tbody>
</table>
<p>7640 rows × 1 columns</p>
</div>




```python
reanalysis
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BARBALHA</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1996-01-01</th>
      <td>26.250031</td>
    </tr>
    <tr>
      <th>1996-01-02</th>
      <td>25.050018</td>
    </tr>
    <tr>
      <th>1996-01-03</th>
      <td>26.850006</td>
    </tr>
    <tr>
      <th>1996-01-04</th>
      <td>26.250031</td>
    </tr>
    <tr>
      <th>1996-01-05</th>
      <td>26.750031</td>
    </tr>
    <tr>
      <th>1996-01-06</th>
      <td>25.450012</td>
    </tr>
    <tr>
      <th>1996-01-07</th>
      <td>24.250031</td>
    </tr>
    <tr>
      <th>1996-01-08</th>
      <td>25.649994</td>
    </tr>
    <tr>
      <th>1996-01-09</th>
      <td>25.450012</td>
    </tr>
    <tr>
      <th>1996-01-10</th>
      <td>27.250031</td>
    </tr>
    <tr>
      <th>1996-01-11</th>
      <td>27.250031</td>
    </tr>
    <tr>
      <th>1996-01-12</th>
      <td>28.950012</td>
    </tr>
    <tr>
      <th>1996-01-13</th>
      <td>27.350006</td>
    </tr>
    <tr>
      <th>1996-01-14</th>
      <td>26.350006</td>
    </tr>
    <tr>
      <th>1996-01-15</th>
      <td>24.950012</td>
    </tr>
    <tr>
      <th>1996-01-16</th>
      <td>24.450012</td>
    </tr>
    <tr>
      <th>1996-01-17</th>
      <td>24.250031</td>
    </tr>
    <tr>
      <th>1996-01-18</th>
      <td>26.149994</td>
    </tr>
    <tr>
      <th>1996-01-19</th>
      <td>24.850006</td>
    </tr>
    <tr>
      <th>1996-01-20</th>
      <td>25.649994</td>
    </tr>
    <tr>
      <th>1996-01-21</th>
      <td>24.149994</td>
    </tr>
    <tr>
      <th>1996-01-22</th>
      <td>24.750031</td>
    </tr>
    <tr>
      <th>1996-01-23</th>
      <td>26.450012</td>
    </tr>
    <tr>
      <th>1996-01-24</th>
      <td>27.750031</td>
    </tr>
    <tr>
      <th>1996-01-25</th>
      <td>26.149994</td>
    </tr>
    <tr>
      <th>1996-01-26</th>
      <td>29.649994</td>
    </tr>
    <tr>
      <th>1996-01-27</th>
      <td>27.050018</td>
    </tr>
    <tr>
      <th>1996-01-28</th>
      <td>26.649994</td>
    </tr>
    <tr>
      <th>1996-01-29</th>
      <td>25.950012</td>
    </tr>
    <tr>
      <th>1996-01-30</th>
      <td>26.649994</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>2016-11-01</th>
      <td>33.950012</td>
    </tr>
    <tr>
      <th>2016-11-02</th>
      <td>33.250000</td>
    </tr>
    <tr>
      <th>2016-11-03</th>
      <td>33.649994</td>
    </tr>
    <tr>
      <th>2016-11-04</th>
      <td>32.950012</td>
    </tr>
    <tr>
      <th>2016-11-05</th>
      <td>33.350006</td>
    </tr>
    <tr>
      <th>2016-11-06</th>
      <td>32.850006</td>
    </tr>
    <tr>
      <th>2016-11-07</th>
      <td>33.849976</td>
    </tr>
    <tr>
      <th>2016-11-08</th>
      <td>32.550018</td>
    </tr>
    <tr>
      <th>2016-11-09</th>
      <td>34.050018</td>
    </tr>
    <tr>
      <th>2016-11-10</th>
      <td>33.749969</td>
    </tr>
    <tr>
      <th>2016-11-11</th>
      <td>33.249969</td>
    </tr>
    <tr>
      <th>2016-11-12</th>
      <td>31.750000</td>
    </tr>
    <tr>
      <th>2016-11-13</th>
      <td>31.649994</td>
    </tr>
    <tr>
      <th>2016-11-14</th>
      <td>32.149994</td>
    </tr>
    <tr>
      <th>2016-11-15</th>
      <td>31.950012</td>
    </tr>
    <tr>
      <th>2016-11-16</th>
      <td>33.450012</td>
    </tr>
    <tr>
      <th>2016-11-17</th>
      <td>34.149994</td>
    </tr>
    <tr>
      <th>2016-11-18</th>
      <td>35.649994</td>
    </tr>
    <tr>
      <th>2016-11-19</th>
      <td>32.649994</td>
    </tr>
    <tr>
      <th>2016-11-20</th>
      <td>32.250000</td>
    </tr>
    <tr>
      <th>2016-11-21</th>
      <td>34.749969</td>
    </tr>
    <tr>
      <th>2016-11-22</th>
      <td>34.849976</td>
    </tr>
    <tr>
      <th>2016-11-23</th>
      <td>35.349976</td>
    </tr>
    <tr>
      <th>2016-11-24</th>
      <td>35.550018</td>
    </tr>
    <tr>
      <th>2016-11-25</th>
      <td>35.450012</td>
    </tr>
    <tr>
      <th>2016-11-26</th>
      <td>33.949982</td>
    </tr>
    <tr>
      <th>2016-11-27</th>
      <td>33.649994</td>
    </tr>
    <tr>
      <th>2016-11-28</th>
      <td>33.949982</td>
    </tr>
    <tr>
      <th>2016-11-29</th>
      <td>34.649994</td>
    </tr>
    <tr>
      <th>2016-11-30</th>
      <td>34.250000</td>
    </tr>
  </tbody>
</table>
<p>7640 rows × 1 columns</p>
</div>



Now, we must to reframe this daily data once the index definition wants "the monthly minimum of daily maximum". So, using pandas again, we resampled daily data using 'm' option and gets its minimum  monthly value


```python
TXn_station = stations.resample('m').min()
TXn_reanalysis = reanalysis.resample('m').min()
```


```python
TXn_station
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BARBALHA</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1996-01-31</th>
      <td>25.3</td>
    </tr>
    <tr>
      <th>1996-02-29</th>
      <td>26.9</td>
    </tr>
    <tr>
      <th>1996-03-31</th>
      <td>28.0</td>
    </tr>
    <tr>
      <th>1996-04-30</th>
      <td>28.1</td>
    </tr>
    <tr>
      <th>1996-05-31</th>
      <td>26.6</td>
    </tr>
    <tr>
      <th>1996-06-30</th>
      <td>27.7</td>
    </tr>
    <tr>
      <th>1996-07-31</th>
      <td>25.5</td>
    </tr>
    <tr>
      <th>1996-08-31</th>
      <td>28.4</td>
    </tr>
    <tr>
      <th>1996-09-30</th>
      <td>31.1</td>
    </tr>
    <tr>
      <th>1996-10-31</th>
      <td>30.1</td>
    </tr>
    <tr>
      <th>1996-11-30</th>
      <td>29.0</td>
    </tr>
    <tr>
      <th>1996-12-31</th>
      <td>28.1</td>
    </tr>
    <tr>
      <th>1997-01-31</th>
      <td>26.7</td>
    </tr>
    <tr>
      <th>1997-02-28</th>
      <td>26.1</td>
    </tr>
    <tr>
      <th>1997-03-31</th>
      <td>26.1</td>
    </tr>
    <tr>
      <th>1997-04-30</th>
      <td>28.5</td>
    </tr>
    <tr>
      <th>1997-05-31</th>
      <td>27.8</td>
    </tr>
    <tr>
      <th>1997-06-30</th>
      <td>29.2</td>
    </tr>
    <tr>
      <th>1997-07-31</th>
      <td>24.9</td>
    </tr>
    <tr>
      <th>1997-08-31</th>
      <td>27.6</td>
    </tr>
    <tr>
      <th>1997-09-30</th>
      <td>30.8</td>
    </tr>
    <tr>
      <th>1997-10-31</th>
      <td>31.1</td>
    </tr>
    <tr>
      <th>1997-11-30</th>
      <td>30.6</td>
    </tr>
    <tr>
      <th>1997-12-31</th>
      <td>31.2</td>
    </tr>
    <tr>
      <th>1998-01-31</th>
      <td>26.1</td>
    </tr>
    <tr>
      <th>1998-02-28</th>
      <td>26.0</td>
    </tr>
    <tr>
      <th>1998-03-31</th>
      <td>27.5</td>
    </tr>
    <tr>
      <th>1998-04-30</th>
      <td>31.0</td>
    </tr>
    <tr>
      <th>1998-05-31</th>
      <td>29.5</td>
    </tr>
    <tr>
      <th>1998-06-30</th>
      <td>30.1</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>2014-06-30</th>
      <td>28.9</td>
    </tr>
    <tr>
      <th>2014-07-31</th>
      <td>28.1</td>
    </tr>
    <tr>
      <th>2014-08-31</th>
      <td>28.6</td>
    </tr>
    <tr>
      <th>2014-09-30</th>
      <td>30.3</td>
    </tr>
    <tr>
      <th>2014-10-31</th>
      <td>29.8</td>
    </tr>
    <tr>
      <th>2014-11-30</th>
      <td>29.5</td>
    </tr>
    <tr>
      <th>2014-12-31</th>
      <td>28.3</td>
    </tr>
    <tr>
      <th>2015-01-31</th>
      <td>30.0</td>
    </tr>
    <tr>
      <th>2015-02-28</th>
      <td>28.2</td>
    </tr>
    <tr>
      <th>2015-03-31</th>
      <td>28.6</td>
    </tr>
    <tr>
      <th>2015-04-30</th>
      <td>30.9</td>
    </tr>
    <tr>
      <th>2015-05-31</th>
      <td>30.9</td>
    </tr>
    <tr>
      <th>2015-06-30</th>
      <td>29.1</td>
    </tr>
    <tr>
      <th>2015-07-31</th>
      <td>28.3</td>
    </tr>
    <tr>
      <th>2015-08-31</th>
      <td>30.2</td>
    </tr>
    <tr>
      <th>2015-09-30</th>
      <td>33.3</td>
    </tr>
    <tr>
      <th>2015-10-31</th>
      <td>30.9</td>
    </tr>
    <tr>
      <th>2015-11-30</th>
      <td>32.5</td>
    </tr>
    <tr>
      <th>2015-12-31</th>
      <td>33.5</td>
    </tr>
    <tr>
      <th>2016-01-31</th>
      <td>25.2</td>
    </tr>
    <tr>
      <th>2016-02-29</th>
      <td>30.6</td>
    </tr>
    <tr>
      <th>2016-03-31</th>
      <td>28.6</td>
    </tr>
    <tr>
      <th>2016-04-30</th>
      <td>30.7</td>
    </tr>
    <tr>
      <th>2016-05-31</th>
      <td>31.3</td>
    </tr>
    <tr>
      <th>2016-06-30</th>
      <td>30.1</td>
    </tr>
    <tr>
      <th>2016-07-31</th>
      <td>31.3</td>
    </tr>
    <tr>
      <th>2016-08-31</th>
      <td>32.1</td>
    </tr>
    <tr>
      <th>2016-09-30</th>
      <td>33.5</td>
    </tr>
    <tr>
      <th>2016-10-31</th>
      <td>35.2</td>
    </tr>
    <tr>
      <th>2016-11-30</th>
      <td>33.9</td>
    </tr>
  </tbody>
</table>
<p>251 rows × 1 columns</p>
</div>




```python
TXn_reanalysis
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BARBALHA</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1996-01-31</th>
      <td>24.149994</td>
    </tr>
    <tr>
      <th>1996-02-29</th>
      <td>23.950012</td>
    </tr>
    <tr>
      <th>1996-03-31</th>
      <td>24.050018</td>
    </tr>
    <tr>
      <th>1996-04-30</th>
      <td>23.649994</td>
    </tr>
    <tr>
      <th>1996-05-31</th>
      <td>24.350006</td>
    </tr>
    <tr>
      <th>1996-06-30</th>
      <td>23.950012</td>
    </tr>
    <tr>
      <th>1996-07-31</th>
      <td>25.350006</td>
    </tr>
    <tr>
      <th>1996-08-31</th>
      <td>25.350006</td>
    </tr>
    <tr>
      <th>1996-09-30</th>
      <td>29.450012</td>
    </tr>
    <tr>
      <th>1996-10-31</th>
      <td>27.750031</td>
    </tr>
    <tr>
      <th>1996-11-30</th>
      <td>26.050018</td>
    </tr>
    <tr>
      <th>1996-12-31</th>
      <td>24.950012</td>
    </tr>
    <tr>
      <th>1997-01-31</th>
      <td>24.450012</td>
    </tr>
    <tr>
      <th>1997-02-28</th>
      <td>24.149994</td>
    </tr>
    <tr>
      <th>1997-03-31</th>
      <td>24.450012</td>
    </tr>
    <tr>
      <th>1997-04-30</th>
      <td>24.750031</td>
    </tr>
    <tr>
      <th>1997-05-31</th>
      <td>25.050018</td>
    </tr>
    <tr>
      <th>1997-06-30</th>
      <td>24.250031</td>
    </tr>
    <tr>
      <th>1997-07-31</th>
      <td>27.250031</td>
    </tr>
    <tr>
      <th>1997-08-31</th>
      <td>28.250031</td>
    </tr>
    <tr>
      <th>1997-09-30</th>
      <td>29.550018</td>
    </tr>
    <tr>
      <th>1997-10-31</th>
      <td>29.550018</td>
    </tr>
    <tr>
      <th>1997-11-30</th>
      <td>25.550018</td>
    </tr>
    <tr>
      <th>1997-12-31</th>
      <td>26.050018</td>
    </tr>
    <tr>
      <th>1998-01-31</th>
      <td>24.350006</td>
    </tr>
    <tr>
      <th>1998-02-28</th>
      <td>25.450012</td>
    </tr>
    <tr>
      <th>1998-03-31</th>
      <td>25.350006</td>
    </tr>
    <tr>
      <th>1998-04-30</th>
      <td>26.050018</td>
    </tr>
    <tr>
      <th>1998-05-31</th>
      <td>24.649994</td>
    </tr>
    <tr>
      <th>1998-06-30</th>
      <td>25.649994</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>2014-06-30</th>
      <td>27.050018</td>
    </tr>
    <tr>
      <th>2014-07-31</th>
      <td>26.850006</td>
    </tr>
    <tr>
      <th>2014-08-31</th>
      <td>27.950012</td>
    </tr>
    <tr>
      <th>2014-09-30</th>
      <td>28.350006</td>
    </tr>
    <tr>
      <th>2014-10-31</th>
      <td>28.149994</td>
    </tr>
    <tr>
      <th>2014-11-30</th>
      <td>25.250000</td>
    </tr>
    <tr>
      <th>2014-12-31</th>
      <td>26.649994</td>
    </tr>
    <tr>
      <th>2015-01-31</th>
      <td>26.149994</td>
    </tr>
    <tr>
      <th>2015-02-28</th>
      <td>23.950012</td>
    </tr>
    <tr>
      <th>2015-03-31</th>
      <td>25.350006</td>
    </tr>
    <tr>
      <th>2015-04-30</th>
      <td>24.850006</td>
    </tr>
    <tr>
      <th>2015-05-31</th>
      <td>27.049988</td>
    </tr>
    <tr>
      <th>2015-06-30</th>
      <td>26.950012</td>
    </tr>
    <tr>
      <th>2015-07-31</th>
      <td>27.450012</td>
    </tr>
    <tr>
      <th>2015-08-31</th>
      <td>28.149994</td>
    </tr>
    <tr>
      <th>2015-09-30</th>
      <td>30.750000</td>
    </tr>
    <tr>
      <th>2015-10-31</th>
      <td>30.049988</td>
    </tr>
    <tr>
      <th>2015-11-30</th>
      <td>30.950012</td>
    </tr>
    <tr>
      <th>2015-12-31</th>
      <td>28.350006</td>
    </tr>
    <tr>
      <th>2016-01-31</th>
      <td>23.850006</td>
    </tr>
    <tr>
      <th>2016-02-29</th>
      <td>24.350006</td>
    </tr>
    <tr>
      <th>2016-03-31</th>
      <td>26.550018</td>
    </tr>
    <tr>
      <th>2016-04-30</th>
      <td>27.949982</td>
    </tr>
    <tr>
      <th>2016-05-31</th>
      <td>27.449982</td>
    </tr>
    <tr>
      <th>2016-06-30</th>
      <td>27.649994</td>
    </tr>
    <tr>
      <th>2016-07-31</th>
      <td>27.249969</td>
    </tr>
    <tr>
      <th>2016-08-31</th>
      <td>27.850006</td>
    </tr>
    <tr>
      <th>2016-09-30</th>
      <td>29.450012</td>
    </tr>
    <tr>
      <th>2016-10-31</th>
      <td>31.749969</td>
    </tr>
    <tr>
      <th>2016-11-30</th>
      <td>31.649994</td>
    </tr>
  </tbody>
</table>
<p>251 rows × 1 columns</p>
</div>



Ploting time series


```python
import matplotlib.pyplot as plt
plt.plot(TXn_station, label = 'Station', linewidth=0.9)
plt.plot(TXn_reanalysis, label = 'Reanalysis', linewidth=0.9)
name = 'Monthly TNx'
plt.ylabel("Temperature (o.C)", fontsize=11)
plt.xlabel("Time", fontsize=11)
plt.title(name, fontsize=16)
plt.legend(bbox_to_anchor=[1, 0.9], loc='right')

plt.grid(False)
plt.show()

```


![png](output_11_0.png)


2. Now it is time to do a regression perse. Lets use pandas 


```python
from statsmodels.formula.api import ols

# Adjusting data
data = pd.concat([a,b], axis=1).dropna().values
data = DataFrame(dict(x = data[:,0], y = data[:,1]))
model = ols("y ~ x", data).fit()
```


```python
 model.summary()
```




<table class="simpletable">
<caption>OLS Regression Results</caption>
<tr>
  <th>Dep. Variable:</th>            <td>y</td>        <th>  R-squared:         </th> <td>   0.520</td>
</tr>
<tr>
  <th>Model:</th>                   <td>OLS</td>       <th>  Adj. R-squared:    </th> <td>   0.518</td>
</tr>
<tr>
  <th>Method:</th>             <td>Least Squares</td>  <th>  F-statistic:       </th> <td>   260.8</td>
</tr>
<tr>
  <th>Date:</th>             <td>Fri, 09 Jun 2017</td> <th>  Prob (F-statistic):</th> <td>2.92e-40</td>
</tr>
<tr>
  <th>Time:</th>                 <td>18:10:18</td>     <th>  Log-Likelihood:    </th> <td> -430.86</td>
</tr>
<tr>
  <th>No. Observations:</th>      <td>   243</td>      <th>  AIC:               </th> <td>   865.7</td>
</tr>
<tr>
  <th>Df Residuals:</th>          <td>   241</td>      <th>  BIC:               </th> <td>   872.7</td>
</tr>
<tr>
  <th>Df Model:</th>              <td>     1</td>      <th>                     </th>     <td> </td>   
</tr>
<tr>
  <th>Covariance Type:</th>      <td>nonrobust</td>    <th>                     </th>     <td> </td>   
</tr>
</table>
<table class="simpletable">
<tr>
      <td></td>         <th>coef</th>     <th>std err</th>      <th>t</th>      <th>P>|t|</th>  <th>[0.025</th>    <th>0.975]</th>  
</tr>
<tr>
  <th>Intercept</th> <td>    7.1765</td> <td>    1.197</td> <td>    5.994</td> <td> 0.000</td> <td>    4.818</td> <td>    9.535</td>
</tr>
<tr>
  <th>x</th>         <td>    0.6631</td> <td>    0.041</td> <td>   16.151</td> <td> 0.000</td> <td>    0.582</td> <td>    0.744</td>
</tr>
</table>
<table class="simpletable">
<tr>
  <th>Omnibus:</th>       <td> 2.618</td> <th>  Durbin-Watson:     </th> <td>   1.231</td>
</tr>
<tr>
  <th>Prob(Omnibus):</th> <td> 0.270</td> <th>  Jarque-Bera (JB):  </th> <td>   1.999</td>
</tr>
<tr>
  <th>Skew:</th>          <td> 0.044</td> <th>  Prob(JB):          </th> <td>   0.368</td>
</tr>
<tr>
  <th>Kurtosis:</th>      <td> 2.565</td> <th>  Cond. No.          </th> <td>    381.</td>
</tr>
</table>



Finally, ploting the fitted rule and data


```python
plt.scatter(a, b,  color='red', s=6)
plt.plot(data['x'], model.predict(), color='black',  linewidth=0.8)
name = 'Monthly TNx'
labelName = 'T (o.C)'
labelName = 'Precipitation (mm)'
plt.title("Regression Analisys for TXn" , fontsize=16)
plt.ylabel(labelName, fontsize=11)
plt.xlabel(labelName, fontsize=11)
plt.show()

```


![png](output_16_0.png)

