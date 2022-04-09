# Crypto-Price-Notifier

<li>It is a command line tool built in Python.</li>
<li>It is based on the concept of web scraping.</li>
<li>This tool helps user to show price of any cryptocurrency automatically.</li>
<li>It is connected to telegram bot, so user can easily check price on telegram or in the notification bar.</li>
<li>Libraries used are: Beautiful Soup, Request, Telegram Bot</li>

# Code:

```.py
import requests
from bs4 import BeautifulSoup
from plyer import notification
import time
import datetime
import os
from webserver import keep_alive

#-------------------------------------------Price of 1 dollar in rupee---------------------------------------------
while True:

 url1="https://www.xe.com/currencyconverter/convert/?Amount=1&From=USD&To=INR"

 page1=requests.get(url1)

 soup = BeautifulSoup(page1.content,"html.parser")

 onedollarinrs=soup.find("p",class_="result__BigRate-sc-1bsijpp-1 iGrAod")
 exactvalueofonedollarinrs=float(onedollarinrs.text.strip("Indian Rupees"))
#  print("Current rate of dollar in rupee is: ",exactvalueofonedollarinrs)



#--------------------------------------------Cryptocurrency Price Details--------------------------------------------
 url="https://coinmarketcap.com/currencies/band-protocol/"

 page = requests.get(url)

 soup = BeautifulSoup(page.content,"html.parser")


 details=soup.find_all("div",class_="sc-16r8icm-0 kjciSH priceSection")

 for p in details:
     name= p.find("h1",class_="priceHeading")
    #  print("Name of cryptocurrency is: "+name.text.strip())

     for pdetails in p.find_all("div",class_="sc-16r8icm-0 kjciSH priceTitle"):
         pricevalue=pdetails.find("div",class_="priceValue")

         pricevalueindollar=pricevalue.text.strip()

         finalpricevalue=float(pricevalue.text.strip("$"))
        
        #  print("Current price in dollar is: "+str(pricevalueindollar)+" and its value is: "+str(finalpricevalue*exactvalueofonedollarinrs))

        # Actual Price
        # currency = "Rs{:,.2f}".format(finalpricevalue*73.83)

        #coinswitch kuber price
         currency = "Rs{:,.2f}".format((finalpricevalue*exactvalueofonedollarinrs)+42)
        #  print("Current price in rupee according to coinswitch is: "+currency)


     break


 #-------------------------------------------Desktop notification code--------------------------------------------

#  notification.notify(title="Band Protocol Current Price {}".format(datetime.date.today())
#                       ,message="Current rate of dollar in rupee is: "+str(exactvalueofonedollarinrs)+"\nCurrent price in dollar is: "+str(pricevalueindollar)+" and its value is: "+str(finalpricevalue*exactvalueofonedollarinrs)+"\nCurrent buy price in rupee according to coinswitch is: "+currency,
#                       app_icon = "test.ico",
#                       timeout  = 60
#                       )
                      
#  time.sleep(20)

#----------------------------------------Telegram notification code---------------------------------------------------

 def telegram_bot_sendtext(bot_message):

   bot_token = os.environ['token']

   bot_chatID = [os.environ['chatid1'],os.environ['chatid2']]

   for i in range(len(bot_chatID)):
       print("Successfully sent the information to bot !"+str(bot_chatID[i]))
       send_text = 'https://api.telegram.org/bot' + str(bot_token) + '/sendMessage?chat_id=' + bot_chatID[i] + '&parse_mode=Markdown&text=' + bot_message
       response = requests.get(send_text)
      #  print(response.json())


 test = telegram_bot_sendtext("Name of coin is: Band Protocol"+"\nCurrent rate of dollar in rupee is: "+str(exactvalueofonedollarinrs)+"\nCurrent price in dollar is: "+str(pricevalueindollar)+" and its value is: "+str(finalpricevalue*exactvalueofonedollarinrs)+"\nCurrent buy price in rupee according to coinswitch is: "+currency)


 time.sleep(60*60)
keep_alive()

```

<li>In telegram notification code token and chatid will be given by user.</li>

# Demonstration

https://user-images.githubusercontent.com/62169991/149666986-85ace92c-844f-44b7-b121-a5dd6



