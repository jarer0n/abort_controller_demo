<template>
    <div id="app">
        <button @click="loadData">Запрос</button>

        <ul>
            <li v-for="(log, i) in logs" :key="i">{{ log }}</li>
        </ul>
    </div>
</template>

<script>
export default {
    name: 'App',
    components: {},

    data() {
        return {
            logs: [],
            abortController: '',
        };
    },
    methods: {
        loadData() {
            if (this.abortController) {
                this.abortController.abort('');
                this.logs.unshift(
                    'Предыдущий запрос отменен, т.к. начат новый'
                );
            }
            this.abortController = new AbortController();
            const signal = this.abortController.signal;

            fetch('https://jsonplaceholder.typicode.com/photos', { signal })
                .then((response) => response.json())
                .then(() => {
                    this.abortController = '';
                    this.logs.unshift('Ответ получен');
                });
        },
    },
};
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    max-width: 1200px;
    margin: 0 auto;
    color: #2c3e50;
    margin-top: 60px;
}
</style>
