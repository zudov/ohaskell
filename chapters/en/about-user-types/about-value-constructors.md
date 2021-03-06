----
title: О конструкторах значений
prevChapter: /en/about-user-types/types-at-a-glance.html
nextChapter: /en/about-user-types/type-context.html
----

Конструктор значения (value constructor) - штука полезная. Как мы увидели в предыдущей главе, конструктор позволяет идентифицировать значения наших собственных типов.

## Иные имена

Конструктор значения может отличаться от имени самого типа. Взгляните:

```haskell
data IPAddress = IP String  -- Имя типа осталось неизменным.

instance Show IPAddress where
    show (IP address) =
        if address == "127.0.0.1" then "localhost" else address
```

Теперь мы будем создавать значения нашего типа так:

```haskell
main = putStrLn $ show $ IP "127.0.0.1"
```

Такой подход позволяет писать более краткий, но при этом ничуть не менее понятный код. Обратите внимание: конструктор `IP` используется лишь в местах конструирования объекта, в то время как в описании экземпляра класса `Show` мы по-прежнему использовали имя типа `IPAddress`.

## Множество конструкторов

Конструкторов может быть более одного, и эта возможность используется очень часто. Например:

```haskell
data IPAddress = IP String | Host String
```

Мы ввели два конструктора, `IP` и `Host`. Здесь используется оператор `|`, знакомый программистам C как оператор бинарного "ИЛИ". Выглядит довольно логично, и поэтому мы можем прочесть эту строку так:

    data IPAddress  =  IP String  |  Host String
    тип  IPAddress это IP String или Host String

Определим метод `show` для каждого из этих конструкторов:

```haskell
instance Show IPAddress where
    show (IP address) =
        address

    show (Host address) =
        if address == "127.0.0.1" then "localhost" else address
```

Теперь, если мы напишем так:

```haskell
main = putStrLn $ show $ IP "127.0.0.1"
```

вывод будет таким:

```bash
127.0.0.1
```

Если же мы используем второй конструктор:

```haskell
main = putStrLn $ show $ Host "127.0.0.1"
```

вывод будет другим:

```haskell
localhost
```

## О нульарных конструкторах

Нульарный конструктор (nullary constructor) используется в Haskell-коде довольно часто. Суть его проста.

Вспомним наш демонстрационный тип:

```haskell
data IPAddress = IP String
```

За конструктором `IP` следует одно значение, поэтому такой конструктор называют одинарным. А нульарным называют такой конструктор, за которым никакого значения нет.

Допустим, нам нужен тип, ассоциированный с протоколами транспортного слоя (Transport layer) модели OSI. Вот как это будет выглядеть:

```haskell
data TransportLayer = TCP | UDP | SCTP | DCCP | SPX
```

Символ `|` вы уже знаете, но значений, как видите, здесь нет. Тип `TransportLayer` имеет пять нульарных конструкторов. И чтобы продемонстрировать практическую пользу такого типа, определим функцию, описывающую конкретный протокол:

```haskell
descriptionOf :: TransportLayer -> String
descriptionOf protocol =
    case protocol of
        TCP  -> "Transmission Control Protocol"
        UDP  -> "User Datagram Protocol"
        SCTP -> "Stream Control Transmission Protocol"
        DCCP -> "Datagram Congestion Control Protocol"
        SPX  -> "Sequenced Packet Exchange"
```

Мы принимаем значение типа `TransportLayer` и возвращаем соответствующее ему строковое описание. Обратите внимание на конструкцию с ключевым словом `case`. Это - родственница конструкции `switch-case` из языка C.

Теперь проверяем:

```haskell
main = print $ descriptionOf TCP
```

Для создания значения типа TransportLayer, к которому будет применена функция `descriptionOf`, мы использовали один из нульарных конструкторов. Никакого дополнительного значения здесь нет, потому что сам нульарный конструктор и представляет собой значение. Результат такой:

```bash
"Transmission Control Protocol"
```

Если вам нужен тип, подразумевающий фиксированное множество именованных значений - нульарные конструкторы вам помогут.

## В сухом остатке

* Конструктор значения (далее КЗ) используется при конструировании значении. Кто бы мог подумать...
* Имя КЗ может отличаться от имени самого типа.
* КЗ может быть сколько угодно, от нуля и больше.

