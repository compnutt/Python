import pyodbc
import csv
import sys
import re
import os

table_name = sys.argv[1]
file_name = sys.argv[1] + '.csv'
#file_name = '.csv'
#table_name = str.replace(file_name,".csv",'')
print('TableName: ' + table_name)
dir_name = os.path.dirname(os.path.abspath(__file__))
file_name = os.path.join(dir_name, file_name)
print('FileName: ' + file_name)

conn = pyodbc.connect("DRIVER={SQL Server};"
	"SERVER=;"
	"Database=;"
	"UID=;"
	"PWD=;", autocommit=True)
cursor = conn.cursor()


#SQL="IF OBJECT_ID('dbo.[%s]', 'U') IS NOT NULL DROP TABLE dbo.[%s]" % (sys.argv[1], sys.argv[1])
#print(SQL)
#conn.execute(SQL)

SQL="IF OBJECT_ID('dbo.[%s]', 'U') IS NOT NULL \
TRUNCATE TABLE dbo.[%s] \
ELSE CREATE TABLE [dbo].[%s]( \
[Accession ID] [varchar](50) NULL \
,[Requisition ID] [varchar](50) NULL \
,[Patient Full Name] [varchar](100) NULL \
,[Date of Service] [varchar](50) NULL \
,[Date In] [varchar](50) NULL \
,[First Submission Date] [varchar](50) NULL \
,[Last Submission Date] [varchar](50) NULL \
,[Last Payment Date] [varchar](50) NULL \
,[Posted Paid Amount] [varchar](50) NULL \
,[Total Posted Bill Adjust Amount] [varchar](50) NULL \
,[Posted Patient Responsible Amount] [varchar](50) NULL \
,[Expected Due Amt] [varchar](50) NULL \
,[Procedure Codes All] [varchar](200) NULL \
,[Client Name] [varchar](200) NULL \
,[Physician Full Name] [varchar](200) NULL \
,[Physician City] [varchar](50) NULL \
,[Physician State] [varchar](50) NULL \
,[Primary Payor Name] [varchar](200) NULL)" % (table_name, table_name, table_name)

print(SQL)
conn.execute(SQL)

csvfile = open(file_name)
csv_data = csv.reader(csvfile)
next(csv_data)
SQL="insert into [%s]([Accession ID],[Requisition ID] \
,[Patient Full Name] \
,[Date of Service] \
,[Date In] \
,[First Submission Date] \
,[Last Submission Date] \
,[Last Payment Date] \
,[Posted Paid Amount] \
,[Total Posted Bill Adjust Amount] \
,[Posted Patient Responsible Amount] \
,[Expected Due Amt] \
,[Procedure Codes All] \
,[Client Name] \
,[Physician Full Name] \
,[Physician City] \
,[Physician State] \
,[Primary Payor Name]) \
values (?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?)" % table_name

for row in csv_data:

   cursor.execute(SQL, row)
conn.commit()

# Write to new csv file
cursor = conn.cursor()
cursor.execute('SELECT TOP 100 * FROM [%s]' % table_name)
with open(file_name, 'w', newline='') as fout:
    writer = csv.writer(fout)
    writer.writerow([ i[0] for i in cursor.description ]) # heading row
    writer.writerows(cursor.fetchall())

conn.close()

