import datetime
import time
from selenium import webdriver
browser = webdriver.Chrome()
def login(uname,pwd):
    browser.get('https://www.taobao.com')                               # 打开淘宝网首页，并点击“亲，请登录”按钮
    if browser.find_element_by_link_text("亲，请登录"):
        browser.find_element_by_link_text("亲，请登录").click()
        input(uname,pwd)
    if browser.find_element_by_xpath('//*[@id=\'J_Order_s_725677994_1\']/div[1]/div/div/label'):       # 单独选中茅台（使用Chrome的“检查”功能，找到其勾选框对应的id为J_Order_s_725677994_1）。根据此方法可以选中购物车中的任意商品。
        browser.find_element_by_xpath('//*[@id=\'J_Order_s_725677994_1\']/div[1]/div/div/label').click()
    # if browser.find_element_by_id('J_SelectAll1'):                      # 全选购物车
    #     browser.find_element_by_id('J_SelectAll1').click()

def input(uname,pwd):
    if browser.find_element_by_name('fm-login-id'):                     # 输入淘宝账户名
        for i in uname:
            browser.find_element_by_name('fm-login-id').send_keys(i)
            time.sleep(0.5)
        time.sleep(3)
    if browser.find_element_by_name('fm-login-password'):               # 输入淘宝账户密码
        for j in pwd:
            browser.find_element_by_name('fm-login-password').send_keys(j)
            time.sleep(0.5)
        time.sleep(3)

    if browser.find_element_by_class_name('fm-btn'):                    # 点击“登录”按钮，跳转到淘宝网首页
        browser.find_element_by_class_name('fm-btn').click()
        time.sleep(3)

    if browser.find_element_by_id('J_MiniCart'):                       # 点击首页“购物车”按钮
        browser.find_element_by_id('J_MiniCart').click()
        time.sleep(2)

def buy(buytime):
    while True:
        now = datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S')
        if now == buytime:
            try:
                if browser.find_element_by_id('J_Go'):
                    browser.find_element_by_id('J_Go').click()
                    browser.find_element_by_link_text('提交订单').click()
            except:
                time.sleep(0.05)

if __name__ == "__main__":
    while True:
        now =  datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S')
        loginTime = '2020-10-11 19:58:00'
        if now == loginTime:
            login('wsd90218', 'wsd13814863913')
            buy('2020-10-11 19:59:59')
    print(now)

