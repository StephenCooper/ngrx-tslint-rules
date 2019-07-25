# ngrx-tslint-rules

## Installation

### Using the Angular CLI

If your project is using the Angular CLI then you can install `ngrx-tslint-rules` to your project with the following ng add command

```bash
ng add ngrx-tslint-rules
```

### Manual install with npm or yarn

First install `ngrx-tslint-rules` as a dependency with the following command

```bash
npm install ngrx-tslint-rules --save-dev
```

Next, add `ngrx-tslint-rules` to your `tslint.json` file

```json
{
  "extends": ["ngrx-tslint-rules"],
  "rules": {
    ...
  }
}
```

## Rules

> By default all rules are enabled

| Rule                                                                | Description                                                   |
| ------------------------------------------------------------------- | ------------------------------------------------------------- |
| [`ngrx-action-hygiene`](#ngrx-action-hygiene)                       | Enforces the use of good action hygiene                       |
| [`ngrx-no-duplicate-action-types`](#ngrx-no-duplicate-action-types) | An action type must be unique                                 |
| [`ngrx-unique-reducer-actions`](#ngrx-unique-reducer-actions)       | An action can't be handled multiple times in the same reducer |

### Examples

#### `ngrx-action-hygiene`

```ts
// Invalid: action type doesn't follow the "[Source] Event" convention
const loadCustomers = createAction('Load Customers')

// Valid
const loadCustomers = createAction('[Customers Page] Load Customers')
```

#### `ngrx-no-duplicate-action-types`

```ts
// Invalid: `loadCustomers` and `createCustomer` have the same action type
const loadCustomers = createAction('[Customers Page] Load Customers')
const createCustomer = createAction(
  '[Customers Page] Load Customers',
  props<{ customer }>(),
)

// Valid
const loadCustomers = createAction('[Customers Page] Load Customers')
const createCustomer = createAction(
  '[Customers Page] Create Customer',
  props<{ customer }>(),
)
```

#### `ngrx-unique-reducer-actions`

```ts
// Invalid: `loadCustomers` is handled two times in the customersReducer reducer
const customersReducer = createReducer(
  {
    customers: [],
  },
  on(loadCustomers, (state, customers) => ({ ...state, customers })),
  on(loadCustomers, state => ({ ...state, customers: [] })),
)

// Valid
const customersReducer = createReducer(
  {
    customers: [],
  },
  on(loadCustomers, (state, customers) => ({ ...state, customers })),
  on(clearCustomers, state => ({ ...state, customers: [] })),
)
```

## License

MIT
