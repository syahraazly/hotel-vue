//
<script setup>
//     // import {ref} from 'vue';
//     import flatPickr from 'vue-flatpickr-component';
//     import 'flatpickr/dist/flatpickr.css';
//     // import func from 'vue-editor-bridge';

//     // const date = ref(null);
</script>
<template>
  <div class="container" style="padding: 50px 0 50px 0">
    <h2 style="text-align: center">Booking Information</h2>
    <h6 style="color: #b0b0b0; text-align: center; margin-bottom: 60px">
      Please fill up the blank fields below
    </h6>
    <div class="row">
      <div class="col-lg-8" v-for="type in list_type" :key="type.type_id">
        <h4 style="text-transform: capitalize">{{ type.type_name }} Room</h4>
        <img
          :src="'http://127.0.0.1:8000/images/' + type.photo_name"
          alt="Gambar"
          style="width: 45rem; margin-bottom: 2rem"
        />
      </div>
      <div class="col-lg-4">
        <h4>Booking Room</h4>
        <!-- <div style="border: 1px solid black; padding-left: 20px; padding-right: 50px"> -->
        <label for="">Customer Name</label>
        <input
          type="text"
          v-model="customer_name"
          class="form-control"
          required
        />

        <label for="">Customer Email</label>
        <input
          type="email"
          v-model="customer_email"
          class="form-control"
          required
        />

        <label for="">Check In</label>
        <flat-pickr
          v-model="check_in"
          :config="checkInConfig"
          class="form-control"
          placeholder="Select date"
          name="date"
          :value="sessionStorage ? sessionStorage.getItem('check_in') : ''"
        />
        {{ check_in }}

        <!-- add this line -->
        <label for="">Check Out</label>
        <flat-pickr
          v-model="check_out"
          :config="checkOutConfig"
          class="form-control"
          placeholder="Select date"
          name="date"
          :value="sessionStorage ? sessionStorage.getItem('check_out') : ''"
        />
        {{ check_out }}
        <!-- add this line -->

        <!-- <div>Check In: {{ sessionStorage.getItem('check_in') }}</div>
<div>Check Out: {{ sessionStorage.getItem('check_out') }}</div> -->

        <label for="">Guest Name</label>
        <input type="text" v-model="guest_name" class="form-control" required />

        <label for="">Rooms Amount</label>
        <input
          type="number"
          v-model="rooms_amount"
          class="form-control"
          required
        />

        <input type="hidden" v-model="type_id" class="form-control" required />

        <button v-on:click="bookNow()" class="btn btn-warning mt-4">
          Book Now
        </button>
      </div>
    </div>
    <router-link class="btn btn-secondary" to="/booknow">Cancel</router-link>
  </div>
</template>

<script>
import flatPickr from "vue-flatpickr-component";
import "flatpickr/dist/flatpickr.css";
// import { fpFormatDate } from "vue-flatpickr-component";

export default {
  name: "BookView",
  components: {
    flatPickr,
  },
  data() {
    return {
      // check_in:null,
      // check_out: null,
      // checkInConfig: {
      //     dateFormat: "Y-m-d",
      //     onChange: () => {
      //         this.checkOutConfig.minDate = this.check_in;
      //     }
      // },
      // checkOutConfig: {
      //     dateFormat: "Y-m-d",
      //     disable: (date) => {
      //         const minDate = fpFormatDate(this.check_in, "Y-m-d");
      //         return date < minDate;
      //     }
      // },
      // check_in : localStorage.getItem('check_in'),
      // check_out : localStorage.getItem('check_out'),
      date: null,
      list_type: [],
      type_id: this.$route.params.type_id,
    };
  },
  methods: {
    // getData: function(){
    //     this.axios.get('/type').then(resp => {
    //         this.list_type = resp.data;
    //     })
    // },
    getDetail: function () {
      this.axios.get("/type/" + this.$route.params.type_id).then((resp) => {
        this.list_type = resp.data;
        console.log(this.list_type);
      });
    },
    bookNow: function () {
      let form = {
        customer_name: this.customer_name,
        customer_email: this.customer_email,
        check_in: this.check_in,
        check_out: this.check_out,
        guest_name: this.guest_name,
        rooms_amount: this.rooms_amount,
        type_id: this.type_id,
      };

      this.axios.post("/orders", form).then((resp) => {
        window.open("/orders/receipt/" + resp.data.data.order_number, "_blank");
        console.log(resp.data.data.order_number);
        // this.$router.push('/orders/receipt/' + this.receipt.order_number)
      });
    },
  },
  mounted() {
    // this.getData()
    this.getDetail();
  },
};
</script>
