<template>
  <div id="app">
    <div class="wrapper">
      <div class="data-inputs">
        <div class="input">
          <label for="" class="input__name">Frequency</label>
          <div class="input__field">
            <input
              :value="frequency"
              @blur="changeFrequency"
              name=""
              type="number"
            />
            <label for="" class="unit">tick/sec</label>
          </div>
        </div>
        <div class="input">
          <label for="" class="input__name">Average</label>
          <div class="input__field">
            <input :value="average" @blur="setAverage" name="" type="number" />
            <label for="" class="unit">pt</label>
          </div>
        </div>
        <div class="input">
          <label for="" class="input__name">Deviation</label>
          <div class="input__field">
            <input
              :value="deviation"
              @blur="setDeviation"
              name=""
              type="number"
            />
            <label for="" class="unit">pt</label>
          </div>
        </div>
      </div>
      <Chart :frequency="frequency" :average="average" :deviation="deviation" />
    </div>
  </div>
</template>

<script>
import Chart from "./components/Chart";
export default {
  name: "App",
  components: { Chart },
  data() {
    return {
      frequency: 2,
      average: 0,
      deviation: 0,
    };
  },
  methods: {
    changeFrequency(e) {
      const val = Number(e.target.value);
      if (this.validate(val)) {
        this.frequency = val;
      } else e.target.value = this.frequency;
    },
    setAverage(e) {
      const val = Number(e.target.value);
      this.average = val;
      e.target.value = val;
    },
    setDeviation(e) {
      const val = Number(e.target.value);
      if (this.validate(val)) {
        this.deviation = val;
      } else e.target.value = this.deviation;
    },
    validate(val) {
      return typeof val === "number" && Math.sign(val) === 1;
    },
  },
};
</script>

<style lang="scss" scoped>
#app {
  width: 100vw;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}
.wrapper {
  width: 100%;
  height: 100%;
  max-height: 600px;
  min-height: 400px;
  padding: 80px;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  .data-inputs {
    background: #fff;
    height: 100%;
    border-radius: 8px 0 0 8px;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: center;
    padding: 0 20px;
    .input {
      margin-bottom: 20px;
      &__name {
        font-weight: bold;
        font-size: 0.9em;
      }
      &__field {
        margin-top: 3px;
        display: flex;
        align-items: center;
        justify-content: center;
        input {
          border: 0;
          border: 1px #6e6e6e solid;
          border-radius: 5px;
          font-size: 1em;
          width: 100px;
          padding: 2px 5px;
        }
        .unit {
          font-size: 0.75em;
          color: #6e6e6e;
          margin-left: 5px;
        }
      }
    }
  }
}

input {
  &::-webkit-outer-spin-button,
  &::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }
  &[type="number"] {
    -moz-appearance: textfield;
  }
}
</style>


