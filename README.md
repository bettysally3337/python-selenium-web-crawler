# python-selenium-web-crawler 
!pip install beautifulsoup4
import requests
from bs4 import BeautifulSoup

#找文章標題#
nature_url = 'https://www.nature.com/articles/s41467-020-19943-y'
req_nature = requests.get(nature_url)
#print文獻標題
if req_nature.status_code == requests.codes.ok:
  soup = BeautifulSoup(req_nature.text, 'html.parser')
  re_ti = soup.find_all('h1', class_='c-article-title')
  for t in re_ti:
    print("標題:"+ t.text+"\n")
    
#print文獻作者
re_au = soup.find_all(attrs={"data-test":"author-name"})
print("作者:")
for a in re_au:
  print(a.text)
  
#print期刊名稱及期號
re_jou = soup.find_all(attrs={"data-test":"journal-title"})
print("期刊名:")
for j in re_jou:
  print(j.text)
re_jou_vol = soup.find_all(attrs={"data-test":"journal-volume"})
print("期刊卷期編號:")
for v in re_jou_vol:
  print(v.text)
 
#print文獻出版年
re_pub = soup.find("time")
print("出版年:")
for p in re_pub:
  print(p)
  
#print文獻摘要
re_abs = soup.find("div",class_ = "c-article-section__content")
print("摘要:")
for a in re_abs:
  print(a.text)

#下載chrome
!apt install chromium-chromedriver

#使用selenium及chromedrive
!pip install selenium
!apt-get update # to update ubuntu to correctly run apt install
!apt install chromium-chromedriver
!cp /usr/lib/chromium-browser/chromedriver/usr/bin
import sys
import time
sys.path.insert(0,'/usr/lib/chromium-browser/chromedriver')
from selenium import webdriver
options = webdriver.ChromeOptions()
chrome_options = webdriver.ChromeOptions()
chrome_options.add_argument('--headless')
chrome_options.add_argument('--no-sandbox')
chrome_options.add_argument('--disable-dev-shm-usage')

wd = webdriver.Chrome('/usr/bin/chromedriver',chrome_options=chrome_options)
wd.get(nature_url)

#點擊下載文獻按鈕
button = wd.find_element_by_link_text("Download PDF")
wd.execute_script("arguments[0].click();", button)
time.sleep(2)
wd.quit()


