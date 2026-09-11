---
## Front matter
title: "Лабораторная работа №1"
subtitle: "Шифры простой замены"
author: "Кадрова Ангелина Александровна"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true # Table of contents
toc-depth: 2
lof: true # List of figures
lot: false # List of tables
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
	- spelling=modern
	- babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: IBM Plex Serif
romanfont: IBM Plex Serif
sansfont: IBM Plex Sans
monofont: IBM Plex Mono
mathfont: STIX Two Math
mainfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
romanfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
sansfontoptions: Ligatures=Common,Ligatures=TeX,Scale=MatchLowercase,Scale=0.94
monofontoptions: Scale=MatchLowercase,Scale=0.94,FakeStretch=0.9
mathfontoptions:
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Введение

## Цели и задачи

**Цель работы**

Основная цель работа — изучить и реализовать шифры Цезаря и Атбаш для русского и английского алфавитов.

**Задание**

С помощью языка программирования Julia реализовать:

- шифр Цезаря с произвольным ключом k,
- шифр Атбаш.

## Теоретическое введение

Julia — высокоуровневый свободный язык программирования с динамической типизацией, созданный для математических вычислений[@julialang]. Эффективен также и для написания программ общего назначения. Синтаксис языка схож с синтаксисом других математических языков, однако имеет некоторые существенные отличия.
Для выполнения заданий была использована официальная документация Julia[@juliadoc].

# Выполнение лабораторной работы

Шифр Цезаря — один из древнейших и простейших шифров. Его суть в том, что каждая буква исходного текста заменяется на букву, сдвинутую на фиксированное число позиций в алфавите. Например, при сдвиге на 3 буква A становится D, B — E и так далее. Такой сдвиг одинаков для всего текста.

Реализация для русского алфавита:

```julia
function caesar_ru(text, k)
    alphabet = collect("абвгдеёжзийклмнопрстуфхцчшщъыьэюя")
    result = []
    for char in text
        lower = lowercase(char)
        index = findfirst(isequal(lower), alphabet)

        if index != nothing
            new_index = mod(index + k - 1, 33) + 1
            new_char = alphabet[new_index]

            if isuppercase(char)
                push!(result, uppercase(new_char))
            else
                push!(result, new_char)
            end
        else
            push!(result, char)
        end
    end
    return join(result)
end
```

Реализация для английского алфавита:

```julia
function caesar_en(text, k)
    result = []
    for char in text
        if 'a' <= char <= 'z'
            new_char = Char(mod(Int(char) - Int('a') + k, 26) + Int('a'))
            push!(result, new_char)
        elseif 'A' <= char <= 'Z'
            new_char = Char(mod(Int(char) - Int('A') + k, 26) + Int('A'))
            push!(result, new_char)
        else
            push!(result, char)
        end
    end
    return join(result)
end
```

Шифр Атбаш — это частный случай аффинного шифра, где буквы алфавита заменяются на «зеркальные» буквы: первая буква заменяется на последнюю, вторая — на предпоследнюю и так далее.

Реализация для русского алфавита:

```julia
function atbash_ru(text)
    alphabet = collect("абвгдеёжзийклмнопрстуфхцчшщъыьэюя")
    result = []
    for char in text
        lower = lowercase(char)
        index = findfirst(isequal(lower), alphabet)
        if index != nothing
            new_index = 34 - index
            new_char = alphabet[new_index]
            if isuppercase(char)
                push!(result, uppercase(new_char))
            else
                push!(result, new_char)
            end
        else 
            push!(result, char)
        end
    end
    return join(result)
end
```

Реализация для английского алфавита:

```julia
function atbash_en(text)
    result = []
    for char in text
        if 'a' <= char <= 'z'
            new_char = Char(Int('z') - (Int(char) - Int('a')))
            push!(result, new_char)
        elseif 'A' <= char <= 'Z'
            new_char = Char(Int('Z') - (Int(char) - Int('A')))
            push!(result, new_char)
        else
            push!(result, char)
        end
    end
    return join(result)
end
```

В результате получим следующее:



Выполним примеры из лабораторной работы (рис. [-@fig:001]).

![Результаты работы шифров](image/1.png){#fig:001 width=70%}


# Вывод

С помощью языка программирования Julia были реализованы:

- шифр Цезаря для русского и английского алфавитов,
- шифр Атбаш для русского и английского алфавитов.

# Список литературы{.unnumbered}

::: {#refs}
:::
