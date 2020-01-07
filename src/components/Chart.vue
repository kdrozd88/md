<template>
<div>

      <v-select :items="listDate"
                  label="Wybierz rodzaj raportu"
            class="input-group--focused"
      />
       <v-btn large color="primary" @click="GenericMeth">Generuj</v-btn>

  <div class="hello" ref="chartdiv"/>
  
  </div>
</template>

<script>
import * as am4core from "@amcharts/amcharts4/core";
import * as am4charts from "@amcharts/amcharts4/charts";
import am4themes_animated from "@amcharts/amcharts4/themes/animated";
import axios from 'axios'

am4core.useTheme(am4themes_animated);


export default {
  name: 'Chart',
    data() {
    return {
      info: null,
      listDate :[
        
          {
            text: "Godzina",
          },
          {
            text: "Dzien",
          },
      ],
      data: [
    {
        "hour": 1,
        "count": 1399
    },
    {
        "hour": 2,
        "count": 178
    },
    {
        "hour": 3,
        "count": 742
    },
    {
        "hour": 4,
        "count": 400
    },
    {
        "hour": 5,
        "count": 827
    },
    {
        "hour": 6,
        "count": 113
    },
    {
        "hour": 7,
        "count": 57
    },
    {
        "hour": 8,
        "count": 2208
    },
    {
        "hour": 9,
        "count": 308
    },
    {
        "hour": 10,
        "count": 172
    },
    {
        "hour": 13,
        "count": 2416
    },
    {
        "hour": 14,
        "count": 1422
    },
    {
        "hour": 15,
        "count": 286
    },
    {
        "hour": 16,
        "count": 18
    },
    {
        "hour": 17,
        "count": 7
    }
]
    }
  },
  methods: {
    GenericMeth () {
    axios.get('https://192.168.42.1:8181')
      .then(response => (this.data = response))
      console.log(this.info)
      this.createChart()
  },
  createChart () {
  let chart = am4core.create(this.$refs.chartdiv, am4charts.XYChart);
    console.log(this.data)
    chart.paddingRight = 20;

    chart.data = this.data;

    var categoryAxis = chart.xAxes.push(new am4charts.CategoryAxis());
    categoryAxis.dataFields.category = "hour";
    categoryAxis.title.text = "Godzinowe nateżenie ruchu";
    categoryAxis.renderer.grid.template.location = 0;
    categoryAxis.renderer.minGridDistance = 20;

    let valueAxis = chart.yAxes.push(new am4charts.ValueAxis());
    valueAxis.tooltip.disabled = true;
    valueAxis.renderer.minWidth = 25;

    var series = chart.series.push(new am4charts.LineSeries());
    series.dataFields.valueY = "count";
    series.dataFields.categoryX = "hour"  ;
  

    series.tooltipText = "{valueY}";
    chart.cursor = new am4charts.XYCursor();

    let scrollbarX = new am4charts.XYChartScrollbar();
    scrollbarX.series.push(series);
    chart.scrollbarX = scrollbarX;

    this.chart = chart;
  }
  },
   mounted() {
    
  },
  beforeDestroy() {
    if (this.chart) {
      this.chart.dispose();
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.hello {
  width: 100%;
  height: 600px;
}
</style>
