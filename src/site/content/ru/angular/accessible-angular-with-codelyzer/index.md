---
layout: Почта
title: Аудит доступности вашего приложения Angular с codelyzer
subhead: Хотите сделать свой сайт Angular доступным для всех? Это правильное место!
hero: accessible.jpg
alt: Гондолы.
date: 2019-07-03
description: Узнайте, как сделать ваше приложение Angular доступным с помощью codelyzer.
authors:
- mohamedzamakhan
- mgechev
tags:
- угловая
- доступность
---

Обеспечение доступности вашего приложения означает, что все пользователи, включая пользователей с особыми потребностями, могут использовать его и понимать его содержимое. Согласно [Докладу о состоянии здравоохранения в мире](https://www.who.int/disabilities/world_report/2011/report.pdf) , более миллиарда человек - около 15% населения мира - имеют какую-либо форму инвалидности. Таким образом, [доступность](/accessible) является приоритетом для любого проекта развития.

В этом посте вы узнаете, как добавить [проверки](https://github.com/mgechev/codelyzer) доступности [codelyzer в](https://github.com/mgechev/codelyzer) процесс сборки приложения Angular. Этот подход позволяет обнаруживать ошибки доступности непосредственно в текстовом редакторе при кодировании.

## Использование Codelyzer для обнаружения недоступных элементов

[codelyzer](https://github.com/mgechev/codelyzer) - это инструмент, который находится поверх [TSLint](https://palantir.github.io/tslint/) и проверяет, соответствуют ли проекты Angular TypeScript правилам линтинга. Проекты, созданные с помощью [интерфейса командной строки Angular (CLI),](https://cli.angular.io/) по умолчанию включают codelyzer.

Codelyzer имеет более 50 правил для проверки того, соответствует ли приложение Angular передовым методам. Из них около 10 правил применения критериев доступности. Чтобы узнать о различных проверках доступности, предоставляемых codelyzer, и их обоснованиях, см. [Новые правила доступности в](https://medium.com/ngconf/new-accessibility-rules-in-codelyzer-v5-0-0-85eec1d3e9bb) статье [Codelyzer](https://medium.com/ngconf/new-accessibility-rules-in-codelyzer-v5-0-0-85eec1d3e9bb) .

В настоящее время все правила доступности являются экспериментальными и по умолчанию отключены. Вы можете включить их, добавив их в файл конфигурации `tslint.json` ( `tslint.json` ):

```json/6-15
{
  "rulesDirectory": [
    "codelyzer"
  ],
  "rules": {
    ...,
    "template-accessibility-alt-text": true,
    "template-accessibility-elements-content": true,
    "template-accessibility-label-for": true,
    "template-accessibility-tabindex-no-positive": true,
    "template-accessibility-table-scope": true,
    "template-accessibility-valid-aria": true,
    "template-click-events-have-key-events": true,
    "template-mouse-events-have-key-events": true,
    "template-no-autofocus": true,
    "template-no-distracting-elements": true
  }
}
```

TSLint работает со всеми популярными текстовыми редакторами и IDE. Чтобы использовать его с VSCode, установите [плагин TSLint](https://marketplace.visualstudio.com/items?itemName=eg2.tslint) . В WebStorm TSLint включен по умолчанию. Для других редакторов, проверьте TSLint [README](https://github.com/palantir/tslint#tslint) .

С настроенными проверками доступности codelyzer вы получаете всплывающее окно, показывающее ошибки доступности в файлах TypeScript или встроенных шаблонах при кодировании:

<figure class="w-figure">
  <img src="../../../en/angular/accessible-angular-with-codelyzer/editor-template.png" alt="A screenshot of a codelyzer popup in a text editor.">
  <figcaption class="w-figcaption">Всплывающее окно codelyzer, показывающее ошибку маркировки элемента формы.</figcaption>
</figure>

Чтобы выполнить связывание всего проекта (включая внешние шаблоны), используйте команду `ng lint` :

![Linting with Angular CLI](../../../en/angular/accessible-angular-with-codelyzer/ng-lint.png "Linting with Angular CLI")

## Дополняющий коделезер

[Lighthouse](https://developers.google.com/web/tools/lighthouse/) - это еще один инструмент, который вы можете использовать для обеспечения соблюдения правил доступности в вашем приложении Angular. Основное различие между Codelyzer и Lighthouse заключается в том, что их проверки выполняются. Codelyzer статически анализирует приложение во время разработки, не запуская его. Это означает, что во время разработки вы можете получить прямую обратную связь в вашем текстовом редакторе или в терминале. Напротив, Lighthouse фактически запускает ваше приложение и выполняет кучу проверок с использованием динамического анализа.

Оба инструмента могут быть полезными частями вашего процесса разработки. Маяк имеет лучшее покрытие, учитывая проверки, которые он выполняет, в то время как codelyzer позволяет вам выполнять итерации быстрее, получая постоянную обратную связь в вашем текстовом редакторе.

## Обеспечение проверки доступности в вашей непрерывной интеграции

Внедрение статических проверок доступности в вашу непрерывную интеграцию (CI) может быть большим улучшением для вашего процесса разработки. С помощью Codelyzer вы можете легко применить определенные правила доступности или другие методы, запустив `ng lint` для каждой модификации кода (например, для каждого нового запроса на извлечение).

Таким образом, даже до того, как вы приступите к проверке кода, ваш CI может сообщить вам, есть ли какие-либо нарушения доступности.

## Вывод

Чтобы улучшить доступность вашего приложения Angular:

1. Включить экспериментальные правила доступности в codelyzer.
2. Выполняйте доступность по всему вашему проекту, используя Angular CLI.
3. Исправьте все проблемы с доступностью, о которых сообщает codelyzer.
4. Рассмотрите возможность использования Lighthouse для проверки доступности во время выполнения.
