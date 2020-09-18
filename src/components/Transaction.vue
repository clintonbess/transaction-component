<template>
  <div class="transaction">
    <div class="transaction-item" v-on:click="onClick">
      <div>{{standardDate}}</div>
      <div>{{transaction.Description}}</div>
      <div>{{transaction.TransactionTypeId}}</div>
      <div>{{status}}</div>
      <div>{{amount}}</div>
      <div>{{startingBalance}}</div>
      <div>{{endingBalance}}</div>
    </div>
    <div class="transaction-details" v-bind:class="{ open: isOpen }">
      <div class="details-row">
        <div>Type</div>
        <div>{{transaction.TransactionTypeId}}</div>
      </div>
      <div class="details-row">
        <div>Account Number</div>
        <div>{{transaction.AccountNumber}}</div>
      </div>
      <div class="details-row">
        <div>Description</div>
        <div>{{transaction.Description}}</div>
      </div>
      <div class="details-row">
        <div>Merchant Name</div>
        <div>{{transaction.MerchantName}}</div>
      </div>
      <div class="details-row">
        <div>Merchant Id</div>
        <div>{{transaction.MerchantId}}</div>
      </div>
      <div class="details-row">
        <div>Date</div>
        <div>{{utcDate}}</div>
      </div>      
    </div>
  </div>
</template>
<script>
export default {
  name: 'Transaction',
  props: ['transaction'],
  data () {
    return {
      isOpen: false
    }
  },
  computed: {
    standardDate () {
      const transactionDate = new Date(this.transaction.TransactionDate)
      return transactionDate.toLocaleDateString()
    },
    utcDate () {
      const transactionDate = new Date(this.transaction.TransactionDate)
      return transactionDate.toUTCString()
    },
    startingBalance () {
      const balance = this.transaction.StartingBalance.toFixed(2)
      return `$${balance}`
    },
    endingBalance () {
      const balance = this.transaction.EndingBalance.toFixed(2)
      return `$${balance}`
    },
    amount () {
      const transactionAmount = this.transaction.Amount.toFixed(2)
      return `-$${transactionAmount}`
    },
    status () {
      const isBilled = this.transaction.Billed
      if (isBilled === true) {
        return 'Charged'
      }
      return 'Pending'
    }
  },
  methods: {
    onClick () {
      if (this.isOpen === true) {
        return this.isOpen = false
      }
      return this.isOpen = true
    }
  }
}
</script>

<style>

.transaction-item {
  cursor: pointer;
  display: flex;
  flex-direction: row;
  font-size: 11px;
  justify-content: space-evenly;
  padding: 10px;
  text-align: center;
}

.details-row {
  display: flex;
  flex-direction: row;
  justify-content: center;
  margin: 10px;
}

.details-row div:first-of-type {
  font-weight: bold;
  width: 40%;
}

.details-row div:nth-of-type(2) {
  width: 60%;
}

.transaction-item > div {
  width: 15%;
}

.transaction {
  background-color:#b9b9b959;
  border: solid 1px black;
  padding: 10px;
  text-align: left;
  transition: background-color 0.2s;
  width: 100%;
}

.transaction:hover {
  background-color: white;
}

.transaction-details {
  background-color:#b9b9b9;
  border-radius: 4px;
  font-size: 12px;
  max-height: 0;
  overflow: hidden;
  transition: all 0.1s ease-in-out;
  width: 80%;
}

.transaction-details.open {
  max-height: 500px;
  padding: 20px 40px;
  width: 100%;
}

</style>
