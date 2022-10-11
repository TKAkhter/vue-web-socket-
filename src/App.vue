<script>
import validator from "isin-validator";
import './index.css';
import './App.css';
import './components/Form/Form.css';
import './components/Table/Table.css';
import Header from './components/Header.vue';
import TextField from './components/TextField.vue';
import Card from './components/Card.vue';
import Button from './components/Button.vue';
import Table from './components/Table/Table.vue';

let id = -1;

const isinClean = (isin) => isin.replace(/\s|-/g, "");

export default {
  components: {
    Header,
    TextField,
    Card,
    Button,
    Table,
  },
  data() {
    return {
      ISIN: [],
      ws: null,
      isinValue: "",
      error: "",
      validClass: "",
      rowData: [],
      columns: ['isin', 'price', 'bid', 'ask']
    }
  },
  methods: {
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
      id++;
      this.ISIN.push(value);
      this.isinValue = ""
      this.validClass = "";
    },
    deleteRow(idx) {
      this.stopMessage(this.ISIN[idx]);
      this.ISIN.splice(idx, 1);
      this.rowData.splice(idx, 1);
    },
    sendMessage(isin) {
      const msg = {
        "subscribe": isin
      }
      this.ws.send(JSON.stringify(msg));
    },
    stopMessage(isin) {
      const msg = {
        "unsubscribe": isin
      }
      this.ws.send(JSON.stringify(msg));
    },
    getStream() {
      console.log("Starting ws to WebSocket Server")
      this.ws = new WebSocket("ws://159.89.15.214:8080/")

      this.ws.addEventListener("message", (event) => {
        const parseData = JSON.parse(event.data);
        if (id === 0) {
          this.rowData[id] = parseData;
          console.log(this.rowData, 'single stream');
        } else {
          for (let i = 0; i < this.ISIN.length; i++) {
            if (this.ISIN[i] == parseData.isin) {
              this.rowData[i] = parseData;
              console.log(this.rowData, 'multi stream');
            }
          }
        }
      })

      this.ws.onopen = function (event) {
        console.log("Successfully connected to the echo websocket server...")
      }

      this.ws.addEventListener("close", (event) => {
        alert(
          "Prices are out of Sync.App is trying to reconnect. If price doesn't change, Please reload page"
        );
        console.log(
          "Socket is closed. Reconnect will be attempted in 5 seconds",
          event.reason
        );
        this.getStream();
        this.ws.addEventListener("open", () => {
          this.sendMessage(this.ISIN);
        })
      });
    }
  },
  created() {
    this.getStream();
  },
  watch: {
    ISIN: {
      handler: function (val) {
        let vm = this;
        val.filter(function (p, idx) {
          return Object.keys(p).some(function (prop) {
            let diff = p[prop] !== vm.ISIN[idx][prop];
            if (diff) {
              p.changed = true;
            }
          })
        });
        this.sendMessage(val)
      },
      deep: true
    }
  }
}

</script>

<template>
  <Header />
  <main class="main">
    <div class="alert alert-danger alert-dismissible fade show" role="alert">
      <strong>Holy guacamole!</strong> You should check in on some of those fields below.
      <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
    </div>
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
    <div className="table-section">
      <div className="container">
        <div className="row">
          <div className="col-sm-8">
            <h2>ISIN WatchList</h2>
            <table className="table">
              <thead>
                <tr>
                  <th v-for="(column, index) in columns" :key="index"> {{column}}</th>
                  <th>Bid-Ask Spread</th>
                  <th>Action</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(row, index) in rowData" :key="index">
                  <td v-for="(column, indexColumn) in columns" :key="indexColumn">{{row[column]}}</td>
                  <td>{{(row['ask']- row['bid'])/row['ask']}}</td>
                  <td><button @click="deleteRow(index)">X</button></td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
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
  padding: 24px;
}

.component-list {
  padding: 0 0 0 12px;
}
</style>
