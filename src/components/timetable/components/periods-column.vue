<template>
  <div
    class="periods-column"
    :class="[
      isDayLast ? 'periods-column--last' : 'periods-column--present',
    ]"
  >
    <p class="periods-column__header">
      <span
        class="periods-column__date"
        :class="!periods.length && 'periods-column__date--off'"
      >
        {{ date | formatDate }}
      </span>
    </p>

    <periods-cell
      v-for="number of Object.keys(periodTimes)"
      :key="number"
      class="periods-column__cell"
      :periods="periods.filter(p => p.number === number)"
      @period-selected="$emit(events.periodSelected, $event)"
    />
  </div>
</template>

<script>
import PeriodsCell from './periods-cell'

import { periodTimes } from '@constants/period-times'
import { DateUtil } from '@utils/date.util'

const events = {
  periodSelected: 'period-selected',
}

export default {
  name: 'periods-column',
  components: {
    PeriodsCell,
  },

  props: {
    date: { type: Date, required: true },
    periods: { type: Array, required: true },
  },

  data: () => ({
    periodTimes,
    events,
  }),

  computed: {
    isDayLast () {
      return DateUtil.getStartTimestamp(this.date) <
        DateUtil.getStartTimestamp(Date.now())
    },
  },
}
</script>

<style lang="scss" scoped>
.periods-column {
  width: 14rem;

  &--last {
    background-color: rgba($color-primary, 0.15);
  }

  &__header {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 5rem;
    padding: 2rem 1rem;
    font-weight: 700;
    font-size: 1.4rem;
    border: 0.1rem solid $color-secondary;
    border-right: none;
    border-top: none;
  }

  &__date {
    background-color: $color-primary;
    color: $color-text-inverse;
    padding: 0.6rem 1rem;
    border-radius: 1rem;

    &--off {
      background-color: $color-parakeet-dark;
    }
  }
}
</style>
