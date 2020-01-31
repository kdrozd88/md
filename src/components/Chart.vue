<template>
<div>
  <v-container>
    <v-row justify="space-around">
      <v-date-picker v-model="dateFrom" :show-current="true"></v-date-picker>
      <v-date-picker v-model="dateTo" :show-current="true"></v-date-picker>
    </v-row>
    </v-container>
    <v-container>
      <v-select
        v-model="selected"
        :items="ListOfObject"
        item-text="text"
        item-value="val"
        filled
        chips
        label="Wybierz obiekty na liście :"
      ></v-select>
      <v-select
          v-model="selectedProbability"
          :items="ListOfOds"
          item-text="val"
          filled
          chips
          label="Wybierz prawdopodobieństwo :"
        ></v-select>
        <v-row justify="start">
        <v-col
          cols="3">
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
            text: "Samochód",
            val : "Car"
          },
          {
            text: "Motor",
            val : "Motorbike"
          },
          {
            text: "Samolot",
            val : "Aeroplane"
          },
          {
            text: "Autobus",
            val : "Bus"
          },
          {
            text: "Pociąg",
            val : "Train"
          },
          {
            text: "Ciężarowka",
            val : "Truck"
          },
          {
            text: "Łódź",
            val : "Boat"
          },
          {
            text: "Sygnalizacja świetlna",
            val : "Traffic_light"
          },
          {
            text: "Hydrant",
            val : "Fire_hydrant"
          },
          {
            text: "Znak stopu",
            val : "Stop_sign"
          },
          {
            text: "Parkomat",
            val : "Parking_meter"
          },
          {
            text: "Ławka",
            val : "Bench"
          },
          {
            text: "Ptak",
            val : "Bird"
          },
          {
            text: "Kot",
            val : "Cat"
          },
          {
            text: "Pies",
            val : "Dog"
          },
          {
            text: "Koń",
            val : "Horse"
          },
          {
            text: "Owca",
            val : "Sheep"
          },
          {
            text: "Krowa",
            val : "Cow"
          },
          {
            text: "Słoń",
            val : "Elephant"
          },
          {
            text: "Niedzwiedź",
            val : "Bear"
          },
          {
            text: "Zebra",
            val : "Zebra"
          },
          {
            text: "Żyrafa",
            val : "Giraffe"
          },
          {
            text: "Plecak",
            val : "Backpack"
          },
          {
            text: "Parasolka",
            val : "Umbrella"
          },
          {
            text: "Torebka",
            val : "Handbag"
          },
          {
            text: "Krawat",
            val : "Tie"
          },
          {
            text: "Walizka",
            val : "Suitcase"
          },
          {
            text: "Frisbee",
            val : "Frisbee"
          },
          {
            text: "Narty",
            val : "Skis"
          },
          {
            text: "Snowboard",
            val : "Snowboard"
          },
          {
            text: "Piłka",
            val : "Sports_ball"
          },
          {
            text: "Latawiec",
            val : "Kite"
          },
          {
            text: "Piłka do koszykówki",
            val : "Baseball_bat"
          },
          {
            text: "Rękawica",
            val : "Baseball_glove"
          },
          {
            text: "Skateboard",
            val : "Skateboard"
          },
          {
            text: "Deska serfingowa",
            val : "Surfboard"
          },
          {
            text: "Rakieta tenisowa",
            val : "Tennis_racket"
          },
          {
            text: "Butelka",
            val : "Bottle"
          },
          {
            text: "Kieliszek",
            val : "Wine_glass"
          },
          {
            text: "Kubek",
            val : "Cup"
          },
          {
            text: "Widelec",
            val : "Fork"
          },
          {
            text: "Nóż",
            val : "Knife"
          },
          {
            text: "Łyżeczka",
            val : "Spoon"
          },
          {
            text: "Miseczka",
            val : "Bowl"
          },
          {
            text: "Banan",
            val : "Banana"
          },
          {
            text: "Jabłko",
            val : "Apple"
          },
          {
            text: "Kanapka",
            val : "Sandwich"
          },
          {
            text: "Pomarańcza",
            val : "Orange"
          },
          {
            text: "Brokuł",
            val : "Broccoli"
          },
          {
            text: "Marchewka",
            val : "Carrot"
          },
          {
            text: "Hotdog",
            val : "Hot_dog"
          },
          {
            text: "Pizza",
            val : "Pizza"
          },
          {
            text: "Donut",
            val : "Donut"
          },
          {
            text: "Ciastko",
            val : "Cake"
          },
          {
            text: "Krzesło",
            val : "Chair"
          },
          {
            text: "Sofa",
            val : "Sofa"
          },
          {
            text: "Kwiatek",
            val : "Pottedplant"
          },
          {
            text: "Łóżko",
            val : "Bed"
          },
          {
            text: "Stół",
            val : "Diningtable"
          },
          {
            text: "Toaleta",
            val : "Toilet"
          },
          {
            text: "Tvmonitor",
            val : "Tvmonitor"
          },
          {
            text: "Laptop",
            val : "Laptop"
          },
          {
            text: "Mysz",
            val : "Mouse"
          },
          {
            text: "Pilot",
            val : "Remote"
          },
          {
            text: "Klawiatura",
            val : "Keyboard"
          },
          {
            text: "Komórka",
            val : "Cell_phone"
          },
          {
            text: "Mikrofalówka",
            val : "Microwave"
          },
          {
            text: "Piec",
            val : "Oven"
          },
          {
            text: "Toster",
            val : "Toaster"
          },
          {
            text: "Zlew",
            val : "Sink"
          },
          {
            text: "Lodówka",
            val : "Refrigerator"
          },
          {
            text: "Książka",
            val : "Book"
          },
          {
            text: "Zegar",
            val : "Clock"
          },
          {
            text: "Wazon",
            val : "Vase"
          },
          {
            text: "Scyzoryk",
            val : "Scissors"
          },
          {
            text: "Pluszak",
            val : "Teddy_bear"
          },
          {
            text: "Szuszarka",
            val : "Hair_drier"
          },
          {
            text: "Szczoteczka",
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

    return await axios.get('http://rest-api/reports/getMotionByHours',
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
