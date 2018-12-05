<template>
  <div id="app" style="padding:30px;">
    <h1>Assignment 2</h1>
    <h2>Food List</h2>
    <ul>
      <li v-for="food in foods">{{ food }}</li>
    </ul>

    <h2>Detailed Food List</h2>
    <ul>
      <li v-for="foods in detailfoods">
        <b>{{ foods.name }}</b> - {{ "Category: " + foods.category }} -
        {{ "Price:   $" + foods.price }}
      </li>
    </ul>

    <h2>Food List by Categories</h2>
    <ul>
      <li v-for="foods in foodByCategories">
        <b>{{ foods.name }}</b>
        <ul>
          <li v-for="item in foods.items">{{ item }}</li>
        </ul>
      </li>
    </ul>

    <h2>Sold Food List</h2>

    <table class="table table-striped">
      <thead>
        <tr>
          <th>Name</th>
          <th>Sold Quantity</th>
          <th>Price</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in soldfood">
          <td>{{ item.name }}</td>
          <td>{{ item.soldQuantity }}</td>
          <td>${{ item.price }}</td>
        </tr>
      </tbody>
      <tfoot v-if="soldfood">
        <tr>
          <td></td>
          <td><b>Total</b></td>
          <td>
            <b>${{ totalOrders(soldfood.price) }}</b>
          </td>
          <td></td>
        </tr>
      </tfoot>
    </table>

    <br />
    <h2>Loss and Gain Amount with Food List</h2>

    <table class="table table-striped">
      <thead>
        <tr>
          <th>Name</th>
          <th>Bought Price</th>
          <th>Sold Price</th>
          <th>Gain or Loss in Price</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in pricingfoodList">
          <td>{{ item.name }}</td>
          <td>${{ item.boughtPrice }}</td>
          <td>${{ item.soldPrice }}</td>

          <td v-if="item.boughtPrice > item.soldPrice">
            <b>
              Loss in Price: ${{
                (item.boughtPrice - item.soldPrice).toFixed(2)
              }}</b
            >
          </td>
          <td v-else>
            Gain in price: ${{ (item.soldPrice - item.boughtPrice).toFixed(2) }}
          </td>
        </tr>
      </tbody>
    </table>

    <br /><br />

    <h2>display only the losses</h2>

    <table class="table table-striped">
      <thead>
        <tr>
          <th>Name</th>
          <th>Bought Price</th>
          <th>Sold Price</th>
          <th>Loss in Price</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in pricingfoodList">
          <td>{{ item.name }}</td>
          <td>${{ item.boughtPrice }}</td>
          <td>${{ item.soldPrice }}</td>

          <td v-if="item.boughtPrice > item.soldPrice">
            <b>
              Loss in price: ${{
                (item.boughtPrice - item.soldPrice).toFixed(2)
              }}</b
            >
          </td>
          <td v-else></td>
        </tr>
      </tbody>
    </table>

    <br /><br />

    <h2>Categorized Food List</h2>
    <div v-for="food in CategorizedfoodList.foods">
      <div
        v-for="product in CategorizedfoodList.categories"
        v-if="food.categoryId === product.id"
      >
        <p>
          <b>{{ food.name }}</b> - {{ product.name }}
        </p>
      </div>
    </div>

    <br /><br />

    <h2>Activities of Daily Living</h2>
    <ul>
      <li v-for="activities in activitiesList">{{ activities }}</li>
    </ul>

    <br /><br />

    <h2>Categorized activities</h2>
    <ul>
      <li v-for="activities in categorizedActivitesList">
        <b>{{ activities.name }}</b>
        <ul>
          <li v-for="item in activities.items">{{ item }}</li>
        </ul>
      </li>
    </ul>

    <br /><br />

    <h2>Merge Categorized Activities</h2>
    <ul>
      <!-- <b>{{// mergeActivities() }}</b> -->
      <li v-for="activities in mergeActivities()">
        <b> {{ activities.name }} </b>

        <ul>
          <li v-for="item in activities.items">{{ item }}</li>
        </ul>
      </li>
    </ul>
    {{ testResponse }}
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "App",
  mounted() {
    axios.get("/foods-list.json").then(response => {
      this.foods = response.data.foods;
    });

    axios.get("/detailed-foods-list.json").then(response => {
      //this.testResponse = response;
      this.detailfoods = response.data.foods;
    });

    axios.get("/categorized-foods-list.json").then(response => {
      this.foodByCategories = response.data.categories;
    });

    axios.get("/sold-foods-list.json").then(response => {
      this.soldfood = response.data.foods;
    });

    axios.get("/pricing-food-list.json").then(response => {
      this.pricingfoodList = response.data.foods;
    });

    axios.get("/complex-cateogorized-food-list.json").then(response => {
      this.CategorizedfoodList = response.data;
    });

    axios.get("/activities-list.json").then(response => {
      this.activitiesList = response.data.activities;
    });

    axios.get("/categorized-activites-list.json").then(response => {
      this.categorizedActivitesList = response.data.activities;
    });

    axios.get("/categorized-activites-list2.json").then(response => {
      this.categorizedActivitesList2 = response.data.activities;
    });

    // 9. create your own 'categorized-activities-list.json' file and display a categorized list of lists in the html using axios
    // 10. create another 'categorized-activities-list-2.json'' file and display a SINGLE cateogrized list of lists, combining the
    // lists from your 2 categorized-acitvities-list.json files. You will need to make 2 network calls and merge 2 arrays
  },
  data() {
    return {
      testResponse: "",
      foods: [],
      detailfoods: [],
      foodByCategories: [],
      soldfood: [],
      pricingfoodList: [],
      CategorizedfoodList: [],
      activitiesList: [],
      categorizedActivitesList: [],
      categorizedActivitesList2: []
    };
  },

  methods: {
    totalOrders: function() {
      let total = [];

      Object.entries(this.soldfood).forEach(([key, val]) => {
        total.push(val.price); // the value of the current key.
      });

      return total.reduce(function(total, num) {
        return total + num;
      }, 0);
    },
    mergeActivities: function() {
      return this.categorizedActivitesList.concat(
        this.categorizedActivitesList2
      );
    }
  }
};
</script>
