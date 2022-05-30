# BeautifulSoup-Project
Handling Web Scrapping for Wuzzuf.net
import requests
from bs4 import BeautifulSoup
import csv
from itertools import zip_longest
import pandas as pd
from openpyxl import Workbook

job_titles = []
company = []
Location = []
result = requests.get("https://wuzzuf.net/search/jobs/?q=&a=hpb")
src = result.content
soup = BeautifulSoup(src, "lxml")
job_titles = soup.find_all("h2", {"class":"css-m604qf"})
company = soup.find_all("a", {"class":"css-17s97q8"})
Location = soup.find_all("span", {"class":"css-5wys0k"})
for i in range(len(job_titles)):
    job_titles.append(job_titles[i].text)
    company.append(company[i].text)
    Location.append(Location[i].text)


#for link in links:
    #result = requests.get(link)
    #src = result.content
    #soup = BeautifulSoup(src, "lxml")
    #salaries = soup.find_all("div", {"class": "css-4xky9y")
 
    
    
print(job_titles, company, Location)
file_list = [job_titles, company, Location]
export1 = zip_longest(*file_list)
with open(r"C:\Users\admin\Desktop\Ahmedr.csv", "w", encoding='utf-8')as myfile:
    wr = csv.writer(myfile)
    wr.writerow(["job_titles","company", "Location"])
    wr.writerows(export1)
    
export1 = pd.read_csv(r"C:\Users\admin\Desktop\Ahmedr.csv", encoding='cp1252')
export1 = export1[export1["job_titles"].str.contains("<h2","<a") == False]
export1 = export1.style.set_properties(**{'background-color': 'black',
                           'color': 'lawngreen',
                           'border-color': 'white'})
writer = pd.ExcelWriter("wuzzuf.xlsx")
export1.to_excel(writer, sheet_name="Wuzzuf")
writer.close()

