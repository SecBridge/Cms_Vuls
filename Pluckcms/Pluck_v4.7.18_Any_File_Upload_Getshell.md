# Pluck v4.7.18 Any File Upload Getshell



### Introduction to Pluck 

Pluck is a small and simple content management system (CMS), written in PHP. With Pluck, you can easily manage your own website. Pluck focuses on simplicity and ease of use. This makes Pluck an excellent choice for every small website. Licensed under the General Public License (GPL), Pluck is completely open source. This allows you to do with the software whatever you want, as long as the software stays open source.

```
v4.7.18 Download Link: https://github.com/pluck-cms/pluck/archive/refs/tags/4.7.18.zip
```

### Vulnerability description

Pluck CMS v4.7.18 has a serious arbitrary file upload vulnerability, which can be found in the /data/inc/modules_install.php file. The triggering condition for the vulnerability is that when the front-end user performs module installation, the uploaded ZIP file contains malicious files. At this time, the backend decompresses the file without effective verification, allowing front-end users to upload any file by inserting any malicious file into the ZIP file.
It should be emphasized that once the uploaded malicious module ZIP installation package is decompressed and referenced, the system will automatically load the contents. Specifically, if the uploaded malicious file is a web shell and is referenced after installation, the attacker can execute arbitrary system commands via shell parameters at any location in Pluck CMS.

#### Vulnerability analysis

This vulnerability exists in /data/inc/modules_install.php：

![image-20231206112846888](https://github.com/SecBridge/Bridge/blob/main/Pluckcms/image/image-20231206112846888.png)

Reference the module in data/inc/modules_manage.php:

![image-20231206113402099](https://github.com/SecBridge/Bridge/blob/main/Pluckcms/image/image-20231206113402099.png)

### 漏洞演示

1、Log in to the backend -> click on ”options“ ->Select "manage modules" -> click "install modules"：

![image-20231206122628760](https://github.com/SecBridge/Bridge/blob/main/Pluckcms/image/image-20231206122628760.png)

2、Upload eval.zip with malicious shell file:

![image-20231206122757780](https://github.com/SecBridge/Bridge/blob/main/Pluckcms/image/image-20231206122757780.png)

3、Check the backend and see that the relevant shell file was successfully uploaded:

![image-20231206123139923](https://github.com/SecBridge/Bridge/blob/main/Pluckcms/image/image-20231206123139923.png)

4、Enter specific parameters anywhere in the system and execute any command:

![image-20231206123001850](https://github.com/SecBridge/Bridge/blob/main/Pluckcms/image/image-20231206123001850.png)

5、You can also access the relevant shell to execute any command:

![image-20231206123312823](https://github.com/SecBridge/Bridge/blob/main/Pluckcms/image/image-20231206123312823.png)
