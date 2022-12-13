from selenium import webdriver
from selenium.webdriver.common.alert import Alert
import pandas as pd
import openpyxl
import time

df = pd.read_excel("12.12.22 Markdown Price Changes.xlsx", "CARD user input ISBN")
wb = openpyxl.load_workbook("12.12.22 Markdown Price Changes.xlsx")
sheet = wb.active
driver = webdriver.Chrome()

driver.get("https://tools.booksamillion.com/service/product/gift#")
for x in range(78):
    str1 = df['ISBN'][x]
    pid = driver.find_element(By.XPATH, "/html/body/div[4]/div/div[1]/input")
    pid.sendKeys(str1)
    pid = driver.find_element(By.XPATH, "/html/body/div[4]/div/div[1]/a")
    pid.click()
    time.sleep(4)
    pid = driver.find_element(By.XPATH, "/html/body/div[4]/div/div[2]/ul/li[2]/a")
    pid.click()
    time.sleep(1)
    
    str2 = df['Retail'][x]
    if (len(str2) == 5):
        str3 = str2[1:2]
    else:
        str3 = str2[1:3]
    pid = driver.find_element(By.XPATH, "/html/body/div[4]/div/div[2]/div[1]/div[2]/ul/li[2]/div[1]/input")
    pid.sendKeys(str3)
    
    if (len(str2) == 5):
        str4 = str2[3:5]
    else:
        str4 = str2[4:6]
    pid = driver.find_element(By.XPATH, "/html/body/div[4]/div/div[2]/div[1]/div[2]/ul/li[2]/div[2]/input")
    pid.sendKeys(str4)
    
    pid = driver.find_element(By.XPATH, "/html/body/div[4]/div/div[2]/div[2]/a[1]")
    pid.click()
    time.sleep(2)
    
    a = driver.switch_to.alert
    a.accept()
    time.sleep(4)
    
driver.close()
wb.save("12.12.22 Markdown Price Changes.xlsx")
