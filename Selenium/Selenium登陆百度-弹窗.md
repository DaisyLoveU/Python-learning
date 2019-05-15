

```python
from selenium import webdriver
```


```python
baidu = webdriver.Chrome()
baidu.get('https://www.baidu.com')

login_button = baidu.find_element_by_link_text('登录')
# tj_login 是不可见的，无法正常访问，所以用link_text找登陆按钮
login_button.click()

baidu.find_element_by_xpath('//div[@class="tang-pass-footerBar"]/p[@title="用户名登录"]').click()
# 点击登录后，会跳出弹窗，选择用户名登录

baidu.find_element_by_xpath('//input[@name="userName"]').send_keys('******@qq.com')
baidu.find_element_by_xpath('//input[@name="password"]').send_keys('******')
baidu.find_element_by_xpath('//input[@value="登录"]').click()
baidu.close()
```
