---
title: 이벤트 데이터 구성 파악
layout: default
parent: Football Analysis
grand_parent: Side Projects
nav_order: 1
---

<head>
  <style>
    table.dataframe {
      white-space: normal;
      width: 100%;
      height: 240px;
      display: block;
      overflow: auto;
      font-family: Arial, sans-serif;
      font-size: 0.9rem;
      line-height: 20px;
      text-align: center;
      border: 0px !important;
    }

    table.dataframe th {
      text-align: center;
      font-weight: bold;
      padding: 8px;
    }

    table.dataframe td {
      text-align: center;
      padding: 8px;
    }

    table.dataframe tr:hover {
      background: #b8d1f3; 
    }

    .output_prompt {
      overflow: auto;
      font-size: 0.9rem;
      line-height: 1.45;
      border-radius: 0.3rem;
      -webkit-overflow-scrolling: touch;
      padding: 0.8rem;
      margin-top: 0;
      margin-bottom: 15px;
      font: 1rem Consolas, "Liberation Mono", Menlo, Courier, monospace;
      color: $code-text-color;
      border: solid 1px $border-color;
      border-radius: 0.3rem;
      word-break: normal;
      white-space: pre;
    }

  .dataframe tbody tr th:only-of-type {
      vertical-align: middle;
  }

  .dataframe tbody tr th {
      vertical-align: top;
  }

  .dataframe thead th {
      text-align: center !important;
      padding: 8px;
  }

  .page__content p {
      margin: 0 0 0px !important;
  }

  .page__content p > strong {
    font-size: 0.8rem !important;
  }

  </style>
</head>

<div style="text-align: right;">
작성일자 : 2023-08-27<br>
</div>

```python
import os
import pandas as pd
import pickle
import glob
```


```python
# 현재 작업 디렉토리 정보 가져오기
current_dir = os.getcwd()
print(current_dir)
```

<pre>
/Users/limjongjun/Desktop/JayJay/Growth/Python/soccer-analytics/data
</pre>

```python
#new_directory = '/path/to/new_directory'
new_directory = '/Users/limjongjun/Desktop/JayJay/Growth/Python/soccer-analytics/data'

# 작업 디렉토리 변경
os.chdir(new_directory)

#변경된 디렉토리 확인
print(current_dir)
```

<pre>
/Users/limjongjun/Desktop/JayJay/Growth/Python/soccer-analytics/Excercise
</pre>
##### (1) 경기 정보 불러오기


- 상대경로 



```python
dataset_name = 'England'
match_df = pd.read_csv(f'data/refined_events/{dataset_name}/matches.csv', index_col= False , encoding='utf-8-sig') #pkl 파일들 가장 밑에 csv 파일 존재
match_df
```


```python
match_df = pd.read_csv('/Users/limjongjun/Desktop/JayJay/Growth/Python/soccer-analytics/data/refined_events/England/matches.csv', index_col= False , encoding='utf-8-sig') #pkl 파일들 가장 밑에 csv 파일 존재
match_df
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>match_id</th>
      <th>gameweek</th>
      <th>datetime</th>
      <th>venue</th>
      <th>team1_id</th>
      <th>team1_name</th>
      <th>team1_goals</th>
      <th>team2_id</th>
      <th>team2_name</th>
      <th>team2_goals</th>
      <th>duration</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2499719</td>
      <td>1</td>
      <td>2017-08-11 18:45:00</td>
      <td>Emirates Stadium</td>
      <td>1609</td>
      <td>Arsenal</td>
      <td>4</td>
      <td>1631</td>
      <td>Leicester City</td>
      <td>3</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2499727</td>
      <td>1</td>
      <td>2017-08-12 11:30:00</td>
      <td>Vicarage Road Stadium</td>
      <td>1644</td>
      <td>Watford</td>
      <td>3</td>
      <td>1612</td>
      <td>Liverpool</td>
      <td>3</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2499726</td>
      <td>1</td>
      <td>2017-08-12 14:00:00</td>
      <td>St. Mary's Stadium</td>
      <td>1619</td>
      <td>Southampton</td>
      <td>0</td>
      <td>10531</td>
      <td>Swansea City</td>
      <td>0</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2499721</td>
      <td>1</td>
      <td>2017-08-12 14:00:00</td>
      <td>Stamford Bridge</td>
      <td>1610</td>
      <td>Chelsea</td>
      <td>2</td>
      <td>1646</td>
      <td>Burnley</td>
      <td>3</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2499728</td>
      <td>1</td>
      <td>2017-08-12 14:00:00</td>
      <td>The Hawthorns</td>
      <td>1627</td>
      <td>West Bromwich Albion</td>
      <td>1</td>
      <td>1659</td>
      <td>AFC Bournemouth</td>
      <td>0</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>375</th>
      <td>2500092</td>
      <td>38</td>
      <td>2018-05-13 14:00:00</td>
      <td>Anfield</td>
      <td>1612</td>
      <td>Liverpool</td>
      <td>4</td>
      <td>1651</td>
      <td>Brighton &amp; Hove Albion</td>
      <td>0</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>376</th>
      <td>2500091</td>
      <td>38</td>
      <td>2018-05-13 14:00:00</td>
      <td>The John Smith's Stadium</td>
      <td>1673</td>
      <td>Huddersfield Town</td>
      <td>0</td>
      <td>1609</td>
      <td>Arsenal</td>
      <td>1</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>377</th>
      <td>2500090</td>
      <td>38</td>
      <td>2018-05-13 14:00:00</td>
      <td>Selhurst Park</td>
      <td>1628</td>
      <td>Crystal Palace</td>
      <td>2</td>
      <td>1627</td>
      <td>West Bromwich Albion</td>
      <td>0</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>378</th>
      <td>2500098</td>
      <td>38</td>
      <td>2018-05-13 14:00:00</td>
      <td>London Stadium</td>
      <td>1633</td>
      <td>West Ham United</td>
      <td>3</td>
      <td>1623</td>
      <td>Everton</td>
      <td>1</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>379</th>
      <td>2500089</td>
      <td>38</td>
      <td>2018-05-13 14:00:00</td>
      <td>Turf Moor</td>
      <td>1646</td>
      <td>Burnley</td>
      <td>1</td>
      <td>1659</td>
      <td>AFC Bournemouth</td>
      <td>2</td>
      <td>Regular</td>
    </tr>
  </tbody>
</table>
<p>380 rows × 11 columns</p>
</div>



```python
match_df.to_csv(path_or_buf = '/Users/limjongjun/Desktop/JayJay/Growth/Python/soccer-analytics/data/pl_matches.csv')
```


```python
# 빈 데이터프레임 생성
union_df = pd.DataFrame()

# pkl 파일들의 경로를 가져옴
file_paths = glob.glob('/Users/limjongjun/Desktop/JayJay/Growth/Python/soccer-analytics/data/refined_events/England/*.pkl')

# 각 파일을 순회하면서 데이터를 읽어와 데이터프레임에 추가
for file_path in file_paths:
    with open(file_path, 'rb') as file:
        data = pickle.load(file)
        df = pd.DataFrame(data)
        union_df = pd.concat([union_df, df], ignore_index=True)
```


```python
union_df
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>match_id</th>
      <th>event_id</th>
      <th>period</th>
      <th>time</th>
      <th>team_id</th>
      <th>team_name</th>
      <th>player_id</th>
      <th>player_name</th>
      <th>event_type</th>
      <th>sub_event_type</th>
      <th>tags</th>
      <th>start_x</th>
      <th>start_y</th>
      <th>end_x</th>
      <th>end_y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2499938</td>
      <td>218665430</td>
      <td>1H</td>
      <td>0.749</td>
      <td>1633</td>
      <td>West Ham United</td>
      <td>41174</td>
      <td>M. Lanzini</td>
      <td>Pass</td>
      <td>Simple pass</td>
      <td>[Accurate]</td>
      <td>50.96</td>
      <td>34.68</td>
      <td>29.12</td>
      <td>20.40</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2499938</td>
      <td>218665431</td>
      <td>1H</td>
      <td>4.264</td>
      <td>1633</td>
      <td>West Ham United</td>
      <td>8553</td>
      <td>W. Reid</td>
      <td>Pass</td>
      <td>High pass</td>
      <td>[Not accurate]</td>
      <td>29.12</td>
      <td>20.40</td>
      <td>84.24</td>
      <td>19.04</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2499938</td>
      <td>218666275</td>
      <td>1H</td>
      <td>7.114</td>
      <td>1627</td>
      <td>West Bromwich Albion</td>
      <td>7914</td>
      <td>J. Evans</td>
      <td>Pass</td>
      <td>Head pass</td>
      <td>[Not accurate]</td>
      <td>19.76</td>
      <td>48.96</td>
      <td>31.20</td>
      <td>44.88</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2499938</td>
      <td>218665432</td>
      <td>1H</td>
      <td>10.018</td>
      <td>1633</td>
      <td>West Ham United</td>
      <td>20620</td>
      <td>Pedro Obiang</td>
      <td>Pass</td>
      <td>Simple pass</td>
      <td>[Accurate]</td>
      <td>72.80</td>
      <td>23.12</td>
      <td>67.60</td>
      <td>62.56</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2499938</td>
      <td>218665433</td>
      <td>1H</td>
      <td>13.153</td>
      <td>1633</td>
      <td>West Ham United</td>
      <td>26499</td>
      <td>A. Masuaku</td>
      <td>Pass</td>
      <td>Simple pass</td>
      <td>[Accurate]</td>
      <td>67.60</td>
      <td>62.56</td>
      <td>59.28</td>
      <td>53.04</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>647247</th>
      <td>2499921</td>
      <td>218383200</td>
      <td>2H</td>
      <td>3148.632</td>
      <td>1628</td>
      <td>Crystal Palace</td>
      <td>38031</td>
      <td>C. Benteke</td>
      <td>Duel</td>
      <td>Air duel</td>
      <td>[Won, Accurate]</td>
      <td>94.64</td>
      <td>41.48</td>
      <td>91.52</td>
      <td>43.52</td>
    </tr>
    <tr>
      <th>647248</th>
      <td>2499921</td>
      <td>218383748</td>
      <td>2H</td>
      <td>3148.632</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>0</td>
      <td>NaN</td>
      <td>Duel</td>
      <td>Air duel</td>
      <td>[Lost, Not accurate]</td>
      <td>9.36</td>
      <td>26.52</td>
      <td>12.48</td>
      <td>24.48</td>
    </tr>
    <tr>
      <th>647249</th>
      <td>2499921</td>
      <td>218383201</td>
      <td>2H</td>
      <td>3149.669</td>
      <td>1628</td>
      <td>Crystal Palace</td>
      <td>38031</td>
      <td>C. Benteke</td>
      <td>Shot</td>
      <td>Shot</td>
      <td>[Head/body, Opportunity, Position: Out low lef...</td>
      <td>91.52</td>
      <td>43.52</td>
      <td>104.00</td>
      <td>34.00</td>
    </tr>
    <tr>
      <th>647250</th>
      <td>2499921</td>
      <td>218383202</td>
      <td>2H</td>
      <td>3156.334</td>
      <td>1628</td>
      <td>Crystal Palace</td>
      <td>0</td>
      <td>NaN</td>
      <td>Interruption</td>
      <td>Ball out of the field</td>
      <td>[]</td>
      <td>102.96</td>
      <td>48.28</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>647251</th>
      <td>2499921</td>
      <td>218383285</td>
      <td>2H</td>
      <td>3161.635</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>71654</td>
      <td>Ederson</td>
      <td>Free kick</td>
      <td>Goal kick</td>
      <td>[]</td>
      <td>0.00</td>
      <td>34.00</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>647252 rows × 15 columns</p>
</div>



```python
union_df.to_csv(path_or_buf = '/Users/limjongjun/Desktop/JayJay/Growth/Python/soccer-analytics/data/pl_events.csv')
```

- 절대경로



```python
#데이터 불러오기
#방법 1 (조건에 col_index = 0)
dataset_name = 'England'
match_df = pd.read_csv('/Users/limjongjun/Desktop/JayJay/Growth/Python/soccer-analytics/data/refined_events/England/matches.csv', index_col=0, encoding='utf-8-sig') #pkl 파일들 가장 밑에 csv 파일 존재
match_df

#방법 2 (확인 후 추후 적용)
dataset_name = 'England'
match_df = pd.read_csv('/Users/limjongjun/Desktop/JayJay/Growth/Python/soccer-analytics/data/refined_events/England/matches.csv', encoding='utf-8-sig') #pkl 파일들 가장 밑에 csv 파일 존재
match_df = match_df.set_index('match_id')
match_df
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>gameweek</th>
      <th>datetime</th>
      <th>venue</th>
      <th>team1_id</th>
      <th>team1_name</th>
      <th>team1_goals</th>
      <th>team2_id</th>
      <th>team2_name</th>
      <th>team2_goals</th>
      <th>duration</th>
    </tr>
    <tr>
      <th>match_id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2499719</th>
      <td>1</td>
      <td>2017-08-11 18:45:00</td>
      <td>Emirates Stadium</td>
      <td>1609</td>
      <td>Arsenal</td>
      <td>4</td>
      <td>1631</td>
      <td>Leicester City</td>
      <td>3</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2499727</th>
      <td>1</td>
      <td>2017-08-12 11:30:00</td>
      <td>Vicarage Road Stadium</td>
      <td>1644</td>
      <td>Watford</td>
      <td>3</td>
      <td>1612</td>
      <td>Liverpool</td>
      <td>3</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2499726</th>
      <td>1</td>
      <td>2017-08-12 14:00:00</td>
      <td>St. Mary's Stadium</td>
      <td>1619</td>
      <td>Southampton</td>
      <td>0</td>
      <td>10531</td>
      <td>Swansea City</td>
      <td>0</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2499721</th>
      <td>1</td>
      <td>2017-08-12 14:00:00</td>
      <td>Stamford Bridge</td>
      <td>1610</td>
      <td>Chelsea</td>
      <td>2</td>
      <td>1646</td>
      <td>Burnley</td>
      <td>3</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2499728</th>
      <td>1</td>
      <td>2017-08-12 14:00:00</td>
      <td>The Hawthorns</td>
      <td>1627</td>
      <td>West Bromwich Albion</td>
      <td>1</td>
      <td>1659</td>
      <td>AFC Bournemouth</td>
      <td>0</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2500092</th>
      <td>38</td>
      <td>2018-05-13 14:00:00</td>
      <td>Anfield</td>
      <td>1612</td>
      <td>Liverpool</td>
      <td>4</td>
      <td>1651</td>
      <td>Brighton &amp; Hove Albion</td>
      <td>0</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2500091</th>
      <td>38</td>
      <td>2018-05-13 14:00:00</td>
      <td>The John Smith's Stadium</td>
      <td>1673</td>
      <td>Huddersfield Town</td>
      <td>0</td>
      <td>1609</td>
      <td>Arsenal</td>
      <td>1</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2500090</th>
      <td>38</td>
      <td>2018-05-13 14:00:00</td>
      <td>Selhurst Park</td>
      <td>1628</td>
      <td>Crystal Palace</td>
      <td>2</td>
      <td>1627</td>
      <td>West Bromwich Albion</td>
      <td>0</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2500098</th>
      <td>38</td>
      <td>2018-05-13 14:00:00</td>
      <td>London Stadium</td>
      <td>1633</td>
      <td>West Ham United</td>
      <td>3</td>
      <td>1623</td>
      <td>Everton</td>
      <td>1</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2500089</th>
      <td>38</td>
      <td>2018-05-13 14:00:00</td>
      <td>Turf Moor</td>
      <td>1646</td>
      <td>Burnley</td>
      <td>1</td>
      <td>1659</td>
      <td>AFC Bournemouth</td>
      <td>2</td>
      <td>Regular</td>
    </tr>
  </tbody>
</table>
<p>380 rows × 10 columns</p>
</div>



```python
match_df['team1_name'].unique()
```

<pre>
array(['Arsenal', 'Watford', 'Southampton', 'Chelsea',
       'West Bromwich Albion', 'Everton', 'Crystal Palace',
       'Brighton & Hove Albion', 'Newcastle United', 'Manchester United',
       'Swansea City', 'Burnley', 'AFC Bournemouth', 'Liverpool',
       'Leicester City', 'Stoke City', 'Huddersfield Town',
       'Tottenham Hotspur', 'Manchester City', 'West Ham United'],
      dtype=object)
</pre>
##### (2) 경기 정보 필터링



```python
## df[ (df['col'] == '') and | (df['col2'] = '') ]
match_df[(match_df['team1_name'] == 'Manchester City') | (match_df['team2_name'] == 'Manchester City')]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>gameweek</th>
      <th>datetime</th>
      <th>venue</th>
      <th>team1_id</th>
      <th>team1_name</th>
      <th>team1_goals</th>
      <th>team2_id</th>
      <th>team2_name</th>
      <th>team2_goals</th>
      <th>duration</th>
    </tr>
    <tr>
      <th>match_id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2499720</th>
      <td>1</td>
      <td>2017-08-12 16:30:00</td>
      <td>The American Express Community Stadium</td>
      <td>1651</td>
      <td>Brighton &amp; Hove Albion</td>
      <td>0</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>2</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2499734</th>
      <td>2</td>
      <td>2017-08-21 19:00:00</td>
      <td>Etihad Stadium</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>1</td>
      <td>1623</td>
      <td>Everton</td>
      <td>1</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2499739</th>
      <td>3</td>
      <td>2017-08-26 11:30:00</td>
      <td>Vitality Stadium</td>
      <td>1659</td>
      <td>AFC Bournemouth</td>
      <td>1</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>2</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2499754</th>
      <td>4</td>
      <td>2017-09-09 11:30:00</td>
      <td>Etihad Stadium</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>5</td>
      <td>1612</td>
      <td>Liverpool</td>
      <td>0</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2499767</th>
      <td>5</td>
      <td>2017-09-16 14:00:00</td>
      <td>Vicarage Road Stadium</td>
      <td>1644</td>
      <td>Watford</td>
      <td>0</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>6</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2499774</th>
      <td>6</td>
      <td>2017-09-23 14:00:00</td>
      <td>Etihad Stadium</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>5</td>
      <td>1628</td>
      <td>Crystal Palace</td>
      <td>0</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2499781</th>
      <td>7</td>
      <td>2017-09-30 16:30:00</td>
      <td>Stamford Bridge</td>
      <td>1610</td>
      <td>Chelsea</td>
      <td>0</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>1</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2499794</th>
      <td>8</td>
      <td>2017-10-14 14:00:00</td>
      <td>Etihad Stadium</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>7</td>
      <td>1639</td>
      <td>Stoke City</td>
      <td>2</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2499802</th>
      <td>9</td>
      <td>2017-10-21 14:00:00</td>
      <td>Etihad Stadium</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>3</td>
      <td>1646</td>
      <td>Burnley</td>
      <td>0</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2499818</th>
      <td>10</td>
      <td>2017-10-28 14:00:00</td>
      <td>The Hawthorns</td>
      <td>1627</td>
      <td>West Bromwich Albion</td>
      <td>2</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>3</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2499822</th>
      <td>11</td>
      <td>2017-11-05 14:15:00</td>
      <td>Etihad Stadium</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>3</td>
      <td>1609</td>
      <td>Arsenal</td>
      <td>1</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2499834</th>
      <td>12</td>
      <td>2017-11-18 15:00:00</td>
      <td>King Power Stadium</td>
      <td>1631</td>
      <td>Leicester City</td>
      <td>0</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>2</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2499841</th>
      <td>13</td>
      <td>2017-11-26 16:00:00</td>
      <td>The John Smith's Stadium</td>
      <td>1673</td>
      <td>Huddersfield Town</td>
      <td>1</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>2</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2499857</th>
      <td>14</td>
      <td>2017-11-29 20:00:00</td>
      <td>Etihad Stadium</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>2</td>
      <td>1619</td>
      <td>Southampton</td>
      <td>1</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2499865</th>
      <td>15</td>
      <td>2017-12-03 16:00:00</td>
      <td>Etihad Stadium</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>2</td>
      <td>1633</td>
      <td>West Ham United</td>
      <td>1</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2499873</th>
      <td>16</td>
      <td>2017-12-10 16:30:00</td>
      <td>Old Trafford</td>
      <td>1611</td>
      <td>Manchester United</td>
      <td>1</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>2</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2499881</th>
      <td>17</td>
      <td>2017-12-13 19:45:00</td>
      <td>Liberty Stadium</td>
      <td>10531</td>
      <td>Swansea City</td>
      <td>0</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>4</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2499895</th>
      <td>18</td>
      <td>2017-12-16 17:30:00</td>
      <td>Etihad Stadium</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>4</td>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>1</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2499904</th>
      <td>19</td>
      <td>2017-12-23 15:00:00</td>
      <td>Etihad Stadium</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>4</td>
      <td>1659</td>
      <td>AFC Bournemouth</td>
      <td>0</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2499915</th>
      <td>20</td>
      <td>2017-12-27 19:45:00</td>
      <td>St. James' Park</td>
      <td>1613</td>
      <td>Newcastle United</td>
      <td>0</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>1</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2499921</th>
      <td>21</td>
      <td>2017-12-31 12:00:00</td>
      <td>Selhurst Park</td>
      <td>1628</td>
      <td>Crystal Palace</td>
      <td>0</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>0</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2499934</th>
      <td>22</td>
      <td>2018-01-02 20:00:00</td>
      <td>Etihad Stadium</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>3</td>
      <td>1644</td>
      <td>Watford</td>
      <td>1</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2499943</th>
      <td>23</td>
      <td>2018-01-14 16:00:00</td>
      <td>Anfield</td>
      <td>1612</td>
      <td>Liverpool</td>
      <td>4</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>3</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2499954</th>
      <td>24</td>
      <td>2018-01-20 17:30:00</td>
      <td>Etihad Stadium</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>3</td>
      <td>1613</td>
      <td>Newcastle United</td>
      <td>1</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2499966</th>
      <td>25</td>
      <td>2018-01-31 20:00:00</td>
      <td>Etihad Stadium</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>3</td>
      <td>1627</td>
      <td>West Bromwich Albion</td>
      <td>0</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2499972</th>
      <td>26</td>
      <td>2018-02-03 12:30:00</td>
      <td>Turf Moor</td>
      <td>1646</td>
      <td>Burnley</td>
      <td>1</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>1</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2499982</th>
      <td>27</td>
      <td>2018-02-10 17:30:00</td>
      <td>Etihad Stadium</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>5</td>
      <td>1631</td>
      <td>Leicester City</td>
      <td>1</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2499990</th>
      <td>28</td>
      <td>2018-03-01 19:45:00</td>
      <td>Emirates Stadium</td>
      <td>1609</td>
      <td>Arsenal</td>
      <td>0</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>3</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2500004</th>
      <td>29</td>
      <td>2018-03-04 16:00:00</td>
      <td>Etihad Stadium</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>1</td>
      <td>1610</td>
      <td>Chelsea</td>
      <td>0</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2500016</th>
      <td>30</td>
      <td>2018-03-12 20:00:00</td>
      <td>Bet365 Stadium</td>
      <td>1639</td>
      <td>Stoke City</td>
      <td>0</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>2</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2500033</th>
      <td>32</td>
      <td>2018-03-31 16:30:00</td>
      <td>Goodison Park</td>
      <td>1623</td>
      <td>Everton</td>
      <td>1</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>3</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2500045</th>
      <td>33</td>
      <td>2018-04-07 16:30:00</td>
      <td>Etihad Stadium</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>2</td>
      <td>1611</td>
      <td>Manchester United</td>
      <td>3</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2500057</th>
      <td>34</td>
      <td>2018-04-14 18:45:00</td>
      <td>Wembley Stadium</td>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>1</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>3</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2500065</th>
      <td>35</td>
      <td>2018-04-22 15:30:00</td>
      <td>Etihad Stadium</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>5</td>
      <td>10531</td>
      <td>Swansea City</td>
      <td>0</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2500078</th>
      <td>36</td>
      <td>2018-04-29 13:15:00</td>
      <td>London Stadium</td>
      <td>1633</td>
      <td>West Ham United</td>
      <td>1</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>4</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2500085</th>
      <td>37</td>
      <td>2018-05-06 12:30:00</td>
      <td>Etihad Stadium</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>0</td>
      <td>1673</td>
      <td>Huddersfield Town</td>
      <td>0</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2500024</th>
      <td>31</td>
      <td>2018-05-09 19:00:00</td>
      <td>Etihad Stadium</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>3</td>
      <td>1651</td>
      <td>Brighton &amp; Hove Albion</td>
      <td>1</td>
      <td>Regular</td>
    </tr>
    <tr>
      <th>2500095</th>
      <td>38</td>
      <td>2018-05-13 14:00:00</td>
      <td>St. Mary's Stadium</td>
      <td>1619</td>
      <td>Southampton</td>
      <td>0</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>1</td>
      <td>Regular</td>
    </tr>
  </tbody>
</table>
</div>


##### 17/18 시즌 18Round Mancity vs Tottenham



```python
match_id = 2499895
match_events = pd.read_pickle('/Users/limjongjun/Desktop/JayJay/Growth/Python/soccer-analytics/data/refined_events/England/2499895.pkl') #pkl 파일을 Dataframe으로 불러오기
match_events
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>match_id</th>
      <th>event_id</th>
      <th>period</th>
      <th>time</th>
      <th>team_id</th>
      <th>team_name</th>
      <th>player_id</th>
      <th>player_name</th>
      <th>event_type</th>
      <th>sub_event_type</th>
      <th>tags</th>
      <th>start_x</th>
      <th>start_y</th>
      <th>end_x</th>
      <th>end_y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2499895</td>
      <td>215108367</td>
      <td>1H</td>
      <td>1.784</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>8325</td>
      <td>S. Agüero</td>
      <td>Pass</td>
      <td>Simple pass</td>
      <td>[Accurate]</td>
      <td>52.00</td>
      <td>34.68</td>
      <td>40.56</td>
      <td>34.68</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2499895</td>
      <td>215108368</td>
      <td>1H</td>
      <td>3.324</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>105339</td>
      <td>Fernandinho</td>
      <td>Pass</td>
      <td>Simple pass</td>
      <td>[Accurate]</td>
      <td>40.56</td>
      <td>34.68</td>
      <td>29.12</td>
      <td>8.16</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2499895</td>
      <td>215108369</td>
      <td>1H</td>
      <td>6.406</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>8277</td>
      <td>K. Walker</td>
      <td>Pass</td>
      <td>Simple pass</td>
      <td>[Accurate]</td>
      <td>29.12</td>
      <td>8.16</td>
      <td>44.72</td>
      <td>10.20</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2499895</td>
      <td>215108370</td>
      <td>1H</td>
      <td>7.124</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>38021</td>
      <td>K. De Bruyne</td>
      <td>Pass</td>
      <td>Simple pass</td>
      <td>[Accurate]</td>
      <td>44.72</td>
      <td>10.20</td>
      <td>69.68</td>
      <td>6.12</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2499895</td>
      <td>215108371</td>
      <td>1H</td>
      <td>8.676</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>11066</td>
      <td>R. Sterling</td>
      <td>Pass</td>
      <td>Simple pass</td>
      <td>[Accurate]</td>
      <td>69.68</td>
      <td>6.12</td>
      <td>58.24</td>
      <td>12.24</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1607</th>
      <td>2499895</td>
      <td>215110122</td>
      <td>2H</td>
      <td>2875.703</td>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>210044</td>
      <td>E. Dier</td>
      <td>Pass</td>
      <td>Simple pass</td>
      <td>[Accurate]</td>
      <td>36.40</td>
      <td>26.52</td>
      <td>48.88</td>
      <td>29.92</td>
    </tr>
    <tr>
      <th>1608</th>
      <td>2499895</td>
      <td>215110123</td>
      <td>2H</td>
      <td>2876.142</td>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>240070</td>
      <td>H. Winks</td>
      <td>Duel</td>
      <td>Ground attacking duel</td>
      <td>[Anticipation, Lost, Not accurate]</td>
      <td>48.88</td>
      <td>29.92</td>
      <td>43.68</td>
      <td>24.48</td>
    </tr>
    <tr>
      <th>1609</th>
      <td>2499895</td>
      <td>215109959</td>
      <td>2H</td>
      <td>2876.768</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>447205</td>
      <td>P. Foden</td>
      <td>Duel</td>
      <td>Ground defending duel</td>
      <td>[Anticipated, Won, Accurate]</td>
      <td>55.12</td>
      <td>38.08</td>
      <td>60.32</td>
      <td>43.52</td>
    </tr>
    <tr>
      <th>1610</th>
      <td>2499895</td>
      <td>215109960</td>
      <td>2H</td>
      <td>2878.046</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>11066</td>
      <td>R. Sterling</td>
      <td>Duel</td>
      <td>Ground attacking duel</td>
      <td>[Free space right, Lost, Not accurate]</td>
      <td>60.32</td>
      <td>43.52</td>
      <td>60.32</td>
      <td>43.52</td>
    </tr>
    <tr>
      <th>1611</th>
      <td>2499895</td>
      <td>215110124</td>
      <td>2H</td>
      <td>2878.216</td>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>240070</td>
      <td>H. Winks</td>
      <td>Duel</td>
      <td>Ground defending duel</td>
      <td>[Free space left, Lost, Not accurate]</td>
      <td>43.68</td>
      <td>24.48</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>1612 rows × 15 columns</p>
</div>


##### 열 인덱싱 및 unique 값 확인 ( df['col'] )



```python
match_events[['player_id', 'player_name' ]]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>player_id</th>
      <th>player_name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>8325</td>
      <td>S. Agüero</td>
    </tr>
    <tr>
      <th>1</th>
      <td>105339</td>
      <td>Fernandinho</td>
    </tr>
    <tr>
      <th>2</th>
      <td>8277</td>
      <td>K. Walker</td>
    </tr>
    <tr>
      <th>3</th>
      <td>38021</td>
      <td>K. De Bruyne</td>
    </tr>
    <tr>
      <th>4</th>
      <td>11066</td>
      <td>R. Sterling</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1607</th>
      <td>210044</td>
      <td>E. Dier</td>
    </tr>
    <tr>
      <th>1608</th>
      <td>240070</td>
      <td>H. Winks</td>
    </tr>
    <tr>
      <th>1609</th>
      <td>447205</td>
      <td>P. Foden</td>
    </tr>
    <tr>
      <th>1610</th>
      <td>11066</td>
      <td>R. Sterling</td>
    </tr>
    <tr>
      <th>1611</th>
      <td>240070</td>
      <td>H. Winks</td>
    </tr>
  </tbody>
</table>
<p>1612 rows × 2 columns</p>
</div>



```python
match_events['player_name'].unique()
```

<pre>
array(['S. Agüero', 'Fernandinho', 'K. Walker', 'K. De Bruyne',
       'R. Sterling', 'H. Winks', 'D. Rose', nan, 'H. Kane',
       'N. Otamendi', 'M. Dembélé', 'E. Dier', 'H. Lloris',
       'J. Vertonghen', 'F. Delph', 'Ederson', 'K. Trippier',
       'Son Heung-Min', 'C. Eriksen', 'L. Sané', 'E. Mangala',
       'İ. Gündoğan', 'D. Alli', 'Gabriel Jesus', 'E. Lamela', 'P. Foden',
       'M. Sissoko', 'Bernardo Silva'], dtype=object)
</pre>
##### 행 인덱싱



```python
match_events.loc[0]
```

<pre>
match_id                  2499895
event_id                215108367
period                         1H
time                        1.784
team_id                      1625
team_name         Manchester City
player_id                    8325
player_name             S. Agüero
event_type                   Pass
sub_event_type        Simple pass
tags                   [Accurate]
start_x                      52.0
start_y                     34.68
end_x                       40.56
end_y                       34.68
Name: 0, dtype: object
</pre>

```python
match_events.loc[0:5]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>match_id</th>
      <th>event_id</th>
      <th>period</th>
      <th>time</th>
      <th>team_id</th>
      <th>team_name</th>
      <th>player_id</th>
      <th>player_name</th>
      <th>event_type</th>
      <th>sub_event_type</th>
      <th>tags</th>
      <th>start_x</th>
      <th>start_y</th>
      <th>end_x</th>
      <th>end_y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2499895</td>
      <td>215108367</td>
      <td>1H</td>
      <td>1.784</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>8325</td>
      <td>S. Agüero</td>
      <td>Pass</td>
      <td>Simple pass</td>
      <td>[Accurate]</td>
      <td>52.00</td>
      <td>34.68</td>
      <td>40.56</td>
      <td>34.68</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2499895</td>
      <td>215108368</td>
      <td>1H</td>
      <td>3.324</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>105339</td>
      <td>Fernandinho</td>
      <td>Pass</td>
      <td>Simple pass</td>
      <td>[Accurate]</td>
      <td>40.56</td>
      <td>34.68</td>
      <td>29.12</td>
      <td>8.16</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2499895</td>
      <td>215108369</td>
      <td>1H</td>
      <td>6.406</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>8277</td>
      <td>K. Walker</td>
      <td>Pass</td>
      <td>Simple pass</td>
      <td>[Accurate]</td>
      <td>29.12</td>
      <td>8.16</td>
      <td>44.72</td>
      <td>10.20</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2499895</td>
      <td>215108370</td>
      <td>1H</td>
      <td>7.124</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>38021</td>
      <td>K. De Bruyne</td>
      <td>Pass</td>
      <td>Simple pass</td>
      <td>[Accurate]</td>
      <td>44.72</td>
      <td>10.20</td>
      <td>69.68</td>
      <td>6.12</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2499895</td>
      <td>215108371</td>
      <td>1H</td>
      <td>8.676</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>11066</td>
      <td>R. Sterling</td>
      <td>Pass</td>
      <td>Simple pass</td>
      <td>[Accurate]</td>
      <td>69.68</td>
      <td>6.12</td>
      <td>58.24</td>
      <td>12.24</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2499895</td>
      <td>215108382</td>
      <td>1H</td>
      <td>9.282</td>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>240070</td>
      <td>H. Winks</td>
      <td>Duel</td>
      <td>Ground loose ball duel</td>
      <td>[Won, Accurate]</td>
      <td>45.76</td>
      <td>55.76</td>
      <td>47.84</td>
      <td>61.20</td>
    </tr>
  </tbody>
</table>
</div>



```python
match_events.iloc[0:5]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>match_id</th>
      <th>event_id</th>
      <th>period</th>
      <th>time</th>
      <th>team_id</th>
      <th>team_name</th>
      <th>player_id</th>
      <th>player_name</th>
      <th>event_type</th>
      <th>sub_event_type</th>
      <th>tags</th>
      <th>start_x</th>
      <th>start_y</th>
      <th>end_x</th>
      <th>end_y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2499895</td>
      <td>215108367</td>
      <td>1H</td>
      <td>1.784</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>8325</td>
      <td>S. Agüero</td>
      <td>Pass</td>
      <td>Simple pass</td>
      <td>[Accurate]</td>
      <td>52.00</td>
      <td>34.68</td>
      <td>40.56</td>
      <td>34.68</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2499895</td>
      <td>215108368</td>
      <td>1H</td>
      <td>3.324</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>105339</td>
      <td>Fernandinho</td>
      <td>Pass</td>
      <td>Simple pass</td>
      <td>[Accurate]</td>
      <td>40.56</td>
      <td>34.68</td>
      <td>29.12</td>
      <td>8.16</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2499895</td>
      <td>215108369</td>
      <td>1H</td>
      <td>6.406</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>8277</td>
      <td>K. Walker</td>
      <td>Pass</td>
      <td>Simple pass</td>
      <td>[Accurate]</td>
      <td>29.12</td>
      <td>8.16</td>
      <td>44.72</td>
      <td>10.20</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2499895</td>
      <td>215108370</td>
      <td>1H</td>
      <td>7.124</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>38021</td>
      <td>K. De Bruyne</td>
      <td>Pass</td>
      <td>Simple pass</td>
      <td>[Accurate]</td>
      <td>44.72</td>
      <td>10.20</td>
      <td>69.68</td>
      <td>6.12</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2499895</td>
      <td>215108371</td>
      <td>1H</td>
      <td>8.676</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>11066</td>
      <td>R. Sterling</td>
      <td>Pass</td>
      <td>Simple pass</td>
      <td>[Accurate]</td>
      <td>69.68</td>
      <td>6.12</td>
      <td>58.24</td>
      <td>12.24</td>
    </tr>
  </tbody>
</table>
</div>



```python
match_events.loc[:,['player_id', 'player_name']]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>player_id</th>
      <th>player_name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>8325</td>
      <td>S. Agüero</td>
    </tr>
    <tr>
      <th>1</th>
      <td>105339</td>
      <td>Fernandinho</td>
    </tr>
    <tr>
      <th>2</th>
      <td>8277</td>
      <td>K. Walker</td>
    </tr>
    <tr>
      <th>3</th>
      <td>38021</td>
      <td>K. De Bruyne</td>
    </tr>
    <tr>
      <th>4</th>
      <td>11066</td>
      <td>R. Sterling</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1607</th>
      <td>210044</td>
      <td>E. Dier</td>
    </tr>
    <tr>
      <th>1608</th>
      <td>240070</td>
      <td>H. Winks</td>
    </tr>
    <tr>
      <th>1609</th>
      <td>447205</td>
      <td>P. Foden</td>
    </tr>
    <tr>
      <th>1610</th>
      <td>11066</td>
      <td>R. Sterling</td>
    </tr>
    <tr>
      <th>1611</th>
      <td>240070</td>
      <td>H. Winks</td>
    </tr>
  </tbody>
</table>
<p>1612 rows × 2 columns</p>
</div>



```python
match_events['event_type'].unique()
```

<pre>
array(['Pass', 'Duel', 'Others on the ball', 'Interruption', 'Free kick',
       'Foul', 'Shot', 'Offside', 'Save attempt',
       'Goalkeeper leaving line', 'Substitution'], dtype=object)
</pre>

```python
match_events[match_events['event_type']=='Shot']
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>match_id</th>
      <th>event_id</th>
      <th>period</th>
      <th>time</th>
      <th>team_id</th>
      <th>team_name</th>
      <th>player_id</th>
      <th>player_name</th>
      <th>event_type</th>
      <th>sub_event_type</th>
      <th>tags</th>
      <th>start_x</th>
      <th>start_y</th>
      <th>end_x</th>
      <th>end_y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>204</th>
      <td>2499895</td>
      <td>215108558</td>
      <td>1H</td>
      <td>549.772</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>8325</td>
      <td>S. Agüero</td>
      <td>Shot</td>
      <td>Shot</td>
      <td>[Head/body, Opportunity, Position: Out low rig...</td>
      <td>95.68</td>
      <td>25.84</td>
      <td>104.00</td>
      <td>34.00</td>
    </tr>
    <tr>
      <th>299</th>
      <td>2499895</td>
      <td>215108640</td>
      <td>1H</td>
      <td>822.817</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>14808</td>
      <td>İ. Gündoğan</td>
      <td>Shot</td>
      <td>Shot</td>
      <td>[Goal, Head/body, Opportunity, Position: Goal ...</td>
      <td>95.68</td>
      <td>38.76</td>
      <td>104.00</td>
      <td>34.00</td>
    </tr>
    <tr>
      <th>452</th>
      <td>2499895</td>
      <td>215108755</td>
      <td>1H</td>
      <td>1380.188</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>8325</td>
      <td>S. Agüero</td>
      <td>Shot</td>
      <td>Shot</td>
      <td>[Left foot, Opportunity, Position: Goal low ce...</td>
      <td>88.40</td>
      <td>49.64</td>
      <td>104.00</td>
      <td>34.00</td>
    </tr>
    <tr>
      <th>456</th>
      <td>2499895</td>
      <td>215108760</td>
      <td>1H</td>
      <td>1385.615</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>11066</td>
      <td>R. Sterling</td>
      <td>Shot</td>
      <td>Shot</td>
      <td>[Left foot, Opportunity, Position: Out high le...</td>
      <td>91.52</td>
      <td>42.84</td>
      <td>104.00</td>
      <td>34.00</td>
    </tr>
    <tr>
      <th>480</th>
      <td>2499895</td>
      <td>215108786</td>
      <td>1H</td>
      <td>1474.472</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>38021</td>
      <td>K. De Bruyne</td>
      <td>Shot</td>
      <td>Shot</td>
      <td>[Left foot, Opportunity, Position: Goal low le...</td>
      <td>80.08</td>
      <td>36.72</td>
      <td>104.00</td>
      <td>34.00</td>
    </tr>
    <tr>
      <th>506</th>
      <td>2499895</td>
      <td>215108826</td>
      <td>1H</td>
      <td>1590.043</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>14808</td>
      <td>İ. Gündoğan</td>
      <td>Shot</td>
      <td>Shot</td>
      <td>[Counter attack, Right foot, Opportunity, Posi...</td>
      <td>91.52</td>
      <td>45.56</td>
      <td>104.00</td>
      <td>34.00</td>
    </tr>
    <tr>
      <th>546</th>
      <td>2499895</td>
      <td>215108879</td>
      <td>1H</td>
      <td>1693.638</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>8325</td>
      <td>S. Agüero</td>
      <td>Shot</td>
      <td>Shot</td>
      <td>[Right foot, Blocked, Not accurate]</td>
      <td>91.52</td>
      <td>22.44</td>
      <td>92.56</td>
      <td>24.48</td>
    </tr>
    <tr>
      <th>606</th>
      <td>2499895</td>
      <td>215108944</td>
      <td>1H</td>
      <td>1959.94</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>8325</td>
      <td>S. Agüero</td>
      <td>Shot</td>
      <td>Shot</td>
      <td>[Left foot, Opportunity, Position: Out low rig...</td>
      <td>85.28</td>
      <td>34.00</td>
      <td>104.00</td>
      <td>34.00</td>
    </tr>
    <tr>
      <th>632</th>
      <td>2499895</td>
      <td>215109180</td>
      <td>1H</td>
      <td>2047.956</td>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>8717</td>
      <td>H. Kane</td>
      <td>Shot</td>
      <td>Shot</td>
      <td>[Right foot, Opportunity, Position: Out low ri...</td>
      <td>85.28</td>
      <td>51.00</td>
      <td>104.00</td>
      <td>34.00</td>
    </tr>
    <tr>
      <th>777</th>
      <td>2499895</td>
      <td>215109097</td>
      <td>1H</td>
      <td>2633.298</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>8325</td>
      <td>S. Agüero</td>
      <td>Shot</td>
      <td>Shot</td>
      <td>[Counter attack, Left foot, Blocked, Not accur...</td>
      <td>90.48</td>
      <td>20.40</td>
      <td>89.44</td>
      <td>29.24</td>
    </tr>
    <tr>
      <th>810</th>
      <td>2499895</td>
      <td>215109134</td>
      <td>1H</td>
      <td>2730.426</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>14808</td>
      <td>İ. Gündoğan</td>
      <td>Shot</td>
      <td>Shot</td>
      <td>[Right foot, Opportunity, Position: Goal cente...</td>
      <td>76.96</td>
      <td>38.08</td>
      <td>104.00</td>
      <td>34.00</td>
    </tr>
    <tr>
      <th>960</th>
      <td>2499895</td>
      <td>215109497</td>
      <td>2H</td>
      <td>381.565</td>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>48</td>
      <td>J. Vertonghen</td>
      <td>Shot</td>
      <td>Shot</td>
      <td>[Right foot, Blocked, Not accurate]</td>
      <td>87.36</td>
      <td>46.24</td>
      <td>93.60</td>
      <td>44.20</td>
    </tr>
    <tr>
      <th>1027</th>
      <td>2499895</td>
      <td>215109572</td>
      <td>2H</td>
      <td>574.986</td>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>8717</td>
      <td>H. Kane</td>
      <td>Shot</td>
      <td>Shot</td>
      <td>[Right foot, Opportunity, Position: Goal cente...</td>
      <td>80.08</td>
      <td>35.36</td>
      <td>104.00</td>
      <td>34.00</td>
    </tr>
    <tr>
      <th>1072</th>
      <td>2499895</td>
      <td>215109661</td>
      <td>2H</td>
      <td>779.26</td>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>8292</td>
      <td>D. Rose</td>
      <td>Shot</td>
      <td>Shot</td>
      <td>[Left foot, Opportunity, Position: Out high ri...</td>
      <td>85.28</td>
      <td>47.60</td>
      <td>104.00</td>
      <td>34.00</td>
    </tr>
    <tr>
      <th>1081</th>
      <td>2499895</td>
      <td>215109454</td>
      <td>2H</td>
      <td>815.724</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>245364</td>
      <td>L. Sané</td>
      <td>Shot</td>
      <td>Shot</td>
      <td>[Left foot, Position: Out high left, Not accur...</td>
      <td>92.56</td>
      <td>47.60</td>
      <td>104.00</td>
      <td>34.00</td>
    </tr>
    <tr>
      <th>1162</th>
      <td>2499895</td>
      <td>215109783</td>
      <td>2H</td>
      <td>1119.05</td>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>14911</td>
      <td>Son Heung-Min</td>
      <td>Shot</td>
      <td>Shot</td>
      <td>[Left foot, Opportunity, Position: Out high le...</td>
      <td>82.16</td>
      <td>26.52</td>
      <td>104.00</td>
      <td>34.00</td>
    </tr>
    <tr>
      <th>1224</th>
      <td>2499895</td>
      <td>215109623</td>
      <td>2H</td>
      <td>1280.933</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>245364</td>
      <td>L. Sané</td>
      <td>Shot</td>
      <td>Shot</td>
      <td>[Right foot, Opportunity, Position: Goal cente...</td>
      <td>91.52</td>
      <td>39.44</td>
      <td>104.00</td>
      <td>34.00</td>
    </tr>
    <tr>
      <th>1226</th>
      <td>2499895</td>
      <td>215109624</td>
      <td>2H</td>
      <td>1283.849</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>11066</td>
      <td>R. Sterling</td>
      <td>Shot</td>
      <td>Shot</td>
      <td>[Left foot, Opportunity, Position: Out high ce...</td>
      <td>96.72</td>
      <td>32.64</td>
      <td>104.00</td>
      <td>34.00</td>
    </tr>
    <tr>
      <th>1261</th>
      <td>2499895</td>
      <td>215109644</td>
      <td>2H</td>
      <td>1486.873</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>38021</td>
      <td>K. De Bruyne</td>
      <td>Shot</td>
      <td>Shot</td>
      <td>[Goal, Counter attack, Left foot, Opportunity,...</td>
      <td>94.64</td>
      <td>48.28</td>
      <td>104.00</td>
      <td>34.00</td>
    </tr>
    <tr>
      <th>1347</th>
      <td>2499895</td>
      <td>215109704</td>
      <td>2H</td>
      <td>1799.53</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>11066</td>
      <td>R. Sterling</td>
      <td>Shot</td>
      <td>Shot</td>
      <td>[Left foot, Opportunity, Position: Out high le...</td>
      <td>92.56</td>
      <td>30.60</td>
      <td>104.00</td>
      <td>34.00</td>
    </tr>
    <tr>
      <th>1434</th>
      <td>2499895</td>
      <td>215109780</td>
      <td>2H</td>
      <td>2094.226</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>11066</td>
      <td>R. Sterling</td>
      <td>Shot</td>
      <td>Shot</td>
      <td>[Goal, Counter attack, Right foot, Opportunity...</td>
      <td>102.96</td>
      <td>29.92</td>
      <td>104.00</td>
      <td>34.00</td>
    </tr>
    <tr>
      <th>1492</th>
      <td>2499895</td>
      <td>215109837</td>
      <td>2H</td>
      <td>2416.323</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>265673</td>
      <td>Bernardo Silva</td>
      <td>Shot</td>
      <td>Shot</td>
      <td>[Left foot, Opportunity, Position: Goal low le...</td>
      <td>96.72</td>
      <td>48.28</td>
      <td>104.00</td>
      <td>34.00</td>
    </tr>
    <tr>
      <th>1537</th>
      <td>2499895</td>
      <td>215109889</td>
      <td>2H</td>
      <td>2526.102</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>11066</td>
      <td>R. Sterling</td>
      <td>Shot</td>
      <td>Shot</td>
      <td>[Left foot, Opportunity, Position: Goal high l...</td>
      <td>81.12</td>
      <td>36.72</td>
      <td>104.00</td>
      <td>34.00</td>
    </tr>
    <tr>
      <th>1566</th>
      <td>2499895</td>
      <td>215109918</td>
      <td>2H</td>
      <td>2645.632</td>
      <td>1625</td>
      <td>Manchester City</td>
      <td>11066</td>
      <td>R. Sterling</td>
      <td>Shot</td>
      <td>Shot</td>
      <td>[Goal, Right foot, Opportunity, Position: Goal...</td>
      <td>95.68</td>
      <td>27.88</td>
      <td>104.00</td>
      <td>34.00</td>
    </tr>
    <tr>
      <th>1600</th>
      <td>2499895</td>
      <td>215110118</td>
      <td>2H</td>
      <td>2826.583</td>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>54</td>
      <td>C. Eriksen</td>
      <td>Shot</td>
      <td>Shot</td>
      <td>[Goal, Left foot, Opportunity, Position: Goal ...</td>
      <td>84.24</td>
      <td>38.08</td>
      <td>104.00</td>
      <td>34.00</td>
    </tr>
  </tbody>
</table>
</div>



```python
match_events[['team_id', 'team_name', 'player_id', 'player_name']].drop_duplicates()
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>team_id</th>
      <th>team_name</th>
      <th>player_id</th>
      <th>player_name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1625</td>
      <td>Manchester City</td>
      <td>8325</td>
      <td>S. Agüero</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1625</td>
      <td>Manchester City</td>
      <td>105339</td>
      <td>Fernandinho</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1625</td>
      <td>Manchester City</td>
      <td>8277</td>
      <td>K. Walker</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1625</td>
      <td>Manchester City</td>
      <td>38021</td>
      <td>K. De Bruyne</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1625</td>
      <td>Manchester City</td>
      <td>11066</td>
      <td>R. Sterling</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>240070</td>
      <td>H. Winks</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>8292</td>
      <td>D. Rose</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>12</th>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>8717</td>
      <td>H. Kane</td>
    </tr>
    <tr>
      <th>14</th>
      <td>1625</td>
      <td>Manchester City</td>
      <td>70086</td>
      <td>N. Otamendi</td>
    </tr>
    <tr>
      <th>19</th>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>11152</td>
      <td>M. Dembélé</td>
    </tr>
    <tr>
      <th>20</th>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>210044</td>
      <td>E. Dier</td>
    </tr>
    <tr>
      <th>21</th>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>25381</td>
      <td>H. Lloris</td>
    </tr>
    <tr>
      <th>22</th>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>48</td>
      <td>J. Vertonghen</td>
    </tr>
    <tr>
      <th>33</th>
      <td>1625</td>
      <td>Manchester City</td>
      <td>8464</td>
      <td>F. Delph</td>
    </tr>
    <tr>
      <th>35</th>
      <td>1625</td>
      <td>Manchester City</td>
      <td>71654</td>
      <td>Ederson</td>
    </tr>
    <tr>
      <th>54</th>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>8945</td>
      <td>K. Trippier</td>
    </tr>
    <tr>
      <th>59</th>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>14911</td>
      <td>Son Heung-Min</td>
    </tr>
    <tr>
      <th>64</th>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>54</td>
      <td>C. Eriksen</td>
    </tr>
    <tr>
      <th>83</th>
      <td>1625</td>
      <td>Manchester City</td>
      <td>245364</td>
      <td>L. Sané</td>
    </tr>
    <tr>
      <th>88</th>
      <td>1625</td>
      <td>Manchester City</td>
      <td>70085</td>
      <td>E. Mangala</td>
    </tr>
    <tr>
      <th>89</th>
      <td>1625</td>
      <td>Manchester City</td>
      <td>14808</td>
      <td>İ. Gündoğan</td>
    </tr>
    <tr>
      <th>107</th>
      <td>1625</td>
      <td>Manchester City</td>
      <td>0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>139</th>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>13484</td>
      <td>D. Alli</td>
    </tr>
    <tr>
      <th>1047</th>
      <td>1625</td>
      <td>Manchester City</td>
      <td>340386</td>
      <td>Gabriel Jesus</td>
    </tr>
    <tr>
      <th>1362</th>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>20441</td>
      <td>E. Lamela</td>
    </tr>
    <tr>
      <th>1452</th>
      <td>1625</td>
      <td>Manchester City</td>
      <td>447205</td>
      <td>P. Foden</td>
    </tr>
    <tr>
      <th>1457</th>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>25804</td>
      <td>M. Sissoko</td>
    </tr>
    <tr>
      <th>1476</th>
      <td>1625</td>
      <td>Manchester City</td>
      <td>265673</td>
      <td>Bernardo Silva</td>
    </tr>
  </tbody>
</table>
</div>



```python
match_events[['team_id', 'team_name', 'player_id', 'player_name']].drop_duplicates().sort_values('team_id')
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>team_id</th>
      <th>team_name</th>
      <th>player_id</th>
      <th>player_name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>59</th>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>14911</td>
      <td>Son Heung-Min</td>
    </tr>
    <tr>
      <th>1362</th>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>20441</td>
      <td>E. Lamela</td>
    </tr>
    <tr>
      <th>54</th>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>8945</td>
      <td>K. Trippier</td>
    </tr>
    <tr>
      <th>139</th>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>13484</td>
      <td>D. Alli</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>240070</td>
      <td>H. Winks</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>8292</td>
      <td>D. Rose</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>12</th>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>8717</td>
      <td>H. Kane</td>
    </tr>
    <tr>
      <th>64</th>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>54</td>
      <td>C. Eriksen</td>
    </tr>
    <tr>
      <th>19</th>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>11152</td>
      <td>M. Dembélé</td>
    </tr>
    <tr>
      <th>20</th>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>210044</td>
      <td>E. Dier</td>
    </tr>
    <tr>
      <th>21</th>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>25381</td>
      <td>H. Lloris</td>
    </tr>
    <tr>
      <th>22</th>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>48</td>
      <td>J. Vertonghen</td>
    </tr>
    <tr>
      <th>1457</th>
      <td>1624</td>
      <td>Tottenham Hotspur</td>
      <td>25804</td>
      <td>M. Sissoko</td>
    </tr>
    <tr>
      <th>0</th>
      <td>1625</td>
      <td>Manchester City</td>
      <td>8325</td>
      <td>S. Agüero</td>
    </tr>
    <tr>
      <th>107</th>
      <td>1625</td>
      <td>Manchester City</td>
      <td>0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1047</th>
      <td>1625</td>
      <td>Manchester City</td>
      <td>340386</td>
      <td>Gabriel Jesus</td>
    </tr>
    <tr>
      <th>1452</th>
      <td>1625</td>
      <td>Manchester City</td>
      <td>447205</td>
      <td>P. Foden</td>
    </tr>
    <tr>
      <th>89</th>
      <td>1625</td>
      <td>Manchester City</td>
      <td>14808</td>
      <td>İ. Gündoğan</td>
    </tr>
    <tr>
      <th>88</th>
      <td>1625</td>
      <td>Manchester City</td>
      <td>70085</td>
      <td>E. Mangala</td>
    </tr>
    <tr>
      <th>33</th>
      <td>1625</td>
      <td>Manchester City</td>
      <td>8464</td>
      <td>F. Delph</td>
    </tr>
    <tr>
      <th>35</th>
      <td>1625</td>
      <td>Manchester City</td>
      <td>71654</td>
      <td>Ederson</td>
    </tr>
    <tr>
      <th>14</th>
      <td>1625</td>
      <td>Manchester City</td>
      <td>70086</td>
      <td>N. Otamendi</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1625</td>
      <td>Manchester City</td>
      <td>11066</td>
      <td>R. Sterling</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1625</td>
      <td>Manchester City</td>
      <td>38021</td>
      <td>K. De Bruyne</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1625</td>
      <td>Manchester City</td>
      <td>8277</td>
      <td>K. Walker</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1625</td>
      <td>Manchester City</td>
      <td>105339</td>
      <td>Fernandinho</td>
    </tr>
    <tr>
      <th>83</th>
      <td>1625</td>
      <td>Manchester City</td>
      <td>245364</td>
      <td>L. Sané</td>
    </tr>
    <tr>
      <th>1476</th>
      <td>1625</td>
      <td>Manchester City</td>
      <td>265673</td>
      <td>Bernardo Silva</td>
    </tr>
  </tbody>
</table>
</div>



```python
#이벤트와 sub event의 고유값 확인 및 정렬
match_events[['event_type','sub_event_type']].drop_duplicates().sort_values(['event_type','sub_event_type'])
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>event_type</th>
      <th>sub_event_type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>16</th>
      <td>Duel</td>
      <td>Air duel</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Duel</td>
      <td>Ground attacking duel</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Duel</td>
      <td>Ground defending duel</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Duel</td>
      <td>Ground loose ball duel</td>
    </tr>
    <tr>
      <th>34</th>
      <td>Foul</td>
      <td>Foul</td>
    </tr>
    <tr>
      <th>650</th>
      <td>Foul</td>
      <td>Hand foul</td>
    </tr>
    <tr>
      <th>298</th>
      <td>Free kick</td>
      <td>Corner</td>
    </tr>
    <tr>
      <th>35</th>
      <td>Free kick</td>
      <td>Free kick</td>
    </tr>
    <tr>
      <th>203</th>
      <td>Free kick</td>
      <td>Free kick cross</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Free kick</td>
      <td>Goal kick</td>
    </tr>
    <tr>
      <th>1346</th>
      <td>Free kick</td>
      <td>Penalty</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Free kick</td>
      <td>Throw in</td>
    </tr>
    <tr>
      <th>954</th>
      <td>Goalkeeper leaving line</td>
      <td>Goalkeeper leaving line</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Interruption</td>
      <td>Ball out of the field</td>
    </tr>
    <tr>
      <th>555</th>
      <td>Interruption</td>
      <td>Whistle</td>
    </tr>
    <tr>
      <th>214</th>
      <td>Offside</td>
      <td></td>
    </tr>
    <tr>
      <th>353</th>
      <td>Others on the ball</td>
      <td>Acceleration</td>
    </tr>
    <tr>
      <th>113</th>
      <td>Others on the ball</td>
      <td>Clearance</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Others on the ball</td>
      <td>Touch</td>
    </tr>
    <tr>
      <th>189</th>
      <td>Pass</td>
      <td>Cross</td>
    </tr>
    <tr>
      <th>145</th>
      <td>Pass</td>
      <td>Hand pass</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Pass</td>
      <td>Head pass</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Pass</td>
      <td>High pass</td>
    </tr>
    <tr>
      <th>313</th>
      <td>Pass</td>
      <td>Launch</td>
    </tr>
    <tr>
      <th>0</th>
      <td>Pass</td>
      <td>Simple pass</td>
    </tr>
    <tr>
      <th>160</th>
      <td>Pass</td>
      <td>Smart pass</td>
    </tr>
    <tr>
      <th>300</th>
      <td>Save attempt</td>
      <td>Reflexes</td>
    </tr>
    <tr>
      <th>481</th>
      <td>Save attempt</td>
      <td>Save attempt</td>
    </tr>
    <tr>
      <th>204</th>
      <td>Shot</td>
      <td>Shot</td>
    </tr>
    <tr>
      <th>1047</th>
      <td>Substitution</td>
      <td>Player in</td>
    </tr>
    <tr>
      <th>1048</th>
      <td>Substitution</td>
      <td>Player out</td>
    </tr>
  </tbody>
</table>
</div>

