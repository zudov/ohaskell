----
title: Функции с побочными эффектами
prevChapter: /ru/io/index.html
nextChapter: /ru/io/IO-a.html
----

О чистых функциях вы уже знаете. Пришла пора поговорить о функциях, имеющих побочные эффекты.

## Чистота vs нечистота

Как вы помните, чистые функции не имеют побочных эффектов, что делает их отражением математического понятия "функция": полученное на входе однозначно определяет то, что будет на выходе. И всё бы замечательно, но наше приложение обязано взаимодействовать с внешним миром, а чистые функции никак не подходят на роль послов во внешний мир.

Допустим, у нас есть функция чтения текста из файла: она принимает строку с путём к этому файлу, а возвращает строку с содержимым файла:

```haskell
readMyFile :: String -> String
readMyFile path =
    -- открываем соответствующий файл,
    -- читаем его и возвращаем строку с его содержимым...
````

Но такая функция не может быть чистой. Если мы два раза применим её к строке с путём к одному и тому же файлу, можем ли мы гарантировать, что возвращённое содержимое будет одним и тем же? Конечно же нет, ведь между первым и вторым применениями этот файл мог быть трижды изменён. В этом случае математичность такой функции лопнет как мыльный пузырь: дважды применили к одному и тому же аргументу, а результат на выходе получили разный. Именно поэтому взаимодействие с внешним миром - это царство функций с побочными эффектами.

## Действие vs бездействие

Чистая функция - это мир бездействия. Мир спокойствия и тишины. Как вы уже знаете, такая функция состоит из совокупности выражений, которые вычисляются в некотором порядке и в конце концов оставляют некое последнее, итоговое значение, которым компилятор просто заменяет место вызова этой функции.

А вот функции с побочными эффектами - это мир действия. Мир, в котором всё меняется. Именно поэтому при работе с вводом и выводом нам понадобятся действия (actions). Действие - это то, что соприкасается с внешним миром, а зачастую ещё и оказывает на него влияние. Нужно прочитать файл? Добро пожаловать во внешний мир. Нужно отправить UDP-дейтаграмму? Вам во внешний мир. Нужно прочесть строку, введённую пользователем с клавиатуры? Внешний мир ждёт вас.