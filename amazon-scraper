from selenium import webdriver
import time
import pandas as pd
import openpyxl
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.by import By

options = webdriver.ChromeOptions()
options.add_argument('--start-maximized')

driver = webdriver.Chrome(r'C:\chromedriver.exe', options = options)
driver.get("https://www.amazon.es/")

buscador = driver.find_element_by_xpath('/html/body/div[1]/header/div/div[1]/div[2]/div/form/div[3]/div[1]/input')
buscador.send_keys('Tarjetas gráficas', Keys.RETURN) # Searches 'Tarjetas gráficas' (graphic cards) on Amazon

btn_cookies = driver.find_element_by_xpath('/html/body/div[1]/span/form/div[2]/span[1]/span').click() # Accept cookies

lista_nombres = [] # List of product's names
lista_precios = [] # List of product's prices
n = 0

for j in range(7):
	n += 1
	s = 0
	time.sleep(1)

	try:
		while True:
			try:
				driver.get('https://www.amazon.es/s?k=Tarjetas+gráficas&page={}'.format(n))
				WebDriverWait(driver, 10).until(EC.presence_of_all_elements_located((By.XPATH, '//*[@id="search"]/div[1]/div/div[1]/div/span[3]/div[2]/div[' + str(s) + ']/div/span/div/div/div/div/div[2]/h2/a')))
				btn_articulo = driver.find_element_by_xpath('//*[@id="search"]/div[1]/div/div[1]/div/span[3]/div[2]/div[' + str(s) + ']/div/span/div/div/div/div/div[2]/h2/a').click()
				WebDriverWait(driver, 10).until(EC.presence_of_element_located((By.ID, 'productTitle')))
				nombre = driver.find_element_by_id('productTitle')
				WebDriverWait(driver, 10).until(EC.presence_of_element_located((By.ID, 'priceblock_ourprice')))
				precio = driver.find_element_by_id('priceblock_ourprice')
				driver.get('https://www.amazon.es/s?k=Tarjetas+gráficas&page={}'.format(n))
				WebDriverWait(driver, 10).until(EC.presence_of_all_elements_located((By.CLASS_NAME, 'a-size-base-plus.a-color-base.a-text-normal')))

				lista_nombres.append(nombre)
				lista_precios.append(precio)
				print(len(lista_nombres)) # I print de length of the two lists to see if the program is scraping the data
				print(len(lista_precios))
				s += 1
			except:
				s += 1
				driver.get('https://www.amazon.es/s?k=Tarjetas+gráficas&page={}'.format(n))
				driver.set_page_load_timeout(10) 
	except:
		print('Finished')



df = pd.DataFrame({'Nombre':lista_nombres, 'Precio':lista_precios})
print(df)

df.to_excel('Tarjetas gráficas Amazon.xlsx', index = False)

time.sleep(5)
driver.close()
