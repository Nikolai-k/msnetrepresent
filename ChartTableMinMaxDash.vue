<template>
  <div class="bnm-chart-table-minmaxdash">
    <span class="bnm-chart-table-minmaxdash__value-title">
      Min: {{ valueFormatter(minValue) }}
    </span>
    <div
      :style="dashStyle"
      class="bnm-chart-table-minmaxdash__dash">
      <div
        :style="cursorPosition"
        class="bnm-chart-table-minmaxdash__cursor"/>
    </div>
    <span class="bnm-chart-table-minmaxdash__value-title">
      Max: {{ valueFormatter(maxValue) }}
    </span>
  </div>
</template>

<script>
export default {
  props: {
    maxValue: {
      type: [Number, String],
      required: true,
    },
    minValue: {
      type: [Number, String],
      required: true,
    },
    currentValue: {
      type: [Number, String, Boolean],
      required: true,
    },
    valueFormatter: {
      type: Function,
      required: true,
    },
  },
  computed: {
    dashRedGradientOffset() {
      let redGradientOffset;

      if (this.maxValue <= 0) {
        redGradientOffset = 100;
      } else if (this.minValue < 0) {
        const interval = this.maxValue - this.minValue;
        const minV = Math.abs(this.minValue);
        redGradientOffset = Math.abs(minV / interval) * 100;
      } else {
        redGradientOffset = 0;
      }
      return redGradientOffset;
    },
    dashStyle() {
      return {
        background: `linear-gradient(90deg, rgba(255,204,188, 0.9) ${this.dashRedGradientOffset}%, rgba(200, 230, 201, 0.9) 0%)`,
      };
    },
    cursorPosition() {
      const interval = Math.abs(this.maxValue - this.minValue);
      let cVal;
      if (this.minValue < 0) {
        cVal = Math.abs(this.minValue) + parseFloat(this.currentValue);
      } else if (this.minValue >= 0) {
        cVal = parseFloat(this.currentValue) - Math.abs(this.minValue);
      }

      const cursorPosition = (cVal / interval) * 100;

      return {
        left: `${cursorPosition}%`,
      };
    },
  },
};
</script>

<style lang="scss">

.bnm-chart-table-minmaxdash{
    padding: 0 27px 0 45px;
    position: relative;

    &__value-title{
        position: absolute;
        font-size: 12px;
        top: -17px;
        &:last-of-type{
            right: 25px;
        }
    }

    &__dash{
        height: 28px;
        background: #c8e6c9;
        box-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);
        position: relative;
    }

    &__cursor{
        height: 33px;
        width: 2px;
        background: gray;
        position: absolute;
        top: -2px;
    }

}
</style>
