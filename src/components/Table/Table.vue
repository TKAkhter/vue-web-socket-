<script>
import TableRows from "./TableRows.vue";
import "./Table.css";

export default {
    props: {
        rowsData: Array,
    },
    components: {
        TableRows
    },
    data() {
        return {
            ws: [],
            isinValue: "",
            error: "",
            validClass: ""
        }
    },
    methods: {
        deleteTableRows(e) {
            console.log("🚀 ~ file: Table.vue ~ line 21 ~ deleteTableRows ~ e", e);
            console.log("🚀 ~ file: Table.vue ~ line 21 ~ deleteTableRows ~ e", this.rowsData);
        }
    },
    mounted() {
        console.log("🚀 ~ file: Table.vue ~ line 21 ~ deleteTableRows ~ e", this.rowsData);
  },
}
</script>
<template>
    <div className="table-section">
        <div className="container">
            <div className="row">
                <div className="col-sm-8">
                    <h2>ISIN WatchList</h2>
                    <table className="table">
                        <thead>
                            <tr>
                                <th>ISIN</th>
                                <th>Price</th>
                                <th>Bid</th>
                                <th>Ask</th>
                                <th>Bid-Ask Spread</th>
                                <th>Status</th>
                            </tr>
                        </thead>
                        <tbody>
                            <TableRows :rowsData="rowsData" :deleteTableRows=deleteTableRows />
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
</template>
