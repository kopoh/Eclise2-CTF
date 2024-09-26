## Решение 1 (JADX)
Для начала скачиваем JADX 
После этого открываем прилагаемый к таску APK файл
И исследуем структуру проекта

Зачастую всё что нужно хранится в package ID в нашем случае это com -> KopohGames.Scheduler (com.KopohGames.Scheduler)
Т.к. наша первая проблема  это экран входа в приложение, мы должны найти этот экран в коде раздекомпилированного приложения.
![Assets/1.png](Assets/1.png)
Изучая файл ==LoginScreen==
Находим участок кода который отвечает за вход (по названию ==LoginResponse==)

	Строчки кода что относятся к обработке входа в приложение отмечены синим квадратиком

Находим строчки что отвечают за состояние переменной  ==LoginResponse==, которые берутся из такой сущности как ViewModel с названием ==LoginViewModel==

	Использование ViewModel это cтандартная практика разработки приложений, паттерн MVVM 
	Строчки обращения к сущности ViewModel отмечены зелеными квадратиками

Далее находим участок кода отвечающий за успешную авторизацию в приложении (обведены пурпурным цветом) 
![Assets/2.png](Assets/2.png)
Переходим в ==LoginViewModel== для изучения механизма входа, находим тут функцию ==loginUser()==, которая внутрь себя принимает логин и пароль, мы на правильном пути
![Assets/3.png](Assets/3.png)
Видим обращение к файлу ==LoginRepository==
Дальше кликая по методу ==userLogin(login, password)== получаем ссылку на этот метод
![Assets/3.png](Assets/3.png)
Перейдя в метод уже внутри ==LoginRepository==, мы видим уже знакомый ==LoginResponse== спустившись немного далее, мы наблюдаем условное выражение которое буквально значит что if(login == "ZEROCTF" && password == "Reverse is useless!")
![Assets/4.png](Assets/4.png)

Данные для входа login это ZEROCTF и password это Reverse is useless!

Далее входим в приложение, видим что ничего нет, и ничего толком не происходит
![Assets/5.jpg|300](Assets/5.jpg)

Но если сильно захотеть то можно пролистать до 30 октября, и после этого можно увидеть подсказку
![Assets/6.jpg|300](Assets/6.jpg)
Либо же если очень сильно захотеть и изучить механизм поставления расписания, то можно найти эту подсказку в ==SharedViewModel==
![Assets/7.jpg](Assets/7.png)

Далее начинаем изучать экран настроек ==SettingsScreen==, и видим странную сущность под названием ==KonamiCodeBox==, начиная изучать эту странную сущность в которой очень много разного кода (ну по факту это означает что она явно отвечает за большую часть экрана)
![Assets/8.jpg](Assets/8.png)
Изучив ==KonamiCodeBox==, мы видим что осуществление проверки кода Konami происходит при помощи свайпов и тапов, в итоге для того чтобы понять какую последовательность действий надо произвести, надо копать дальше в enum class ==Gesture==
![Assets/9.jpg](Assets/9.png)
![Assets/10.jpg](10.png)
Чтобы найти последовательность мы должны найти все использования класса ==Gesture==, и видим список что задает последовательность действий свайпы: лево, право, лево, право, верх, них, тап, тап.
![Assets/11.jpg](11.png)
Вводим эту последовательность жестов на экране настроек (может получиться не с первого раза)
И в итоге высвечивается такое поле 
![Assets/12.jpg|300](12.jpg)
И флаг это 
```
kpkCTF{$ch3dule_w@s_t3ribb1e}
```

## Решение 2 (IDA)

Если это можно решить через JADX/ApkTools и остальные инструменты для декомпиляции AKP файлов, значит и в IDA можно найти нужные места и блоки кода