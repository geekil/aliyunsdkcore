# 阿里云python sdk文档地址
[阿里云python sdk最新文档地址](https://help.aliyun.com/zh/sdk/developer-reference/v2-python-integrated-sdk?spm=a2c4g.11186623.0.0.3c15795et8ipN5)

阿里将sdk接入分为特化调用和泛化调用，特化调用安装安装地址为：
```pip install alibabacloud_dysmsapi20170525==2.0.24```,

泛化地址为：
```pip install alibabacloud-tea-openapi```
 
---

目前阿里云好像官方出了aliyun-python-sdk-core-v3的Python3的支持包，地址是[https://pypi.org/project/aliyun-python-sdk-core-v3/](https://pypi.org/project/aliyun-python-sdk-core-v3/),经过测试可以正常发送短信，所以推荐大家安装这个新的包。[官方文档](https://help.aliyun.com/document_detail/55491.html?spm=a2c4g.11186623.6.570.dfgplP)

# 安装方式
```
pip install aliyun-python-sdk-core-v3

```

# 官方文档中的发送例子 demo_sms_send.py
```python
# -*- coding: utf-8 -*-
import sys
from aliyunsdkdysmsapi.request.v20170525 import SendSmsRequest
from aliyunsdkdysmsapi.request.v20170525 import QuerySendDetailsRequest
from aliyunsdkcore.client import AcsClient
import uuid
from aliyunsdkcore.profile import region_provider
from aliyunsdkcore.http import method_type as MT
from aliyunsdkcore.http import format_type as FT

"""
短信业务调用接口示例，版本号：v20170525

Created on 2017-06-12

"""
try:
    reload(sys)
    sys.setdefaultencoding('utf8')
except NameError:
    pass
except Exception as err:
    raise err

# 注意：不要更改
REGION = "cn-hangzhou"
PRODUCT_NAME = "Dysmsapi"
DOMAIN = "dysmsapi.aliyuncs.com"

# ACCESS_KEY_ID/ACCESS_KEY_SECRET 根据实际申请的账号信息进行替换
ACCESS_KEY_ID = "yourAccessKeyId"
ACCESS_KEY_SECRET = "yourAccessKeySecret"

acs_client = AcsClient(ACCESS_KEY_ID, ACCESS_KEY_SECRET, REGION)
region_provider.add_endpoint(PRODUCT_NAME, REGION, DOMAIN)

def send_sms(business_id, phone_numbers, sign_name, template_code, template_param=None):
    smsRequest = SendSmsRequest.SendSmsRequest()
    # 申请的短信模板编码,必填
    smsRequest.set_TemplateCode(template_code)

    # 短信模板变量参数
    if template_param is not None:
        smsRequest.set_TemplateParam(template_param)

    # 设置业务请求流水号，必填。
    smsRequest.set_OutId(business_id)

    # 短信签名
    smsRequest.set_SignName(sign_name)
	
    # 数据提交方式
	# smsRequest.set_method(MT.POST)
	
	# 数据提交格式
    # smsRequest.set_accept_format(FT.JSON)
	
    # 短信发送的号码列表，必填。
    smsRequest.set_PhoneNumbers(phone_numbers)

    # 调用短信发送接口，返回json
    smsResponse = acs_client.do_action_with_exception(smsRequest)

    # TODO 业务处理

    return smsResponse


if __name__ == '__main__':
    __business_id = uuid.uuid1()
    #print(__business_id)
    params = "{\"code\":\"12345\",\"product\":\"云通信\"}"
	#params = u'{"name":"wqb","code":"12345678","address":"bz","phone":"13000000000"}'
    print(send_sms(__business_id, "13000000000", "云通信测试", "SMS_5250008", params))
```

文档中将ACCESS_KEY_ID和ACCESS_KEY_SECRET放在了const.py的文件中，此处直接放在了测试代码里。



----

# **Deprecated**

# aliyunsdkcore
阿里短信Python3 API，基于[阿里官网](https://help.aliyun.com/document_detail/55491.html?spm=5176.sms-account.109.3.66e36217NuxCf) Python2 API的翻版，
主要修改为Python2转Python3时不兼容包的替换。

# Requirements
- Python (3.3, 3.4, 3.5, 3.6)

# Installation
```python
$ pip install aliyunsdkcore
```

# Sample
```python
import sys
from aliyunsdkcore import SendSmsRequest, QuerySendDetailsRequest
from aliyunsdkcore.client import AcsClient
import uuid


REGION = "cn-hangzhou"
# ACCESS_KEY_ID/ACCESS_KEY_SECRET 根据实际申请的账号信息进行替换
ACCESS_KEY_ID = "YOUR ACCESS_KEY_ID"
ACCESS_KEY_SECRET = "YOUR ACCESS_KEY_SECRET"

acs_client = AcsClient(ACCESS_KEY_ID, ACCESS_KEY_SECRET, REGION)


def send_sms(business_id, phone_numbers, sign_name, template_code, template_param=None):
    smsRequest = SendSmsRequest.SendSmsRequest()
    # 申请的短信模板编码,必填
    smsRequest.set_TemplateCode(template_code)

    # 短信模板变量参数
    if template_param is not None:
        smsRequest.set_TemplateParam(template_param)

    # 设置业务请求流水号，必填。
    smsRequest.set_OutId(business_id)

    # 短信签名
    smsRequest.set_SignName(sign_name);

    # 短信发送的号码列表，必填。
    smsRequest.set_PhoneNumbers(phone_numbers)

    # 调用短信发送接口，返回json
    smsResponse = acs_client.do_action_with_exception(smsRequest)

    # TODO 业务处理

    return smsResponse


def query_send_detail(biz_id, phone_number, page_size, current_page, send_date):
    queryRequest = QuerySendDetailsRequest.QuerySendDetailsRequest()
    # 查询的手机号码
    queryRequest.set_PhoneNumber(phone_number)
    # 可选 - 流水号
    queryRequest.set_BizId(biz_id)
    # 必填 - 发送日期 支持30天内记录查询，格式yyyyMMdd
    queryRequest.set_SendDate(send_date)
    # 必填-当前页码从1开始计数
    queryRequest.set_CurrentPage(current_page)
    # 必填-页大小
    queryRequest.set_PageSize(page_size)

    # 调用短信记录查询接口，返回json
    queryResponse = acs_client.do_action_with_exception(queryRequest)

    # TODO 业务处理

    return queryResponse


__name__ = 'send'
if __name__ == 'send':
    __business_id = uuid.uuid1()
    print(__business_id)
    params = {"code": "456789"}
    print(send_sms(__business_id, "13000000000", "测试", "SMS_1234567", params))

```

