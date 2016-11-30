
# My MongoDB Project 2

### By: Kyle Killion
__Goals of the Project__:
* Load Dependencies of the Project using Python
* Connect to MySQL University DataBase
* Convert into Pandas DataFrame and/or CSVs
* __Init__ MongoDB
* Write the Data to MongoDB
* Conduct a query within MongoDB to find 
  salaries greater than the mean


```python
#Load Dependencies
from pprint import pprint
import pandas as pd
import pymongo as mdb
import MySQLdb
import pandas.io.sql as psql

#Establish Connection to MySQLDataBase::University
cnxn = MySQLdb.connect(host = 'localhost',
                       user = 'root',
                       passwd = input('PWD: '),
                       db = 'university')

#Get the Tables in the DB
cur = cnxn.cursor()
cur.execute("SHOW tables")
tables = cur.fetchall()

#Use Tables to Loop through and build DataFrame
df = pd.DataFrame()
for table in tables:
   
    try:
        sql = "SELECT * FROM %s" % table

        #Uncomment if you want to write all the files to your desktop
        #df = psql.read_sql(sql, cnxn)
        #df.to_csv(r'C:/Users/hb13316/Desktop/%s.csv' % table)
        
        df = df.append(psql.read_sql(sql, cnxn))
        
    except Exception as e:
        print(str(e), 'found nothing')
        pass
        
#Close Connection
cur.close()
cnxn.close()



```

    PWD: no no no 
    


    ---------------------------------------------------------------------------

    OperationalError                          Traceback (most recent call last)

    <ipython-input-6-a6213df35339> in <module>()
         10                        user = 'root',
         11                        passwd = input('PWD: '),
    ---> 12                        db = 'university')
         13 
         14 #Get the Tables in the DB
    

    C:\Anaconda3\Lib\site-packages\MySQLdb\__init__.py in Connect(*args, **kwargs)
         79     """Factory function for connections.Connection."""
         80     from MySQLdb.connections import Connection
    ---> 81     return Connection(*args, **kwargs)
         82 
         83 connect = Connection = Connect
    

    C:\Anaconda3\Lib\site-packages\MySQLdb\connections.py in __init__(self, *args, **kwargs)
        189         self.waiter = kwargs2.pop('waiter', None)
        190 
    --> 191         super(Connection, self).__init__(*args, **kwargs2)
        192         self.cursorclass = cursorclass
        193         self.encoders = dict([ (k, v) for k, v in conv.items()
    

    OperationalError: (1045, "Access denied for user 'root'@'localhost' (using password: YES)")


# Look at the DataBase Info
<hr>


```python
df.info()
df.describe()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 29928 entries, 0 to 19
    Data columns (total 40 columns):
    Address_1       0 non-null object
    Budget          0 non-null object
    City_1          0 non-null object
    Email           0 non-null object
    F_Name          0 non-null object
    ID              62 non-null object
    L_Name          0 non-null object
    M_Name          0 non-null object
    Password        0 non-null object
    State_1         0 non-null object
    Status          0 non-null object
    User_id         0 non-null object
    Zip_1           0 non-null object
    budget          7 non-null float64
    building        27 non-null object
    capacity        5 non-null float64
    city            29738 non-null object
    course_id       72 non-null object
    credits         13 non-null float64
    day             20 non-null object
    dept_name       45 non-null object
    end_hr          20 non-null float64
    end_min         20 non-null float64
    grade           21 non-null object
    i_ID            9 non-null object
    name            25 non-null object
    prereq_id       7 non-null object
    room_number     20 non-null object
    s_ID            9 non-null object
    salary          12 non-null float64
    sec_id          52 non-null object
    semester        52 non-null object
    start_hr        20 non-null float64
    start_min       20 non-null float64
    state           52 non-null object
    state_code      29790 non-null object
    time_slot_id    35 non-null object
    title           13 non-null object
    tot_cred        13 non-null float64
    year            52 non-null float64
    dtypes: float64(10), object(30)
    memory usage: 9.4+ MB
    

    C:\Anaconda3\Lib\site-packages\numpy\lib\function_base.py:3834: RuntimeWarning: Invalid value encountered in percentile
      RuntimeWarning)
    




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>budget</th>
      <th>capacity</th>
      <th>credits</th>
      <th>end_hr</th>
      <th>end_min</th>
      <th>salary</th>
      <th>start_hr</th>
      <th>start_min</th>
      <th>tot_cred</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>7.000000</td>
      <td>5.00000</td>
      <td>13.000000</td>
      <td>20.000000</td>
      <td>20.000000</td>
      <td>12.000000</td>
      <td>20.000000</td>
      <td>20.00000</td>
      <td>13.000000</td>
      <td>52.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>85000.000000</td>
      <td>132.00000</td>
      <td>3.384615</td>
      <td>11.750000</td>
      <td>48.000000</td>
      <td>74833.333333</td>
      <td>11.450000</td>
      <td>6.00000</td>
      <td>65.692308</td>
      <td>2009.480769</td>
    </tr>
    <tr>
      <th>std</th>
      <td>22173.557826</td>
      <td>206.92994</td>
      <td>0.506370</td>
      <td>2.788605</td>
      <td>4.701623</td>
      <td>16055.774002</td>
      <td>2.742934</td>
      <td>12.31174</td>
      <td>34.649157</td>
      <td>0.504505</td>
    </tr>
    <tr>
      <th>min</th>
      <td>50000.000000</td>
      <td>10.00000</td>
      <td>3.000000</td>
      <td>8.000000</td>
      <td>30.000000</td>
      <td>40000.000000</td>
      <td>8.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>2009.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>max</th>
      <td>120000.000000</td>
      <td>500.00000</td>
      <td>4.000000</td>
      <td>16.000000</td>
      <td>50.000000</td>
      <td>95000.000000</td>
      <td>16.000000</td>
      <td>30.00000</td>
      <td>120.000000</td>
      <td>2010.000000</td>
    </tr>
  </tbody>
</table>
</div>



# MongoDB Setup and Kung Fu
<hr>
___Found this link very helpful into ensuring an operating MongoDB___:
* _http://stackoverflow.com/questions/26585433/mongodb-failed-to-connect-to-127-0-0-127017-reason-errno10061_


```python
#Initialize the MongoDB Connection and database
client = mdb.MongoClient('localhost')
db = client.university

#Take my DataFrame and insert the whole thing in one go
db.university.insert_many(df.to_dict('records'))

```




    <pymongo.results.InsertManyResult at 0x9ae86c0>




```python
#Query in MongoDB that finds all the salaries above the Mean
mongCur = db.university.aggregate([
    {'$match': {"salary":{"$gte":74833.333333}}}
    ])
            
for result in mongCur:
    pprint(result)
```

    {'Address_1': nan,
     'Budget': nan,
     'City_1': nan,
     'Email': nan,
     'F_Name': nan,
     'ID': '12121',
     'L_Name': nan,
     'M_Name': nan,
     'Password': nan,
     'State_1': nan,
     'Status': nan,
     'User_id': nan,
     'Zip_1': nan,
     '_id': ObjectId('583f2c828404fe2294aaeb2a'),
     'budget': nan,
     'building': nan,
     'capacity': nan,
     'city': nan,
     'course_id': nan,
     'credits': nan,
     'day': nan,
     'dept_name': 'Finance',
     'end_hr': nan,
     'end_min': nan,
     'grade': nan,
     'i_ID': nan,
     'name': 'Wu',
     'prereq_id': nan,
     'room_number': nan,
     's_ID': nan,
     'salary': 90000.0,
     'sec_id': nan,
     'semester': nan,
     'start_hr': nan,
     'start_min': nan,
     'state': nan,
     'state_code': nan,
     'time_slot_id': nan,
     'title': nan,
     'tot_cred': nan,
     'year': nan}
    {'Address_1': nan,
     'Budget': nan,
     'City_1': nan,
     'Email': nan,
     'F_Name': nan,
     'ID': '22222',
     'L_Name': nan,
     'M_Name': nan,
     'Password': nan,
     'State_1': nan,
     'Status': nan,
     'User_id': nan,
     'Zip_1': nan,
     '_id': ObjectId('583f2c828404fe2294aaeb2c'),
     'budget': nan,
     'building': nan,
     'capacity': nan,
     'city': nan,
     'course_id': nan,
     'credits': nan,
     'day': nan,
     'dept_name': 'Physics',
     'end_hr': nan,
     'end_min': nan,
     'grade': nan,
     'i_ID': nan,
     'name': 'Einstein',
     'prereq_id': nan,
     'room_number': nan,
     's_ID': nan,
     'salary': 95000.0,
     'sec_id': nan,
     'semester': nan,
     'start_hr': nan,
     'start_min': nan,
     'state': nan,
     'state_code': nan,
     'time_slot_id': nan,
     'title': nan,
     'tot_cred': nan,
     'year': nan}
    {'Address_1': nan,
     'Budget': nan,
     'City_1': nan,
     'Email': nan,
     'F_Name': nan,
     'ID': '33456',
     'L_Name': nan,
     'M_Name': nan,
     'Password': nan,
     'State_1': nan,
     'Status': nan,
     'User_id': nan,
     'Zip_1': nan,
     '_id': ObjectId('583f2c828404fe2294aaeb2e'),
     'budget': nan,
     'building': nan,
     'capacity': nan,
     'city': nan,
     'course_id': nan,
     'credits': nan,
     'day': nan,
     'dept_name': 'Physics',
     'end_hr': nan,
     'end_min': nan,
     'grade': nan,
     'i_ID': nan,
     'name': 'Gold',
     'prereq_id': nan,
     'room_number': nan,
     's_ID': nan,
     'salary': 87000.0,
     'sec_id': nan,
     'semester': nan,
     'start_hr': nan,
     'start_min': nan,
     'state': nan,
     'state_code': nan,
     'time_slot_id': nan,
     'title': nan,
     'tot_cred': nan,
     'year': nan}
    {'Address_1': nan,
     'Budget': nan,
     'City_1': nan,
     'Email': nan,
     'F_Name': nan,
     'ID': '45565',
     'L_Name': nan,
     'M_Name': nan,
     'Password': nan,
     'State_1': nan,
     'Status': nan,
     'User_id': nan,
     'Zip_1': nan,
     '_id': ObjectId('583f2c828404fe2294aaeb2f'),
     'budget': nan,
     'building': nan,
     'capacity': nan,
     'city': nan,
     'course_id': nan,
     'credits': nan,
     'day': nan,
     'dept_name': 'Comp. Sci.',
     'end_hr': nan,
     'end_min': nan,
     'grade': nan,
     'i_ID': nan,
     'name': 'Katz',
     'prereq_id': nan,
     'room_number': nan,
     's_ID': nan,
     'salary': 75000.0,
     'sec_id': nan,
     'semester': nan,
     'start_hr': nan,
     'start_min': nan,
     'state': nan,
     'state_code': nan,
     'time_slot_id': nan,
     'title': nan,
     'tot_cred': nan,
     'year': nan}
    {'Address_1': nan,
     'Budget': nan,
     'City_1': nan,
     'Email': nan,
     'F_Name': nan,
     'ID': '76543',
     'L_Name': nan,
     'M_Name': nan,
     'Password': nan,
     'State_1': nan,
     'Status': nan,
     'User_id': nan,
     'Zip_1': nan,
     '_id': ObjectId('583f2c828404fe2294aaeb31'),
     'budget': nan,
     'building': nan,
     'capacity': nan,
     'city': nan,
     'course_id': nan,
     'credits': nan,
     'day': nan,
     'dept_name': 'Finance',
     'end_hr': nan,
     'end_min': nan,
     'grade': nan,
     'i_ID': nan,
     'name': 'Singh',
     'prereq_id': nan,
     'room_number': nan,
     's_ID': nan,
     'salary': 80000.0,
     'sec_id': nan,
     'semester': nan,
     'start_hr': nan,
     'start_min': nan,
     'state': nan,
     'state_code': nan,
     'time_slot_id': nan,
     'title': nan,
     'tot_cred': nan,
     'year': nan}
    {'Address_1': nan,
     'Budget': nan,
     'City_1': nan,
     'Email': nan,
     'F_Name': nan,
     'ID': '83821',
     'L_Name': nan,
     'M_Name': nan,
     'Password': nan,
     'State_1': nan,
     'Status': nan,
     'User_id': nan,
     'Zip_1': nan,
     '_id': ObjectId('583f2c828404fe2294aaeb33'),
     'budget': nan,
     'building': nan,
     'capacity': nan,
     'city': nan,
     'course_id': nan,
     'credits': nan,
     'day': nan,
     'dept_name': 'Comp. Sci.',
     'end_hr': nan,
     'end_min': nan,
     'grade': nan,
     'i_ID': nan,
     'name': 'Brandt',
     'prereq_id': nan,
     'room_number': nan,
     's_ID': nan,
     'salary': 92000.0,
     'sec_id': nan,
     'semester': nan,
     'start_hr': nan,
     'start_min': nan,
     'state': nan,
     'state_code': nan,
     'time_slot_id': nan,
     'title': nan,
     'tot_cred': nan,
     'year': nan}
    {'Address_1': nan,
     'Budget': nan,
     'City_1': nan,
     'Email': nan,
     'F_Name': nan,
     'ID': '98345',
     'L_Name': nan,
     'M_Name': nan,
     'Password': nan,
     'State_1': nan,
     'Status': nan,
     'User_id': nan,
     'Zip_1': nan,
     '_id': ObjectId('583f2c828404fe2294aaeb34'),
     'budget': nan,
     'building': nan,
     'capacity': nan,
     'city': nan,
     'course_id': nan,
     'credits': nan,
     'day': nan,
     'dept_name': 'Elec. Eng.',
     'end_hr': nan,
     'end_min': nan,
     'grade': nan,
     'i_ID': nan,
     'name': 'Kim',
     'prereq_id': nan,
     'room_number': nan,
     's_ID': nan,
     'salary': 80000.0,
     'sec_id': nan,
     'semester': nan,
     'start_hr': nan,
     'start_min': nan,
     'state': nan,
     'state_code': nan,
     'time_slot_id': nan,
     'title': nan,
     'tot_cred': nan,
     'year': nan}
    {'Address_1': nan,
     'Budget': nan,
     'City_1': nan,
     'Email': nan,
     'F_Name': nan,
     'ID': '12121',
     'L_Name': nan,
     'M_Name': nan,
     'Password': nan,
     'State_1': nan,
     'Status': nan,
     'User_id': nan,
     'Zip_1': nan,
     '_id': ObjectId('583f2eb18404fe2294ab6013'),
     'budget': nan,
     'building': nan,
     'capacity': nan,
     'city': nan,
     'course_id': nan,
     'credits': nan,
     'day': nan,
     'dept_name': 'Finance',
     'end_hr': nan,
     'end_min': nan,
     'grade': nan,
     'i_ID': nan,
     'name': 'Wu',
     'prereq_id': nan,
     'room_number': nan,
     's_ID': nan,
     'salary': 90000.0,
     'sec_id': nan,
     'semester': nan,
     'start_hr': nan,
     'start_min': nan,
     'state': nan,
     'state_code': nan,
     'time_slot_id': nan,
     'title': nan,
     'tot_cred': nan,
     'year': nan}
    {'Address_1': nan,
     'Budget': nan,
     'City_1': nan,
     'Email': nan,
     'F_Name': nan,
     'ID': '22222',
     'L_Name': nan,
     'M_Name': nan,
     'Password': nan,
     'State_1': nan,
     'Status': nan,
     'User_id': nan,
     'Zip_1': nan,
     '_id': ObjectId('583f2eb18404fe2294ab6015'),
     'budget': nan,
     'building': nan,
     'capacity': nan,
     'city': nan,
     'course_id': nan,
     'credits': nan,
     'day': nan,
     'dept_name': 'Physics',
     'end_hr': nan,
     'end_min': nan,
     'grade': nan,
     'i_ID': nan,
     'name': 'Einstein',
     'prereq_id': nan,
     'room_number': nan,
     's_ID': nan,
     'salary': 95000.0,
     'sec_id': nan,
     'semester': nan,
     'start_hr': nan,
     'start_min': nan,
     'state': nan,
     'state_code': nan,
     'time_slot_id': nan,
     'title': nan,
     'tot_cred': nan,
     'year': nan}
    {'Address_1': nan,
     'Budget': nan,
     'City_1': nan,
     'Email': nan,
     'F_Name': nan,
     'ID': '33456',
     'L_Name': nan,
     'M_Name': nan,
     'Password': nan,
     'State_1': nan,
     'Status': nan,
     'User_id': nan,
     'Zip_1': nan,
     '_id': ObjectId('583f2eb18404fe2294ab6017'),
     'budget': nan,
     'building': nan,
     'capacity': nan,
     'city': nan,
     'course_id': nan,
     'credits': nan,
     'day': nan,
     'dept_name': 'Physics',
     'end_hr': nan,
     'end_min': nan,
     'grade': nan,
     'i_ID': nan,
     'name': 'Gold',
     'prereq_id': nan,
     'room_number': nan,
     's_ID': nan,
     'salary': 87000.0,
     'sec_id': nan,
     'semester': nan,
     'start_hr': nan,
     'start_min': nan,
     'state': nan,
     'state_code': nan,
     'time_slot_id': nan,
     'title': nan,
     'tot_cred': nan,
     'year': nan}
    {'Address_1': nan,
     'Budget': nan,
     'City_1': nan,
     'Email': nan,
     'F_Name': nan,
     'ID': '45565',
     'L_Name': nan,
     'M_Name': nan,
     'Password': nan,
     'State_1': nan,
     'Status': nan,
     'User_id': nan,
     'Zip_1': nan,
     '_id': ObjectId('583f2eb18404fe2294ab6018'),
     'budget': nan,
     'building': nan,
     'capacity': nan,
     'city': nan,
     'course_id': nan,
     'credits': nan,
     'day': nan,
     'dept_name': 'Comp. Sci.',
     'end_hr': nan,
     'end_min': nan,
     'grade': nan,
     'i_ID': nan,
     'name': 'Katz',
     'prereq_id': nan,
     'room_number': nan,
     's_ID': nan,
     'salary': 75000.0,
     'sec_id': nan,
     'semester': nan,
     'start_hr': nan,
     'start_min': nan,
     'state': nan,
     'state_code': nan,
     'time_slot_id': nan,
     'title': nan,
     'tot_cred': nan,
     'year': nan}
    {'Address_1': nan,
     'Budget': nan,
     'City_1': nan,
     'Email': nan,
     'F_Name': nan,
     'ID': '76543',
     'L_Name': nan,
     'M_Name': nan,
     'Password': nan,
     'State_1': nan,
     'Status': nan,
     'User_id': nan,
     'Zip_1': nan,
     '_id': ObjectId('583f2eb18404fe2294ab601a'),
     'budget': nan,
     'building': nan,
     'capacity': nan,
     'city': nan,
     'course_id': nan,
     'credits': nan,
     'day': nan,
     'dept_name': 'Finance',
     'end_hr': nan,
     'end_min': nan,
     'grade': nan,
     'i_ID': nan,
     'name': 'Singh',
     'prereq_id': nan,
     'room_number': nan,
     's_ID': nan,
     'salary': 80000.0,
     'sec_id': nan,
     'semester': nan,
     'start_hr': nan,
     'start_min': nan,
     'state': nan,
     'state_code': nan,
     'time_slot_id': nan,
     'title': nan,
     'tot_cred': nan,
     'year': nan}
    {'Address_1': nan,
     'Budget': nan,
     'City_1': nan,
     'Email': nan,
     'F_Name': nan,
     'ID': '83821',
     'L_Name': nan,
     'M_Name': nan,
     'Password': nan,
     'State_1': nan,
     'Status': nan,
     'User_id': nan,
     'Zip_1': nan,
     '_id': ObjectId('583f2eb18404fe2294ab601c'),
     'budget': nan,
     'building': nan,
     'capacity': nan,
     'city': nan,
     'course_id': nan,
     'credits': nan,
     'day': nan,
     'dept_name': 'Comp. Sci.',
     'end_hr': nan,
     'end_min': nan,
     'grade': nan,
     'i_ID': nan,
     'name': 'Brandt',
     'prereq_id': nan,
     'room_number': nan,
     's_ID': nan,
     'salary': 92000.0,
     'sec_id': nan,
     'semester': nan,
     'start_hr': nan,
     'start_min': nan,
     'state': nan,
     'state_code': nan,
     'time_slot_id': nan,
     'title': nan,
     'tot_cred': nan,
     'year': nan}
    {'Address_1': nan,
     'Budget': nan,
     'City_1': nan,
     'Email': nan,
     'F_Name': nan,
     'ID': '98345',
     'L_Name': nan,
     'M_Name': nan,
     'Password': nan,
     'State_1': nan,
     'Status': nan,
     'User_id': nan,
     'Zip_1': nan,
     '_id': ObjectId('583f2eb18404fe2294ab601d'),
     'budget': nan,
     'building': nan,
     'capacity': nan,
     'city': nan,
     'course_id': nan,
     'credits': nan,
     'day': nan,
     'dept_name': 'Elec. Eng.',
     'end_hr': nan,
     'end_min': nan,
     'grade': nan,
     'i_ID': nan,
     'name': 'Kim',
     'prereq_id': nan,
     'room_number': nan,
     's_ID': nan,
     'salary': 80000.0,
     'sec_id': nan,
     'semester': nan,
     'start_hr': nan,
     'start_min': nan,
     'state': nan,
     'state_code': nan,
     'time_slot_id': nan,
     'title': nan,
     'tot_cred': nan,
     'year': nan}
    {'Address_1': nan,
     'Budget': nan,
     'City_1': nan,
     'Email': nan,
     'F_Name': nan,
     'ID': '12121',
     'L_Name': nan,
     'M_Name': nan,
     'Password': nan,
     'State_1': nan,
     'Status': nan,
     'User_id': nan,
     'Zip_1': nan,
     '_id': ObjectId('583f3c698404fe2294abd4fc'),
     'budget': nan,
     'building': nan,
     'capacity': nan,
     'city': nan,
     'course_id': nan,
     'credits': nan,
     'day': nan,
     'dept_name': 'Finance',
     'end_hr': nan,
     'end_min': nan,
     'grade': nan,
     'i_ID': nan,
     'name': 'Wu',
     'prereq_id': nan,
     'room_number': nan,
     's_ID': nan,
     'salary': 90000.0,
     'sec_id': nan,
     'semester': nan,
     'start_hr': nan,
     'start_min': nan,
     'state': nan,
     'state_code': nan,
     'time_slot_id': nan,
     'title': nan,
     'tot_cred': nan,
     'year': nan}
    {'Address_1': nan,
     'Budget': nan,
     'City_1': nan,
     'Email': nan,
     'F_Name': nan,
     'ID': '22222',
     'L_Name': nan,
     'M_Name': nan,
     'Password': nan,
     'State_1': nan,
     'Status': nan,
     'User_id': nan,
     'Zip_1': nan,
     '_id': ObjectId('583f3c698404fe2294abd4fe'),
     'budget': nan,
     'building': nan,
     'capacity': nan,
     'city': nan,
     'course_id': nan,
     'credits': nan,
     'day': nan,
     'dept_name': 'Physics',
     'end_hr': nan,
     'end_min': nan,
     'grade': nan,
     'i_ID': nan,
     'name': 'Einstein',
     'prereq_id': nan,
     'room_number': nan,
     's_ID': nan,
     'salary': 95000.0,
     'sec_id': nan,
     'semester': nan,
     'start_hr': nan,
     'start_min': nan,
     'state': nan,
     'state_code': nan,
     'time_slot_id': nan,
     'title': nan,
     'tot_cred': nan,
     'year': nan}
    {'Address_1': nan,
     'Budget': nan,
     'City_1': nan,
     'Email': nan,
     'F_Name': nan,
     'ID': '33456',
     'L_Name': nan,
     'M_Name': nan,
     'Password': nan,
     'State_1': nan,
     'Status': nan,
     'User_id': nan,
     'Zip_1': nan,
     '_id': ObjectId('583f3c698404fe2294abd500'),
     'budget': nan,
     'building': nan,
     'capacity': nan,
     'city': nan,
     'course_id': nan,
     'credits': nan,
     'day': nan,
     'dept_name': 'Physics',
     'end_hr': nan,
     'end_min': nan,
     'grade': nan,
     'i_ID': nan,
     'name': 'Gold',
     'prereq_id': nan,
     'room_number': nan,
     's_ID': nan,
     'salary': 87000.0,
     'sec_id': nan,
     'semester': nan,
     'start_hr': nan,
     'start_min': nan,
     'state': nan,
     'state_code': nan,
     'time_slot_id': nan,
     'title': nan,
     'tot_cred': nan,
     'year': nan}
    {'Address_1': nan,
     'Budget': nan,
     'City_1': nan,
     'Email': nan,
     'F_Name': nan,
     'ID': '45565',
     'L_Name': nan,
     'M_Name': nan,
     'Password': nan,
     'State_1': nan,
     'Status': nan,
     'User_id': nan,
     'Zip_1': nan,
     '_id': ObjectId('583f3c698404fe2294abd501'),
     'budget': nan,
     'building': nan,
     'capacity': nan,
     'city': nan,
     'course_id': nan,
     'credits': nan,
     'day': nan,
     'dept_name': 'Comp. Sci.',
     'end_hr': nan,
     'end_min': nan,
     'grade': nan,
     'i_ID': nan,
     'name': 'Katz',
     'prereq_id': nan,
     'room_number': nan,
     's_ID': nan,
     'salary': 75000.0,
     'sec_id': nan,
     'semester': nan,
     'start_hr': nan,
     'start_min': nan,
     'state': nan,
     'state_code': nan,
     'time_slot_id': nan,
     'title': nan,
     'tot_cred': nan,
     'year': nan}
    {'Address_1': nan,
     'Budget': nan,
     'City_1': nan,
     'Email': nan,
     'F_Name': nan,
     'ID': '76543',
     'L_Name': nan,
     'M_Name': nan,
     'Password': nan,
     'State_1': nan,
     'Status': nan,
     'User_id': nan,
     'Zip_1': nan,
     '_id': ObjectId('583f3c698404fe2294abd503'),
     'budget': nan,
     'building': nan,
     'capacity': nan,
     'city': nan,
     'course_id': nan,
     'credits': nan,
     'day': nan,
     'dept_name': 'Finance',
     'end_hr': nan,
     'end_min': nan,
     'grade': nan,
     'i_ID': nan,
     'name': 'Singh',
     'prereq_id': nan,
     'room_number': nan,
     's_ID': nan,
     'salary': 80000.0,
     'sec_id': nan,
     'semester': nan,
     'start_hr': nan,
     'start_min': nan,
     'state': nan,
     'state_code': nan,
     'time_slot_id': nan,
     'title': nan,
     'tot_cred': nan,
     'year': nan}
    {'Address_1': nan,
     'Budget': nan,
     'City_1': nan,
     'Email': nan,
     'F_Name': nan,
     'ID': '83821',
     'L_Name': nan,
     'M_Name': nan,
     'Password': nan,
     'State_1': nan,
     'Status': nan,
     'User_id': nan,
     'Zip_1': nan,
     '_id': ObjectId('583f3c698404fe2294abd505'),
     'budget': nan,
     'building': nan,
     'capacity': nan,
     'city': nan,
     'course_id': nan,
     'credits': nan,
     'day': nan,
     'dept_name': 'Comp. Sci.',
     'end_hr': nan,
     'end_min': nan,
     'grade': nan,
     'i_ID': nan,
     'name': 'Brandt',
     'prereq_id': nan,
     'room_number': nan,
     's_ID': nan,
     'salary': 92000.0,
     'sec_id': nan,
     'semester': nan,
     'start_hr': nan,
     'start_min': nan,
     'state': nan,
     'state_code': nan,
     'time_slot_id': nan,
     'title': nan,
     'tot_cred': nan,
     'year': nan}
    {'Address_1': nan,
     'Budget': nan,
     'City_1': nan,
     'Email': nan,
     'F_Name': nan,
     'ID': '98345',
     'L_Name': nan,
     'M_Name': nan,
     'Password': nan,
     'State_1': nan,
     'Status': nan,
     'User_id': nan,
     'Zip_1': nan,
     '_id': ObjectId('583f3c698404fe2294abd506'),
     'budget': nan,
     'building': nan,
     'capacity': nan,
     'city': nan,
     'course_id': nan,
     'credits': nan,
     'day': nan,
     'dept_name': 'Elec. Eng.',
     'end_hr': nan,
     'end_min': nan,
     'grade': nan,
     'i_ID': nan,
     'name': 'Kim',
     'prereq_id': nan,
     'room_number': nan,
     's_ID': nan,
     'salary': 80000.0,
     'sec_id': nan,
     'semester': nan,
     'start_hr': nan,
     'start_min': nan,
     'state': nan,
     'state_code': nan,
     'time_slot_id': nan,
     'title': nan,
     'tot_cred': nan,
     'year': nan}
    
