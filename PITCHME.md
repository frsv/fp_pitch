@title[Introduction]
# Functional Programming (in JS)

---
@title[Ссылки]

## Ссылки

* [Functional Programming in Business](https://www.youtube.com/watch?v=ybSBCVhVWs8)
* [Ramda.js](http://ramdajs.com/)
* [So You Want to be a Functional Programmer](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-1-1f15e387e536)
* [Immutable.js](https://facebook.github.io/immutable-js/docs/#/)
---
@title[Что это такое]

## Что это такое?

Функциональное программирование – это парадигма программирования, противопоставляемая классическому ООП.

---
@title[Особенночти]

## Особенности

* Чистые функции
* Функции высшего порядка и композиция функций
* Неизменяемые данные
* Отсутствие общего состояния
* Декларативный стиль программирования
* Stateless way of programming

---
@title[Зачем это надо]

## Зачем это надо?

* Меньше кода и больше выразительности
* Безопасность и устойчивость к багам
* Ленивые вычисления
* Переиспользуемость
* Увеличение продуктивности

---
@title[Чистые функции]

* Возвращают тот же результат при одинаковом входе
* Не имеют сайд-эффектов
  1. Ввод-вывод
  2. Чтение файла
  3. Логгирование
  4. Запросы в сеть/бд
* Имеют прозрачность ссылок: вызов функции можно заменить на её результат

+++
@title[Examples]

```javascript
// pure or not ?
const isInGameCurrency = currency =>
    Object.values(Currency.TYPES).includes(currency)

// pure or not ?
const isSpecialOffer = ({ promotion }) => {
    const endDate = moment.utc(promotion.endDate).local()
    return moment().isBefore(endDate)
}

// pure or not ?
const setData = data => {
    console.log(`Incremenenting {data.id}`)
    data.value = prompt('Set value')
    return data
}
```

+++
@title[Грязные функции и их опасность]

Проблема грязных функций:

```javascript
const f = x => ++x.y
const data = {y: 0}
f(data) // { y: 1 }
f(data) // { y: 2 }
f(data) // { y: 3 }
```

---
@title[Неизменяемость данных]

## Неизменяемость данных

Это ключевое понятие функционального программирования, 
без которого невозможна работы программ без общего состояния, 
которого в ФП языках нет совсем

Использование:
* map, filter, reduce
* Array/Object spreading, Object.assign
* Object.freeze
* Immutable.js

+++
@title[Опасность изменяемых данных]

## Опасность изменяемых данных

При изменении функции необходимо держать в голове понимание, 
как фукнция работает с этими данными и как её работа может повлиять на эти переменные

```javascript
const appendSizes = bundlesData =>
    bundlesData.forEach(item => item.cardSize = CARD_SIZES.SIZE_3X1)
    
const data = getDataFromAnywhere()
appendSizes(data)
// необходимо помнить, что data изменена
```

+++
@title[Пример]

Относительно реальный пример, намеренно ухудшенный:

```javascript
// используем в контейнере, так как в компоненте нам нужны только названия техники
const appendVehiclesNames = (data, vehiclesData) => {
    const ids = [...data.vehicles]
    data.vehicles = []
    vehiclesData.forEach(item => ids.includes(item.id) && data.vehicles.push(item.name))
}

const compareVehiclesWithNew = (data, newIds) => {
    newIds.forEach(id =>
        data.vehicles.includes(id) ?
            data.vehicles.popId(id) : // assume it is find value and remove it
            data.vehicles.push(id)
    )
}

const dataWithVehicles = {
    vehicles: [1,2,3]
}
const vehiclesData = [{id:1, name: '1 vehicle'}, {id:2, name: '2 vehicle'}, {id:3, name: '3 vehicle'}]
const newIds = [2,3,4]

appendVehiclesNames(dataWithVehicles, vehiclesData)
compareVehiclesWithNew(dataWithVehicles, newIds)

// ожидаем
// dataWithVehicles.vehicles === [1,3,4]
// в компоненте: 1 vehicle, 2 vehicle, 3 vehicle

// получаем баги
// dataWithVehicles.vehicles === ['1 vehicle', '2 vehicle', '3 vehicle', 2, 3, 4]
```

@[1-6]
@[7-14]
@[15-24]
@[25-31]

---
@title[Как нам с этим жить]

## Как нам с этим жить

Всю красоту ФП можно осознать, только используя ФП-языки, 
но применение различных техник и инструментов может существенно 
увеличить нашу продуктивность и упростить жизнь

---
@title[Конец]

# 🦄
## Спасибо за внимание
