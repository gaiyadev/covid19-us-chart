<template>
  <v-container>
    <v-row class="text-center">
      <v-col cols="12" v-if="arrPositive.length > 0">
        <h1>Covid 19 data visualization</h1>
        <div>
          <h2>positive</h2>
          <LineChart
            :chartColors="positiveChartColors"
            :chartData="arrPositive"
            :options="chartOptions"
            :label="Positive"
          ></LineChart>
        </div>
      </v-col>
    </v-row>=
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
      positiveChartColors: {
        borderColor: "#43474f",
        pointBackgroundColor: "fff555",
        backgroudColor: "#fff734"
      },
      arrPositive: [],
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
      //console.log(this.arrDeaths);
    });
  }
};
</script>
