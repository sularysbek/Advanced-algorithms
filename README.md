# CSS 503 - Advanced Programming Technologies

Project 1
Team ID: 17 (Chiron)

Team members:
Full name: Dinara Tursyn Student ID: 191107035
Full name: Beibarys Rysbek Student ID: 201107030
Full name: Mohamed Ibrahim Student ID: 201117001
# Import libraries
import requests
import re # regular expressions
from bs4 import BeautifulSoup
from selenium import webdriver
import pandas as pd
# use beautiful soup and selenium
CHROME_DRIVER_PATH = r'D:/загрузки/chromedriver.exe'
driver = webdriver.Chrome(CHROME_DRIVER_PATH)
total = []
# set the necessary pages
# items on the front page has tags <div class="item ddl_product a-card"...</div>
# search dataset of mobile phones
# output the full list
pages = 50

for page in range(1,pages):
    url = "https://market.kz/elektronika/telefony/mobilnye-telefony/?page=" + str(page) + "/"
    driver.get(url)
    content = driver.page_source
    soup = BeautifulSoup(content)
    for block in soup.findAll('div', attrs={'class':'item ddl_product a-card'}): 
        name_text = block.find('a', attrs={'class': 'a-card__link'}).get_text(strip = True)
        price = block.find('div', attrs={'class':'a-card__price'}).text
        description = block.find('span', attrs={'class':'description-name'}).text
        phone_model = block.find('p', attrs={'class':'a-card__breadcrumbs'}).text
        city = block.find('div', attrs={'class':'card-stats__item'}).get_text(strip = True)
        date = block.find('span', attrs={'class':'add-date'}).text
        view = block.find('span', attrs={'class':'card-stats__views views'}).text
        new = ((name_text, price, description, phone_model, city, date, view))
        total.append(new)
len(total)
1020
total
[('Телефон',
  'Договорная цена',
  'Память 32гб \nСостояние среднее \nОтпечаток работает \nЗарядка есть',
  'Мобильные телефоны » Apple',
  'Алматы',
  '29 ноября 2020 г.',
  '  15 '),
 ('Продам iPhone 11 Pro Max 64 gb',
  '365 000 ₸',
  'Продам IPhone 11 Pro Max 64 gb \n\nТелефон в идеальном состоянии, на экране плёнка с момента покупки \n\nПокупался в... ',
  'Мобильные телефоны » Apple',
  'Алматы',
  '29 ноября 2020 г.',
  '  89 '),
 ('IPhone 24Kt Gold Kazakhstan',
  '180 000 ₸',
  'Продам эксклюзивный iPhone 7/128GB.\nСостояние отличное, без сколов. Герб Казахстана выполнен из золота 24 карата.... ',
  'Мобильные телефоны » Apple',
  'Алматы',
  '29 ноября 2020 г.',
  '  817 '),
 ('iPhone 6 32 GB',
  '35 000 ₸',
  'Все в хорошем состоянии все работает с коробкой\nСостояние батареи 85%\nОтпечатка работает \nВсе есть коробка зарядка с... ',
  
  df = pd.DataFrame(total, columns=['names','prices', 'description', 'phone_model', 'city', 'date', 'view'])
df
names	prices	description	phone_model	city	date	view


# write the saved data as csv file
df.to_csv('project_chiron.csv', index=False)
# write the saved data as excel file
df.to_excel('project_chiron.xlsx', index=False)
driver.close()
