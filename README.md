# aliyunsdkcore
阿里短信Python3 API，基于[阿里官网](https://help.aliyun.com/document_detail/55491.html?spm=5176.sms-account.109.3.66e36217NuxCf) Python2 API的翻版，
主要修改为Python2转Python3时不兼容包的替换。

# Requirements
- Python (2.7, 3.3, 3.4, 3.5, 3.6)

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
ACCESS_KEY_ID = b"YOUR ACCESS_KEY_ID"
ACCESS_KEY_SECRET = b"YOUR ACCESS_KEY_SECRET"

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


