# Калькулятор ипотечного кредита

Веб-приложение для расчета ипотечного кредита, разработанное с использованием Vue 3 и SCSS.

## Функциональность

- Расчет суммы кредита
- Выбор тарифного плана
- Учет материнского капитала
- Расчет ежемесячного платежа
- Интерактивные слайдеры для ввода данных
- Адаптивный дизайн

## Технологии

- Vue 3
- Vite
- SCSS
- JavaScript (ES6+)

## Установка и запуск

1. Клонируйте репозиторий:
```bash
git clone [url-репозитория]
cd mortgage-calculator
```

2. Установите зависимости:
```bash
npm install
```

3. Запустите проект в режиме разработки:
```bash
npm run dev
```

4. Для сборки проекта:
```bash
npm run build
```

## Структура проекта

```
mortgage-calculator/
├── src/
│   ├── assets/
│   │   └── styles/
│   │       ├── variables.scss
│   │       └── main.scss
│   ├── components/
│   │   ├── LoanPurpose.vue
│   │   ├── TariffSelection.vue
│   │   ├── PropertyCost.vue
│   │   ├── DownPayment.vue
│   │   ├── LoanTerm.vue
│   │   └── CalculationResults.vue
│   └── App.vue
├── index.html
├── package.json
├── vite.config.js
└── README.md
```

## Лицензия

MIT
