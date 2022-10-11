<script>
const formatValue = (value, d) => {
    return parseFloat(value).toFixed(d);
};

export default {
    props: {
        rowsData: Array,
        deleteTableRows: Function
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
        deleteTableData(e) {
            // let rowUpdate = row;
            // delete rowUpdate[index];
            // setRow(row);
            console.log("🚀 ~ file: Table.vue ~ line 21 ~ deleteTableRows ~ e", e);
            console.log("🚀 ~ file: Table.vue ~ line 21 ~ deleteTableRows ~ this.rowsData", this.rowsData);
        }
    }
}
// console.log("🚀 ~ file: Table.vue ~ line 21 ~ deleteTableRows ~ rowsData", props);
</script>

<template>
    <tr v-for="row in rowsData" :key="row.id">
        <td>
            <p>isin</p>
        </td>
        <td>
            <p>price</p>
        </td>
        <td>
            <p>bid</p>
        </td>
        <td>
            <p>ask</p>
        </td>
        <td>
            <p>
                bid-ask
            </p>
        </td>
        <td>
            <button className="btn btn-table inactive" @click=deleteTableData(e)>
                Delete
            </button>
        </td>
    </tr>
</template>