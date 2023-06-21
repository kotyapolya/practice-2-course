<template>
    <div class="container">
        <div class="search">
            <img @click="apply_filter" src="./../assets/search.png" alt="Previoos">
            <input id="serch-input" type="text" placeholder="Enter music" v-model="filter">
        </div>
    </div>
</template>

<script>
    export default {
        name: "Music-search",
        data() {
            return {
                filter: ''
            }
        },
        methods: {
            apply_filter() {
                this.$emit('apply_filter', this.filter);
            }
        }
    }


</script>

<style scoped>
    .search {
        height: 80px;
        margin: 0;
        padding: 0 20px;
        background-color: #8193F5;
        display: flex;
        flex-direction: row;
        align-items: center;

    }

    .search input {
      color:black;
    }

    #serch-input {
        font-family: inherit;
        border: 0;
        border-bottom: 2px solid black;
        outline: 0;
        font-size: 1.3rem;
        color: white;
        padding: 10px 10px;
        background: transparent;
        transition: border-color 0.2s;

    }

    #serch-input:focus {
        padding-bottom: 6px;
        font-weight: 700;
        border-width: 2px;

        border-image-slice: 1;
    }
</style>