<script>

export default {
    props: {
        rowsData: Array,
        columns: Array,
        deleteRow: Function
    },
}
</script>
<template>
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
</template>

<style>
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