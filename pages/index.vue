<template>
  <div class="container layout">
    <div id="total"><div class="inner-text">Total : {{ total }}</div></div>

    <div id="income"><div class="inner-text">Total Income : {{ income }}</div></div>

    <div id="expense"><div class="inner-text">Total Expensed : {{ expense }}</div></div>
  </div>
</template>

<script>
export default {
  components: {},
  data() {
    return {
      income: 0,
      incomeState: null,
      yakit: null,
      maas: null,
      tahsilat: null,
      faiz: null,
      cektahsilati: null,
      kira: null,
      submittedIncomes: [],
      expense: null,
      total: null,
      iSeries: [this.yakit, this.maas],
      ichartOptions: {
        chart: {
          width: 380,
          type: 'pie',
        },
        labels: ['Yakıt', 'Kira'],
        responsive: [
          {
            breakpoint: 480,
            options: {
              chart: {
                width: 200,
              },
              legend: {
                position: 'bottom',
              },
            },
          },
        ],
      },
      //burdan sonrası gider grafiği

      eSeries: [this.maas, this.yakit, this.kira],
      echartOptions: {
        chart: {
          width: 380,
          type: 'pie',
        },
        labels: ['Gelirler', 'Giderler'],
        responsive: [
          {
            breakpoint: 480,
            options: {
              chart: {
                width: 200,
              },
              legend: {
                position: 'bottom',
              },
            },
          },
        ],
      },
    }
  },
  beforeMount() {
    this.getData()
  },
  computed: {
    getTotal: function () {
      console.log(this.submittedIncomes)
      if (!this.submittedIncomes) return 0
      return Object.values(this.submittedIncomes).reduce(
        (a, b) => b.income + a.income
      )
    },
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

          Object.values(this.submittedIncomes).map(
            (d) => (t += parseInt(d.expense))
          )
          this.expense = t

          this.series[0] = this.income
          this.series[1] = this.expense
        })
    },
  },
}
</script>

<style  scoped>
*{
  justify-content: center;
}
.layout{
  max-width: 500rem;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-gap: 5rem;
}


#income {
  border: 7px solid rgb(4, 39, 4);
  font-size: 25px;
  box-shadow: 2px 2px 2px 2px rgb(76, 212, 76);
  font-weight: bolder;
  text-align: center;
  border-radius: 12px;
  height: 600px;
}
#total {
  border: 7px solid rgb(7, 7, 37);
  font-size: 25px;
  box-shadow: 2px 2px 2px 2px rgb(113, 113, 204);
  text-align: center;
  font-weight: bolder;
  border-radius: 12px;
}

#expense {
  border: 7px solid rgb(110, 9, 9);
  font-size: 25px;
  box-shadow: 2px 2px 2px 2px rgba(218, 132, 132, 0.863);
  text-align: center;
  font-weight: bolder;
  border-radius: 12px;
}

.inner-text{
padding-top: 200px;
font-size: 4rem;
font-style: inherit;
font-family:Verdana, Geneva, Tahoma, sans-serif
}
</style>


