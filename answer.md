<!-- 1. Couldn't figure out how to add job = 'programmer' -->
today = datetime.now() 

sql = "INSERT INTO employee (emp_num, emp_fname, emp_lname, emp_initial, emp_hiredate) VALUES (%s, %s, %s, %s, %s)"
value = (168, "John", "Doe", "JD", today)

cursor.execute(sql, value)

DB.commit()

print(cursor.rowcount, "record inserted.")

<!-- 2. -->
sql = """ SELECT 
    E.EMP_NUM, E.EMP_FNAME, E.EMP_LNAME, E.EMP_INITIAL, J.JOB_DESCRIPTION, J.JOB_CHG_HOUR, E.EMP_HIREDATE
    FROM employee AS E 
    JOIN job AS J ON E.JOB_CODE = J.JOB_CODE 
    WHERE E.EMP_NUM = 168; """

cursor.execute(sql)

result = cursor.fetchone()

print("Employee ID:", result[0],
      "\nFirst Name:", result[1], 
      "\nLast Name:", result[2], 
      "\nInitial:", result[3],
      "\nJob Description:", result[4],
      "\nCharge per Hour:", result[5],
      "\nHire Date:", result[6])

<!-- 3. -->
sql = """
    UPDATE employee
    SET job_code = (SELECT job_code FROM job WHERE job_description = 'Database Designer')
    WHERE emp_num = 168;
"""

cursor.execute(update_sql)

DB.commit()

print(cursor.rowcount, "employee updated.") 

<!-- 4. -->
sql = """ SELECT
    P.PROJ_NUM, P.PROJ_NAME, E.EMP_FNAME, E.EMP_LNAME
    FROM project AS P
    JOIN `assignment` AS A ON P.PROJ_NUM = A.PROJ_NUM
    JOIN employee AS E ON A.EMP_NUM = E.EMP_NUM
    JOIN job AS J ON E.JOB_CODE = J.JOB_CODE
    WHERE J.JOB_DESCRIPTION = 'Programmer';
"""
cursor.execute(sql)

result = cursor.fetchall()

print(result)

<!-- 5. -->
sql = """
    DELETE FROM employee 
    WHERE EMP_NUM = 168;
"""

cursor.execute(delete_sql)

DB.commit()

print(cursor.rowcount, "employee deleted.")
