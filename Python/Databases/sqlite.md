```python
import sqlite3

def connect(file):
    return sqlite3.connect(file);

def select(conn, table):
    data = []
    cursor = conn.execute("SELECT * from "+table)

    for row in cursor:
        rr = []
        for field in row:
            rr.append(field)
        data.append(rr)

    conn.close()
    return data



#conn.execute('''CREATE TABLE COMPANY
#         (ID INT PRIMARY KEY     NOT NULL,
#         NAME           TEXT    NOT NULL,
#         AGE            INT     NOT NULL,
#         ADDRESS        CHAR(50),
#         SALARY         REAL);''')

#conn.execute("INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY) \
#      VALUES (4, 'Mark', 25, 'Rich-Mond ', 65000.00 )");
#conn.commit()
#conn.close()

#conn.execute("UPDATE COMPANY set SALARY = 25000.00 where ID = 1")
#conn.commit()
#cursor = conn.execute("SELECT id, name, address, salary from COMPANY")
#conn.close()


#conn.execute("DELETE from COMPANY where ID = 2;")
#conn.commit()
#conn.close()
```