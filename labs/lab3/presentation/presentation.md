---
## Front matter
lang: ru-RU
title: Лабораторная работа №3
subtitle: "Шифрование гаммированием"
author:
  - Кадрова Ангелина Александровна
teacher:
  - Кулябов Д. С.
  - д.ф.-м.н., профессор
  - профессор кафедры теории вероятностей и кибербезопасности 
institute:
  - Российский университет дружбы народов имени Патриса Лумумбы, Москва, Россия
date: 10 октября 2026

## i18n babel
babel-lang: russian
babel-otherlangs: english

## Formatting pdf
toc: false
toc-title: Содержание
slide_level: 2
aspectratio: 169
section-titles: true
theme: metropolis
header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
---

## Докладчик

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

  * Кадрова Ангелина Александровна
  * Cтудентка НФИмд-02-26
  * Российский университет дружбы народов имени Патриса Лумумбы
  * [1032262509@rudn.ru](mailto:1032262509@rudn.ru)
  * <https://github.com/aakadrova>

:::
::: {.column width="30%"}

![](./image/angelina.jpg)

:::
::::::::::::::

## Цели и задачи

**Цель работы**

Основная цель работы — изучить и реализовать шифрование гаммированием.

**Задание**

С помощью языка программирования Julia реализовать алгоритм шифрования гаммированием конечной гаммой.

## Выполнение лабораторной работы

**Шифрование гаммированием** — симметричный метод шифрования, при котором к открытым данным(тексту) применяется операция наложения с последовательностью чисел, называемой гаммой.

## Выполнение лабораторной работы

```julia
alphabet = collect("абвгдежзийклмнопрстуфхцчшщьыъэюя ")
MOD = 33

function char_to_num(c)
    return findfirst(==(c), alphabet)
end

function num_to_char(n)
    if n == 0
        n = MOD
    end
    return alphabet[n]
end
```

## Выполнение лабораторной работы

```julia
function encrypt(text, gamma)
    text_chars  = collect(lowercase(text))
    gamma_chars = collect(lowercase(gamma))

    encrypted = ""
    for i in 1:length(text_chars)
        p = char_to_num(text_chars[i])
        k = char_to_num(gamma_chars[(i - 1) % length(gamma_chars) + 1])
        c = (p + k) % MOD
        encrypted *= num_to_char(c)
    end

    return encrypted
end
```

## Выполнение лабораторной работы

```julia
function decrypt(cryptogram, gamma)
    crypto_chars = collect(lowercase(cryptogram))
    gamma_chars  = collect(lowercase(gamma))

    decrypted = ""
    for i in 1:length(crypto_chars)
        c = char_to_num(crypto_chars[i])
        k = char_to_num(gamma_chars[(i - 1) % length(gamma_chars) + 1])
        p = mod(c - k, MOD)
        decrypted *= num_to_char(p)
    end

    return decrypted
end
```

## Выполнение лабораторной работы

```julia
function main()
    text  = "приказ"
    gamma = "гамма"

    cryptogram = encrypt(text, gamma)
    decrypted  = decrypt(cryptogram, gamma)

    println("Исходный текст:   ", text)
    println("Криптограмма:     ", cryptogram)
    println("Расшифрованный:   ", decrypted)
    println("Совпадает:        ", decrypted == text)
end

main()
```

## Выполнение лабораторной работы

![Результаты работы шифра](image/1.png){#fig:001 width=100%}


## Вывод

С помощью языка программирования Julia был реализован алгоритм шифрования гаммированием конечной гаммой.
