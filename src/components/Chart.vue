<template>
<div>
  <v-container>
    <v-row justify="space-around">
      <v-date-picker v-model="dateFrom" :show-current="true"></v-date-picker>
      <v-date-picker v-model="dateTo" :show-current="true"></v-date-picker>
    </v-row>
    </v-container>
            <v-select
              v-model="selected"
              :items="ListOfObject"
              filled
              chips
              label="Chips"
              multiple
            ></v-select>
    <v-btn large color="primary" @click="createChart">Generuj</v-btn>

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
    data: vm => ({
      ListOfObject :[
        
          {
            text: "person",
          },
          {
            text: "bicycle",
          },
          {
            text: "car",
          },
          {
            text: "motorbike",
          },
          {
            text: "aeroplane",
          },
          {
            text: "bus",
          },
          {
            text: "train",
          },
          {
            text: "truck",
          },
          {
            text: "boat",
          },
          {
            text: "traffic_light",
          },
          {
            text: "fire_hydrant",
          },
          {
            text: "stop_sign",
          },
          {
            text: "parking_meter",
          },
          {
            text: "bench",
          },
          {
            text: "bird",
          },
          {
            text: "cat",
          },
          {
            text: "dog",
          },
          {
            text: "horse",
          },
             {
            text: "sheep",
          },
          {
            text: "cow",
          },
          {
            text: "elephant",
          },
          {
            text: "bear",
          },
          {
            text: "zebra",
          },
          {
            text: "giraffe",
          },
          {
            text: "backpack",
          },
          {
            text: "umbrella",
          },
          {
            text: "Chair",
          },
             {
            text: "person",
          },
          {
            text: "bicycle",
          },
          {
            text: "car",
          },
          {
            text: "motorbike",
          },
          {
            text: "aeroplane",
          },
          {
            text: "bus",
          },
          {
            text: "train",
          },
          {
            text: "truck",
          },
          {
            text: "boat",
          },
          {
            text: "traffic_light",
          },
          {
            text: "fire_hydrant",
          },
          {
            text: "stop_sign",
          },
          {
            text: "parking_meter",
          },
          {
            text: "bench",
          },
          {
            text: "bird",
          },
          {
            text: "cat",
          },
          {
            text: "dog",
          },
          {
            text: "horse",
          },
             {
            text: "sheep",
          },
          {
            text: "cow",
          },
          {
            text: "elephant",
          },
          {
            text: "bear",
          },
          {
            text: "zebra",
          },
          {
            text: "giraffe",
          },
          {
            text: "backpack",
          },
          {
            text: "umbrella",
          },
          {
            text: "Chair",
          },
          {
            text: "handbag",
          },
          {
            text: "tie",
          },
          {
            text: "suitcase",
          },
          {
            text: "frisbee",
          },
          {
            text: "skis",
          },
          {
            text: "snowboard",
          },
          {
            text: "sports_ball",
          },
          {
            text: "kite",
          },
          {
            text: "baseball_bat",
          },
          {
            text: "baseball_glove",
          },
          {
            text: "skateboard",
          },
          {
            text: "surfboard",
          },
          {
            text: "tennis_racket",
          },
          {
            text: "bottle",
          },
          {
            text: "wine_glass",
          },
          {
            text: "cup",
          },
          {
            text: "fork",
          },
          {
            text: "knife",
          },
          {
            text: "spoon",
          },
          {
            text: "bowl",
          },
          {
            text: "banana",
          },
          {
            text: "apple",
          },
          {
            text: "zebra",
          },
          {
            text: "giraffe",
          },
          {
            text: "backpack",
          },
          {
            text: "umbrella",
          },
          {
            text: "Chair",
          },
      ],
    dateFrom: new Date().toISOString().substr(0, 10),
    dateTo: new Date().toISOString().substr(0, 10),
    dateFormatted: vm.formatDate(new Date().toISOString().substr(0, 10)),
    selected : [],
}),
  methods: {
  async getData() {
  try {
    console.log(this.selected)

    return await axios.get('http://192.168.45.103:5002/reports/getMotionByHours',
    {
    params: {
      Begin: this.dateFrom,
      End: this.dateTo,
      ObjectType : this.selected,
      NumberOfPixels : 1000,
    }
    })
  } catch (error) {
    console.error(error);
  }
  },
  async createChart () {

   let tmp = await this.getData();
      this.genRap(tmp.data)
  },
  genRap (data) {
        
    let chart = am4core.create(this.$refs.chartdiv, am4charts.XYChart);
    chart.paddingRight = 20;
    chart.data = data;

    var categoryAxis = chart.xAxes.push(new am4charts.CategoryAxis());
    categoryAxis.dataFields.category = "date";
    categoryAxis.title.text = "Godzinowe nateżenie ruchu";
    categoryAxis.renderer.grid.template.location = 0;
    categoryAxis.renderer.minGridDistance = 20;

    let valueAxis = chart.yAxes.push(new am4charts.ValueAxis());
    valueAxis.tooltip.disabled = true;
    valueAxis.renderer.minWidth = 25;

    var series = chart.series.push(new am4charts.LineSeries());
    series.dataFields.valueY = "count";
    series.dataFields.categoryX = "date"  ;
  

    series.tooltipText = "{valueY}";
    chart.cursor = new am4charts.XYCursor();

    let scrollbarX = new am4charts.XYChartScrollbar();
    scrollbarX.series.push(series);
    chart.scrollbarX = scrollbarX;

    this.chart = chart;
  },
  formatDate (date) {
    if (!date) return null
      const [year, month, day] = date.split('-')
    return `${month}/${day}/${year}`
  },
  },
  beforeDestroy() {
    if (this.chart) {
      this.chart.dispose();
    }
  }
}
</script>

<style scoped>
.hello {
  width: 100%;
  height: 600px;
}
</style>
