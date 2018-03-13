<template>
  <div class="time-slot">
   <div v-for="(n,index) in timeSet" :class="'item time-slot-'+index">
    <el-checkbox-group v-model="selected[index]" class="">
      <el-checkbox-button v-for="day in timeSet[index].days" :label="day" :key="day">{{day}}</el-checkbox-button>
    </el-checkbox-group>
     <div v-for="(n,i) in timeSet[index].time">   
          <div class="d-b">
              <div class="start-time ">
              <span> Start time </span>
              <el-time-picker
              format="hh:mm:A"
              v-model="timeSet[index].timePickers[i].start"
              placeholder="Start time">
            </el-time-picker>
          </div>
          <div class="end-time">
          <span>End time</span>
            <el-time-picker
              arrow-control
              format="hh:mm:A"
              v-model="timeSet[index].timePickers[i].end"
              placeholder="End time">
            </el-time-picker>
          </div>
          </div>
      </div>
  <el-button plain size="mini" v-if="timeSet[index].time < 3" 
  @click="timeSet[index].time = timeSet[index].time+1" class="add-time">Add Time</el-button>
  </div>
  <el-button class="mt" primary v-if="!noMoreTimeSet" @click="setStates">Add Another Slot</el-button>
  </div>
</template>

<script>
const days = [
  "Monday",
  "Tuesday",
  "Wednesday",
  "Thursday",
  "Friday",
  "Saturday",
  "Sunday"
];

export default {
  name: "HelloWorld",
  data() {
    return { 
      timeSet: [
        {
          days: [ 
            "Monday",
            "Tuesday",
            "Wednesday",
            "Thursday",
            "Friday",
            "Saturday",
            "Sunday"
          ],
          time:1,
      timePickers: [
        {
          start: "",
          end: ""
        },
        {
          start: "",
          end: ""
        },
        {
          start: "",
          end: ""
        }
      ]
        }
      ],
      filled: [],
      timePickers: [
        {
          start: "",
          end: ""
        },
        {
          start: "",
          end: ""
        },
        {
          start: "",
          end: ""
        }
      ],
      days: days,
      selected: [["Sunday"], [], [], [], [], [], []]
    };
  },
  computed: {
    noMoreTimeSet() {
      let select = new Set();
           this.selected.map(selectedArr => {
        selectedArr.map(selectedDay => {
          days.map(day => {
            if (day == selectedDay) {
              select.add(day);
            }
          });
        });
      });
      // let selectedDays = Array.from(select);
      let d = Array.from(select)
      // console.log(d)
      if(d.length == 7) {
        return true;
      }
      return false;
    }
  },
  mounted() {},
  methods: {
    setStates() {
      // console.log(this.selected[this.timeSet.length-1].length)
      if (!this.selected[this.timeSet.length - 1].length) {
        return;
      }
      let select = new Set();
      // this.timeSet++;
      this.selected.map(selectedArr => {
        selectedArr.map(selectedDay => {
          days.map(day => {
            if (day == selectedDay) {
              select.add(day);
            }
          });
        });
      });
      let selectedDays = Array.from(select);
      let d = days.filter(day => {
        if (selectedDays.includes(day)) {
          return false;
        }
        return true;
      });
      // if(d.length==0){
      //   this.noMoreTimeSet=true
      // }
      this.timeSet.push({ days: d,time:1,timePickers: [
        {
          start: "",
          end: ""
        },
        {
          start: "",
          end: ""
        },
        {
          start: "",
          end: ""
        }
      ] });
      this.selected[this.timeSet.length - 1].push(...d);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.time-slot .item{
  border:0.5px solid #68717A;
   margin-top: 1rem;
   padding: 1rem;
   position: relative;
}
.time-slot {
  text-align: left;
}
.time-slot .el-checkbox-group{
  /*margin-top: 1rem;*/
}
.add-time{
position: absolute;
bottom: 10px;
right: 10px;
background: #fff;
    border-color: #409eff;
    color: #409eff;
}
.d-b,.mt{
  display: block;
  margin-top: 20px;
}
.end-time {
  margin-left:1.5rem;
}
.start-time,.end-time {
  display: inline-block;
}
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>