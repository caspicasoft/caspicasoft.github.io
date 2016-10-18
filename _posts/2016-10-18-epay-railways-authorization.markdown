---
layout: post
title: "Уязвимость на сайте epay.railways.kz (Authorization)"
date: '2016-10-18T19:39:32+06:00'
categories: xss security authorization
about: 'about.html'
---
Проблема с авторизацией на сайте позволяет скачать все купленные билеты пользователей. Вендор знает про эту уязвимость.
<!--more-->

>**Недостаточная авторизация** (Insufficient Authorization) - отсутствие проверки прав (привилегий) при попытке выполнения определённых действий.

После регистрации на сайте (www.epay.railways.kz) и покупки билета, у пользователя есть возможность распечатать свой билет.
В купленных билетах указаны такие персональные данные, как Ф.И.О пользователя, дата рождения и номер удостоверения или паспорта.

Ссылка для распечатки HTML (или PDF) билета выглядит так: hxxps://epay.railways.kz/ktz4/proc?pa=orders&sa=PRINT_EVENT&ORDER_ID=**XXXXXXX**&PRINT_TYPE=PRINT_TYPE_HTML&TICKET_ID=

![image][img0]

Вы не можете увидеть чужие билеты через личный кабинет. Однако при запросе на распечатку, сервер не проверяет принадлежит ли нам 
запрашиваемый параметр **ORDER_ID**. Это значит, что мы можем получить доступ к чужим билетам меняя значение этого параметра. 
 
Вот результат такого запроса.
![image][img1]

Ещё один пример:
![image][img2]

Если вспомнить, что продажу новых билетов через epay.railways.kz запустили в 2014 году, то можно понять, что это абсолютно все билеты.
![image][img3]

Вот и первые пользователи (или разработчики?).
![image][img4]
![image][img5]
![image][img6]

***
Конечно, не обошлось без XSS. Не обработаны почти все параметры форм и даже [путь файлов]. О свойствах XSS можете прочитать в [другом посте].
![image][img7]
![image][img8]

Вот такой вот грустный #opendata. 

[другом посте]: https://caspicasoft.com/blog/2015/12/03/kaspi-bank-xss-beef
[путь файлов]: https://epay.railways.kz/SVPayments/%3Cdiv%20align='center'%3E%3Cimg%20src='https://bit.ly/1VTPJj3'/%3E%3Ch1%3Eso%20secure%3C/h1%3E%3Cbr%3E%3Clabel%3EUsername%3C/label%3E%3Cinput/%3E%3Cbr%3E%3Clabel%3EPassword%3C/label%3E%3Cinput%20/%3E%3Cbr%3E%3Cbutton%3Esubmit%3C/button%3E%3Cp%3E%3C/p%3E%3C/div%3E%3Cp%20style=%22.jsf
[img0]: /assets/images/{{page.slug}}/img0.png
[img1]: /assets/images/{{page.slug}}/img1.png
[img2]: /assets/images/{{page.slug}}/img2.png
[img3]: /assets/images/{{page.slug}}/img3.png
[img4]: /assets/images/{{page.slug}}/img4.png
[img5]: /assets/images/{{page.slug}}/img5.png
[img6]: /assets/images/{{page.slug}}/img6.png
[img7]: /assets/images/{{page.slug}}/img7.png
[img8]: /assets/images/{{page.slug}}/img8.png