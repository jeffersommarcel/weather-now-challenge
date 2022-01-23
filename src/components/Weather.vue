<template>
  <div class="card">
    <div class="header-card">
      {{titleCard}}
    </div>
    <div class="card-content" v-if="status">
      <Loading :active='isLoading'/>
      <div class="body-card">
        <p :class='classColor'>{{temp}}º</p>
      </div>
      <div class="footer-card">
        <div>
          <span>
            HUMIDITY
            <p>{{humidity}}%</p>
          </span>
          <span>
            PRESSURE
            <p>{{pressure}}hPa</p>
          </span>
        </div>
        <p>Updated at {{updateAt}}</p>
      </div>
    </div>
    <div class="try-again" v-else>
      <p>Something went wrong</p>
      <button class="try-aga" @click="getTemp()">Try Again</button>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';

  import Loading from '@/components/Loading.vue';

  export default {
    props: {
      city: String,
      country: String
    },
    components: {
      Loading
    },
    data() {
      return {
        isLoading: false,
        temp: null,
        humidity: null,
        pressure: null,
        updateAt: null,
        status: true,
        id: this.city+','+this.country,
        search: {
          name: null,
          country: null
        }
      }
    },
    created() {
      const validation = this.verifyStorage();

      if(validation) {
        this.getTemp();
      }

      setInterval(() => {
        this.getTemp();
      }, 600000);

    },
    methods: {
      getTemp(){
        this.isLoading = true;
        this.status = true;
        axios
        .get('https://api.openweathermap.org/data/2.5/weather', {params: {q: this.city, units: 'metric', appid: 'a109d4a8eb934f1ec4d2c1f3fb39459f'}})
        .then((response) => {
          const now = new Date();

          const data = {
            temp: response.data.main.temp,
            humidity: response.data.main.humidity,
            pressure: response.data.main.pressure,
            city: response.data.name,
            country: response.data.sys.country,
            createdAt: now,
            updatedAt: now.toLocaleString('en-US', {hour: 'numeric', minute: 'numeric', hour12: true})
          };

          this.mountLayout(data);
          this.saveSearch(data);

          setTimeout(() => this.isLoading = false, 1500)
        })
        .catch(() => {
          this.status = false;
        });
      },
      verifyStorage(){
        const storage = localStorage.getItem(this.id);

        if(storage != null){
          const obj = JSON.parse(storage);

          let expires = new Date(obj.createdAt);

          expires.setMinutes(expires.getMinutes() + 10);

          if(new Date() > expires) {
            return true;
          }

          this.mountLayout(obj);
        }

      },
      mountLayout(data){
        this.temp = data.temp;
        this.humidity = data.humidity;
        this.pressure = data.pressure;
        this.updateAt = data.updatedAt;

        this.search.name = data.city;
        this.search.country = data.country;

        this.status = true;
      },
      saveSearch(data) {
        localStorage.setItem(this.id, JSON.stringify(data));
      }
    },
    computed: {
      classColor(){
        let className = null;
        if (this.temp <= 5) {
          className = "color-blue";
        } else if ( this.temp > 5 && this.temp <= 25) {
          className = "color-orange";
        } else if ( this.temp > 25) {
          className = "color-red";
        } else {
          className = ""
        }
        return className;
      },
      titleCard() {
        let fullName = this.city.charAt(0).toUpperCase()+ this.city.slice(1)+', '+this.country.toUpperCase();
        return fullName;
      }
    }
  }
</script>

<style lang="scss">
  @import "./src/assets/scss/_variables.scss";

  .card{
    background:white;
    width:23%;
    margin:0rem 2.8rem;
    box-shadow: $box-shadow 0px 2px 8px 0px;
    border-radius: 0.2rem;

    &:not(:first-child):not(:last-child){
      transform: scale(1.2);
    }

    .header-card{
      width:100%;
      border-bottom:1px solid $divider;
      padding:0.8rem;
      text-align:center;

      font-size:1.3rem;
      color:$gray;
    }

    .card-content{
      position:relative;
    }

    .body-card{
      height:9.5rem;

      display:flex;
      align-items:center;
      justify-content: center;

      p {
        font-size: 3.8rem;
        font-weight: 500;
      }
    }

    .footer-card{
      background:$card-footer-background;

      padding:1.2rem 0.1rem 0.5rem 0.1rem;

      div{
        display:grid;
        grid-template-columns: 1fr 1fr;

        span{
          width:100%;

          text-align:center;
          color:$gray;
          font-weight: 500;
          font-size: 0.85rem;

          p{
            margin:1px;
            font-size: 0.80rem;
            color:$dark-gray;
            font-weight: 600;
          }

        }
      }
    
      p{
        margin-top:10px;
        text-align:center;
        width:100%;
        font-size: 0.70rem;
        color: $gray;
      }
    }

    .try-again{
      height: 12.8rem;
      text-align:center;
      padding-top:3rem;

      p {
        color:$red;
      }

      button {
        background:white;
        color:$gray;
        padding:0.5rem 1.7rem;
        margin-top:0.8rem;
        
        border-radius: 1.8rem;
        border: 2px solid $gray;

        font-size: 1rem;

        transition: 0.2s;

        &:hover {
          background:$gray;
          color:white;
          transition: 0.2s;
        }
      }

    }
  }

  .display-none {
    display: none;
  }

  .color-orange{
    color:$orange;
  }
  .color-blue{
    color:$blue;
  }
  .color-red{
    color:$red;
  }


</style>
