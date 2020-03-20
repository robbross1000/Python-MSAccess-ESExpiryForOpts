# Python-MSAccess-ESExpiryForOpts
Python code for accessing ES fut expiry dates and syncing that ES Fut Contract as the underlying to each option

from datetime import date
from datetime import datetime, timedelta
import datetime
import calendar
import pyodbc

connOptDB = pyodbc.connect(r'Driver={Microsoft Access Driver (*.mdb, *.accdb)};DBQ=C:\Users\rross\Dropbox\OptionHistory.accdb;')
cursorESInfo = connOptDB.cursor()
#cursorOptInfo = connOptDB.cursor()

sSQL = "select * from UnderlyingInfo order by Expiry"
print(sSQL)
cursorESInfo.execute(sSQL)
print(str(cursorESInfo.rowcount))
i = 0
ESInfoList = []


for row in cursorESInfo.fetchall():
    i = i + 1
    #print (row)
    #print (row[1])
    UnderlyingInfoID = row[0]
    ESSymbol = row[1]
    ESExpiry = row[3]
    CurrentList = []
    CurrentList.append(UnderlyingInfoID)
    CurrentList.append(ESSymbol) 
    CurrentList.append(ESExpiry) 
    ESInfoList.append(CurrentList)
    #ESInfoList.Append(CurrentList)

i = 0
for x in ESInfoList:
    i += 1
    print(x)
    print(x[2])
    strExpiry = '{:%m/%d/%Y}'.format(x[2])
    if (i>1):
        sSQLOpt = "Select * from OptionDescriptions "
        sSQLOpt += " Where OptionDescriptions.Expiry > #" + strPrevExpiry + "# "
        sSQLOpt += " and OptionDescriptions.Expiry <= #" + strExpiry + "# "
        
        
        
        sSQLOptzzz = """
        Select * from OptionDescriptions Where OptionDescriptions.Expiry > ?  and OptionDescriptions.Expiry <= ?
        """
        params = (strPrevExpiry, strExpiry)
        
        
        print('SQL:')
        print(sSQLOpt)
        
        cursorESInfo.execute(sSQLOpt)
        j = 0
        for row in cursorESInfo.fetchall():
            j += 1
            if (j<10):
                print(row)
        
    strPrevExpiry = strExpiry
    if (i>100):
        break;
        
connOptDB.close()   

            
print('<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<')
print(' ')
print(' PROCESS FINISHED ')
print(' ')
print('>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>')
        
    
    
    
