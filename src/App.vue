<template>
  <div
    class="container font-jakarta p-3.5 sm:px-4 md:px-8 lg:px-16 xl:px-16 mx-auto my-2"
  >
    <div class="container p-3.5 sm:px-4 md:px-8 lg:px-32 xl:px-32 mx-auto my-2">
      <div class="grid grid-cols-1 lg:grid-cols-2 shadow-md">
        <!-- First Grid -->
        <div class="col-span-1 rounded-tl-2xl rounded-bl-2xl p-4">
          <!-- Head Actions -->
          <div class="flex justify-between">
            <h1 class="text-xl font-semibold">Mortgage Calculator</h1>
            <a
              @click="clear()"
              class="hover:underline cursor-pointer text-[#8f9aa4]"
              >Clear all</a
            >
          </div>

          <!-- Mortgage Amount -->
          <div class="flex flex-col mt-4">
            <label for="mortgage" class="text-[#8f9aa4] mb-2"
              >Mortgage Amount</label
            >
            <div
              class="relative flex items-center border rounded-md overflow-hidden shadow-sm"
              :class="{ 'border-red-500': errors.amount }"
            >
              <div class=" px-3 py-2  border-r"
              :class="{ 'bg-red-500 text-white': errors.amount, 'bg-[#e3f4fc] text-[#8f9aa4]' : !errors.amount }"
              >£</div>
              <input
                id="mortgage"
                type="text"
                v-model="form.amount"
                placeholder="Enter amount"
                class="flex-1 px-3 py-2 outline-none"
              />
            </div>
            <p v-if="errors.amount" class="text-red-500 text-sm mt-1">
              Field is empty
            </p>
          </div>

          <!-- Mortgage Term -->
          <div class="grid grid-cols-1 lg:grid-cols-2 gap-2 mt-4">
            <div class="flex flex-col">
              <label for="mortgageTerm" class="text-[#8f9aa4] mb-2"
                >Mortgage Term</label
              >
              <div
                class="relative flex items-center border rounded-md overflow-hidden shadow-sm"
                :class="{ 'border-red-500': errors.year }"
              >
                <input
                  id="mortgageTerm"
                  type="text"
                  v-model="form.year"
                  placeholder="Mortgage Term"
                  class="flex-1 px-3 py-2 outline-none"
                />
                <div class=" px-3 py-2  border-r"
                :class="{ 'bg-red-500 text-white': errors.year , 'bg-[#e3f4fc] text-[#8f9aa4]': !errors.year }"
                >
                  years
                </div>
              </div>
              <p v-if="errors.year" class="text-red-500 text-sm mt-1">
                Field is empty
              </p>
            </div>

            <div class="flex flex-col">
              <label for="interestRate" class="text-[#8f9aa4] mb-2"
                >Interest Rate</label
              >
              <div
              class="relative flex items-center border rounded-md overflow-hidden shadow-sm"
              :class="{ 'border-red-500': errors.rate }"
              >
                <input
                  id="interestRate"
                  type="text"
                  v-model="form.rate"
                  placeholder="Interest Rate"
                  class="flex-1 px-3 py-2 outline-none"
                />
                <div class=" px-3 py-2  border-r"
                :class="{ 'bg-red-500 text-white': errors.rate, 'bg-[#e3f4fc] text-[#8f9aa4]' : !errors.rate }"
                >%</div>
              </div>
              <p v-if="errors.rate" class="text-red-500 text-sm mt-1">
                Field is empty
              </p>
            </div>
          </div>

          <!-- Mortgage Type -->
          <div class="flex flex-col mt-4">
            <label class="text-[#8f9aa4] mb-2">Mortgage Type</label>
            <div class="space-y-2">
              <label
                class="flex items-center justify-between border rounded-md p-4 cursor-pointer hover:shadow-md"
                :class="{
                  'border-yellow bg-[#d9dd98] opacity-50':
                    selectedOption === 'repayment',
                }"
              >
                <input
                  type="radio"
                  value="repayment"
                  v-model="selectedOption"
                  class="hidden"
                />
                <div class="flex items-center">
                  <div
                    class="w-5 h-5 border-2 rounded-full flex items-center justify-center mr-3"
                  >
                    <div
                      v-if="selectedOption === 'repayment'"
                      class="w-3 h-3 bg-[#d3d637] rounded-full"
                    ></div>
                  </div>
                  <span class="font-medium text-black">Repayment</span>
                </div>
              </label>

              <label
                class="flex items-center justify-between border rounded-md p-4 cursor-pointer hover:shadow-md"
                :class="{
                  'border-yellow bg-[#d9dd98] opacity-50':
                    selectedOption === 'interestOnly',
                }"
              >
                <input
                  type="radio"
                  value="interestOnly"
                  v-model="selectedOption"
                  class="hidden"
                />
                <div class="flex items-center">
                  <div
                    class="w-5 h-5 border-2 rounded-full flex items-center justify-center mr-3"
                  >
                    <div
                      v-if="selectedOption === 'interestOnly'"
                      class="w-3 h-3 bg-[#d3d637] rounded-full"
                    ></div>
                  </div>
                  <span class="font-medium text-black">Interest Only</span>
                </div>
              </label>
            </div>
          </div>

          <!-- Calculate Button -->
          <div class="mt-4">
            <button
              @click="validateForm"
              class="py-2 px-4 bg-yellow text-dark shadow-md rounded-2xl hover:bg-[#d9dd98]"
            >
              <i class="fas fa-calculator mr-2"></i>
              Calculate Repayments
            </button>
          </div>
        </div>
        <!-- Second Grid -->
        <div
          class="col-span-1 p-16 flex flex-col items-center justify-center bg-dark lg:rounded-bl-[6rem] lg:rounded-tr-2xl lg:rounded-br-2xl"
        >
          <img src="/images/illustration-empty.svg" v-if="!showResults" />
          <p class="text-white text-lg mt-8" v-if="!showResults">Results shown Here</p>
          <p class="text-[#8f9aa4] font-thin text-xs text-center mt-8" v-if="!showResults">
            Complete the form and click "calculate repayments" <br />
            to see what your monthly repayments would be
          </p>
          <p class="text-white text-lg " v-if="showResults">Your results</p>
          <p class="text-[#8f9aa4] text-md mt-8" v-if="showResults">Your results are shown below based on the information you provided. To adjust the results, edit the form and click "calculate repayments" again.
            Your monthly repayments</p>
          <div class="bg-[#0e2431] py-2  border-t-4 border-t-[#d9d934] rounded-lg mt-8" v-if="showResults">
            <div class="flex flex-col gap-2">
              <p class="text-[#8f9aa4] text-lg">Your monthly repayment</p>
              <div class="text-yellow font-bold text-3xl mt-2">
                 {{ monthlyRepaymentFormatted }}
              </div>
            </div>
            <hr />
            <div class="flex flex-col gap-2">
              <p class="text-[#8f9aa4] text-lg">Total you'll pay over the term</p>
              <div class="text-yellow font-bold text-3xl mt-2">
                 {{ totalRepaymentFormatted }}
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      form: {
        amount: "",
        year: "",
        rate: "",
      },
      showResults:false,
      selectedOption: null,
      monthlyRepayment:0,
      totalRepayment:0,
      totalRepaymentFormatted:"",
      monthlyRepaymentFormatted:"",
      errors: {
        amount: false,
        year: false,
        rate: false,
      },
    };
  },
  methods: {
    clear() {
      this.form.amount = "";
      this.form.year = "";
      this.form.rate = "";
      this.selectedOption = null;
      this.errors.amount=false;
      this.errors.rate=false;
      this.errors=false;
      this.showResults=false;
    },
    validateForm() {
  this.errors = { amount: false, year: false, rate: false };
  if (!this.form.amount) this.errors.amount = true;
  if (!this.form.year) this.errors.year = true;
  if (!this.form.rate) this.errors.rate = true;

  if (!this.errors.amount && !this.errors.year && !this.errors.rate) {
    // Calculate monthly interest rate
    const monthlyInterestRate = (parseFloat(this.form.rate) / 100) / 12;

    if (this.selectedOption === 'repayment') {
      // Calculate monthly repayment for repayment mortgage
      const numerator = monthlyInterestRate * Math.pow(1 + monthlyInterestRate, parseFloat(this.form.year) * 12);
      const denominator = Math.pow(1 + monthlyInterestRate, parseFloat(this.form.year) * 12) - 1;
      this.monthlyRepayment = parseFloat(this.form.amount) * (numerator / denominator);
      this.monthlyRepaymentFormatted=this.monthlyRepayment.toLocaleString('en-GB', {
        style: 'currency',
        currency: 'GBP',
        minimumFractionDigits: 2,
        maximumFractionDigits: 2,
      });
    } else {
      // Calculate monthly repayment for interest-only mortgage
      this.monthlyRepayment = parseFloat(this.form.amount) * (parseFloat(this.form.rate) / 100) / 12;
      this.monthlyRepaymentFormatted=this.monthlyRepayment.toLocaleString('en-GB', {
        style: 'currency',
        currency: 'GBP',
        minimumFractionDigits: 2,
        maximumFractionDigits: 2,
      });
    }

    // Calculate total repayment
    this.totalRepayment = this.monthlyRepayment * parseFloat(this.form.year) * 12;
    this.totalRepaymentFormatted=this.totalRepayment.toLocaleString('en-GB', {
        style: 'currency',
        currency: 'GBP',
        minimumFractionDigits: 2,
        maximumFractionDigits: 2,
      });
    // Show results
    this.showResults = true;
  } else {
    alert("Please Fill the fields...");
  }
},
  },
};
</script>
