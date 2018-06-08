# Official Documents
[vue-chartjs](https://github.com/apertureless/vue-chartjs)

# Example

1. components

```
import { Bar } from 'vue-chartjs'

export default {
  extends: Bar,
  props: ['data', 'options']  ,
  mounted() {
    this.renderChart(this.data, this.options)
  }
}
```

2. Used Vue

```
<template>
  <div>
    <bar-chart :data="lineData" :options="ratio"></bar-chart>
  </div>
</template>

<script>
import BarChart from '~/components/bar-chart'

export default {
  data:() => {(
    lineData: {
      labels: ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'],
      datasets: [
        {
          label: '2017년도 판매내역',
          backgroundColor: '#f87979',
          data: [410, 220, 312, 339, 355, 400, 439, 480, 521, 533, 562, 611]
        }
      ]
    },
    options: { maintainAspectRatio: false }
  )}
}

</script>
```