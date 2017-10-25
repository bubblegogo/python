#coding : UTF-8
import csv
import random
import time
import socket
import http.client
from bs4 import BeautifulSoup
import urllib.request #适合使用python 3.0以上
#import requests #适合使python3.0 以下的版本
import  zlib

def get_content(url,data = None):
    header = {
        'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8',
        'Accept-Encoding': 'gzip, deflate, sdch',
        'Accept-Language': 'zh-CN,zh;q=0.8',
        'Connection': 'keep-alive',
        'User-Agent': 'Mozilla/5.0 (Windows NT 6.3; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/43.0.235',
        'Type': 'application / x - www - form - urlencoded'
    }
    timeout = random.choice(range(80,180))
    while True:
        try:
            # req = requests.get(url,headers =header,timeout =  timeout)
            #req.encoding = 'utf-8'

            req = urllib.request.Request(url,data,header)
            response = urllib.request.urlopen(req,data,timeout=timeout)
            print(response.info()) # 打印出爬虫的编码格式 Content-Encoding 这里是  gzip格式
            mybytes =  response.read()
            decom_data = zlib.decompress(mybytes,16+zlib.MAX_WBITS)

            html = decom_data.decode('utf8')
            response.close
            break
        except urllib.request.HTTPError as e:
            print('1:',e)
            time.sleep(random.choice(range(5,10)))

        except urllib.request.URLError as e:
            print('2',e)
            time.sleep(random.choice(range(8,15)))

        # except socket.timeout as e:
        #     print('3:',e)
        #     time.sleep(random.choice(range(8,15)))
        #
        # except socket.error as e:
        #     print('4:',e)
        #     time.sleep(random.choice(range(20,60)))
        #
        # except http.client.BadStatusLine as e:
        #     print('5:',e)
        #     time.sleep(random.choice(range(30,80)))
        #
        # except http.client.IncompleteRead as e:
        #     print('6:',e)
        #     time.sleep(random.choice(range(5,15)))
    return html
    #return req

def get_data(html):
    final =[]
    bs = BeautifulSoup(html,'html.parser') #创建 beautifulsoup 对象
    body = bs.body  #获取 body 部分
    data = body.find('div',{'id':'7d'}) # 找到 id 为7d的div
    ul =data.find('ul')  # 获取ul 部分
    li = ul.find_all('li') #获取所有的li的部分

    for day in li :
        temp =[]
        date = day.find('h1').string
        temp.append(date)
        inf = day.find_all('p')
        temp.append(inf[0].string)
        if inf[1].find('span') is None :
            temperature_highest = None
        else:
            temperature_highest = inf[1].find('span').string
            temperature_highest = temperature_highest.replace('℃', '')

        temperature_lowest = inf[1].find('i').string
        temperature_lowest = temperature_lowest.replace('℃', '')

        temp.append(temperature_highest)
        temp.append(temperature_lowest)
        final.append(temp)

    return  final


def write_data(data, name):
    file_name = name
    with open(file_name, 'a', errors='ignore', newline='') as f:
        f_csv = csv.writer(f)
        f_csv.writerows(data)




if __name__ == '__main__':
    url ='http://www.weather.com.cn/weather/101010100.shtml'
    html = get_content(url) #通过 url进行 抓取html
    result = get_data(html) # 把 html进行过滤
    result.insert(0,['日期','天气','最高温度','最低温度'])
    write_data(result, 'weather.csv')








