<template>
  <div class="vis-charts-container">

    <template
      v-if="settingsFetched && rowsLevel_1.length">

      <ChartTable
        v-show="isActiveChartType('table')"
        v-if="initializedCharts['table']"

        :tt-table-data="ttTableData"
        :row-names-level_1="rowNamesLevel_1"
        :row-names-level_2="rowNamesLevel_2"
        :display-column="chartTableDisplayColumn"
        :report-group1-name="reportGroup1Selected.name"
        :report-group2-name="reportGroup2Selected.name"
        :reversed-axis="chartTableReversedAxis"
        :tt-table-columns="ttTableColumns"
        :tt-table-columns-names="ttTableColumnsNames"
        :current-vis-type="currentVisType"
        :showed="isActiveChartType('table') && visContainerShowed"
      />

      <ChartPies
        v-show="isActiveChartType('pie')"
        v-if="initializedCharts['pie']"

        :rows-level_1="rowsLevel_1"
        :showed="isActiveChartType('pie') && visContainerShowed"
      />

      <ComparativeChart
        v-show="isActiveChartType('comparative')"
        v-if="initializedCharts['comparative']"

        :tt-current-day-data="rowsLevel_1"
        :tt-table-columns="ttTableColumns"
        :display-tt-column="comparativeChartDisplayColumn"
        :showed="isActiveChartType('comparative') && visContainerShowed"

        @setPreloaderVisibility="setPreloaderVisibility"
      />

      <SimpleChart
        v-show="isActiveChartType('bar')"
        v-if="initializedCharts['bar']"

        :rows-level_1="rowsLevel_1"
        :row-names-level_1="rowNamesLevel_1"
        :showed="isActiveChartType('bar') && visContainerShowed"
      />
    </template>

  </div>
</template>

<script>

import Chartjs from './Chartjs.vue';
import ChartPies from './ChartPies.vue';
import ChartTable from './ChartTable.vue';
import SimpleChart from './SimpleChart.vue';
import ComparativeChart from './ComparativeChart.vue';

export default {
  components: {
    ComparativeChart,
    SimpleChart,
    ChartTable,
    ChartPies,
    Chartjs,
  },
  props: {
    visContainerShowed: {
      type: Boolean,
      required: true,
    },
    rowNamesLevel_1: {
      type: Array,
      requred: true,
    },
    rowNamesLevel_2: {
      type: Array,
      requred: true,
    },
    ttTableData: {
      type: Array,
      required: true,
    },
    currentVisType: {
      type: String,
      required: true,
    },
    ttTableColumns: {
      type: Array,
      required: true,
    },
    comparativeChartDisplayColumn: {
      type: String,
      required: true,
    },
    chartTableDisplayColumn: {
      type: String,
      required: true,
    },
    reportGroup1Selected: {
      type: Object,
      required: true,
    },
    reportGroup2Selected: {
      type: Object,
      required: true,
    },
    ttTableColumnsNames: {
      type: Object,
      required: true,
    },
    chartTableReversedAxis: {
      type: Boolean,
      required: true,
    },
    settingsFetched: {
      type: Boolean,
      required: true,
    },
  },
  data() {
    return {
      initializedCharts: {},
    };
  },
  computed: {
    rowsLevel_1() {
      return this.ttTableData.filter(row => row.level == 1);
    },
  },
  watch: {
    settingsFetched() {
      this._initChart();
    },
    currentVisType() {
      this._initChart();
    },
    visContainerShowed() {
      if (this.settingsFetched) {
        this._initChart();
      }
    }
  },
  methods: {
    isActiveChartType(chartType) {
      return chartType == this.currentVisType;
    },
    setPreloaderVisibility(state) {
      this.$emit('setPreloaderVisibility', state);
    },
    _initChart() {
      const initializedCharts = Object.assign({}, this.initializedCharts);
      initializedCharts[this.currentVisType] = true;
      this.initializedCharts = initializedCharts;
    },
  },
};
</script>

<style lang="scss">
  .vis-charts-container {
    height: 100%;
    max-height: calc(100% - 100px);
  }
</style>
