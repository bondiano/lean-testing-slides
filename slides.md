---
theme: ./theme
background: /background.png
class: text-center
highlighter: shiki
routerMode: 'hash'
onts:
  sans: 'Roboto'
  serif: 'Roboto Slab'
  mono: 'Fira Code'
info: |
  Доклад к sber market js conf
title: Бережливый React Testing Library в расточительном мире
---

# Бережливый *React Testing Library*
в расточительном мире

Василий Кузенков, Adapty
<!--

Всем привет, меня зовут Василий Кузенков, я Frontend тимлид в Adapty, мы стартап, занимаемся аналитикой подписок в мобильных приложениях. Также наставничаю на Хекслете на курсе по тестированию фронтенда.

Сейчас многие говорят про важность тестов, но зачастую разработчики не знают к чему подступиться, когда дело касается практики.

Многие вспоминают Мартина Фаулера с пирамидой тестирования.
Те, кто ближе к фронтенду говорят о Кент Доддсе и трофее.
Вокруг тестов множество метафор, мифов и в целом недостаточно прагматичного подхода, превращая все в карго-культ.
Также важно уточнить, что мы будем в основном говорить о тестировании в разрезе клиентских приложений.

-->

---
layout: intro
---

# О чем этот доклад

* Что такое бережливое тестирование?
* В каких случаях оно может помочь нам?
* Что предоставляет *React Testing Library*?
* Каково тестировать React Native c *RTL*?
* Основные принципы *RTL* и как они пересекаются с бережливым тестированием?
* А нужно ли бережливое тестирование?

<!--

Расскажу в чем же заключается бережливость тестирования, посмотрим примеры тестов на реакт нативе и реакте; с RTL и где эти принципы пересекаются с бережливым тестиованием и вообще нужно ли оно

-->

---
layout: two-cols
---


<template v-slot:default>
<h1>В мире бережливости</h1>

<img src="/lean_startup.png">

</template>
<template v-slot:right>

  <div class="slidev-layout section w-full h-full grid">
    <div class="my-auto text-center">
      Также как в LEAN-стартапе мы не знаем, во что вырастет проект, и мы хотим избежать траты сил на лишние и дорогие тесты
    </div>
  </div>

</template>

<!--

Сам подход пошел с бережливого или lean-производства. Где цель lean - создавать ценность, сокращая расходы на производство.

Цель Lean ― создавать ценность, сокращая расходы на ее производство.

Стартап действует в условиях чрезвычайной неопределенности, и это нужно учитывать при его запуске. На первоначальном этапе развития стартапу необходимо оставаться гибким, чтобы учиться на ошибках и максимально быстро проверять гипотезы основателей, а значит, необходимо избегать крупных вливаний и затрат. Именно такой подход лежит в основе метода Lean, цель которого помочь предпринимателям повысить шансы стартапа на успех.

По сути, задача метода Lean startup — помочь предпринимателю избежать риска траты огромных денег и сил ради создания никому ненужного продукта. Со временем методика Lean стала пользоваться большой популярностью и применяться не только в стартапах и не только в рамках отрасли IT.

-->

---

# Инвестиции в тесты

* Подготовка локального окружения
* Подготовка инфраструктуры
* Тестируемая архитектура
* Тесты - часть кодовой базы

<v-click>
  <div class="flex justify-end">
    <img src="/ice_rock.jpeg" style="height: 400px">
  </div>
</v-click>


<!--

Думаю, многие, кто настраивал тесты в проекте с нуля, сталкивался с проблемами настройки окружения, настройкой CI и если говорим про тестирование в браузере или на эмуляторах, то сразу возникают дополнительные сложности:
* Два окружения (Тесты и Браузер), Сетап окружения, Восстановление состояния, Запросы во внешние системы

К этому добавляется настройка и поддержка инфраструктуры: селениум, веб-драйвера, проблемы с паралельным запуском тестов и многое другое.

Скорее всего есть и те, кто сталкивался со сложной и запутанной кодовой базой, которую очень сложно покрыть тестами. То есть еще один из пунктов инвестирования в тесты нужно включить выстраивание тестируемой архитектуры.
Такой, где есть выделение побочных эффектов, есть способы борьбы с недетерминированностью, нет неуправляемой асинхронности, и не тяжелое окружение

Ну и собственно сами тесты: дополнительные линтеры, договоренности на проекте и архитектура тестов. Всем хорошо, когда тестов не очень много, они быстро выполняются,не зависят друг от друга, тестируют функциональность независимо и не переписываются при рефакторинге


-->

---

# Окупаемость тестов

* Отражают проблемы архитектуры
* Основная функциональность работает
* Не боимся, что все развалится при деплое от новых фич
* Получаем документацию в виде кода

<!--

Вопрос окупаемости тестов в том, что же мы считаем важными метриками, когда пишем тесты. Каких плюшек для проекта мы ждем от них

-->

---

# Автоматическое тестирование

<div class="pb-6">

* **Arrange** (настройка) — в этом блоке кода мы настраиваем тестовое окружение тестируемого юнита;
* **Act** — выполнение или вызов тестируемого сценария;
* **Assert** — проверка того, что тестируемый вызов ведет себя определенным образом

</div>

```js {all|4|6|8|all}
import billingCenterApp from '../billingCenterApp';

test('bill money correctly', () => {
  const user = billingCenterApp.getCustomer();

  billingCenterApp.bill(user, 10);

  expect(user.money).toEqual(38);
});
```

<!--
Как разработчиков, нас не так интересует ручное тестирование. И ставку мы делаем именно на автоматизированное тестирование и требуемые для этого инструменты.

Часто используемым подходом в автоматизированном тестировании является паттерн AAA.

При этом мы всегда помним, что мы тестировать следует именно свой код, а не внешний.
-->

---
layout: quote
---

# Цель бережливого тестирования

I get paid for code that works, not for tests, so my philosophy is to test as little as possible to reach a given level of confidence. [**Kent Beck (TDD)**](https://stackoverflow.com/questions/153234/how-deep-are-your-unit-tests/153565#153565)

Убедиться что приложение работает наиболее дешевым способом.

<!--
Нехватка чётких эмпирических данных провоцирует людей на громкие заявления. Я выступаю за экономический, прагматичный взгляд на тестирование. Чрезмерная сосредоточенность на модульных тестах — не самый экономичный подход. Такую философию я буду называть бережливым тестированием.

Как более сформулированная цель тут: "Убедиться что приложение работает наиболее дешевым способом".
-->

---

# React Testing Library

[React Testing Library](https://testing-library.com/) — библиотека построенная поверх `DOM Testing Library`, содержащая в себе инструментарий `react-dom` и `react-dom/test-utils`.

Чем больше процесс тестирования напоминает реальный сеанс работы с приложением — тем увереннее можно говорить о том, что, когда приложение попадёт в продакшн, оно будет работать так, как ожидается.

```javascript
// Создается после генерации CRA
import { render, screen } from '@testing-library/react';
import App from './App';

test('renders learn react link', () => {
  render(<App />);
  const linkElement = screen.getByText(/learn react/i);
  expect(linkElement).toBeInTheDocument();
});
```

<!--

React Testing Library является рекомендацией оффициальной документации React, в замен Enzyme.

Его принцип "Чем больше процесс тестирования напоминает реальный сеанс работы с приложением — тем увереннее можно говорить о том, что, когда приложение попадёт в продакшн, оно будет работать так, как ожидается.",- сформулирован Кентом Додсом и также максимально близок к прагматичному подходу к тестированию.

В качестве тест раннера мы будем использовать Jest со включенным jsdom.

-->

---
layout: statement
---

# React Native Testing Library

[React Native Testing Library](https://callstack.github.io/react-native-testing-library/) — библиотека построенная поверх `react-test-renderer`, использующая схожие принципы c React Testing Library.

<!--

Это активно развивающийся младший брат React Testing Library, со схожей философией и принципами. Все еще отстаёт от фич оригинала. И в отличии от старшего барата использует react-test-renderer в качестве рендера. Но его вполне достаточно для тестирования компонентов и начнем знакомиться с концепциями именно на нем

-->

---

# Тестируем форму на React Native

<div class="flex">
  <div class="pr-30">
    <h2>Create Account</h2>
    <img src="/signup_screenshot.png" style="height: 300px">
  </div>
  <div>
    <h2>Sign in</h2>
    <img src="/signin_screenshot.png" style="height: 300px">
  </div>
</div>

<!--

Тестировать мы будем простую форму регистрации с валидацией и отправкой формы.

-->

---

# <logos-jest class="inline-block" /> Пишем тесты

`jest` + `jest-expo` + `@testing-library/react-native` + `@testing-library/jest-native`

```javascript{7,16-19|7-14|8|9-13|all}
import React from 'react';
import { render } from '@testing-library/react-native';

import { CreateAccount } from '../CreateAccount';

describe('CreateAccount', () => {
  test('вижу форму с полями и кнопку отправки', () => {
    const { getByText, getByAccessibilityLabel } = render(<CreateAccount />);

    expect(getByAccessibilityLabel('Email')).toBeEnabled();
    expect(getByAccessibilityLabel('Password')).toBeEnabled();
    expect(getByAccessibilityLabel('Confirm Password')).toBeEnabled();
    expect(getByText('Submit')).toBeEnabled();
  });

  test.todo('заполняю все поля -> получаю сообщение, что успешно зарегистрирован');
  test.todo('заполняю не все поля -> получаю ошибку валидации');
  test.todo('заполняю поле не верно -> получаю ошибку валидации');
  test.todo('заполняю все поля -> приходит ошибка -> показываю сообщение об ошибке');
});
```

<!--

Прежде всего нам надо определить достаточные тест кейсы, схожие с пользовательскими сценариями на самой странице.

1. Вижу форму с полями и кнопку отправки формы
2. Заполняю все поля -> получаю сообщение, что успешно зарегестрирован
3. Заполняю не все поля -> получаю ошибку валидации
4. Заполняю поле не верно -> получаю ошибки валидации
5. Заполняю все поля -> приходит ошибка -> показываю сообщение об ошибке

Также есть некоторое количество других сценариев, в том числе связанных с сетью, поведением пользователя, различные варианты валидации и пр
Покрыть их сложнее и при этом не повысит в разы надежность тестов. Так что как правило выбор самых простых в реализации сценариев дает те самые 80% надежности за минимальные усилия

-->
---

# Mock Service Worker

[Mock Service Worker](https://mswjs.io/) — библиотека позволяющая перехватить запрос на сетевом уровне. Подходит для тестирования, разработки, дебага

```javascript
import { rest } from 'msw';

const postmanPost = rest.post('https://postman-echo.com/post', (req, res, ctx) => res(
  ctx.status(200),
));

export const handlers = [postmanPost];
```

```javascript
import { setupServer } from 'msw/node';

import { handlers } from './handlers';

export const server = setupServer(...handlers);
```

<!--

Тесты не должны влиять друг на друга
Стараемся замокать побочные эффекты

Плюс от использование сервис воркера в качестве подмены - в дев тулзах показывается запрос, те происходит реальный запрос с точки зрения браузера.

-->

---

# Пишем тесты (success)

```javascript{2-8|9-23|10-15|1,16-19|20-22}
import { render, waitFor, fireEvent } from '@testing-library/react-native';
import { server } from '__mocks__/server';
import { errorHandler } from '__mocks__/handlers';

beforeAll(() => server.listen());
afterEach(() => server.resetHandlers());
afterAll(() => server.close());

test('заполняю все поля -> получаю сообщение, что успешно зарегистрирован', async () => {
 const { getByText, getByAccessibilityLabel } = render(<CreateAccount />);
 const emailInput = getByAccessibilityLabel('Email');
 const passwordInput = getByAccessibilityLabel('Password');
 const confirmPasswordInput = getByAccessibilityLabel('Confirm Password');
 const submitButton = getByText('Submit');

 fireEvent.changeText(emailInput, 'test@test.com');
 fireEvent.changeText(passwordInput, 'test');
 fireEvent.changeText(confirmPasswordInput, 'test');
 fireEvent.press(submitButton, 'click');

 await waitFor(() => getByText('Account was successfully created'));
 expect(submitButton).toBeEnabled();
});
```

---

# Пишем тесты (не заполнены поля)

```javascript{11}
test('заполняю не все поля -> получаю ошибку валидации', async () => {
 const { getByText, getByAccessibilityLabel } = render(<CreateAccount />);
 const emailInput = getByAccessibilityLabel('Email');
 const passwordInput = getByAccessibilityLabel('Password');
 const submitButton = getByText('Submit');

 fireEvent.changeText(emailInput, 'test@test.com');
 fireEvent.changeText(passwordInput, 'test');
 fireEvent.press(submitButton, 'click');

 await waitFor(() => getByText('Validation error'));
 expect(submitButton).toBeEnabled();
});
```

---

# Пишем тесты (не верный повтор пароля)

```javascript{9,13}
test('заполняю поле не верно -> получаю ошибку валидации', async () => {
 const { getByText, getByAccessibilityLabel } = render(<CreateAccount />);
 const emailInput = getByAccessibilityLabel('Email');
 const passwordInput = getByAccessibilityLabel('Password');
 const confirmPasswordInput = getByAccessibilityLabel('Confirm Password');
 const submitButton = getByText('Submit');

 fireEvent.changeText(emailInput, 'test@test.com');
 fireEvent.changeText(passwordInput, 'test');
 fireEvent.changeText(confirmPasswordInput, 'error');
 fireEvent.press(submitButton, 'click');

 await waitFor(() => getByText('Password confirmation error'));
 expect(submitButton).toBeEnabled();
});
```

---

# Пишем тесты (ошибка сети)

```javascript{1,10|17}
const errorHandler = rest.post(URL, (_, res, ctx) => res(ctx.status(500)));

test('заполняю все поля -> приходит ошибка -> показываю сообщение об ошибке', async () => {
 const { getByText, getByAccessibilityLabel } = render(<CreateAccount />);
 const emailInput = getByAccessibilityLabel('Email');
 const passwordInput = getByAccessibilityLabel('Password');
 const confirmPasswordInput = getByAccessibilityLabel('Confirm Password');
 const submitButton = getByText('Submit');

 server.use(errorHandler);

 fireEvent.changeText(emailInput, 'test@test.com');
 fireEvent.changeText(passwordInput, 'test');
 fireEvent.changeText(confirmPasswordInput, 'test');
 fireEvent.press(submitButton, 'click');

 await waitFor(() => getByText('Something went wrong'));
 expect(submitButton).toBeEnabled();
});
```

---

# Разница с React Testing Library

Почти никакой 🚀

* [screen](https://testing-library.com/docs/queries/about/#screen)
* render ([react-test-renderer](https://reactjs.org/docs/test-renderer.html) vs [jsdom](https://github.com/jsdom/jsdom))
* [User Event](https://github.com/testing-library/user-event)

<!--

Как мы видим отличия фундаментально не значительны, хотя благодаря им тесты получаются проще и кода становится меньше

-->
---

# Пример теста

```javascript{all|9|15-20}
import React from 'react';
import { render, waitFor, screen } from '@testing-library/react';

import { server } from '__mocks__/server';
import { errorHandler } from '__mocks__/handlers';

import { CreateAccount } from '../CreateAccount';

beforeEach(() => render(<CreateAccount />));
beforeAll(() => server.listen());
afterEach(() => server.resetHandlers());
afterAll(() => server.close());

describe('CreateAccount', () => {
  test('вижу форму с полями и кнопку отправки', () => {
    expect(screen.getByLabelText(/email/i)).toBeEnabled();
    expect(screen.getByLabelText(/^password/i)).toBeEnabled();
    expect(screen.getByLabelText(/confirm password/i)).toBeEnabled();
    expect(screen.getByRole('button', { name: /submit/i })).toBeEnabled();
  });
});
```

---

# User events

```javascript{all|13-14}
import React from 'react';
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';

test('click', () => {
  render(
    <div>
      <label htmlFor="checkbox">Check</label>
      <input id="checkbox" type="checkbox" />
    </div>,
  );

  userEvent.click(screen.getByText('Check'));
  expect(screen.getByLabelText('Check')).toBeChecked();
});
```

---

# Пример теста (ошибка сети)

```javascript{all|2-5|7|9-12|14}
test('заполняю все поля -> приходит ошибка -> показываю сообщение об ошибке', async () => {
  const emailInput = screen.getByLabelText(/email/i);
  const passwordInput = screen.getByLabelText(/^password/i);
  const confirmPasswordInput = screen.getByLabelText(/confirm password/i);
  const submitButton = screen.getByRole('button', { name: /submit/i });

  server.use(errorHandler);

  userEvent.type(emailInput, 'test@test.com');
  userEvent.type(passwordInput, 'test');
  userEvent.type(confirmPasswordInput, 'test');
  userEvent.click(submitButton);

  await waitFor(() => screen.getByText('Something went wrong'));
  expect(submitButton).toBeEnabled();
});
```

---

# Testing Playground

[Chrome](https://chrome.google.com/webstore/detail/testing-playground/hejbmebodbijjdhflfknehhcgaklhano)

<div class="flex justify-center">
  <img src="/testing_playground.png" style="height: 400px">
</div>

---

# Где мы сэкономили

<v-clicks>

* Простая настройка окружения
* Дешевый мок запросов
* Проверяем только основной флоу
* Интеграционное тестирование благодаря отказу от неглубокой отрисовки
* Устойчивость при изменении кода

</v-clicks>

---

# Не теряем ли сильно в надежности

* Полная отрисовка, как в e2e тестах
* С MSW моками backend'а можно пользоваться в мануальном режиме
* Любое отличие от пользовательского окружения достаточно не надежно
* Тестирование показывает присутствие ошибок, а не их отсутствие. *Эдсгер Дейкстра*

<!--

Тут мы говорим о выбранных трейдофах и уверенности в тестах.

Enzyme по-умолчанию предлагал делать shallow mount

Тестирование обнаруживает присутствие, а не отсутствие багов

-->

---

# Что с unit тестами

<div class="flex justify-center">
  <img src="/unit_test_meme_1.png" style="height: 300px">
</div>

Модульные (unit) тесты могут проверять исключительные случаи, которые на практике происходят очень редко или никогда.

---

# Подходящие инструменты

<div class="pb-2">

  * [Cypress](https://www.cypress.io/) — сквозные тесты, которые достаточно просто писать и достаточно быстро запускать
  * [Storybook](https://storybook.js.org/) — удобные проверки на этапе разработки и дешевый инструмент коммуникации

</div>

<div class="flex justify-center">
  <img src="/cypress.png" style="height: 350px">
</div>

<!--

Надеюсь мне удалось показать чем React Testing Library помогает бережливому тестированию
К бережливому подходу я также отношу стенды компонентов со Storybook

Сквозные тесты обеспечивают наибольшую уверенность. Если бы их не было так дорого писать и так долго проводить, мы бы использовали намного больше сквозных тестов. Впрочем, хорошие инструменты, такие как Cypress, уравновешивают эти недостатки. Модульные тесты дешевле писать и быстрее проводить, но они проверяют лишь малую часть и, возможно, не самую важную. Интеграционные тесты лежат где-то посередине между модульными и сквозными, так что они обеспечивают наибольшее равновесие. Такие образом, они обладают наивысшей окупаемостью инвестиций.

-->

---

# Где менее применимо бережливое тестирование

* Системы с высокой требованию к надежности
* Библиотеки
* Выделенная команда тестирования
* У проекта много ресурсов

<!--

Есть разница между библиотеками и кодом приложений. У первых другие требования и ограничения, где 100-процентное покрытие кода модульными тестами, возможно, имеет смысл. Есть разница между кодом бэкенда и фронтенда. Есть разница между кодом ядерного реактора и игрой. Каждый проект — особенный. Ограничения и риски в каждом случае свои. Следовательно, чтобы быть бережливыми, вы должны приспособить свой подход к тестированию под тот проект, над которым работаете

-->

---

# Ссылки

* [Бережливое тестирование, или Почему модульные тесты хуже, чем вы думаете](https://medium.com/nuances-of-programming/%D0%B1%D0%B5%D1%80%D0%B5%D0%B6%D0%BB%D0%B8%D0%B2%D0%BE%D0%B5-%D1%82%D0%B5%D1%81%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5-%D0%B8%D0%BB%D0%B8-%D0%BF%D0%BE%D1%87%D0%B5%D0%BC%D1%83-%D0%BC%D0%BE%D0%B4%D1%83%D0%BB%D1%8C%D0%BD%D1%8B%D0%B5-%D1%82%D0%B5%D1%81%D1%82%D1%8B-%D1%85%D1%83%D0%B6%D0%B5-%D1%87%D0%B5%D0%BC-%D0%B2%D1%8B-%D0%B4%D1%83%D0%BC%D0%B0%D0%B5%D1%82%D0%B5-24670e16ab0)
* [Начинаем писать тесты (правильно)](https://ru.hexlet.io/blog/posts/how-to-test-code)
* [Write tests. Not too many. Mostly integration](https://kentcdodds.com/blog/write-tests)
* Приложения с примеров: [React Native](https://github.com/bondiano/react-native-test-form), [React](https://github.com/bondiano/react-test-form)

<div class="flex items-end flex-col">
  <span class="mb-4">Слайды: <a href="https://lean-testing.bondiano.io">lean-testing.bondiano.io</a></span>
  <img src="/qr_code.gif" style="height: 150px; width: 150px"/>
</div>

---
layout: section
---

# Начните тестировать бережливо

<div class="flex justify-center">
  <img src="/just_do_it.jpeg" style="height: 300px">
</div>

Вместо разработки сложных планов, основанных на предположениях, вы сможете вносить постоянные коррективы легким поворотом руля. Это и называется циклом обратной связи "создать-оценить-научиться".

<!--

В книге LEAN startup также есть замечательная фраза, отлично подходящая под концепцию бережливого тестирования

Вместо разработки сложных планов, основанных на предположениях, вы сможете вносить постоянные коррективы легким поворотом руля. Это и называется циклом обратной связи „создать-оценить-научиться“

-->
