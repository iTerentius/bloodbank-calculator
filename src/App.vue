<template>
  <header><h4>Calling Cost Calculator</h4></header>
  <section class="container">
    <div v-if="ui.approachOne" id="call-costs">
      <form>
        <div class="row">
          <div class="col-md-12">
            <div class="form-group">
              <label for="callsPerHour" class="form-label"
                >Staff Calls per Hour</label
              >
              <number-input
                class="form-control"
                type="text"
                id="callsPerHour"
                name="callsPerHour"
                v-model="inputs.callsPerHour"
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
              <float-input
                class="form-control"
                type="text"
                id="apptsPerHour"
                name="apptsPerHour"
                v-model="inputs.apptsPerHour"
              />
            </div>
          </div>
        </div>
        <div class="row">
          <div class="col-md-12">
            <div class="form-group">
              <label for="avgCallerPayPerHour" class="form-label"
                >Average Caller Pay per Hour</label
              >
              <currency-input
                v-model="inputs.avgCallerPayPerHour"
                ref="cInput"
              ></currency-input>
            </div>
          </div>
        </div>
        <div class="row">
          <div class="col-md-12">
            <div class="form-group">
              <label for="callListSize" class="form-label"
                >Call List Size</label
              >
              <number-input
                class="form-control"
                type="text"
                id="callListSize"
                name="callListSize"
                v-model="inputs.callListSize"
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
                <p>{{ this.displayUSD(Math.round(callerCostPerDonation)) }}</p>
              </div>
              <label>Your Center</label>
            </div>
            <div class="vs"><p>vs.</p></div>
            <div class="output">
              <div class="hac">
                <p>{{ this.displayUSD(haCostPerDonation) }}</p>
              </div>
              <label>HealthAware Collect</label>
            </div>
          </div>
        </div>
      </div>
      <div class="output-compare">
        <label>Est. WB collections</label>
        <div class="outputs">
          <div class="output">
            <div class="user">
              <p>{{ this.displayNumber(Math.round(aproxWBColledted)) }}</p>
            </div>
            <label>Your Center</label>
          </div>
          <div class="vs"><p>vs.</p></div>
          <div class="output">
            <div class="hac">
              <p>
                {{ this.displayNumber(Math.round(haCollectionsFromAppts)) }}
              </p>
            </div>
            <label>HealthAware Collect</label>
          </div>
        </div>
      </div>
      <div class="output-compare">
        <label
          >Total monthly cost to achieve HealthAware ({{
            Math.round(haCollectionsFromAppts)
          }}) collections
        </label>
        <div class="outputs">
          <div class="output">
            <div class="user">
              <p>{{ this.displayUSD(totMonthlyDiffInCost1, 0) }}</p>
            </div>
            <label>Your Center</label>
          </div>
          <div class="vs"><p>vs.</p></div>
          <div class="output">
            <div class="hac">
              <p>{{ this.displayUSD(haMonthlyCost, 0) }}</p>
            </div>
            <label>HealthAware Collect</label>
          </div>
        </div>
      </div>
      <div class="output-compare">
        <label
          >Total monthly calling hrs. to achiev HealthAware ({{
            Math.round(haCollectionsFromAppts)
          }}) collections
        </label>
        <div class="outputs">
          <div class="output">
            <div class="user">
              <p>
                {{ this.displayNumber(Math.round(haCollectionsFromAppts)) }}
              </p>
            </div>
            <label>Your Center</label>
          </div>
          <div class="vs"><p>vs.</p></div>
          <div class="output">
            <div class="hac">
              <p>--</p>
            </div>
            <label>HealthAware Collect</label>
          </div>
        </div>
      </div>
      <hr />
      <div class="output-compare sum">
        <label>Annual Savings with HealthAware Collect </label>
        <div class="outputs">
          <div class="output">
            <div class="hac">
              <p>{{ this.displayUSD(haAnnualSavingsCosts) }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import CurrencyInput from "./components/CurrencyInput.vue";
// import PercentInput from "./components/PercentInput.vue";
import NumberInput from "./components/NumberInput.vue";
import FloatInput from "./components/FloatInput.vue";

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
        haMontlyMin: 1500,
        haAvgCostPerDonation: 2.9,
        haApptRate: 0.13,
      },
      ui: {
        approachOne: true,
      },
    };
  },
  computed: {
    // Client Calcs One
    // B14
    totCallingHoursComp1() {
      // C12/B12 * B45
      return (
        (this.haCollectionsFromAppts / this.aproxWBColledted) *
        this.callingHoursForlist
      );
    },
    // B13
    totMonthlyDiffInCost1() {
      // (C12/B12) * D47
      return (
        (this.haCollectionsFromAppts / this.aproxWBColledted) *
        this.totalCallingCost
      );
    },
    // B45
    callingHoursForlist() {
      //B41/B43
      return this.inputs.callListSize / this.inputs.callsPerHour;
    },
    // D47
    totalCallingCost() {
      //D45 * B45
      return this.inputs.avgCallerPayPerHour * this.callingHoursForlist;
    },
    //B49
    approxNumAppts() {
      //B47 * B45
      return this.inputs.apptsPerHour * this.callingHoursForlist;
    },
    // B57
    aproxWBColledted() {
      // B49*B53*B55
      console.log(this.approxNumAppts);
      return Math.round(
        this.approxNumAppts *
          this.constants.wbShowRate *
          this.constants.wbCollected
      );
    },
    // D57
    callerCostPerDonation() {
      //D47/D57
      return this.totalCallingCost / this.aproxWBColledted;
    },
    // HA Calcs One
    // F48
    haApproxAppts() {
      // F51 * B41
      return this.constants.haApptRate * this.inputs.callListSize;
    },
    // F55
    haCollectionsFromAppts() {
      // F48 * B53 * B55
      return (
        this.haApproxAppts *
        this.constants.wbShowRate *
        this.constants.wbCollected
      );
    },
    // F46
    haMonthlyCost() {
      // if B41 < 7500, G42 else F55 * G43
      var cost = 0;
      if (this.inputs.callListSize < 7500) {
        cost = this.constants.haMontlyMin;
      } else {
        cost =
          this.haCollectionsFromAppts * this.constants.haAvgCostPerDonation;
      }

      return cost;
    },
    // F57
    haCostPerDonation() {
      // F46/F55
      return this.haMonthlyCost / this.haCollectionsFromAppts;
    },
    // C15
    haAnnualSavingsCosts() {
      // (B13 - C13) * 12
      return (this.totMonthlyDiffInCost1 - this.haMonthlyCost) * 12;
    },
  },
  methods: {
    displayNumber(value) {
      return value.toFixed(0).replace(/\d{1,3}(?=(\d{3})+(?!\d))/g, "$&,");
    },
    displayPerc(value) {
      return value
        .toFixed(2)
        .replace(/^[%$][-+]?\d+([,.]\d{1,2})?|^[-+]?\d+([,.]\d{1,2})?[$%]/g);
    },
    displayUSD(value, precision = 2) {
      return (
        "$" +
        value.toFixed(precision).replace(/(\d)(?=(\d{3})+(?:\.\d+)?$)/g, "$1,")
      );
    },
  },
  components: {
    CurrencyInput,
    // PercentInput,
    NumberInput,
    FloatInput,
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

.output {
  min-width: 150px;
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

.sum .output {
  width: 100%;
}

.sum label,
.sum .hac {
  color: #006837;
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
