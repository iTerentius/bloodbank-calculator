<template>
  <header><h4>Calling Cost Calculator</h4></header>
  <section class="container">
    <div v-if="ui.approachOne" id="call-costs">
      <form>
        <div class="row">
          <div class="col-md-12">
            <div class="form-group">
              <label for="callsPerHour" class="form-label"
                >Call List Size</label
              >
              <input
                class="form-control"
                type="text"
                id="callsPerHour"
                name="callsPerHour"
                v-bind:value="inputs.callsPerHour"
              />
            </div>
          </div>
        </div>
        <div class="row">
          <div class="col-md-12">
            <div class="form-group">
              <label for="apptsPerHour" class="form-label"
                >Approx. Appts per Hour</label
              >
              <input
                class="form-control"
                type="text"
                id="apptsPerHour"
                name="apptsPerHour"
                v-bind:value="inputs.apptsPerHour"
              />
            </div>
          </div>
        </div>
        <div class="row">
          <div class="col-md-12">
            <div class="form-group">
              <label for="avgCallerPayPerHour" class="form-label"
                >Avergage Caller Pay per Hour</label
              >
              <input
                class="form-control"
                type="text"
                id="avgCallerPayPerHour"
                name="avgCallerPayPerHour"
                v-bind:value="inputs.avgCallerPayPerHour"
              />
            </div>
          </div>
        </div>
        <div class="row">
          <div class="col-md-12">
            <div class="form-group">
              <label for="avgCallerPayPerHour" class="form-label"
                >Avergage Caller Pay per Hour</label
              >
              <input
                class="form-control"
                type="text"
                id="avgCallerPayPerHour"
                name="avgCallerPayPerHour"
                v-bind:value="inputs.avgCallerPayPerHour"
              />
            </div>
          </div>
        </div>
      </form>
      <div class="comparison">
        <h4>Cost Comparison</h4>
        <div class="output-compare">
          <label>Cost per Donation</label>
          <div class="outputs">
            <div class="output">
              <div class="user">
                <p>{{ aproxwbcolledted }}</p>
              </div>
              <label>Your Center</label>
            </div>
            <div class="vs"><p>vs.</p></div>
            <div class="output">
              <div class="hac">
                <p>$2.59</p>
              </div>
              <label>HealthAware Collect</label>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
// import CallCosts from "./components/CallCosts.vue";

export default {
  name: "App",
  data() {
    return {
      inputs: {
        callsPerHour: 26,
        apptsPerHour: 1.57,
        avgCallerPayPerHour: 15,
        callListSize: 7000,
        rightPartyConnectRate: 0.16,
        apptConversionRate: 0.2,
      },
      constants: {
        wbShowRate: 0.75,
        wbCollected: 0.85,
      },
      ui: {
        approachOne: true,
      },
    };
  },
  computed: {
    // B45
    callinghoursforlist() {
      //B41/B43
      return this.inputs.callListSize / this.inputs.callsPerHour;
    },
    // D47
    totalcallingcost() {
      //D45 * B45
      return this.inputs.avgCallerPayPerHour * this.callinghoursforlist;
    },
    //B49
    approxnumappts() {
      //B47 * B45
      return this.inputs.apptsPerHour * this.callinghoursforlist;
    },
    // B57
    aproxwbcolledted() {
      // B49*B53*B55
      return Math.round(
        this.approxnumappts *
          this.constants.wbShowRate *
          this.constants.wbCollected
      );
    },
    // D57
    callercostperdonation() {
      //D47/D57
      return this.totalcallingcost / this.approxwbcollected;
    },
    toCurrency(value) {
      if (typeof value !== "number") {
        return value;
      }
      var formatter = new Intl.NumberFormat("en-US", {
        style: "currency",
        currency: "USD",
      });
      return formatter.format(value);
    },
  },
  components: {
    // CallCosts,
  },
};
</script>

<style>
#app {
  font-family: "Open Sans", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  /* margin-top: 60px; */
}

.container form {
  margin: 30px 0 60px;
}

.container form label {
  font-weight: bold;
}

.form-control {
  padding: 15px;
  font-weight: bold;
  font-size: 1.2rem;
}

header {
  text-align: center;
  padding: 10px 0;
  background: #2c3e50;
  color: white;
}

header h4 {
  margin: 0;
  font-weight: bold;
}

.row {
  margin-bottom: 20px;
}

.output-compare {
  text-align: center;
  margin: 30px 0;
}

.output-compare label {
  font-weight: bold;
  margin-bottom: 10px;
}

.outputs {
  display: flex;
  justify-content: space-around;
  text-align: center;
}

.outputs .user p,
.output-compare .hac p {
  font-size: 1.3rem;
  font-weight: bold;
  margin: 0;
}

.vs p {
  line-height: 68px;
  margin: 0;
}

.outputs .user,
.outputs .hac {
  padding: 20px;
  border-radius: 5px;
}

.output label {
  text-align: center;
  font-size: 0.8rem !important;
  font-weight: normal;
}

.user {
  background: #f2f2f2;
}

.hac {
  background: #d9e8e1;
}
</style>
