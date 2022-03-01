<template>
  <div>
    <!-- removed ids, use refs instead -->
    <div 
    class="number" 
    v-for="number in shuffledNumbers"
    :key="number" 
    :ref="`n-${number}`" 
    @mouseover="hov(number)" 
    @mouseout="reset">
      {{number}}
    </div>
  </div>
</template>

<script>
export default {
  // accepts limit as prop
  props: {
    limit: Number,
  },
  data() {
    return {
      divisors: [],
    };
  },
  computed: {
    // renamed n to more descriptive shuffledNumbers
    // changed to computed prop, let vue handle reactivity
    shuffledNumbers() {
      // removed loop with unnecessary nested copying of numbers (spread syntax was cloning array each iteration) 
      return [...Array(this.limit).keys()].sort(() => Math.random() - 0.5);
    },
  },
  methods: {
    // moved algo into own function
    // (pure function, doesn't touch component state, can be moved to own module later if need be)
    findDivisors(number) {
      // all divisors of n are <= n/2, or n itself
      // we only have to search half the range 1 -> number
      let divisors = [];
      for(let i = 1; i <= Math.floor(number / 2); i++) {
        if(number % i === 0) {
          divisors.push(i);
        }
      }
      divisors.push(number);
      return divisors;
    },
    hov(number) {
      this.divisors = this.findDivisors(number);

      // we use refs instead of accessing DOM directly
      // we build dynamic ref name for constant time lookup of element 
      this.divisors.forEach(d => this.$refs[`n-${d}`][0].classList.add('active'));
    },
    reset() {
      this.divisors.forEach(d => this.$refs[`n-${d}`][0].classList.remove('active'));
      this.divisors = [];
    },
  },
}
</script>

<style scoped>
	.number {
		display: inline-block;
		padding: 5px;
		background-color: lightgrey;
		margin: 5px;
	}

	.active {
		background-color: red;
	}
</style>
