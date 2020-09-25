<template>
  <div>
    <table class="table" cellspacing="2" border="1" cellpadding="5">
      <thead>
        <tr>
          <td></td>
          <td v-for="(date, idx) in dates" :key="idx">{{ date }}</td>
        </tr>
      </thead>

      <tbody>
        <tr v-for="(work, idx) in works" :key="idx">
          <td>{{ work }}</td>
          <td v-for="(date, headerIdx) in dates" :key="headerIdx">
            <input
              v-if="innerData[createObjectKeyFromArgs(work, date)]"
              v-model.number="
                innerData[createObjectKeyFromArgs(work, date)].cost
              "
            />
          </td>

          <td>{{ calculateSummByPropertyAndVal("work", work) }}</td>
        </tr>

        <tr>
          <td></td>
          <td v-for="(item, headerIdx) in dates.length" :key="headerIdx">
            {{ calculateSummByPropertyAndVal("date", dates[headerIdx]) }}
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  props: {
    data: {
      default: null,
      type: Array
    }
  },

  data() {
    return {
      innerData: null,
      dates: null,
      works: null
    };
  },

  watch: {
    data: {
      immediate: true,
      handler(val) {
        this.dates = [...new Set(val.map(el => el.date))];
        this.works = [...new Set(val.map(el => el.work))];
        const obj = {};
        val.forEach(el => {
          obj[this.createObjectKeyFromArgs(el.work, el.date)] = el;
        });
        this.innerData = obj;
      }
    }
  },

  methods: {
    createObjectKeyFromArgs() {
      var args = Array.from(arguments);
      const concatArgs = args.reduce((acc, el) => {
        return (acc += this.removeSpaceFromName(el));
      }, "");
      return concatArgs;
    },

    removeSpaceFromName(data) {
      return data.split(" ").join("");
    },

    calculateSummByPropertyAndVal(property, val) {
      const filteredInnerData = Object.values(this.innerData).filter(
        el => el[property] === val
      );
      const summ = filteredInnerData.reduce((acc, el) => (acc += +el.cost), 0);
      return summ;
    }
  }
};
</script>

<style scoped>
.table {
  margin: 0 auto;
  width: 600px;
}
</style>
