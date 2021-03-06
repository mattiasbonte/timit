<template>
  <!-- START FORM -->
  <form @submit.prevent="submitStartForm" class="form">
    <!-- FEATURE DESCRIPTION -->
    <input
      type="text"
      class="form__description"
      :class="getStartFormDisabledState ? 'cursor-not-allowed' : ''"
      v-model="description"
      name="description"
      id="description"
      title="Please describe the feature you would like to add"
      placeholder="Describe new feature..."
      :disabled="getStartFormDisabledState"
      required
    />

    <div v-if="!getStartFormDisabledState" class="form__datetime__wrapper">
      <!-- START DATE -->
      <div class="form__date__wrapper">
        <button
          type="button"
          class="form__date__button rounded-l-md"
          @click="subtractDate"
        >
          <svg
            class="w-4 h-4 mx-auto"
            fill="currentColor"
            viewBox="0 0 20 20"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              fill-rule="evenodd"
              d="M15.707 15.707a1 1 0 01-1.414 0l-5-5a1 1 0 010-1.414l5-5a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 010 1.414zm-6 0a1 1 0 01-1.414 0l-5-5a1 1 0 010-1.414l5-5a1 1 0 011.414 1.414L5.414 10l4.293 4.293a1 1 0 010 1.414z"
              clip-rule="evenodd"
            ></path>
          </svg>
        </button>
        <input
          type="date"
          class="form__date flex-grow"
          name="start_date"
          id="start_date"
          v-model="start.date"
          @input="setStartDate"
          required
        />
        <button
          v-if="dateBeforeToday"
          type="button"
          class="form__date__button rounded-r-md"
          @click="addDate"
        >
          <svg
            class="w-4 h-4 mx-auto"
            fill="currentColor"
            viewBox="0 0 20 20"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              fill-rule="evenodd"
              d="M10.293 15.707a1 1 0 010-1.414L14.586 10l-4.293-4.293a1 1 0 111.414-1.414l5 5a1 1 0 010 1.414l-5 5a1 1 0 01-1.414 0z"
              clip-rule="evenodd"
            ></path>
            <path
              fill-rule="evenodd"
              d="M4.293 15.707a1 1 0 010-1.414L8.586 10 4.293 5.707a1 1 0 011.414-1.414l5 5a1 1 0 010 1.414l-5 5a1 1 0 01-1.414 0z"
              clip-rule="evenodd"
            ></path>
          </svg>
        </button>
        <button
          v-else
          type="button"
          class="form__date__button rounded-r-md cursor-not-allowed"
        >
          <svg
            class="w-4 h-4 mx-auto"
            fill="currentColor"
            viewBox="0 0 20 20"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              fill-rule="evenodd"
              d="M13.477 14.89A6 6 0 015.11 6.524l8.367 8.368zm1.414-1.414L6.524 5.11a6 6 0 018.367 8.367zM18 10a8 8 0 11-16 0 8 8 0 0116 0z"
              clip-rule="evenodd"
            ></path>
          </svg>
        </button>
      </div>

      <!-- START TIME -->
      <div class="form__time__wrapper">
        <label class="form__time__label" for="start_date">
          <div class="mx-auto uppercase">{{ start.day }}</div>
        </label>
        <select
          class="form__time"
          v-model="start.time"
          name="start_time"
          id="start_time"
          required
        >
          <option
            v-for="time_stamp in time_stamps"
            :key="time_stamp"
            :value="time_stamp"
            :selected="time_stamp === start.time"
          >
            {{ time_stamp }}
          </option>
        </select>
        <!-- SUBMIT -->
        <button
          type="submit"
          class="btn__base btn__submit"
          name="start_submit"
          id="start_submit"
          :disabled="getStartFormDisabledState"
        >
          START
        </button>
      </div>
    </div>
  </form>
</template>

<script>
  import { nanoid } from 'nanoid';
  import dayjs from 'dayjs';

  export default {
    emits: ['toggle-feature-button'],
    props: {
      project_id: { type: String, required: true },
    },
    data() {
      return {
        time_stamps: [],
        description: '',
        start: { date: '', day: '', time: '' },
      };
    },
    created() {
      this.time_stamps = this.generateTimeStamps;
      this.start.date = this.setDate();
      this.start.day = this.setDay(this.start.date);
      this.start.time = this.calcClosestTimeStamp('start');
    },
    methods: {
      setStartDate() {
        if (dayjs(this.start.date).isAfter(dayjs())) {
          this.start.date = this.setDate();
        }
        this.start.day = this.setDay(this.start.date);
      },
      setDate() {
        return dayjs().format('YYYY-MM-DD');
      },
      setDay(input_date) {
        // Format: MONDAY, TUESDAY...
        if (input_date !== '') {
          return dayjs(input_date).format('ddd');
        } else {
          return `...`;
        }
      },
      subtractDate() {
        // when input is empty, add today as a the starting date
        if (this.start.date === '') {
          this.start.date = dayjs().format('YYYY-MM-DD');
        }

        // Format: 2020-10-20
        const decreased_date = dayjs(this.start.date)
          .subtract(1, 'day')
          .format('YYYY-MM-DD');

        this.start.date = decreased_date;
        this.start.day = this.setDay(this.start.date);
      },
      addDate() {
        // when input is empty, add today as a the starting date
        if (this.start.date === '') {
          this.start.date = dayjs().format('YYYY-MM-DD');
        }

        // Format: 2020-10-20
        const increased_date = dayjs(this.start.date)
          .add(1, 'day')
          .format('YYYY-MM-DD');

        this.start.date = increased_date;
        this.start.day = this.setDay(this.start.date);

        const now = dayjs().format('YYYY-MM-DD');
        if (this.start.date > now) return false;
        return true;
      },
      calcClosestTimeStamp(submit) {
        // Calculates which timestamp is closest to the current time.
        const date = new Date();
        let hours = date.getHours();
        let minutes = date.getMinutes();

        // set closest 15min interval
        if (submit === 'start') {
          if (minutes < 15) minutes = '00';
          else if (minutes < 30) minutes = '15';
          else if (minutes < 45) minutes = '30';
          else minutes = '45';
        } else if (submit === 'stop') {
          if (minutes < 15) minutes = '15';
          else if (minutes < 30) minutes = '30';
          else if (minutes < 45) minutes = '45';
          else {
            minutes = '00';
            hours++;
            if (hours === 24) {
              hours = 0;
            }
          }
        }

        //add 0 in front when hour < 10
        hours = hours < 10 ? `0${hours}` : hours.toString();

        return `${hours}:${minutes}`;
      },
      submitStartForm() {
        // Save new feature in vuex store
        this.$store.commit({
          type: 'addNewFeature',
          project_id: this.project_id,
          feature: {
            id: nanoid(),
            description: this.description,
            segments: [
              {
                id: nanoid(),
                start_date: this.start.date,
                start_time: this.start.time,
                stop_date: '',
                stop_time: '',
              },
            ],
          },
        });

        // Reset, Hide
        this.description = '';
        this.$emit('toggle-feature-button');

        // Disable Start Form
        this.$store.commit('toggleStartForm', {
          id: this.project_id,
          disable: true,
        });
      },
    },
    computed: {
      generateTimeStamps() {
        // generate array of times format hh:mm, 15 minute interval
        let time_stamps = [];
        let hours = 0;

        // 24 hour loop
        for (let index = 0; index < 24; index++) {
          let times = [];
          let minutes = 0;

          // 4 x 15m interval loop
          for (let i = 0; i < 4; i++) {
            times.push(
              `${hours < 10 ? '0' : ''}${hours}:${
                minutes < 10 ? '0' : ''
              }${minutes}`
            );
            minutes += 15;
          }

          hours++;
          time_stamps.push(...times);
        }
        return time_stamps;
      },
      dateBeforeToday() {
        if (
          dayjs(this.start.date).isBefore(dayjs()) &&
          dayjs().isSame(this.start.date, 'day')
        ) {
          return false;
        }
        return true;
      },
      getStartFormDisabledState() {
        return this.$store.getters.getProject({ project_id: this.project_id })
          .disable_start_form;
      },
    },
  };
</script>

<style scoped>
  .form {
    @apply flex flex-col justify-center space-y-3;
    @apply sm:space-x-0 sm:flex-row sm:flex-wrap sm:items-stretch;
    @apply lg:space-y-0 lg:space-x-3 lg:flex-nowrap;
  }

  .form__description {
    @apply w-full px-3 py-6;
    @apply sm:py-4;
    @apply border-gray-300 rounded-md shadow-sm;
    @apply hover:border-light-blue-600 ring-light-blue-600;
    @apply focus:ring-light-blue-600 focus:border-light-blue-600;
    /* dark */
    @apply dark:bg-gray-800 dark:text-white;
    @apply dark:placeholder-gray-400;
    @apply dark:hover:border-white dark:hover:bg-gray-900;
    @apply dark:focus:ring-white dark:focus:border-white dark:focus:bg-gray-900;
  }

  .form__datetime__wrapper {
    @apply sm:flex-row sm:space-y-0 sm:space-x-3 flex flex-col justify-between w-full;
  }

  .form__date__wrapper {
    @apply flex-1 hidden;
    @apply sm:flex;
    @apply rounded-md;
    @apply border border-gray-300;
    @apply hover:border-light-blue-600;
    /* dark */
    @apply dark:hover:border-white;
  }
  .form__date {
    @apply px-3 p-3 text-center shadow-sm sm:w-auto w-full;
    @apply bg-white;
    @apply border border-transparent;
    @apply cursor-pointer;
    @apply hover:bg-gray-50;
    /* dark */
    @apply dark:bg-gray-800 dark:hover:bg-gray-900;
  }
  .form__date__button {
    @apply sm:w-auto w-24;
    @apply px-3 p-3 text-center shadow-sm;
    @apply bg-white;
    @apply border border-transparent;
    @apply hover:bg-gray-50;
    @apply focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-light-blue-600;
    /* dark */
    @apply dark:bg-gray-800 dark:hover:bg-gray-900;
  }

  .form__time__wrapper {
    @apply flex flex-1;
    @apply sm:flex-row;
    @apply rounded-md shadow-sm;
    @apply border border-gray-300;
    @apply hover:border-light-blue-600;
    /* dark */
    @apply dark:hover:border-white;
  }
  .form__time__label {
    @apply rounded-l-md bg-white inline-flex items-center px-3 cursor-pointer;
    @apply sm:w-auto w-24;
    @apply text-gray-500;
    @apply hover:border-light-blue-600 hover:bg-gray-50;
    /* dark */
    @apply dark:bg-gray-800 dark:text-white;
    @apply dark:placeholder-gray-400;
    @apply dark:hover:bg-gray-900;
    @apply dark:focus:ring-white dark:focus:border-white dark:focus:bg-gray-900;
  }
  .form__time {
    @apply text-center flex-1 sm:w-auto cursor-pointer;
    @apply border-none;
    @apply hover:bg-gray-50;
    @apply focus:outline-none focus:ring-transparent;
    /* dark */
    @apply dark:bg-gray-800 dark:text-white;
    @apply dark:placeholder-gray-400;
    @apply dark:hover:bg-gray-900;
    @apply dark:focus:bg-gray-900;
  }

  .btn__submit {
    @apply w-24 rounded-r-md;
    @apply sm:w-auto;
  }
</style>
