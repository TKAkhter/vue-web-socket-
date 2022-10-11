<template>
    <div id="slider" class="slide-in orange" v-for="event in eventsFromServer" :key="event.time">
        <ul>
            <p>
                {{ new Date(event.time).toISOString() }}
            </p>
            <li>
                {{ event.data }}
            </li>
        </ul>
    </div>
</template>
  
<script>
import { ref } from 'vue';

export default {
    name: 'WebSocket',
    setup() {
        const eventsFromServer = ref([
            // { data: 'hii', time: '32234' },
            // { data: 'hii324', time: '3223422' },
        ]);
        const serverUrl = "ws://159.89.15.214:8080/";

        const socket = new WebSocket(serverUrl);
        socket.addEventListener('open', (event) => {
            socket.send({
                "subscribe": "DE000BASF111"
            });
        });

        socket.addEventListener('message', function (event) {
            console.log('Message from server ', event.data);
            eventsFromServer.value.push({ time: new Date(), data: event.data });
        });

        return { eventsFromServer };
    },
};
</script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
body {
    cursor: default;
    font-size: 14px;
    line-height: 21px;
    font-family: 'Segoe UI', 'Helvetica', Garuda, Arial, sans-serif;
    padding: 18px 18px 18px 18px;
}

ul {
    margin-bottom: 14px;
    list-style: none;
}

li {
    /* width: 300px; */
    height: 30px;
    margin: 0 0 7px 0;
}

.orange {
    border-left: 5px solid #f5876e;
}

#slider {
    /* position: absolute; */
    transform: translateX(-100%);
    -webkit-transform: translateX(-100%);
}

.slide-in {
    animation: slide-in 0.5s forwards;
    -webkit-animation: slide-in 0.5s forwards;
}

.slide-out {
    animation: slide-out 0.5s forwards;
    -webkit-animation: slide-out 0.5s forwards;
}

@keyframes slide-in {
    100% {
        transform: translateX(0%);
    }
}

@-webkit-keyframes slide-in {
    100% {
        -webkit-transform: translateX(0%);
    }
}

@keyframes slide-out {
    0% {
        transform: translateX(0%);
    }

    100% {
        transform: translateX(-100%);
    }
}

@-webkit-keyframes slide-out {
    0% {
        -webkit-transform: translateX(0%);
    }

    100% {
        -webkit-transform: translateX(-100%);
    }
}
</style>
  