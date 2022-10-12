<script>
import validator from "isin-validator";
import './index.css';
import Header from './components/Header.vue';
import TextField from './components/TextField.vue';
import Card from './components/Card.vue';
import Button from './components/Button.vue';
// import Table from './components/Table/Table.vue';
import Alert from './components/Alert.vue'
import Form from './components/Form.vue'

let id = -1;

const isinClean = (isin) => isin.replace(/\s|-/g, "");

export default {
  components: {
    Header,
    TextField,
    Card,
    Form,
    Button,
    Alert,
  },
  data() {
    return {
      ISIN: [],
      alert: false,
      ws: null,
      isinValue: "",
      error: "",
      validClass: "",
      rowData: [],
      columns: ['price', 'bid', 'ask']
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
      localStorage.setItem('ISIN', JSON.stringify(this.ISIN));
      this.isinValue = ""
      this.validClass = "";
    },
    deleteRow(idx) {
      this.stopMessage(this.ISIN[idx]);
      this.ISIN.splice(idx, 1);
      this.rowData.splice(idx, 1);
      localStorage.setItem('ISIN', JSON.stringify(this.ISIN));
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
      // this.ws.onopen = () => this.ws.send(JSON.stringify(msg));
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

      this.ws.onopen = function () {
        console.log("Successfully connected to the echo websocket server...")
      };

      this.ws.addEventListener("close", (event) => {
        this.alert = true;
        console.log(
          "Socket is closed. Trying to Reconnect...",
          event.reason
        );
        this.getStream();
        this.ws.addEventListener("open", () => {
          this.sendMessage(this.ISIN);
          this.alert = false;
        })
        // this.ws.onopen = () => this.sendMessage(this.ISIN);
        // this.alert = false;
        // this.sendMessage(this.ISIN);
      });
    }
  },
  created() {
    
  },
  mounted() {
    const storeISIN = JSON.parse(localStorage.getItem('ISIN'));
    if(!storeISIN) return;
    this.getStream();
    for (let i = 0; i < storeISIN.length; i++) {
      this.ws.addEventListener("open", () => {
        id++;
        console.log(storeISIN[i],'storeISIN');
        this.ISIN.push(storeISIN[i]);
      });
    }
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
    <Alert v-if="alert" />
    <Form :handleSubmit="handleSubmit" :isinValue="isinValue" :handleValidation="handleValidation" />
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
                  <th>ISIN</th>
                  <th v-for="(column, index) in columns" :key="index"> {{column}}</th>
                  <th>Bid-Ask Spread</th>
                  <th>Action</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(row, index) in rowData" :key="index">
                  <td>{{row['isin']}}</td>
                  <td v-for="(column, indexColumn) in columns" :key="indexColumn">{{parseFloat(row[column]).toFixed(3)}}
                  </td>
                  <td>{{(parseFloat(row['ask']- row['bid'])/row['ask']).toFixed(5)}}</td>
                  <td><button class="delete-button" @click="deleteRow(index)">X</button></td>
                </tr>
              </tbody>
            </table>
            <!-- <Table :rowData="rowData" :columns="columns" :deleteRow="deleteRow" /> -->
          </div>
        </div>
      </div>
    </div>
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

.table-section {
  padding: 20px;
  background-color: #fff;
  margin: 40px;
  border-radius: 5px;
  box-shadow: 0px 4px 4px rgb(0 0 0 / 25%);
  overflow-x: auto;
}

.table-section h2 {
  color: #000000;
  padding-bottom: 10px;
  margin-top: 0px;
  margin-bottom: 10px;
  display: block;
  font-family: 'Urbanist', sans-serif;
}

.table {
  width: 100%;
  text-align: center;
}

.table th {
  font-size: 20px;
  text-transform: capitalize;
  padding: 10px 5px;
  /* border: 1px solid; */
  background: #041e42;
  color: #fff;
  font-weight: 500;
}

.table td {
  padding: 10px 5px;
  font-size: 18px;
  width: 20%;
}

.table tr:nth-child(odd) {
  background-color: #fafafa;
}

table thead tr th:first-child {
  border-top-left-radius: 5px;
}

table thead tr th:last-child {
  border-top-right-radius: 5px;
}

table tbody:last-child tr:last-child td:first-child {
  border-bottom-left-radius: 5px;
}

table tbody:last-child tr:last-child td:last-child {
  border-bottom-right-radius: 5px;
}
</style>
