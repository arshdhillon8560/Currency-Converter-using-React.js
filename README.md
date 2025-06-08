# Currency Converter & Custom Hooks in React

## Currency Converter

A **currency converter** is a tool that allows users to convert an amount from one currency to another using real-time exchange rates. In a React application, this typically involves:

- Fetching exchange rates from an API (e.g., [exchangerate-api.com](https://www.exchangerate-api.com/))
- Allowing users to input an amount and select source/target currencies
- Displaying the converted value

## Custom Hooks

**Custom hooks** in React are JavaScript functions that start with `use` and allow you to extract and reuse stateful logic across components.

### Example: `useCurrencyConverter` Hook

A custom hook for currency conversion might:

- Fetch and store exchange rates
- Provide a function to convert amounts
- Handle loading and error states

```jsx
import { useState, useEffect } from 'react';

function useCurrencyConverter(base = 'USD') {
    const [rates, setRates] = useState({});
    const [loading, setLoading] = useState(true);

    useEffect(() => {
        fetch(`https://api.exchangerate-api.com/v4/latest/${base}`)
            .then(res => res.json())
            .then(data => {
                setRates(data.rates);
                setLoading(false);
            });
    }, [base]);

    const convert = (amount, to) => {
        if (!rates[to]) return null;
        return amount * rates[to];
    };

    return { rates, convert, loading };
}
```

### Usage

```jsx
const { rates, convert, loading } = useCurrencyConverter('USD');
const amountInEur = convert(100, 'EUR');
```

## Benefits

- **Separation of concerns:** UI and logic are decoupled
- **Reusability:** Use the hook in multiple components
- **Testability:** Easier to test logic in isolation
