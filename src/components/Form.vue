<script>

import Button from './Button.vue';

export default {
    props: {
        rowsData: Array,
        isinValue: String,
        handleSubmit: Function,
        handleValidation: Function,
    },
    components: {
        Button,
    },
}
</script>

<template>
    <form @submit.prevent="handleSubmit">
        <div className="input-box">
            <input :class="validClass" placeholder="Enter ISIN to add on Wishlist" name="isin" type="text"
                :value="isinValue" @input="handleValidation" />
        </div>
        <div className="button-row">
            <Button type="submit" text="Submit" />
        </div>
    </form>
</template>

<style>
form {
    display: flex;
    align-items: center;
    flex-wrap: wrap;
    justify-content: center;
}

.input-box {
    width: -webkit-fill-available;
    margin-right: 10px;
    flex: 6;
}

.input-box label {
    color: #999;
    padding-bottom: 10px;
    display: block;
    font-family: 'Urbanist', sans-serif;
}

.input-box input {
    width: -webkit-fill-available;
    padding: 15px;
    border-radius: 5px;
    background: #ffffff;
    font-family: 'Urbanist', sans-serif;
    pointer-events: all;
}

.input-box input:active,
.input-box input:focus,
.input-box input:focus-visible {
    outline: none;
}

input:focus {
    border-color: #86b7fe;
    box-shadow: 0 0 0 0.25rem rgba(13, 110, 253, 0.25);
}

input {
    transition: border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
    border-width: 1px;
    border-style: solid;
    border-color: #ededed;
}

input.success {
    border-color: #198754;
    background-image: url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 8 8'%3e%3cpath fill='%23198754' d='M2.3 6.73L.6 4.53c-.4-1.04.46-1.4 1.1-.8l1.1 1.4 3.4-3.8c.6-.63 1.6-.27 1.2.7l-4 4.6c-.43.5-.8.4-1.1.1z'/%3e%3c/svg%3e");
}

input.error {
    border-color: #dc3545;
    background-image: url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 12 12' width='12' height='12' fill='none' stroke='%23dc3545'%3e%3ccircle cx='6' cy='6' r='4.5'/%3e%3cpath stroke-linejoin='round' d='M5.8 3.6h.4L6 6.5z'/%3e%3ccircle cx='6' cy='8.2' r='.6' fill='%23dc3545' stroke='none'/%3e%3c/svg%3e");
}

input.success,
input.error {
    background-repeat: no-repeat;
    background-position: right calc(0.375em + 0.1875rem) center;
    background-size: calc(0.75em + 0.375rem) calc(0.75em + 0.375rem);
}

input.success:focus {
    box-shadow: 0 0 0 0.25rem rgba(25, 135, 84, 0.25);
}

input.error:focus {
    border-color: #dc3545;
    box-shadow: 0 0 0 0.25rem rgb(220 53 69 / 25%);
}

.invalid-error {
    font-size: 0.85rem;
    color: #dc3545;
    display: block;
}

.info {
    color: #BC514C;
    padding-bottom: 10px;
    font-size: 0.85rem;
}

.button-row button {
    background: #0e3386;
    flex: 1;
}
</style>
