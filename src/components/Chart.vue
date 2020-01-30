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
              item-text="text"
              item-value="val"
              filled
              chips
              label="Wybierz obiekty na liście :"
              multiple
            ></v-select>
    <v-container>
      <v-row justify="space-around">
          <v-text-field v-model="value1" v-mask="'##'" type="number" label="Ilosc pixeli"></v-text-field>
          <v-text-field v-model="value" v-mask="mask" type="number" label="prawdopodobienstwo"></v-text-field>
      </v-row>
    </v-container>
    <v-btn large color="primary" @click="createChart">Generuj</v-btn>

  <div class="chartCl" ref="chartdiv"/>
  
  </div>
</template>

<script>
import * as am4core from "@amcharts/amcharts4/core";
import * as am4charts from "@amcharts/amcharts4/charts";
import am4themes_animated from "@amcharts/amcharts4/themes/animated";
import axios from 'axios'
import mask  from 'vue-the-mask'


am4core.useTheme(am4themes_animated);

export default {
  name: 'Chart',
   directives: {
      mask
    },
    data: vm => ({
      
      mask: '##',
      value: '',
      value1: '',
      ListOfObject : [
        
          {
            text: "osoba",
            val : "person"
          },
          {
            text: "rower",
            val : "bicycle"
          },
          {
            text: "samochod",
            val : "car"
          },
          {
            text: "motor",
            val : "motorbike"
          },
          {
            text: "samolot",
            val : "aeroplane"
          },
          {
            text: "autobus",
            val : "bus"
          },
          {
            text: "pocaig",
            val : "train"
          },
          {
            text: "ciezarowka",
            val : "truck"
          },
          {
            text: "lodz",
            val : "boat"
          },
          {
            text: "sygnalizacja swietlna",
            val : "traffic_light"
          },
          {
            text: "hydrant",
            val : "fire_hydrant"
          },
          {
            text: "znak stopu",
            val : "stop_sign"
          },
          {
            text: "parkomat",
            val : "parking_meter"
          },
          {
            text: "lawka",
            val : "bench"
          },
          {
            text: "ptak",
            val : "bird"
          },
          {
            text: "kot",
            val : "cat"
          },
          {
            text: "pies",
            val : "dog"
          },
          {
            text: "kon",
            val : "horse"
          },
          {
            text: "owca",
            val : "sheep"
          },
          {
            text: "krowa",
            val : "cow"
          },
          {
            text: "slon",
            val : "elephant"
          },
          {
            text: "niedzwiedz",
            val : "bear"
          },
          {
            text: "zebra",
            val : "zebra"
          },
          {
            text: "zyrafa",
            val : "giraffe"
          },
          {
            text: "plecak",
            val : "backpack"
          },
          {
            text: "parasolka",
            val : "umbrella"
          },
          {
            text: "torebka",
            val : "handbag"
          },
          {
            text: "krawat",
            val : "tie"
          },
          {
            text: "walizka",
            val : "suitcase"
          },
          {
            text: "frisbee",
            val : "frisbee"
          },
          {
            text: "narty",
            val : "skis"
          },
          {
            text: "snowboard",
            val : "snowboard"
          },
          {
            text: "pilka",
            val : "sports_ball"
          },
          {
            text: "latawiec",
            val : "kite"
          },
          {
            text: "pilka do koszykowki",
            val : "baseball_bat"
          },
          {
            text: "rekawica",
            val : "baseball_glove"
          },
          {
            text: "skateboard",
            val : "skateboard"
          },
          {
            text: "deska serfingowa",
            val : "surfboard"
          },
          {
            text: "rakieta tenisowa",
            val : "tennis_racket"
          },
          {
            text: "butelka",
            val : "bottle"
          },
          {
            text: "kieliszek",
            val : "wine_glass"
          },
          {
            text: "kubek",
            val : "cup"
          },
          {
            text: "widelec",
            val : "fork"
          },
          {
            text: "noz",
            val : "knife"
          },
          {
            text: "lyzeczka",
            val : "spoon"
          },
          {
            text: "miseczka",
            val : "bowl"
          },
          {
            text: "banan",
            val : "banana"
          },
          {
            text: "jablko",
            val : "apple"
          },
          {
            text: "kanapka",
            val : "sandwich"
          },
          {
            text: "pomarancza",
            val : "orange"
          },
          {
            text: "brokul",
            val : "broccoli"
          },
          {
            text: "marchewka",
            val : "carrot"
          },
          {
            text: "hotdog",
            val : "hot_dog"
          },
          {
            text: "pizza",
            val : "pizza"
          },
          {
            text: "donut",
            val : "donut"
          },
          {
            text: "ciastko",
            val : "cake"
          },
          {
            text: "krzeslo",
            val : "chair"
          },
          {
            text: "sofa",
            val : "sofa"
          },
          {
            text: "kwiatek",
            val : "pottedplant"
          },
          {
            text: "lozko",
            val : "bed"
          },
          {
            text: "stol",
            val : "diningtable"
          },
          {
            text: "toaleta",
            val : "toilet"
          },
          {
            text: "tvmonitor",
            val : "tvmonitor"
          },
          {
            text: "laptop",
            val : "laptop"
          },
          {
            text: "mysz",
            val : "mouse"
          },
          {
            text: "pilot",
            val : "remote"
          },
          {
            text: "klawiatura",
            val : "keyboard"
          },
          {
            text: "komorka",
            val : "cell_phone"
          },
          {
            text: "mikrofalowka",
            val : "microwave"
          },
          {
            text: "piec",
            val : "oven"
          },
          {
            text: "toster",
            val : "toaster"
          },
          {
            text: "zlew",
            val : "sink"
          },
          {
            text: "lodowka",
            val : "refrigerator"
          },
          {
            text: "ksiazka",
            val : "book"
          },
          {
            text: "zegar",
            val : "clock"
          },
          {
            text: "wazon",
            val : "vase"
          },
          {
            text: "scyzoryk",
            val : "scissors"
          },
          {
            text: "pluszak",
            val : "teddy_bear"
          },
          {
            text: "szuszarka",
            val : "hair_drier"
          },
          {
            text: "szczoteczka",
            val : "toothbrush"
          }
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
.chartCl {
  width: 100%;
  height: 700px;
}
</style>
