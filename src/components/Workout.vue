<template>
  <div class="main">
    <Navbar />

    <!-- Workout A -->
    <div id="workout-A" v-if="workoutHistory.lastWorkout == 'B'">
      <div class="workout-container">
        <h1>Workout A</h1>
        <hr />
        <h2>Deadlift</h2>
        <div class="lift">
          <!-- Deadlift Container -->
          <div class="exercise-container">
            <div class="label-container">
              <label for="">Weight</label>
              <input v-model="deadliftWeight" class="text-input" type="text" />
            </div>
            <div class="label-container">
              <label for="">Sets </label>
              <input v-model="deadliftSets" class="text-input" type="text" />
            </div>
            <div class="label-container">
              <label for="">Reps </label>
              <input v-model="deadliftReps" class="text-input" type="text" />
            </div>
            <label for="">Failed?</label>
            <input v-model="deadliftFail" type="checkbox" name="" id="" />
          </div>
          <Graph title="Deadlift" :data="workoutHistory.deadlift" />
        </div>
        <hr />
        <h2>Shoulder Press</h2>
        <div class="lift">
          <!-- Shoulder Press Container -->
          <div class="exercise-container">
            <div class="label-container">
              <label for="">Weight</label>
              <input v-model="pressWeight" class="text-input" type="text" />
            </div>
            <div class="label-container">
              <label for="">Sets</label>
              <input v-model="pressSets" class="text-input" type="text" />
            </div>
            <div class="label-container">
              <label for="">Reps</label>
              <input v-model="pressReps" class="text-input" type="text" />
            </div>
            <label for="">Failed?</label>
            <input v-model="pressFail" type="checkbox" name="" id="" />
          </div>
          <Graph title="Shoulder-Press" :data="workoutHistory.press" />
        </div>
        <hr />
        <h2>Pullups</h2>
        <div class="lift">
          <!-- Pullups Container -->
          <div class="exercise-container">
            <div class="label-container">
              <label for="">Weight</label>
              <input v-model="pullupWeight" class="text-input" type="text" />
            </div>
            <div class="label-container">
              <label for="">Sets</label>
              <input v-model="pullupSets" class="text-input" type="text" />
            </div>
            <div class="label-container">
              <label for="">Reps</label>
              <input v-model="pullupReps" class="text-input" type="text" />
            </div>
          </div>
        </div>
        <hr />
        <button @click="submitA">Submit</button>
      </div>
    </div>
    <!-- End Workout A -->

    <!-- Workout B -->
    <div id="workout-B" v-if="workoutHistory.lastWorkout == 'A'">
      <div class="workout-container">
        <h1>Workout B</h1>
        <hr />
        <h2>Squat</h2>
        <div class="lift">
          <div class="exercise-container">
            <div class="label-container">
              <label for="">Weight</label>
              <input v-model="squatWeight" class="text-input" type="text" />
            </div>
            <div class="label-container">
              <label for="">Sets</label>
              <input v-model="squatSets" class="text-input" type="text" />
            </div>
            <div class="label-container">
              <label for="">Reps</label>
              <input v-model="squatReps" class="text-input" type="text" />
            </div>
            <label for="">Failed?</label>
            <input v-model="squatFail" type="checkbox" name="" id="" />
          </div>
          <Graph title="Squats" :data="workoutHistory.squat" />
        </div>
        <hr />
        <h2>Bench Press</h2>
        <div class="lift">
          <div class="exercise-container">
            <div class="label-container">
              <label for="">Weight</label>
              <input v-model="benchWeight" class="text-input" type="text" />
            </div>
            <div class="label-container">
              <label for="">Sets</label>
              <input v-model="benchSets" class="text-input" type="text" />
            </div>
            <div class="label-container">
              <label for="">Reps</label>
              <input v-model="benchReps" class="text-input" type="text" />
            </div>
            <label for="">Failed?</label>
            <input v-model="benchFail" type="checkbox" name="" id="" />
          </div>
          <Graph title="Bench-Press" :data="workoutHistory.bench" />
        </div>
        <hr />
        <h2>Row</h2>
        <div class="lift">
          <div class="exercise-container">
            <h2>Row</h2>
            <div class="label-container">
              <label for="">Weight</label>
              <input v-model="rowWeight" class="text-input" type="text" />
            </div>
            <div class="label-container">
              <label for="">Sets</label>
              <input v-model="rowSets" class="text-input" type="text" />
            </div>
            <div class="label-container">
              <label for="">Reps</label>
              <input v-model="rowReps" class="text-input" type="text" />
            </div>
            <label for="">Failed?</label>
            <input v-model="rowFail" type="checkbox" name="" id="" />
          </div>
          <Graph title="Row" :data="workoutHistory.row" />
        </div>
        <hr />
        <button @click="submitB">Submit</button>
      </div>
    </div>
    <!-- End Workout B -->

    <Footer />
  </div>
</template>


<script>
// TODO: Refactor the data() exports.
import API from "../services/Api";
import Graph from "./Graph.vue";
import Footer from "./Footer.vue";
import Navbar from "./Navbar.vue";

// Helper Functions
/**
 * Take workout session data and package it as an object.
 * 
 * @arg {Number} weight - this.weight
 * @arg {Number} sets - this.sets
 * @arg {Number} reps - this.reps
 * @arg {Number} fails - this.fail
 */
function workoutSession(weight, sets, reps, fails) {
  return {
    date: Date.now(),
    weight: weight,
    sets: sets,
    reps: reps,
    failed: fails,
  }
}

/**
 * Send workout history data to API endpoint in a PUT request
 * to update database with the data taken from user input.
 * 
 * @arg {Object} hx - this.workoutHistory
 * @arg {VueRouter} router - this.$router
 */
function sendData(hx, router) {
  fetch(`${API.baseURL}/workout/${window.localStorage.getItem("username")}`, {
    method: "PUT",
    mode: "cors",
    headers: {
      "Content-Type": "application/json",
      authorization: `Bearer ${window.localStorage.getItem("token")}`,
    },
    body: JSON.stringify(hx),
  }).then((res) => {
    console.log(res);
  });
  alert("Workout logged.");
  router.push("/#nav");
}

export default {
  components: {
    Graph,
    Footer,
    Navbar,
  },
  data() {
    return {
      // Initialize blank workout history data.
      workoutHistory: "",

      benchWeight: 0,
      benchSets: 3,
      benchReps: 5,
      benchFail: false,

      pressWeight: 0,
      pressSets: 0,
      pressReps: 0,
      pressFail: false,

      squatWeight: 0,
      squatSets: 0,
      squatReps: 0,
      squatFail: false,

      deadliftWeight: 0,
      deadliftSets: 0,
      deadliftReps: 0,
      deadliftFail: false,

      rowWeight: 0,
      rowSets: 0,
      rowReps: 0,
      rowFail: false,

      pullupWeight: 0,
      pullupSets: 0,
      pullupReps: 0,
    };
  },
  async mounted() {
    // Fetch workout history data from API.
    try {
      console.log("Loading data...");
      let username = window.localStorage.getItem("username");
      if (!username) {
        // If token doesn't exist.
        this.$router.push("/login");
      }

      // Handle token authentication and redirect prn.
      await fetch(API.baseURL + "/workout" + "/" + username, {
        method: "GET",
        mode: "cors",
        headers: {
          authorization: `Bearer ${window.localStorage.getItem("token")}`,
        },
      })
        .then((res) => {
          if (res.status == 401) {
            // If token is bad.
            this.$router.push("/login");
          }
          return res;
        })
        .then((res) => res.json())
        .then((data) => {
          if (data.success == false) {
            this.$router.push("/login");
          }

          // TODO: Refactor, because a lot of the code is reused below.

          // After token auth, we handle the JSON data from the server.
          // We use the data to calculate suggested weight for each lift.
          this.workoutHistory = data.workoutHistory;
          let hx = this.workoutHistory;

          // Bench.
          if (hx.bench[hx.bench.length - 1].failed == true) {
            this.benchWeight =
              Math.round((hx.bench[hx.bench.length - 1].weight * 0.9) / 10) *
              10;
          } else {
            this.benchWeight =
              hx.bench[hx.bench.length - 1].weight +
              2.5 * 2 ** Math.max(3 - hx.benchFails, 1);
          }
          this.benchSets = 3;
          this.benchReps = 5;
          this.benchFail = false;

          // Shoulder Press.
          if (hx.press[hx.press.length - 1].failed == true) {
            this.pressWeight =
              Math.round((hx.press[hx.press.length - 1].weight * 0.9) / 10) *
              10;
          } else {
            this.pressWeight =
              hx.press[hx.press.length - 1].weight +
              2.5 * 2 ** Math.max(3 - hx.pressFails, 1);
          }
          this.pressSets = 3;
          this.pressReps = 5;
          this.pressFail = false;

          // Squat.
          if (hx.squat[hx.squat.length - 1].failed == true) {
            this.squatWeight =
              Math.round((hx.squat[hx.squat.length - 1].weight * 0.9) / 10) *
              10;
          } else {
            this.squatWeight =
              hx.squat[hx.squat.length - 1].weight +
              2.5 * 2 ** Math.max(3 - hx.squatFails, 1);
          }
          this.squatSets = 3;
          this.squatReps = 5;
          this.squatFail = false;

          // Deadlift.
          if (hx.deadlift[hx.deadlift.length - 1].failed == true) {
            this.deadliftWeight =
              Math.round(
                (hx.deadlift[hx.deadlift.length - 1].weight * 0.9) / 10
              ) * 10;
          } else {
            this.deadliftWeight =
              hx.deadlift[hx.deadlift.length - 1].weight +
              2.5 * 2 ** Math.max(3 - hx.deadliftFails, 1);
          }
          this.deadliftSets = 3;
          this.deadliftReps = 5;
          this.deadliftFail = false;

          // Row.
          if (hx.row[hx.row.length - 1].failed == true) {
            this.rowWeight =
              Math.round((hx.row[hx.row.length - 1].weight * 0.9) / 10) * 10;
          } else {
            this.rowWeight =
              hx.row[hx.row.length - 1].weight +
              2.5 * 2 ** Math.max(3 - hx.rowFails, 1);
          }
          this.rowSets = 3;
          this.rowReps = 5;
          this.rowFail = false;

          // Pullups.
          this.pullupWeight = hx.pullups[hx.pullups.length - 1].weight;
          this.pullupSets = 3;
          this.pullupReps = 8;

          this.workoutHistory.lastWorkout = hx.lastWorkout;
        });
    } catch (err) {
      console.log(err);
    }
  },
  methods: {
    /**
     * Update workout history with data from user input and
     * send a PUT request to API.
     */
    submitB() {
      let squat = workoutSession(
        this.squatWeight, this.squatSets, this.squatReps, this.squatFail
        );

      let bench = workoutSession(
        this.benchWeight, this.benchSets, this.benchReps, this.benchFail
        );

      let row = workoutSession(
        this.rowWeight, this.rowSets, this.rowReps, this.rowFail
        );

      this.workoutHistory.bench.push(bench);
      this.workoutHistory.squat.push(squat);
      this.workoutHistory.row.push(row);

      if (this.squatFail) {
        this.workoutHistory.squatFails += 1;
      }
      if (this.benchFail) {
        this.workoutHistory.benchFails += 1;
      }
      if (this.rowFail) {
        this.workoutHistory.rowFails += 1;
      }
      this.workoutHistory.lastWorkout = "B";
      console.log(this.workoutHistory);

      sendData(this.workoutHistory, this.$router);
    },
    /**
     * Update workout history with data from user input and
     * send a PUT request to API.
     */
    submitA() {
      let deadlift = workoutSession(
        this.deadliftWeight, this.deadliftSets, this.deadliftReps, this.deadliftFail
        );

      let press = workoutSession(
        this.pressWeight, this.pressSets, this.pressReps, this.pressFail
        );

      let pullups = {
        date: Date.now(),
        weight: this.pullupWeight,
        sets: this.pullupSets,
        reps: this.pullupReps,
      };

      this.workoutHistory.deadlift.push(deadlift);
      this.workoutHistory.press.push(press);
      this.workoutHistory.pullups.push(pullups);

      if (this.deadliftFail) {
        this.workoutHistory.deadliftFails += 1;
      }
      if (this.pressFail) {
        this.workoutHistory.pressFails += 1;
      }
      this.workoutHistory.lastWorkout = "A";
      console.log(this.workoutHistory);

      sendData(this.workoutHistory, this.$router);
    },
  },
};
</script>


<style scoped>
.workout-container {
  max-width: 50em;
  margin: auto;
  padding: 3em;
}

.lift {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  padding: 2em;
}

.exercise-container {
  margin: auto;
}

.label-container {
  display: flex;
  flex-wrap: wrap;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
}

.text-input {
  width: 4em;
  height: 2em;
  border-radius: 5px;
  border: 1px solid lightgray;
  text-align: right;
}

.text-input:focus {
  outline: none;
  border: 1px solid cornflowerblue;
  box-shadow: 0 0 0 1pt cornflowerblue;
}

label {
  padding: 0 5px 0 0;
}

button {
  color: white;
  background-color: cornflowerblue;
  border: 0;
  height: 3.5em;
  width: 10em;
  border-radius: 8px;
  margin: 30px auto;
}

button:hover {
  cursor: pointer;
  background-color: rgb(88, 139, 233);
}
</style>