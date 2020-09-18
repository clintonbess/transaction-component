# transaction-component
## Overview
For this project I kept everything simple by using the Vue-CLI to generate the project bones. Below are a few assumptions:
- The transaction feed data from the "API" lists transactions in ascending order by date
- The goal was to create a pure component that does not require the previous state
- The transactions labeled `NotSettled` are the same as the transactions where `Billed: false`
- Correct data will be provided when testing the components
- No extra packages should be used since the components will be tested out side of this app

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

By default the application is served on http://localhost:8080/

## Project Details
The project contains two vue components that should be able to be easily tested if taken away from this project. To see an example set up [click here](src/App.vue)

### TransactionFeed.vue

[TransactionFeed.vue](src/Components/TransactionFeed.vue) is the component that was contracted to be written. Once initialized it creates a list of transactions that are to be used by the `Transaction` component by appending the `StartingBalance` and `EndingBalance` to the transaction objects. Afterwards, the transaction list is reversed in order to display the most recent transactions first.

The `availableBalance` is calculated by reduction. The first available balance is used as the starting value while the transaction amounts are continuously subtracted from it.

The `pendingTransactionCount` is found by filtering the transaction list and selecting the transactions that are not billed (`Billed: false`)

### Transaction.vue

[Transaction.vue](src/Components/Transaction.vue) is a clickable component that will show the details of a transaction once clicked. It requires that a `transaction` prop be passed in order to be rendered. All of the heavy lifting for retrieving the details from the transaction is performed in the `TransactionFeed` component. All of the computed properties are used to display the values inside of the details view.
