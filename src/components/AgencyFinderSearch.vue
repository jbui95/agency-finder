<template>
    <div class="map-container">
        <div class="search-container">
            <form class="search-form">
                <label for="zipcode">Enter your zip code</label>
                <input id="zipcode" type="text" v-model="zipcode" class="search-box" placeholder="Enter a 5-digit zip code" />
                <button class="search-btn" v-on:click="search">search</button>
            </form>
            <AgencyFinderList v-bind:matches="matches"/>
        </div>
        <AgencyFinderMap v-bind:matches="matches" :key="componentKey"/>
    </div>
</template>

<script>
    import AgencyFinderList from "./AgencyFinderList";
    import AgencyFinderMap from "./AgencyFinderMap";
    
    export default {
        props: ['agency'],
        components: {
            AgencyFinderList,
            AgencyFinderMap
        },
        data() {
            return {
                zipcode: null,
                matches: null,
                componentKey: 0,   
            }
        },
        methods: {
            forceRerender: function() {
                this.componentKey += 1;
            },
            search: function(e) {
                var data = [];
                for(let i = 0; i < this.agency.length; i++) {
                    if (this.zipcode == this.agency[i].properties.zipcode) {
                        data.push(this.agency[i]);
                    }  
                }
                if (!data.length) {
                    alert("Sorry, we couldn't find any agencies with the zip code you entered. Please try again!");
                }
                e.preventDefault();
                this.forceRerender();
                return this.matches = data;
            },
        }
    }
</script>

<style>
    .map-container {
        background-color: #ffffff;
        width: 90vw;
        height: 70vh;
        margin: 5em auto;
        display: flex;
        padding: 0;
    }
    input:focus {
        outline: none;
    }
    .search-container {
        width: 25vw;
        overflow-y: scroll;
    }
    .search-form {
        padding: 1.5em;                
        border-bottom: .1em solid #e5e5e4;
        margin: .5em 0;
    }
    .search-box {
        border-radius: 0.3em;
        padding: 1em;
        margin-top: .5em;
        margin-bottom: .5em;
        border: .1em solid #d8d8d6;
        width: 100%;
        box-sizing: border-box;
        background-color: #ffffff;
    }
    label {
        letter-spacing: 2px;
        font-size: .8em;
        font-weight: 600;
    }
    .search-btn {
        border-radius: 0.3em;
        padding: 1em;
        text-transform: uppercase;
        background-color: #815F99;
        color: #ffffff;
        border: none;
        width: 100%;
        letter-spacing: 2px;
    }
    @media only screen and (max-width: 600px) {
        .map-container {
            margin: 0;
            display: block;
        }
        .search-container {
            width: 100%;
        }
    }
    @media only screen and (min-width: 600px) and (max-width: 800px) {
        .map-container {
            margin: 2em 0;
        }
        .search-container {
            width: 35vw;
        }
    }
</style>