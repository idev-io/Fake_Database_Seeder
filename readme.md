# 📌 **IDev_IO**


## 📍 Code Explanation

### 📍 Requirements:
- [x] Python Installed	👉	[Python](https://www.python.org)
- [x] Editor or IDE	👉	[VS Code](https://code.visualstudio.com)
- [x] Local Server	👉	[XAMPP](https://www.apachefriends.org/index.html)

### 📍 Used Packages:
- [x] prettytable	👉	To make data pretty and readable when i show it to users.
- [x] Faker		👉	To seed fake data in database.
- [x] mysql.connector	👉	To create mysql database connection.



> For more about packages:
- 🔗 [Prettytable](https://github.com/jazzband/prettytable)
- 🔗 [Faker](https://faker.readthedocs.io/en/master)
- 🔗 [Mysql Connector](https://github.com/mysql/mysql-connector-python)
- 🔗 [To Learn more about Mysql](https://dev.mysql.com)




```python
# -----------
# IDev.
# Coding Tips
# Copyright © IDev_IO. All rights reserved
# -----------

# 📌 Mysql Fake Database Seeder

# •••••••••••••••••••• Import any package i want it in project •••••••••••••••••••• #
from prettytable import PrettyTable
from faker import *              
import mysql.connector
from mysql.connector import connect


# •••••••••••••••••••• Create Mysql Database Connection •••••••••••••••••••• #
user_name = 'Your_Database_User_Name'
user_password = 'Your_Database_Password'
host_name = 'Your_Database_host_Name 👉 Locally: localhost'
db_name = 'Your_Database_Name'

db = connect(user = user_name, 
             password = user_password, 
             host = host_name, 
             database = db_name)

# •••••••••••••••••••• cursor •••••••••••••••••••• #
# In order to put our new connnection to good use we need to create a cursor object. 
# The cursor object is an abstraction specified in Python.
# It gives us the ability to have multiple seperate working environments through the same connection to the database.

cursor = db.cursor(buffered = True)


# •••••••••••••••••••• Create Mysql Database table  •••••••••••••••••••• #
create_query = """CREATE TABLE developers( 
            id int(255) not null auto_increment primary key, 
            name varchar(255), 
            job varchar(255),
            phoneNO varchar(255),
            address varchar(255),
            bio text(255)
            );"""

# •••••••••••••••••••• Execute Mysql query  •••••••••••••••••••• #
cursor.execute(create_query)

# •••••••••••••••••••• Create fake object  •••••••••••••••••••• #
fake = Faker('en_US')

# •••••••••••••••••••• Create function to execute mysql queries  •••••••••••••••••••• #
def execute_queries(query):
    try:
        cursor.execute(query)
        db.commit()	# You must use [ db.commit() ] to save changes.
    except:
        db.rollback()	# To rollback if there is error ( Don't execute query)

 
 # •••••••••••••••••••• Seed Fake Data  •••••••••••••••••••• #
for _ in range(0, 100):
    
    dev_name = fake.name()
    dev_job = fake.job()
    dev_phoneNO = fake.phone_number()
    dev_address = fake.address()
    dev_bio = fake.paragraph(nb_sentences=3)
    
     # •••••••••••••••••••• Insert Mysql query  •••••••••••••••••••• #
    insert_query = "INSERT INTO developers(name, job, phoneNO, address, bio) VALUES ('%s', '%s', '%s', '%s', '%s')"\
                    %  (dev_name, dev_job, dev_phoneNO, dev_address, dev_bio)
    
     # •••••••••••••••••••• Use function execute_queries() To execute insert query  •••••••••••••••••••• #
    execute_queries(insert_query)


# •••••••••••••••••••• Show Data from Mysql Database in Prettytable  •••••••••••••••••••• #
def get_data(query):
    try:
        cursor.execute(query)
        data = cursor.fetchall()	# [ fetchall() ] To fetch all Database rows.
        
        fields = PrettyTable()
        fields.field_names = ["ID", "Developer Name", "Job", "Phone No."]

        for row in data:
            fields.add_row([row[0], row[1], row[2], row[3]])
        
        print(fields)
            
    except:
        db.rollback()
    finally:
        db.close()	# [ close() ] To close Database connection.

# •••••••••••••••••••• Fetch data Mysql query  •••••••••••••••••••• #
read_query = "SELECT * FROM developers"

get_data(read_query)
