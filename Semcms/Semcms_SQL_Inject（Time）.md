# Semcms v4.8 SEMCMS_Function.php SQL Injection



### Introduction to Semcms

SEMCMS is a foreign trade website content management system (CMS) that supports multiple languages.

```
v4.8 Download Link: http://www.sem-cms.com/TradeCmsdown/php/semcms_php_4.8.zip
```

### Vulnerability description

There is a SQL injection vulnerability in SEMCMS v4.8. The vulnerability stems from the lack of verification of the external input SQL statement in the parameter AID of SEMCMS_Function.php. An attacker can use this vulnerability to execute illegal SQL commands to obtain sensitive data in the database.

### Vulnerability analysis

The vulnerability exists on line 316 of SEMCMS_Function.php:

```
$db_conn->query("update sc_products set products_zt='$indext' where ID in ($area_arr)");
```

![image-20231205155250284](https://github.com/SecBridge/Bridge/blob/main/Semcms/Image/image-20231205155250284.png)

$area_arr gets value at line 17 of SEMCMS_Function.php:

```
if (isset($_POST["AID"])){$area_arr = $_POST["AID"];$area_arr = implode(",",$area_arr);}else{$area_arr="";}
```

![image-20231205160411690](https://github.com/SecBridge/Bridge/blob/main/Semcms/Image/image-20231205160411690.png)

It can be found that the AID value passed in by the front end has not been verified by the verify_str() function. The verify_str() function is mainly used to prevent SQL injection. Its code is as follows:

![image-20231205161241614](https://github.com/SecBridge/Bridge/blob/main/Semcms/Image/image-20231205161241614.png)

### Vulnerability demonstration

1、1. Log in to the backend -> click on ”商品管理“ -> select products for sale -> click on ”批量上架“：

![image-20231205162804277](https://github.com/SecBridge/Bridge/blob/main/Semcms/Image/image-20231205162804277.png)

2、Burpsuite captures packets and constructs SQL injection(TIme) payload：

```
payload: 8)+and+sleep(5)%23
```

![image-20231205163338912](https://github.com/SecBridge/Bridge/blob/main/Semcms/Image/image-20231205163338912.png)

3、Use the SQLmap tool for injection:

![image-20231205163652968](https://github.com/SecBridge/Bridge/blob/main/Semcms/Image/image-20231205163652968.png)

Successfully obtained the database name:

![image-20231205163803939](https://github.com/SecBridge/Bridge/blob/main/Semcms/Image/image-20231205163803939.png)
