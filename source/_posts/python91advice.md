---
title: python91advice
date: 2016-08-16 20:55:38
tags: 工具
---

### CH1 pythonic

learn from pythonic framework, like Flask, generators

```
def quicksort(array):
	less=[]; greater=[]
	if len(array) <=1:
		return array
	pivot = array.pop()
	for x in array:
		if x<= pivot: less.append(x)
		else: greater.append(x)
	return quicksort(less)+[pivot]+quicksort(greater)

a,b = b,a 

with open(path,'r') as f:
	do_something(f)

print 'hello %(name)s!' % {'name': 'Tom'}

##

from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return "Hi"

if __name__ == "__main__":
    app.run()

```

包和模块的潮流是：
 + 包和模块的命名采用小写，单数形式，而且短小。
 + 包通常仅作为命名空间，如只包含空的__init__.py文件

<!-- more -->

```
person_info={'name':'john','email':'xxx@gmail.com'}
#check lang reference, and library reference.
#say requests library

import requests
r = requests.get('https://api.github.com',auth=('user','pass'))
print r.status_code
print r.headers('content-type')

#for code review just install pep8
pip install -U pep8
#then
pep8 --first optparse.py
#google use Pychecker do style guide check

from dateutil import tz
from datetime import datetime

# UTC Zone
from_zone = tz.gettz('UTC')
# China Zone
to_zone = tz.gettz('Asia/Shanghai')

utc = datetime.utcnow()

# Tell the datetime object that it's in UTC time zone
utc = utc.replace(tzinfo=from_zone)

# Convert time zone
local = utc.astimezone(to_zone)
print datetime.strftime(local, "%Y-%m-%d %H:%M:%S")
```

Python Cookbook

