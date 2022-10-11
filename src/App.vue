<script>
import validator from "isin-validator";
import './index.css'
import './components/Form/Form.css'
import './components/Table/Table.css'
import Header from './components/Header.vue'
import TextField from './components/TextField.vue'
import Card from './components/Card.vue'
import Button from './components/Button.vue'

const isinClean = (isin) => isin.replace(/\s|-/g, "");

let id = 0;

export default {
  components: {
    Header,
    TextField,
    Card,
    Button
  },
  data() {
    return {

      text: '',
      ISIN: [],
      ws: [],
      isinValue: "",
      error: "",
      validClass: ""
    }
  },
  methods: {
    onInput(e) {
      this.text = e.target.value
    },
    handleValidation(e) {
      const value = isinClean(e.target.value);
      this.isinValue = value;

      if (!value) {
        this.validClass = "error";
        return this.error = "Please provide a valid ISIN";
      } else {
        this.validClass = "success";
        this.error = "";
      }
      if (validator(value)) {
        this.validClass = "error";
        return this.error = "ISIN is invalid!";
      }
    },
    handleSubmit(e) {
      const value = isinClean(e.target.isin.value);

      if (!value) {
        this.validClass = "error";
        return this.error = "ISIN is empty, please add enter correct ISIN";
      }
      if (validator(value)) {
        this.validClass = "error";
        return this.error = "ISIN is invalid!";
      }
      if (this.ISIN.includes(value)) {
        this.validClass = "error";
        return this.error = "ISIN is already in Watch List";
      }
      this.ISIN.push(value);
      this.isinValue = ""
    },
  }
}

</script>

<template>
  <Header />
  <main class="main">
    <form @submit.prevent="handleSubmit">
      <div className="input-box">
        <input :class="validClass" placeholder="Enter ISIN to add on Wishlist" name="isin" type="text"
          :value="isinValue" @input="handleValidation" />
      </div>
      <div className="button-row">
        <Button type="submit" text="Button Label" />
      </div>
    </form>
    <p className="invalid-error">{{error ? error : null}}</p>
    <p className="info">Ex: US0378330015, US0004026250 or DE000BASF111</p>
    <li v-for="item in ISIN" :key="item.id">
      {{ item.value }}
    </li>
    <TextField placeholder="Placeholder" />
    <Card>
      <p>Generic card content</p>
      <p>
        Lorem ipsum dolor sit amet consectetur adipisicing elit. Reiciendis ex, totam ducimus quam eligendi quasi
        laborum optio, tenetur, aperiam reprehenderit voluptates? A velit quia inventore amet excepturi pariatur
        praesentium. Quibusdam.
      </p>
    </Card>
    <Button text="Button Label" />
    <!-- <WebSocket /> -->
  </main>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.main {
  max-width: var(--max-width);
  margin: 0 auto;
}

.component-list {
  padding: 0 0 0 12px;
}
</style>
