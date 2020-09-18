<template>
  <div class="transaction-feed-component">
    <h3>Transaction Feed</h3>
    <div class="account-details">
      <div class="account-details-row">
        <div>Available balance (after pending transactions)</div>
        <div class="available-balance" v-bind:class="{negative: isNegative}">{{availableBalance}}</div>
      </div>
      <div class="account-details-row">
        <div>Pending Transactions</div>
        <div>{{pendingTransactionsCount}}</div>
      </div>
      <div class="account-details-row">
        <div>Total Transactions</div>
        <div>{{transactions.length}}</div>
      </div>
    </div>
    <div class="transaction-header">
      <div>Date</div>
      <div>Details</div>
      <div>Type</div>
      <div>Status</div>
      <div>Amount</div>
      <div>Starting Balance</div>
      <div>Ending Balance</div>
    </div>
    <div class="transaction-feed">
      <Transaction
        v-for="(transaction, index) in transactions"
        v-bind:transaction="transaction"
        v-bind:key="index"
      />
    </div>
  </div>
</template>

<script>
import Transaction from './Transaction.vue'

export default {
  name: 'TransactionFeed',
  props: ['transactionData'],
  components: {
    Transaction
  },
  computed: {
    transactions () {
      try {
        const rawTransactions = this.transactionData.Statement.Transactions.slice()
        let availableBalance = rawTransactions[0].AvailableBalance
        
        const transactions = rawTransactions.map((transaction) => {
          const transactionAmount = transaction.Amount
          const endingBalance = availableBalance - transactionAmount
          const transactionWithBalances = {
            ...transaction,
            StartingBalance: availableBalance,
            EndingBalance: endingBalance
          }

          availableBalance = endingBalance

          return transactionWithBalances
        })

        return transactions.reverse()
      } catch (err) {
        return []
      }
    },
    availableBalance () {
      try {
        const rawTransactions = this.transactionData.Statement.Transactions
        const startingBalance = rawTransactions[0].AvailableBalance

        const _availableBalance = rawTransactions.reduce((balance, transaction) => {
          const transactionAmount = transaction.Amount
          return (balance - transactionAmount)
        }, startingBalance)
        
        return _availableBalance.toFixed(2)
      } catch (err) {
        return 0
      }
    },
    pendingTransactionsCount () {
      const rawTransactions = this.transactionData.Statement.Transactions
      return rawTransactions.filter(transaction => transaction.Billed === false).length
    },
    isNegative () {
      return this.availableBalance < 0
    }
  }
}
</script>

<style scoped>

div.transaction-feed-component {
  align-items: center;
  display: flex;
  flex-direction: column;
}

div.transaction-feed-component > div {
  width: 90%;
}

.available-balance {
  color: green;
  font-size: 14px;
  font-weight: bold;
}

.available-balance.negative {
  color: red;
}

.account-details {
  border: solid 1px gray;
  display: flex;
  flex-direction: row;
  font-size: 12px;
  justify-content: space-evenly;
  padding: 20px;
}

.account-details-row div:first-of-type {
  font-weight: bold;
  margin-bottom: 10px;
}

.transaction-feed {
  align-items: center;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.transaction-header {
  border: solid 1px gray;
  display: flex;
  flex-direction: row;
  font-size: 11px;
  justify-content: space-evenly;
  padding: 10px;
  text-align: center;
}

.transaction-header > div {
  width: 15%;
}

.transaction-feed .transaction:nth-of-type(even) {
  background-color: #90fb9075;
}

.transaction-feed .transaction:nth-of-type(even):hover {
  background-color: white;
}

</style>
