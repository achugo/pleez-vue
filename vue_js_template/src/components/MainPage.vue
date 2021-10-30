<template>
  <div>
    <h2>Main Page</h2>
    <button @click="shuffleData" class="shuffle">Shuffle</button>

    <div class="userdata">
      <div>username</div>
      <div>email</div>
      <div>gender</div>
    </div>

    <div v-if="shuffled">
      <div class="results" v-for="s in shuffledLists" v-bind:key="s.uid">
        <div class="userdata">
          <div>{{ s.username }}</div>
          <div>
            {{ s.email.includes("@") ? s.email : "invalid email" }}
          </div>
          <div>{{ s.gender }}</div>
        </div>
      </div>
    </div>
    <div v-else><p>Fetching data</p></div>
  </div>
</template>

<script>
//https://random-data-api.com/api/users/random_user?size=12
//username, email, gender
export default {
  name: "MainPage",
  data() {
    return {
      shuffled: null,
    };
  },

  computed: {
    shuffledLists() {
      console.log("shuffled is", this.shuffled);
      return this.shuffled;
    },
  },

  methods: {
    shuffleData() {
      this.shuffled = this.shuffled
        .map((value) => ({ value, sort: Math.random() }))
        .sort((a, b) => a.sort - b.sort)
        .map(({ value }) => value);
    },
  },

  mounted() {
    fetch("https://random-data-api.com/api/users/random_user?size=12")
      .then((res) => res.json())
      .then((data) => {
        this.shuffled = data;
      })
      .catch((err) => err.message);
  },
};
</script>

<style scoped>
.userdata {
  max-width: 500px;
  margin: 20px auto;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}
.shuffle {
  margin: 0px auto;
  cursor: pointer;
  padding: 15px 35px;
  background-color: crimson;
  color: white;
  font-size: 20px;
  border: none;
  border-radius: 10px;
  outline: none;
}
</style>
