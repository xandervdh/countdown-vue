<template>
  <div v-if="loaded">
    <section class="text-3xl flex justify-center content-center flex-col mx-auto  text-center">
      <h5 v-if="!expired">{{ text }} ({{ day }}/{{ month + 1 }}/{{ year }} {{hours}}h {{minutes}}min)</h5>
      <h5 v-else>{{ done }}</h5>
    </section>
    <section class="flex text-6xl fx justify-center content-center">
      <div class="days mr-2 relative">
        {{ displayDays }}
        <div class="label text-sm absolute bottom-0">Days</div>
      </div>
      <span class="leading-snug">:</span>
      <div class="hours mx-2 relative">
        {{ displayHours }}
        <div class="label text-sm absolute bottom-0">Hours</div>
      </div>
      <span class="leading-snug">:</span>
      <div class="minutes mx-2 relative">
        {{ displayMinutes }}
        <div class="label text-sm absolute bottom-0">Minutes</div>
      </div>
      <span class="leading-snug">:</span>
      <div class="seconds mx-2 relative">
        {{ displaySeconds }}
        <div class="label text-sm absolute bottom-0">Seconds</div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  props: ['year', 'month', 'day', 'hours', 'minutes', 'seconds', 'miliseconds', 'text', 'done'],
  data: () => ({
    displayDays: 0,
    displayHours: 0,
    displayMinutes: 0,
    displaySeconds: 0,
    loaded: false,
    expired: false
  }),
  computed: {
    _seconds: () => 1000,
    _minutes() {
      return this._seconds * 60
    },
    _hours() {
      return this._minutes * 60
    },
    _days() {
      return this._hours * 24
    },
    end() {
      return new Date(
          this.year,
          this.month,
          this.day,
          this.hours,
          this.minutes,
          this.seconds,
          this.miliseconds
      );
    }
  },
  mounted() {
    this.showRemaining();
  },
  methods: {
    formatNum(num) {
      return num < 10 ? '0' + num : num;
    },

    showRemaining() {
      const timer = setInterval(() => {
        const now = new Date();
        // const end = new Date(2021, 1,15, 9, 0, 0, 0);
        const distance = this.end.getTime() - now.getTime();

        if (distance < 0) {
          clearInterval(timer);
          this.expired = true;
          this.loaded = true;
          return
        }

        const days = Math.floor(distance / this._days);
        const hours = Math.floor((distance % this._days) / this._hours);
        const minutes = Math.floor((distance % this._hours) / this._minutes);
        const seconds = Math.floor((distance % this._minutes) / this._seconds);
        this.displayDays = this.formatNum(days);
        this.displayHours = this.formatNum(hours);
        this.displayMinutes = this.formatNum(minutes)
        this.displaySeconds = this.formatNum(seconds);
        this.loaded = true;
      }, 1000);
    }
  }
};
</script>

<style scoped>
.seconds {
  max-width: 60px;
}
</style>
