<template>
  <v-container>
    <!-- FOR POSITIVE CASES -->
    <v-row class="text-center">
      <v-col cols="12" v-if="arrPositive.length > 0">
        <v-card class="mx-auto" mt-10 mb-6 pr-8 pl-8 pb-8 pt-4>
          <h1 class="text-uppercase">Covid 19 data visualization</h1>
          <v-divider></v-divider>
          <v-list-item three-line>
            <v-list-item-content>
              <v-list-item-title class="headline mb-1">Positive Corona Cases</v-list-item-title>
              <v-card elevation="11" class="pt-10">
                <LineChart :chartData="arrPositive" :options="chartOptions" label="Positive Cases"></LineChart>
              </v-card>
            </v-list-item-content>
          </v-list-item>
        </v-card>
      </v-col>
    </v-row>
    <!-- FOR HOSPITALIZE -->
    <v-row class="text-center">
      <v-col cols="12" v-if="arrHospitalized.length > 0">
        <v-card class="mx-auto" mt-10 mb-6 pr-8 pl-8 pb-8 pt-4>
          <v-list-item three-line>
            <v-list-item-content>
              <v-list-item-title class="headline mb-1">Hospitalized Corona Patients</v-list-item-title>
              <v-card elevation="11" class="pt-10">
                <LineChart
                  :chartData="arrHospitalized"
                  :options="chartOptions"
                  label="Hospitalize Cases"
                ></LineChart>
              </v-card>
            </v-list-item-content>
          </v-list-item>
        </v-card>
      </v-col>
    </v-row>
    <!-- FOR DEATH -->
    <v-row class="text-center">
      <v-col cols="12" v-if="arrDeaths.length > 0">
        <v-card class="mx-auto" mt-10 mb-6 pr-8 pl-8 pb-8 pt-4>
          <v-list-item three-line>
            <v-list-item-content>
              <v-list-item-title class="headline mb-1 red--text">Deaths Corona Patients</v-list-item-title>
              <v-card elevation="11" class="pt-10">
                <LineChart :chartData="arrDeaths" :options="chartOptions" label="Death Cases"></LineChart>
              </v-card>
            </v-list-item-content>
          </v-list-item>
        </v-card>
      </v-col>
    </v-row>
    <!-- FOR VENTILATOR -->
    <v-row class="text-center">
      <v-col cols="12" v-if="arrOnVentilators.length > 0">
        <v-card class="mx-auto" mt-10 mb-6 pr-8 pl-8 pb-8 pt-4>
          <v-list-item three-line>
            <v-list-item-content>
              <v-list-item-title class="headline mb-1 red--text">Corona Patients ON Ventilators</v-list-item-title>
              <v-card elevation="11" class="pt-10">
                <LineChart
                  :chartData="arrOnVentilators"
                  :options="chartOptions"
                  label=" On Ventilators Cases"
                ></LineChart>
              </v-card>
            </v-list-item-content>
          </v-list-item>
        </v-card>
      </v-col>
    </v-row>
    <!-- FOR RECOVERED -->
    <v-row class="text-center">
      <v-col cols="12" v-if="arrRecovered.length > 0">
        <v-card class="mx-auto" mt-10 mb-6 pr-8 pl-8 pb-8 pt-4>
          <v-list-item three-line>
            <v-list-item-content>
              <v-list-item-title class="headline mb-1 primary--text">Recovered Corona Patients</v-list-item-title>
              <v-card elevation="11" class="pt-10">
                <LineChart
                  :chartData="arrRecovered"
                  :options="chartOptions"
                  label="Recovered Cases"
                ></LineChart>
              </v-card>
            </v-list-item-content>
          </v-list-item>
        </v-card>
      </v-col>
    </v-row>
    <!-- FOR RECOVERED -->
    <v-row class="text-center">
      <v-col cols="12" v-if="arrIncu.length > 0">
        <v-card class="mx-auto" mt-10 mb-6 pr-8 pl-8 pb-8 pt-4>
          <v-list-item three-line>
            <v-list-item-content>
              <v-list-item-title class="headline mb-1">inIcuCurrently Corona Cases</v-list-item-title>
              <v-card elevation="11" class="pt-10">
                <LineChart
                  :chartData="arrIncu"
                  :options="chartOptions"
                  label="inIcuCurrently Cases"
                ></LineChart>
              </v-card>
            </v-list-item-content>
          </v-list-item>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import LineChart from "../components/LineChat";
import axios from "axios";
import moment from "moment";

export default {
  name: "HelloWorld",
  components: {
    LineChart
  },
  data() {
    return {
      arrPositive: [],
      // positiveChartColors: {
      //   borderColor: "#ff0000",
      //   pointBackgroundColor: "#004d99",
      //   backgroudColor: "#74A57F",
      //   pointBorderColor: "#33cccc"
      // },
      arrHospitalized: [],
      arrIncu: [],
      arrOnVentilators: [],
      arrRecovered: [],
      arrDeaths: [],
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false
      }
    };
  },
  async created() {
    const { data } = await axios.get("https://covidtracking.com/api/us/daily");
    //console.log(data);
    data.forEach(d => {
      const date = moment(d.date, "YYYYMMDD").format("MM/DD");
      const {
        positive,
        hospitalizedCurrently,
        inIcuCurrently,
        onVentilatorCurrently,
        recovered,
        death
      } = d;
      this.arrPositive.push({ date, total: positive });
      this.arrHospitalized.push({ date, total: hospitalizedCurrently });
      this.arrIncu.push({ date, total: inIcuCurrently });
      this.arrOnVentilators.push({ date, total: onVentilatorCurrently });
      this.arrRecovered.push({ date, total: recovered });
      this.arrDeaths.push({ date, total: death });
      // console.log(this.arrDeaths);
    });
  }
};
</script>
