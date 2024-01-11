# Домашнее задание к занятию 11 «Teamcity»

---
## Подготовка к выполнению
1. В Yandex Cloud создайте новый инстанс (4CPU4RAM) на основе образа `jetbrains/teamcity-server`.
2. Дождитесь запуска teamcity, выполните первоначальную настройку.
3. Создайте ещё один инстанс (2CPU4RAM) на основе образа `jetbrains/teamcity-agent`. Пропишите к нему переменную окружения `SERVER_URL: "http://<teamcity_url>:8111"`.
4. Авторизуйте агент.
5. Сделайте fork [репозитория](https://github.com/aragastmatb/example-teamcity).
6. Создайте VM (2CPU4RAM) и запустите [playbook](./infrastructure).


## Основная часть

1. Создайте новый проект в teamcity на основе fork

2. Сделайте autodetect конфигурации     

![image](pic/1.png)

3. Сохраните необходимые шаги, запустите первую сборку master'a

![image](pic/2.png)

4. Поменяйте условия сборки: если сборка по ветке `master`, то должен происходит `mvn clean deploy`, иначе `mvn clean test`

![image](pic/3.png)

5. Для deploy будет необходимо загрузить [settings.xml](./teamcity/settings.xml) в набор конфигураций maven у teamcity, предварительно записав туда креды для подключения к nexus

![image](pic/4.png)

6. В pom.xml необходимо поменять ссылки на репозиторий и nexus

7. Запустите сборку по master, убедитесь что всё прошло успешно, артефакт появился в nexus

![image](pic/5.png)

8. Мигрируйте `build configuration` в репозиторий

![image](pic/6.png)

9. Создайте отдельную ветку `feature/add_reply` в репозитории

10. Напишите новый метод для класса Welcomer: метод должен возвращать произвольную реплику, содержащую слово `hunter`

Welcomer.java  

```
    public String sayHunter() {
            return "When the first hunter succumbed to the curse of blood, nightmare filled the streets of Yarnam.";
    }
```

11. Дополните тест для нового метода на поиск слова `hunter` в новой реплике

WelcomerTest.java  

```
	@Test
	public void netologySaysHunter() {
		assertThat(welcomer.sayHunter(), containsString("hunter"));
	}
```

12. Сделайте push всех изменений в новую ветку в репозиторий

13. Убедитесь что сборка самостоятельно запустилась, тесты прошли успешно

![image](pic/7.png.png)

14. Внесите изменения из произвольной ветки `feature/add_reply` в `master` через `Merge`

[Ссылка на Merge](https://github.com/Mrachneyshiy/example-teamcity/pull/1/commits)

15. Убедитесь, что нет собранного артефакта в сборке по ветке `master`

16. Настройте конфигурацию так, чтобы она собирала `.jar` в артефакты сборки

![image](pic/8.png)

17. Проведите повторную сборку мастера, убедитесь, что сбора прошла успешно и артефакты собраны

![image](pic/9.png)

18. Проверьте, что конфигурация в репозитории содержит все настройки конфигурации из teamcity

![image](pic/10.png)

19. В ответ предоставьте ссылку на репозиторий

[Ссылка на репозиторий](https://github.com/Mrachneyshiy/example-teamcity)