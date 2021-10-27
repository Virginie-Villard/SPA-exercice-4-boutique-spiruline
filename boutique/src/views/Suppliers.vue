<template>
  <div class="suppliers">

    <section class="displaySuppliers" v-if="errored">
      <h3>
        We're sorry, we're not able to retrieve this information at the moment, please try back later
      </h3>
    </section>

    <section v-else>
      <h3 class="displayLoading" v-if="loading">
        Loading...
      </h3>

      <City class="displaySupplier" v-else v-for="item in suppliers" :key="item.id" :name="item.name" :weather="item.weather[0].description" :temperature="item.main.temp" :date="item.dt*1000" />
      
    </section>

  </div>
</template>

<script>
// @ is an alias to /src
import Supplier from '@/components/Supplier.vue'
import axios from 'axios';

export default {
  name: 'Suppliers',
  components: {
    Supplier
  },

  data( {
      return {
          suppliers:[],
          loading: true,
          error: false
      }
  })

  mounted() {
      axios
      .get('https://api.openweathermap.org/data/2.5/find?lat=45.188&lon=5.724&cnt=20&cluster=yes&lang=fr&units=metric&APPID=672927972a328d5ebce768af4abcb9c5')
      
      .then(response => {
        this.suppliers = response.data.list
        console.log(response)
      })

      .catch(error => {
        console.log(error)
        this.errored = true
      })

      .finally(() => this.loading = false)
  }
}
</script>