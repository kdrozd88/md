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
            ></v-select>
    <v-container>
      <v-row justify="space-around">
            <v-col cols="6">    
              <v-select
                v-model="selectedProbability"
                :items="ListOfOds"
                item-text="val"
                filled
                chips
                label="Wybierz obiekty na liście :"
              ></v-select>
          </v-col>
          <v-col
              cols="6"
            >              
              <v-text-field v-model="pixels" v-mask="'##'" type="number" label="Ilosc pixeli"></v-text-field>
        </v-col>
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
      pixels: '',
      ListOfOds : [
         {
            val : "Small"
          }, 
          {
            val : "Medium"
          }, 
          {
            val : "Big"
          },
      ],
      ListOfObject : [
        
          {
            text: "Osoba",
            val : "Person"
          },
          {
            text: "Rower",
            val : "Bicycle"
          },
          {
            text: "samochod",
            val : "Car"
          },
          {
            text: "motor",
            val : "Motorbike"
          },
          {
            text: "samolot",
            val : "Aeroplane"
          },
          {
            text: "autobus",
            val : "Bus"
          },
          {
            text: "pocaig",
            val : "Train"
          },
          {
            text: "ciezarowka",
            val : "Truck"
          },
          {
            text: "lodz",
            val : "Boat"
          },
          {
            text: "sygnalizacja swietlna",
            val : "Traffic_light"
          },
          {
            text: "hydrant",
            val : "Fire_hydrant"
          },
          {
            text: "znak stopu",
            val : "Stop_sign"
          },
          {
            text: "parkomat",
            val : "Parking_meter"
          },
          {
            text: "lawka",
            val : "Bench"
          },
          {
            text: "ptak",
            val : "Bird"
          },
          {
            text: "kot",
            val : "Cat"
          },
          {
            text: "pies",
            val : "Dog"
          },
          {
            text: "kon",
            val : "Horse"
          },
          {
            text: "owca",
            val : "Sheep"
          },
          {
            text: "krowa",
            val : "Cow"
          },
          {
            text: "slon",
            val : "Elephant"
          },
          {
            text: "niedzwiedz",
            val : "Bear"
          },
          {
            text: "zebra",
            val : "Zebra"
          },
          {
            text: "zyrafa",
            val : "Giraffe"
          },
          {
            text: "plecak",
            val : "Backpack"
          },
          {
            text: "parasolka",
            val : "Umbrella"
          },
          {
            text: "torebka",
            val : "Handbag"
          },
          {
            text: "krawat",
            val : "Tie"
          },
          {
            text: "walizka",
            val : "Suitcase"
          },
          {
            text: "frisbee",
            val : "Frisbee"
          },
          {
            text: "narty",
            val : "Skis"
          },
          {
            text: "snowboard",
            val : "Snowboard"
          },
          {
            text: "pilka",
            val : "Sports_ball"
          },
          {
            text: "latawiec",
            val : "Kite"
          },
          {
            text: "pilka do koszykowki",
            val : "Baseball_bat"
          },
          {
            text: "rekawica",
            val : "Baseball_glove"
          },
          {
            text: "skateboard",
            val : "Skateboard"
          },
          {
            text: "deska serfingowa",
            val : "Surfboard"
          },
          {
            text: "rakieta tenisowa",
            val : "Tennis_racket"
          },
          {
            text: "butelka",
            val : "Bottle"
          },
          {
            text: "kieliszek",
            val : "Wine_glass"
          },
          {
            text: "kubek",
            val : "Cup"
          },
          {
            text: "widelec",
            val : "Fork"
          },
          {
            text: "noz",
            val : "Knife"
          },
          {
            text: "lyzeczka",
            val : "Spoon"
          },
          {
            text: "miseczka",
            val : "Bowl"
          },
          {
            text: "banan",
            val : "Banana"
          },
          {
            text: "jablko",
            val : "Apple"
          },
          {
            text: "kanapka",
            val : "Sandwich"
          },
          {
            text: "pomarancza",
            val : "Orange"
          },
          {
            text: "brokul",
            val : "Broccoli"
          },
          {
            text: "marchewka",
            val : "Carrot"
          },
          {
            text: "hotdog",
            val : "Hot_dog"
          },
          {
            text: "pizza",
            val : "Pizza"
          },
          {
            text: "donut",
            val : "Donut"
          },
          {
            text: "ciastko",
            val : "Cake"
          },
          {
            text: "krzeslo",
            val : "Chair"
          },
          {
            text: "sofa",
            val : "Sofa"
          },
          {
            text: "kwiatek",
            val : "Pottedplant"
          },
          {
            text: "lozko",
            val : "Bed"
          },
          {
            text: "stol",
            val : "Diningtable"
          },
          {
            text: "toaleta",
            val : "Toilet"
          },
          {
            text: "tvmonitor",
            val : "Tvmonitor"
          },
          {
            text: "laptop",
            val : "Laptop"
          },
          {
            text: "mysz",
            val : "Mouse"
          },
          {
            text: "pilot",
            val : "Remote"
          },
          {
            text: "klawiatura",
            val : "Keyboard"
          },
          {
            text: "komorka",
            val : "Cell_phone"
          },
          {
            text: "mikrofalowka",
            val : "Microwave"
          },
          {
            text: "piec",
            val : "Oven"
          },
          {
            text: "toster",
            val : "Toaster"
          },
          {
            text: "zlew",
            val : "Sink"
          },
          {
            text: "lodowka",
            val : "Refrigerator"
          },
          {
            text: "ksiazka",
            val : "Book"
          },
          {
            text: "zegar",
            val : "Clock"
          },
          {
            text: "wazon",
            val : "Vase"
          },
          {
            text: "scyzoryk",
            val : "Scissors"
          },
          {
            text: "pluszak",
            val : "Teddy_bear"
          },
          {
            text: "szuszarka",
            val : "Hair_drier"
          },
          {
            text: "szczoteczka",
            val : "Toothbrush"
          }
      ],
    dateFrom: new Date().toISOString().substr(0, 10),
    dateTo: new Date().toISOString().substr(0, 10),
    dateFormatted: vm.formatDate(new Date().toISOString().substr(0, 10)),
    selected : [],
    selectedProbability : [],

}),
  methods: {
  async getData() {
  try {

    return await axios.get('http://192.168.45.103:5002/reports/getMotionByHours',
    {
    params: {
      Begin: this.dateFrom,
      End: this.dateTo,
      ObjectType : this.selected,
      NumberOfPixels : this.pixels,
      Probability : this.selectedProbability
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
