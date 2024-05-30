<template>
  <div class="container">
    <h1>Change Calculator</h1>
    <div>
      <label for="bill">Bill Amount (₱):</label>
      <input v-model.number="bill" id="bill" type="number" />
    </div>
    <div>
      <label for="owed">Amount Owed (₱):</label>
      <input v-model.number="owed" id="owed" type="number" />
    </div>
    <button @click="calculateChange">Calculate Change</button>
    <div v-if="change">
      <h2>Change</h2>
      <ul>
        <li v-for="(count, denomination) in change" :key="denomination">
          ₱{{ denomination }}: {{ count }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      bill: 0,
      owed: 0,
      change: null,
    };
  },

  computed: {
    sortedItems() {
    const sorted = [...this.change];
      return this.sorted.sort((a, b) => {
        return this.getKey(a) - this.getKey(b);
      });
    }
  },

  computed: {
    sortedChange() {
      if (!this.change) return [];
      return Object.entries(this.change)
        .sort((a, b) => b[0] - a[0])
        .map(entry => ({ denomination: entry[0], count: entry[1] }));
    }
  },

  methods: {
    calculateChange() {
      if (this.owed > this.bill) {
        alert("Amount owed cannot be greater than the bill.");
        return;
      } else if (this.bill === 0) {
        alert("Enter a valid number");
        return
      }

      let changeAmount = this.bill - this.owed;
      const denominations = [1000, 500, 200, 100, 50, 20, 10, 5, 1];
      const changeDict = {};

      denominations.forEach((denomination) => {
        const count = Math.floor(changeAmount / denomination);
        if (count > 0) {
          changeDict[denomination] = count;
          changeAmount -= denomination * count;
        }
      });
      this.change = changeDict;
    },
  }
};
</script>

<style scoped>


.container {
  display: flex;
  flex-direction: column;
  border: 2px solid maroon;
  border-radius: 8px;
}

div {
  margin: 20px;
}

label {
  margin-right: 10px;
}

input {
  margin-bottom: 20px;
  border-radius: 8px;
}

button {
  margin-top: 20px;
  border-radius: 8px;
}
</style>
