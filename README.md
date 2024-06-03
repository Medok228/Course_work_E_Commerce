# Курсовая работа "E-Commerce"
## Описание

Компания Y предоставляет комплексное веб-приложение в сфере электронной коммерции `Necommerce`. Компания недавно столкнулась с массовой утечкой данных о своих пользователях и их покупках. «Брешь», через которую произошла утечка, вроде как устранили.

Саму разработку приложения компания Y заказывает у подрядчика — фирмы-разработчика.

Своих специалистов по информационной безопасности у компании Y нет, поэтому вас пригласили провести комплексный анализ процесса разработки и протестировать на наличие других веб-уязвимостей. Детали по найденным и исправленным уязвимостям не дали, так как вы профессионал и должны проверить всё с начала.

Разработчики со стороны подрядчика также максимально пошли вам на встречу и предоставили исчерпывающую информацию о приложении — две ссылки на репозитории с кодом:
* [Frontend](https://github.com/netology-code/necommerce-frontend).
* [Backend](https://github.com/netology-code/necommerce-backend).

**По словам разработчиков:**
1. Вся разработка ведётся в приватных репозиториях в GitHub. *Это легенда, для удобства мы вам дали открытые репозитории*.

2. Весь код, а также зависимости и контейнеры, регулярно  проходят проверку открытыми инструментами:
    * [Dependabot](https://dependabot.com).
    * [CodeQL](https://docs.github.com/ru/code-security/code-scanning/introduction-to-code-scanning/about-code-scanning-with-codeql)
    * [GitHub Secret Scaning](https://docs.github.com/ru/code-security/secret-scanning/about-secret-scanning)
    * [SonarQube\*](https://docs.sonarsource.com/sonarqube/latest/)
    * [Semgrep\*](https://semgrep.dev/)
    * [Trivy\*](https://trivy.dev/)
    
    \* - опциональные инструменты из [библиотеки GitHub Actions (workflows)](https://docs.github.com/en/actions/security-guides/security-hardening-for-github-actions)
3. Сам код покрыт автоматическими тестами, включая проверку механизмов безопасности (отработка неверных логинов и паролей), которые регулярно прогоняются при каждом push в репозиторий.

4. Разработчики прекрасно знакомы с `OWASP Top 10`, а некоторые даже с `ASVS` и `WSTG`.

5. Они придерживаются строгих правил разработки и не разрешают отправлять `push` в `master` без соответствующего `Code Review` как минимум двух человек и прохождения автоматизированных проверок.

6. После прохождения всех проверок автоматически собираются образы `Docker` и публикуются в `GitHub Container Registry` для дальнейшего разворачивания в Production.

Отчёты этих инструментов скрыты из публичного доступа. При желании вы можете `Fork`'нуть репозиторий и самостоятельно настроить репозиторий, либо обратиться к руководителю курса, чтобы он вас добавил временно в репозиторий для просмотра настроек инструментов.

## Инструкция по запуску

Используйте файл `docker compose`:
```
version: '3.7'
services:
  backend:
    image: ghcr.io/netology-code/necommerce-backend
    ports:
      - 9999:9999
  frontend:
    image: ghcr.io/netology-code/necommerce-frontend
    environment:
      - API=http://backend:9999
      - MEDIA=http://backend:9999
    ports:
      - 8888:80
    depends_on:
      - backend
```
## Задача

**Ключевая задача** — провести комплексное исследование функционирующего приложения и исходных кодов.
<h2>Отчет по работе</h2>
<h3>Статический анализ</h3>
<h4>Frontend</h4>
<ol>
<li>При статистическом анализе Frontend части был использован <b>ESLint v8.55.0</b>, которая при сканировании выявила 7 ошибок 
<img src="https://github.com/Medok228/Course_work_E_Commerce/blob/main/images/image8.png">
  <h5>Рекомендация по оптимизации:</h5>
	Убрать неиспользуемые параметры и объявить параметры указанные в ошибках

</li>
<li>
  При статистическом анализе Frontend части был использован <b>sonarqube Version 10.3</b>, которая при сканировании не выявила ошибок<br>
  <img src="https://github.com/Medok228/Course_work_E_Commerce/blob/main/images/image7.png">
</li>
  
</ol>
<h4>Backend</h4>
<ol>
<li>
  При статистическом анализе backend части был использован <b>sonarqube Version 10.3</b>, которая при сканировании не выявила никаких ошибок кода.<br>
  <img src="https://github.com/Medok228/Course_work_E_Commerce/blob/main/images/image2.png">
</li>
<li>
  При статистическом анализе backend части был использован <b>detekt version 1.23.4</b>, которая при сканировании выявила 63 ошибки.
</li>
  
</ol>