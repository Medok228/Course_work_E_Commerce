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
	<ul>
	<li>
	Убрать неиспользуемые параметры и объявить параметры указанные в ошибках</li></ul>

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
  При статистическом анализе backend части был использован <b>detekt version 1.23.4</b>, которая при сканировании выявила 63 ошибки.<br>
  <a href="https://github.com/Medok228/Course_work_E_Commerce/blob/main/detekt%20report.html">Отчет</a>
 <h5>Рекомендация по оптимизации:</h5>
	<ul>
	<li>В отчете detekt есть ссылки на описание ошибок и их устранение</li>
	</ul>

	
</li>
</ol>
<h3>OSA: анализ используемых библиотек</h3>
<ol>
	<li>
		При анализе используемых библиотек был использован <b>dependency-check version: 9.0.3</b>,
при его работе было выявлено 57 уязвимый зависимостей и 153 уязвимостей.<br>
		<a href="https://github.com/Medok228/Course_work_E_Commerce/blob/main/Dependency-Check%20Report.html">Отчет</a>
		<h4>Рекомендация по оптимизации:</h4>
	Практически все уязвимости в OSA связаны с неактуальностью версий библиотек, так что рекомендация - обновить библиотеки
	</li>
</ol>
<h3>Динамический анализ</h3>
<ol>
	<li>
	При динамическом анализе был использован <b>owasp-zap version 2.12.0</b>, при анализе веб приложения на ответы от разных запросов было выявлено уязвимое место, а именно уязвимая форма “Статус заказа” выдающая нелегетимным пользователям конфиденциальные данные пользователей сделавшие заказ.
		<img src="https://github.com/Medok228/Course_work_E_Commerce/blob/main/images/image10.png">
		<img src="https://github.com/Medok228/Course_work_E_Commerce/blob/main/images/image1.png">
		<h4>Рекомендация по оптимизации:</h4>
При  post запросе к серверу на странице “статус заказа” оставить в ответе только информацию о “STATUS”, остальные данные(ownerID, ownerName, ownerPhone…) убрать
	</li>
	<li>
		При автоматическом сканировании веб приложения было выявлено 9 уязвимостей<br>
		<a href="https://github.com/Medok228/Course_work_E_Commerce/blob/main/owasp-zap.zip">Отчет</a>
		<h4>Рекомендация по оптимизации:</h4>
		В отчете owasp-zap указаны ошибки и ссылки на уязвимости и их устранение
	</li>
</ol>
<h3>Анализ образов-контейнеров</h3>
<h4>Frontend</h4>
<ul>
	<li>
		При анализе образ-контейнеров был использован <b>sonarqube Version 10.3</b>, который выявил 3 потенциальные уязвимости в dockerfile:
		<ol>
			<li>
				<img src="https://github.com/Medok228/Course_work_E_Commerce/blob/main/images/image3.png">
				<h5>Рекомендация по оптимизации:</h5>
				<ul>
					<li>
						Ограничьте использование подстановок в COPY определение источников.
					</li>
					<li>
						Не копируйте весь контекстный каталог в файловую систему образа.
					</li>
					<li>
						Предпочитаю предоставлять явный список файлов и каталогов, которые необходимы для правильной работы образа.
					</li>
				</ul>
			</li>
			<li>
				<img src="https://github.com/Medok228/Course_work_E_Commerce/blob/main/images/image6.png">
			</li>
			<li>
				<img src="https://github.com/Medok228/Course_work_E_Commerce/blob/main/images/image11.png">
			</li>
		</ol>
	</li>
	<li>
		При развертывании приложения через dockerfile, Node.js выдает ошибку о том что некоторые из компонентов не экспортированы:
		<img src="https://github.com/Medok228/Course_work_E_Commerce/blob/main/images/image.png">
	</li>
</ul>
<h4>Backend</h4>
<ul>
	<li>
		При анализе образ-контейнеров был использован <b>sonarqube Version 10.3</b>, который выявил 2 потенциальные уязвимости в dockerfile:
		<ol>
			<li>
				<img src="https://github.com/Medok228/Course_work_E_Commerce/blob/main/images/image5.png">
			</li>
			<li>
				<img src="https://github.com/Medok228/Course_work_E_Commerce/blob/main/images/image4.png">
			</li>
		</ol>
	</li>
	<li>
		При анализе с помощью <b>trivy version 0.48.0</b> не было выявлено уязвимостей
		<img src="https://github.com/Medok228/Course_work_E_Commerce/blob/main/images/image9.png">
	</li>
</ul>


  

