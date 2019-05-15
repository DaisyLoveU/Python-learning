

```python
from selenium import webdriver
import time
```


```python
driver = webdriver.Chrome()
driver.implicitly_wait(10)
driver.get('http://mail.126.com')
```


```python
# driver.switch_to.frame("x-URS-iframe1557905687308.065") id是变化的，失败
# iframe 没有name 只能用iframe的序号

driver.switch_to.frame(0)  # river.switch_to.frame("x-URS-iframe")这个括号内可以写入frame的序号，如有多个frame,最外层的为0，里面为1，以此类推。
                           # 所以  driver.switch_to.frame(0),这样就可以了。
userName = driver.find_element_by_xpath('//input[@name="email"]')
userName.click()
userName.clear()
userName.send_keys('******')
```


```python
pwd = driver.find_element_by_xpath('//input[@name="password"]')
pwd.click()
pwd.clear()
pwd.send_keys('******')
```


```python
loginButton = driver.find_element_by_xpath('//a[@id="dologin"]')
loginButton.click()
```


```python
write_button = driver.find_element_by_xpath("//span[contains(text(),'写 信')]")
# 注意这个 定位方法！
write_button.click()
```


```python
rcvdPerson = driver.find_element_by_xpath('//input[@class="nui-editableAddr-ipt"]')
rcvdPerson.send_keys('zhangz_da@126.com;')
```


```python
theme = driver.find_element_by_xpath('//input[@class="nui-ipt-input" and @tabindex="1"]')
theme.send_keys('test selenium')
```


```python
driver.switch_to.frame(driver.find_element_by_class_name("APP-editor-iframe"))
# 进入输入正文的frame
content = driver.find_element_by_class_name('nui-scroll')
content.send_keys('hell, i am  zz.\n  I love you. \n\n')
```


```python
driver.switch_to.default_content()
# 输入正文完毕,返回到之前的frame
```


```python
send = driver.find_element_by_xpath("//span[contains(text(),'发送') and @class='nui-btn-text']")
# 该xpath语句可以匹配到两个发送按钮，不知道为什么就执行成功了
send.click()
```


```python
try:
    assert '发送成功' in driver.page_source
except Exception:
    print('sucess')
else:
    print('failed')
```

    failed
    


```python
driver.close()
```
