<template>
  <div id="ordersStats">
      <Admin-nav/>
      <div class="wrapper">
        <div style="margin-top: 50px; margin-bottom: 50px; text-align:center; font-size: 1.2vw">Statystki płatności</div>
        <table class="table">
              <thead style="background-color: aliceblue">
                <tr>
                    <th scope="col">Rok</th>
                    <th scope="col">Miesiąc</th>
                    <th scope="col">Całkowity przychód</th>
                </tr>
            </thead>
            <tbody>
            <tr
                v-for="stat in statistics"
                :key="stat.month"
            >
                <td>{{stat.year}}</td>
                <td>{{stat.month}}</td>
                <td>{{stat.price}} zł</td>
            </tr>
            </tbody>
        </table>
      <div style="margin-top: 50px; margin-bottom: 50px; text-align:center; font-size: 1.2vw">Zamówienia Web</div>
      <table class="table">
            <thead style="background-color: aliceblue">
                <tr>
                    <th scope="col">Id. zamówienia</th>
                    <th scope="col">Cena</th>
                    <th scope="col">Status</th>
                    <th scope="col">Data zamówienia</th>
                    <th scope="col">Data zakończenia</th>
                    <th scope="col">Id. zamawiającego</th>
                </tr>
            </thead>
            <tbody>
            <tr
                v-for="order in paidOrders"
                :key="order.OrderId"
            >
                <td>{{order.OrderId}}</td>
                <td>{{order.OrderPrice}} zł</td>
                <td>{{order.Status}}</td>
                <td>{{order.OrderDate}}</td>
                <td>{{order.EndDate}}</td>
                <td>{{order.UserId}}</td>
            </tr>
            </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import AdminNav from '../../components/admin/AdminNav.vue'

export default {
components: {AdminNav },
    data () {
        return {
            statistics: [],
            paidOrders: [],
            statuses: [],
    }
},
methods: {
getStats() {
    axios.get('ordersStats').then(response => {
        if(response.status >= 200 && response.status < 300){
            this.statistics = response.data
        }
    })
},
getPaidOrders() {
    axios.get('getPaidOrdersWeb').then(response => {
        if(response.status >= 200 && response.status < 300){
            this.paidOrders = response.data
            if(this.paidOrders != ""){
                this.statusNameInsert();
            }
        }
    })
},
getStatuses(){
    axios.get('order-statuses').then(response => {
        if(response.status >= 200 && response.status < 300){
            this.statuses = response.data
        }
    })
},
statusNameInsert(){
    this.paidOrders.forEach(order => {
        for(var i=0 ; i < Object.keys(this.statuses).length; i++){
        if(order.Status == Object.keys(this.statuses)[i]){console.log(order.Status = Object.values(this.statuses)[i])}
        }
    })
},
},
mounted () {
    this.getStats();
    this.getStatuses();
    this.getPaidOrders();
}
}
</script>

<style>
.wrapper{
    width: 90vw;
    margin: auto;
}
</style>