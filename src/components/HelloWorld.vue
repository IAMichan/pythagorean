<template>
  <div>
    <form @submit.prevent="calculatePythagorean">
      <label for="sideA">Side A</label>
      <input v-model="input.sideA" type="number" id="sideA" name="sideA" required><br>

      <label for="sideB">Side B</label>
      <input v-model="input.sideB" type="number" id="sideB" name="sideB" required><br>

      <label for="sideC">Side C</label>
      <input v-model="input.sideC" type="number" id="sideC" name="sideC" required><br>

      <button type="submit">Calculate</button>
    </form>

    <p v-if="result !== null">Result: {{ result }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      input: {
        sideA: null,
        sideB: null,
        sideC: null,
      },
      result: null,
    };
  },
  methods: {
    calculatePythagorean() {
      this.result = null;

      if ((this.input.sideA && this.input.sideB) || (this.input.sideA && this.input.sideC) || (this.input.sideB && this.input.sideC)) {
        fetch('http://localhost:7106/api/pythagorean', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(this.input),
        })
          .then(response => {
            if (!response.ok) {
              throw new Error('Network response was not ok');
            }
            console.log(response);
            return response.json();
          })
          .then((data) => {
            this.result = data;
            console.log('Success:', data);
          })
          .catch((error) => {
            console.error('Error:', error);
          });

      } else {
        alert('Please provide at least two sides.');
      }
    },
  },
};
</script>