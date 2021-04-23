<template>
  <div id="app">
    <div class="header">
      <div style="display:block">
        <p class="title-caption">Ready to Get Started?</p>
        <h1>Find an Agent <br/> Near You</h1>
      </div>
        <div class="page-caption">
          <p>Openly sells exclusively through independent agents. Use the map below to find a recommended, knowledgeable, and licensed agent near you.</p>
        </div>
    </div>
    <AgencyFinderSearch v-bind:agency="agency"/>  
  </div>
</template>

<script>
  import AgencyFinderSearch from './components/AgencyFinderSearch';
  import axios from 'axios';

  //NOT SECURE: for the sake of this project, I will encode the API key into the code for now. -JAB
  const token = '6081716b28bf9b609975a55c';
  
  export default {
    name: 'App',
    components: {
      AgencyFinderSearch
    },

    data() {
      return {
        agency: null,
      }
    },

    mounted () {
      axios.get('https://insurance-84be.restdb.io/rest/agency-data', {
          headers: {
            "x-apikey":   `${token}`,
            "content-type": "application/json"
          }
        })
        .then(response => (this.agency = response.data[0].features))
      },  
  }
</script>

<style>
@import url("https://use.typekit.net/eka1mni.css");
  * {
      margin: 0;
      padding: 0;
    }
  #app {
    background-color: #F8F2EF;
    margin: 0;
    padding: 1em;
    overflow-y: auto;
  }
  .header {
    display: flex;
    margin-left: 10em;
  }
  .title-caption {
    margin-bottom: 1em;
    text-transform: uppercase; 
    color: #815F99;
  }
  h1 {
    letter-spacing: 3px;
    font-size: 2.4em;
    font-family: bookmania, serif;
    font-weight: lighter;
    color: #2F2047;
    line-height: 1.2em;
  }
  .page-caption {
    width: 35vw; 
    margin: 5em 0 0 10em; 
    letter-spacing: .1em;
  }
  body {
    font-family: libre-franklin, sans-serif;
  } 
  @media only screen and (max-width: 600px) {
    .header {
      margin-left: 1em;
      display: block;
      text-align: center;
    }
    h1 {
      font-size: 1.6em;
    }
    .title-caption {
      font-size: .7em;
    }
    .page-caption {
      margin: 2em 0;
      width: 100%;
      font-size: .7em;
    }
  }
  @media only screen and (min-width: 600px) and (max-width: 800px) {
    .header {
      margin-left: 1em;
    }
    .page-caption {
      width: 40vw;
      margin: 5em 0 0 5em; 
    }
  }
</style>