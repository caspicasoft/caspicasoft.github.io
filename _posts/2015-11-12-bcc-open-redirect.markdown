---
layout: post
title: "Уязвимость в Банк ЦентрКредит (Open Redirect)"
date: '2015-11-12T08:25:54+06:00'
categories: bank security openredirect
about: 'about.html'
---
*Update*: Теперь исправлено

Сегодня расскажу как мошенники могут связать вместе уязвимость и защиту сайта для контроля жертвы. О проблеме на домене 
**www.bcc.kz** я сообщил пресс-службе 17 дней назад.
<!--more-->
У данного банка тоже были проблемы с мошенниками:
![image][img0]

>**Open Redirect** (открытое перенаправление) – это редирект который позволяет использовать произвольный URL для конечной цели редиректа. 
Эта уязвимость используется мошенниками, чтобы заставить пользователей посетить фишинг сайты (не осознавая этого).

На сайте есть различные рекламные баннеры.
![image][img1]

Полная ссылка баннера: hxxp://www.bcc.kz/bitrix/rk.php?id=17&amp;site_id=s1&amp;event1=banner&amp;event2=click&amp;event3=1+%2F+%5B17%5D+%5BMAINBOT%5D+
main_bottom_ru_2&amp;goto=**http%3A%2F%2Fmastercard.kz%2F**

Если кликнуть на ссылку, то вы перейдете на другой сайт **без каких либо предупреждений** как *”Вы покидаете сайт 
банка. Будьте осторожны.”*.

Банк использует Битрикс (”профессиональная” система управления сайтом). На форуме битрикса пишут, что у 
продукта есть встроенная защита.
![image][img2]

В итоге уязвимы 3 файла (rk.php, click.php, redirect.php). Пример ссылок:

- hxxp://www.bcc.kz/bitrix/click.php?anything=here&amp;goto=http://homebank.kz/
- hxxp://www.bcc.kz/bitrix/rk.php?id=17&amp;site_id=s1&amp;event1=banner&amp;event2=click&amp;goto=http%3A%2F%2Fhomebank.kz%2F
- hxxp://www.bcc.kz/bitrix/redirect.php?event1=click_to_call&amp;event2=&amp;event3=&amp;goto=http://homebank.kz/

***

В то же время, атаки на западные банки используют полный комплекс. После того как мошенники перевели деньги, например, [Dyre Wolf], запускает мощную DDoS 
атаку (отказ в обслуживании) и перекрывает доступ к сайту банка.
![image][img3]

***

Так вот, многие сайты казахстанских банков не очень удобны. Если WAF (Web Application Firewall) банка заметит опасные тэги в URL, то ваш IP-адрес 
блокируется(!) и вы временно не можете зайти на сайт.
![image][img4]

"User-friendly" результат для пользователя:
![image][img5]

Если вы мошенник переживающий кризисные дни и не можете себе позволить DDoS, значит вам повезло.

«Эконом вариант» атаки на пользователей банка:

1. Отправьте письмо с ссылкой на bcc.kz + редирект на фишинг сайт.
2. Заполучите логин/пароль и заставьте браузер жертвы сделать запрос на сайт банка с опасным тэгом в URL (теперь жертва
заблокирована банком).
3. ???
4. **PROFIT!!!**

Как мы видим, open redirect может иметь серьезные последствия. Я также думаю, что неэффективно блокировать IP-адреса. Во-первых, 
мошенникам очень легко сменить IP-адрес. Во-вторых, ваш сайт все равно должен быть неуязвим к injection атакам.


[Dyre Wolf]: https://portal.sec.ibm.com/mss/html/en_US/support_resources/pdf/Dyre_Wolf_MSS_Threat_Report.pdf
[img0]: /assets/images/{{page.slug}}/img0.png
[img1]: /assets/images/{{page.slug}}/img1.png
[img2]: /assets/images/{{page.slug}}/img2.png
[img3]: /assets/images/{{page.slug}}/img3.jpg
[img4]: /assets/images/{{page.slug}}/img4.png
[img5]: /assets/images/{{page.slug}}/img5.png