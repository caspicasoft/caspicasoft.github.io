---
layout: post
title: "Уязвимость в Казком (HTML Injection)"
date: '2015-10-26T13:00:10+06:00'
categories: bank security
about: 'about.html'
---
В последнее время, у Казкома [были проблемы] с мошенниками которые отправляли фишинг-письма.

Главная проблема фишеров - это чтобы пользователи поверили, что они находятся на настоящем сайте банка 
(правильный адрес и, надеюсь, зеленый замочек). А зачем мошенникам покупать свой домен, если есть уязвимый сайт банка?
<!--more-->
Вот что сайт ePay (центр авторизации и обработки онлайн-платежей по пластиковым картам) говорит о безопасности:
![image][img0]
Все довольно хорошо и имеется картинка щита. Так есть ли на таком сайте простая уязвимость?

>**HTML Injection**(внедрение кода) - это когда пользователь может вставлять HTML тэги в веб-приложение и изменить 
вид страницы или добавить новые элементы.

Проблема была на hxxps://**epay.kkb.kz/jsp/process/Error_session.jsp?**

Параметры запроса исполняются без надлежащей обработки и отображаются на странице. Пример:
![image][img1]
На самом деле можно убрать лишние параметры и вставить красивый текст с призывами на действие (*отправьте смс или позвоните по номеру*) 
или попросить пользователя пройти по ссылке для завершения платежа на ваш фальшивый сайт (всё-таки мошенникам нужно покупать домен).


После того как вы изменили страницу ePay “под себя”, смело отправляйте письма о том, что всем срочно нужно “подтвердить оплату” 
или “принять перевод средств” в системе Казкома.

Вообще, уровень безопасности и “user experience” многих казахстанских банков оставляет желать лучшего, но об этом напишу уже в новом посте.



[были проблемы]: http://wiki.homebank.kz/page/ProjectNews_blogentry_200814_1
[img0]: /assets/images/{{page.slug}}/img0.png
[img1]: /assets/images/{{page.slug}}/img1.png