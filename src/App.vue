<template>
  <div class="h-[100vh]" :class="dark ? 'dark' : ''">
    <div class="min-h-full bg-slate-100 dark:bg-slate-900">
      <a
        class="absolute right-3 top-3 flex h-12 w-12 cursor-pointer items-center justify-center rounded-lg border-2 border-slate-400 bg-slate-300 dark:border-slate-600 dark:bg-slate-500"
        @click="switchTheme()"
        ><i
          class="fa-solid fa-sun fa-lg z-20 text-slate-300 shadow-lg"
          :class="dark ? '' : 'hidden'"
        ></i
        ><i
          class="fa-solid fa-moon fa-lg z-20 text-slate-500 shadow-lg"
          :class="dark ? 'hidden' : ''"
        ></i
      ></a>
      <div class="container mx-auto flex flex-col items-center">
        <h1
          class="mb-24 mt-16 text-4xl font-extrabold text-slate-700 dark:text-slate-200"
        >
          Tip Calculator
        </h1>
        <div class="grid grid-flow-row gap-y-6 sm:grid-cols-3 sm:gap-x-6">
          <div class="flex">
            <p
              class="flex h-8 w-9 items-center justify-center rounded-s border-2 border-e-0 border-gray-400 bg-slate-300 font-sans text-base font-semibold text-slate-800"
            >
              $
            </p>
            <input
              type="number"
              placeholder="Bill"
              v-model="bill"
              @input="calculateTip()"
              class="h-8 w-full rounded-e border-2 border-s-0 border-gray-400 bg-slate-200 pl-2 transition placeholder:text-slate-500/50 focus:bg-white focus:outline-none sm:w-48"
            />
          </div>
          <div class="flex">
            <p
              class="flex h-8 w-9 items-center justify-center rounded-s border-2 border-e-0 border-gray-400 bg-slate-300 font-sans text-base font-bold text-slate-800"
            >
              %
            </p>
            <input
              type="number"
              placeholder="Tip"
              v-model="tipPercent"
              @input="calculateTip()"
              class="h-8 w-full rounded-e border-2 border-s-0 border-gray-400 bg-slate-200 pl-2 transition placeholder:text-slate-500/50 focus:bg-white focus:outline-none sm:w-48"
            />
          </div>
          <div class="flex">
            <p
              class="flex h-8 w-9 items-center justify-center rounded-s border-2 border-e-0 border-gray-400 bg-slate-300 font-sans text-base font-bold text-slate-800"
            >
              <i class="fa-solid fa-user"></i>
            </p>
            <input
              type="number"
              placeholder="People"
              v-model="people"
              @input="calculateTip()"
              class="h-8 w-full rounded-e border-2 border-s-0 border-gray-400 bg-slate-200 pl-2 transition placeholder:text-slate-500/50 focus:bg-white focus:outline-none sm:w-48"
            />
          </div>
        </div>
        <div
          class="col-span-3 my-8 h-0.5 w-11/12 rounded bg-slate-600/50"
        ></div>
        <div class="grid grid-flow-row gap-y-6 sm:grid-cols-2 sm:gap-x-6">
          <div class="flex sm:place-self-end">
            <p
              class="flex h-8 w-20 items-center justify-center rounded-s border-2 border-e-0 border-gray-400 bg-slate-300 font-sans text-sm font-semibold text-slate-800"
            >
              Tip
            </p>
            <input
              type="number"
              v-model="tip"
              disabled
              class="h-8 w-full rounded-e border-2 border-s-0 border-gray-400 bg-slate-200 pl-2 placeholder:text-slate-500/50 focus:outline-none disabled:bg-slate-200 sm:w-48"
            />
          </div>
          <div class="flex">
            <p
              class="flex h-8 w-20 items-center justify-center rounded-s border-2 border-e-0 border-gray-400 bg-slate-300 font-sans text-sm font-semibold text-slate-800"
            >
              Total
            </p>
            <input
              type="number"
              v-model="total"
              disabled
              class="h-8 w-full rounded-e border-2 border-s-0 border-gray-400 bg-slate-200 pl-2 placeholder:text-slate-500/50 focus:outline-none disabled:bg-slate-200 sm:w-48"
            />
          </div>
          <div
            class="flex sm:place-self-end"
            :class="people > 1 ? '' : 'hidden'"
          >
            <p
              class="flex h-8 w-20 items-center justify-center rounded-s border-2 border-e-0 border-gray-400 bg-slate-300 font-sans text-sm font-semibold text-slate-800"
            >
              Tip&nbsp;<i class="fa-solid fa-caret-right">&nbsp;</i
              ><i class="fa-solid fa-user"></i>
            </p>
            <input
              type="number"
              v-model="tipPerPerson"
              disabled
              class="h-8 w-full rounded-e border-2 border-s-0 border-gray-400 bg-slate-200 pl-2 placeholder:text-slate-500/50 focus:outline-none disabled:bg-slate-200 sm:w-48"
            />
          </div>
          <div class="flex" :class="people > 1 ? '' : 'hidden'">
            <p
              class="flex h-8 w-20 items-center justify-center rounded-s border-2 border-e-0 border-gray-400 bg-slate-300 font-sans text-sm font-semibold text-slate-800"
            >
              Total&nbsp;<i class="fa-solid fa-caret-right">&nbsp;</i
              ><i class="fa-solid fa-user"></i>
            </p>
            <input
              type="number"
              v-model="totalPerPerson"
              disabled
              class="h-8 w-full rounded-e border-2 border-s-0 border-gray-400 bg-slate-200 pl-2 placeholder:text-slate-500/50 focus:outline-none disabled:bg-slate-200 sm:w-48"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
export default {
  data() {
    return {
      dark: false,
      bill: '',
      tipPercent: '',
      people: 1,
      tip: 0,
      total: 0,
      tipPerPerson: 0,
      totalPerPerson: 0
    };
  },
  mounted() {
    this.dark = localStorage.theme === 'dark';
  },
  methods: {
    calculateTip() {
      if (!this.bill || !this.tipPercent) {
        this.tip = 0;
        this.total = 0;
        return;
      }

      const bill = Number(this.bill);
      const tipPercent = Number(this.tipPercent);

      this.tip = Math.round(bill * (tipPercent / 100) * 100) / 100;
      this.total = Math.round((bill + bill * (tipPercent / 100)) * 100) / 100;

      if (this.people > 1) {
        this.tipPerPerson = Math.round((this.tip / this.people) * 100) / 100;
        this.totalPerPerson =
          Math.round((this.total / this.people) * 100) / 100;
      }
      return;
    },
    switchTheme() {
      localStorage.theme = localStorage.theme === 'light' ? 'dark' : 'light';
      this.dark = localStorage.theme === 'dark';
    }
  }
};
</script>
