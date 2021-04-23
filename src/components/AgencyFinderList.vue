<template>
    <div class="listing-container" v-if="matches">
        <ul>
            <li class="listing-cards" v-for="match in matches" :key="match.properties.name">
                <h4 style="font-size: 1.1rem; color: #3b3052; margin-bottom: .75em">{{match.properties.name}}</h4>
                <div class="address">
                    <p v-if="match.properties.address_2">
                        {{match.properties.address_1}}, {{match.properties.address_2}} <br/> {{match.properties.city}}, {{match.properties.state}} {{match.properties.zipcode}} 
                    </p>
                    <p v-else>
                        {{match.properties.address_1}} <br/> {{match.properties.city}}, {{match.properties.state}} {{match.properties.zipcode}}
                    </p>
                </div>
                <div class="phone"> 
                    <p v-if="match.properties.phone">
                        {{this.phoneConvert(match.properties.phone)}}
                    </p>
                </div>
                <div v-if="match.properties.email">
                    <div style="margin-bottom: .3em;" v-for="email in match.properties.email" :key="email">
                        <a class="email" :href="`mailto:${email}`">{{email.toLowerCase()}}</a><br/>
                    </div>
                </div>
                <div style="margin-top: 1em">
                    <a class="directions" href="#">Get Directions</a>
                </div>
            </li>
        </ul>
    </div>
</template>

<script>
    export default {
        name:  "List",
        props: ["matches"],
        methods: {
            phoneConvert: function(phone) {
                return phone.replace(/[^0-9]/g, '')
                .replace(/(\d{3})(\d{3})(\d{4})/, '($1) $2-$3');
            }
        },
    }
</script>

<style>
    .listing-container {
        color: #2F2047;
        font-size: 1.4em;
        height: 100%;
    }
    .listing-cards {
        list-style-type: none;
        padding: 1.5em;
        border-bottom: .1em solid #e5e5e4;
        font-size: .7em;
        line-height: 1.2em;
        color: #2F2047;
    }
    .email {
        text-decoration: none;
        font-size: 1em;
        color: #2F2047;
    }
    .address, .phone{
        margin: .7em 0;
        line-height: 1.2em;
    }
    .directions {
        text-transform: uppercase;
        font-size: .85em;
        font-weight: bold;
        color: #2F2047;
        text-decoration: none;
        padding-bottom: .5em;
        border-bottom: 2px solid #815F99
    }
</style>