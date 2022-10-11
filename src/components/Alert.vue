<template>
  <div class="alert alert-danger alert-dismissible fade show" role="alert">
    <strong>Reconnecting... </strong>Prices are out of Sync. App is trying to reconnect. If watch list doesn't change,
    Please reload page.
  </div>
</template>
  
<style>
.alert {
  transition: opacity 0.15s linear;
  position: relative;
  padding: 1rem 1rem;
  border: 1px solid transparent;
  border-radius: 0.25rem;
  padding-right: 3rem;
  margin-bottom: 20px;
}

.alert-danger,
.alert-danger * {
  color: #842029;
  background-color: #f8d7da;
  border-color: #f5c2c7;
}

.delete-button {
  padding: 5px 10px;
  background: #dc3545;
  border-radius: 100%;
}
</style>
  