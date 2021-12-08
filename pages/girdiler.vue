<template>
  <div>
    <div class="container">
      <div class="row">
        <div class="col-sm-6">
          <b-button variant="success" v-b-modal.modal-prevent-income
            >Add Income</b-button
          >
        </div>
        <div class="col-sm">
          <b-button variant="danger" v-b-modal.modal-prevent-expense
            >Add Expense</b-button
          >
        </div>
        <div class="col-sm">
          <b-button variant="info" v-b-modal.modal-prevent-expense
            >Total:{{ total }}</b-button
          >
        </div>
        <div class="col-sm"></div>
      </div>
    </div>
    <b-container class="bv-example-row"><b-row> </b-row> </b-container>
    <div class="mt-5"></div>

    <b-modal
      id="modal-prevent-income"
      ref="modal"
      title="Add Income"
      @show="resetModal"
      @hidden="resetModal"
      @ok="handleOk"
    >
      <form ref="form" @submit.stop.prevent="handleSubmit">
        <b-form-group
          label="Income"
          label-for="name-input"
          invalid-feedback="required"
          :state="incomeState"
        >
          <b-form-input
            id="name-input"
            v-model="income"
            :state="incomeState"
            required
          ></b-form-input>
          Income Type
          <select
            class="form-control"
            v-model="category"
            placeholder="Category"
          >
            <option v-for="m in incomeTypes" v-bind:value="m.name" :key="m">
              {{ m.name }}
            </option>
          </select>
          Description
          <b-form-input
            id="name-input"
            v-model="description"
            :state="incomeState"
            required
          ></b-form-input>
        </b-form-group>
      </form>
    </b-modal>
    <!-- //buradan sonrası Gider -->

    <b-modal
      id="modal-prevent-expense"
      ref="modal"
      title="Add Expense"
      @show="resetModal"
      @hidden="resetModal"
      @ok="handleOk"
    >
      <form ref="form" @submit.stop.prevent="handleSubmit">
        <b-form-group
          label="Gider"
          label-for="name-input"
          invalid-feedback="required"
          :state="expenseState"
        >
          <b-form-input
            id="name-input"
            v-model="expense"
            :state="expenseState"
            required
            type="number"
          ></b-form-input>
          Expense Type
          <select
            class="form-control"
            v-model="category"
            placeholder="Category"
          >
            <option v-for="m in expenseTypes" v-bind:value="m.name" :key="m">
              {{ m.name }}
            </option>
          </select>
          Description
          <b-form-input
            id="name-input"
            v-model="description"
            :state="expenseState"
            required
          ></b-form-input>
        </b-form-group>
      </form>
    </b-modal>
    <b-table-simple responsive>
      <b-thead>
        <b-tr>
          <b-th></b-th>
          <b-th>Gelir</b-th>
          <b-th>Gider</b-th>
          <b-th>Açıklama</b-th>
          <b-th>Tür</b-th>
        </b-tr>
      </b-thead>
      <b-tbody>
        <b-tr v-for="income in submittedIncomes" :key="income.id">
          <b-td>100{{ income.id }}</b-td>
          <b-td class="income">{{ income.income }}</b-td>
          <b-td class="expense">{{ income.expense }}</b-td>
          <b-td>{{ income.description }}</b-td>
          <b-td>{{ income.category }}</b-td>
        </b-tr>
      </b-tbody>
    </b-table-simple>
  </div>
</template>


<script>
export default {
  data() {
    return {
      income: 0,
      incomeState: null,
      submittedIncomes: null,
      expense: null,
      expenseState: null,
      total: null,
      description: null,
      incomeTypes: [
        { id: null, name: 'Seçiniz' },
        { id: 1, name: 'Tahsilat' },
        { id: 2, name: 'Faiz' },
        { id: 3, name: 'Çek Tahsilatı' },
      ],
      expenseTypes: [
        { id: null, name: 'Seçiniz' },
        { id: 1, name: 'Maaş Ödemesi' },
        { id: 2, name: 'Yakıt' },
        { id: 3, name: 'Kira' },
      ],
      category: null,
    }
  },
  beforeMount() {
    this.getData()
  },

  methods: {
    getData() {
      this.$axios
        .get('https://6175abc003178d00173da971.mockapi.io/muhasebe/')
        .then((response) => {
          ;(this.submittedIncomes = response.data), (this.total = 'selam')
          var t = 0

          Object.values(this.submittedIncomes).map(
            (d) => (t += parseInt(d.income) + -parseInt(d.expense))
          )
          this.total = t

          var t = 0

          Object.values(this.submittedIncomes).map(
            (d) => (t += parseInt(d.income))
          )
          this.income = t

          var t = 0

          this.series[0] = this.income
          this.series[1] = this.expense
        })
    },
    checkFormValidity() {
      const valid = this.$refs.form.checkValidity()
      this.incomeState = valid
      return valid
    },
    resetModal() {
      this.income = ''
      this.incomeState = null
      this.description = null
      this.category = null
    },
    handleOk(bvModalEvt) {
      bvModalEvt.preventDefault()
      this.handleSubmit()
    },
    handleSubmit() {
      if (!this.checkFormValidity()) {
        return
      }

      this.$axios
        .post('https://6175abc003178d00173da971.mockapi.io/muhasebe/', {
          income: Number(this.income),
          expense: Number(this.expense),
          total: this.total,
          description: this.description,
          category: this.category,
        })
        .then(() => {
          this.getData()
          this.income = null
          this.expense = null
          this.total = null
          this.description = null
          this.category = 'Seçiniz'
        })
      this.$nextTick(() => {
        this.$bvModal.hide('modal-prevent-closing')
      })
    },
    getTotal: function () {
      let total = []

      Object.entries(this.total).forEach(([key, val]) => {
        total.push(val.income) // the value of the current key.
      })

      return total.reduce(function (total, num) {
        return total + num
      }, 0)
    },
  },
}
</script>

<style>
.expense {
  color: black;
  background-color: darkred;
}
.income {
  color: aliceblue;
  background-color: green;
}
</style>