import re
from urllib.request import urlopen
import pandas as pd
import json
import requests
import random
import time
from bs4 import BeautifulSoup
import smtplib
from email.mime.text import MIMEText
from email.header import Header

# 该脚本可用于监控雪球组合的变动情况，现在只监控yyb凌波的“可转债轮动策略”这一个组合。一旦组合调仓，会自动触发一条短信提示。
# 该脚本只看源代码，不涉及json请求
# 获得最后一次更新id的函数
# 如遇无法输出结果，可尝试更新headers中的Cookies值，取值来源：右键-检查-Network-XHR-web?category=web_behavior

def getUpdateTime():
    # url = 'https://xueqiu.com/cubes/rank/summary.json?symbol=ZH1332574&ua=web'
    headers = {
        'Referer': 'https://xueqiu.com/',
        'User-Agent': 'Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.77 Safari/537.36',
        'Cookie': 'device_id=910cd33140aeebd0b66b98fab2dea75c; s=bz11iwagua; bid=26ceee9e0169639cf3f1f07c20590c40_ksfr664c; __utmz=1.1629185829.1.1.utmcsr=(direct)|utmccn=(direct)|utmcmd=(none); xq_is_login=1; u=1091574384; __utmc=1; snbim_minify=true; Hm_lvt_1db88642e346389874251b5a1eded6e3=1652251323; xq_a_token=1f725df090a7f30078ca4ca15aa908d966c25dd8; xqat=1f725df090a7f30078ca4ca15aa908d966c25dd8; xq_id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ1aWQiOjEwOTE1NzQzODQsImlzcyI6InVjIiwiZXhwIjoxNjU0ODUyMzk5LCJjdG0iOjE2NTIyNjAzOTk5MTMsImNpZCI6ImQ5ZDBuNEFadXAifQ.kZSQw8GJHqgu90PB7bn-HRtjE36s7BSdyXsumBAVUaRW0podxYgY0xUxSAGCyGD8BR3J9hhMlQ3-DHTzC5-rM8REhazuaWsqi9VeSUvY8F5BPAtTJRQ3oEVfboTGsYsL3d2xdns_J5NL5lj6Sh1FwUB_LBqvpNZy-X5AfFxzLifJhDWFQAXbDpFR0i6Oh6E0NggVLdKXzhhokfeAiZoj-TKIJpD9mxwc55MmoqZwN278vuyzrtay1cAc7H_xgVfq4w0xRdeOKZA3ygVDerBU8idegy9F0f0CsSSowbC1quAq_cwl4o3BmIxwsjmU2X9GSI1UlpCWJkqBmRcJ4vGrMw; xq_r_token=b518b9e3ffcb2b6c4f394fa6ac18682fd2262b89; acw_tc=276077ae16532952279257263e1b3e06dad850b6102370c5a1e2abb14c8fe1; __utma=1.2018680337.1629185829.1653291758.1653295128.192; __utmt=1; __utmb=1.1.10.1653295128; Hm_lpvt_1db88642e346389874251b5a1eded6e3=1653295128'
    }
    url = 'https://xueqiu.com/P/ZH1332574'
    session = requests.Session()
    response = session.get('https://xueqiu.com/P/ZH1332574', headers=headers)

    html = response.text
    try:
        zhizhen = re.search('rebalancing_id', html).span()[0]
        rebalancing_time = []
        for i in range(9):
            rebalancing_time.append(html[zhizhen + 16 + i])
        return rebalancing_time
    except(AttributeError,TimeoutError,ConnectionError,ConnectionRefusedError,NewConnectionError,MaxRetryError):
        return ['E','R','R','O','R']



# 如果有更新，通过126邮箱向139邮箱发一封邮件，139邮箱收信有短信提示
def SendMail():
    mail_host = 'smtp.126.com'
    mail_user = 'wangshundao'
    mail_pass = 'wsd13814863913'

    sender = 'wangshundao@126.com'
    receivers = ['18362676001@139.com']

    message = MIMEText('打开雪球APP查看', 'plain', 'utf-8')
    message['From'] = sender
    message['To'] = receivers[0]

    subject = 'yyb凌波调仓了'
    message['Subject'] = Header(subject, 'utf-8')

    try:
        smtpObj = smtplib.SMTP()
        smtpObj.connect(mail_host, 25)
        smtpObj.login(mail_user, mail_pass)
        smtpObj.sendmail(sender, receivers, message.as_string())
        print('Successfully Sended')
    except smtplib.SMTPException:
        print('Error')

if __name__ == '__main__':
    Original_Rebalancing_Id = ['1', '1', '7', '5', '7', '5', '9', '5', '3'] # 最后一次调仓对应的ID（每次调仓后更新）
    # Original_Rebalancing_Id = getUpdateTime()
    while True:
        time.sleep(5) # 程序暂停5秒
        Rebalancing_Id = getUpdateTime()
        if Rebalancing_Id[0] != 'E':
            if Rebalancing_Id != Original_Rebalancing_Id:
                SendMail()
                print('Just Updated',time.strftime("%Y-%m-%d  %H:%M:%S",time.localtime()))
                Original_Rebalancing_Id = Rebalancing_Id
                continue
            else:
                print('No Update',time.strftime("%Y-%m-%d  %H:%M:%S",time.localtime()))
                continue
        else:
            continue
    # print(getUpdateTime())



