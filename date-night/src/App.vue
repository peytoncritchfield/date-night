<template>
  <div id="app">

    <div id="createNewTask">
      <div @click="templateMode = 0" class="header">Date Night Ideas</div>
    </div>

    <div class="tab restaurants" @click="loadIdeas('restaurant');restaurant.isActive = !restaurant.isActive">Restaurants</div>
    <div v-if="restaurant.isActive">
      <div v-for="restaurant in db.restaurants" :key="restaurant.id">
        <div class="column">  
          <button @click="updateStatus(restaurant, 'restaurant')" class="button" :class="{'button-checked' : restaurant.isComplete}"></button>
          <input class="listItems" v-model="restaurant.name" @change="updateIdea(restaurant, 'restaurant')"/>
          <button class="button2" @click="deleteIdea(restaurant, 'restaurant')"> X </button>
        </div>
      </div>
      <button class="button add" @click="addIdea(newRestaurant, 'restaurant')">+</button>
      <input class="input" v-model="newRestaurant.name"/> 
    </div>

    <div class="tab paid-ideas" @click="loadIdeas('paid-idea');paidActivities.isActive = !paidActivities.isActive">Paid Activities</div>
    <div v-if="paidActivities.isActive">
      <div v-for="paidIdea in db.paidIdeas" :key="paidIdea.id">
        <div class="column">
          <button @click="updateStatus(paidIdea, 'paid-idea')" class="button" :class="{'button-checked' : paidIdea.isComplete}"/>
          <input class="listItems" v-model="paidIdea.name" @change="updateIdea(paidIdea, 'paid-idea')"/>
          <button class="button2" @click="deleteIdea(paidIdea, 'paid-idea')"> X </button>
        </div>
      </div>
      <button class="button add" @click="addIdea(newPaidIdea, 'paid-idea')">+</button>
      <input class="input" v-model="newPaidIdea.name"/> 
    </div>

    <div class="tab free-ideas" @click="loadIdeas('free-idea');freeActivities.isActive = !freeActivities.isActive">Free Activities</div>
    <div v-if="freeActivities.isActive">
      <div v-for="freeIdea in db.freeIdeas" :key="freeIdea.id">
        <div class="column">
          <button @click="updateStatus(freeIdea, 'free-idea')" class="button" :class="{'button-checked' : freeIdea.isComplete}"/>
          <input class="listItems" v-model="freeIdea.name" @change="updateIdea(freeIdea, 'free-idea')"/>
          <button class="button2" @click="deleteIdea(freeIdea, 'free-idea')"> X </button>
        </div>
      </div>
      <button class="button add" @click="addIdea(newFreeIdea, 'free-idea')">+</button>
      <input class="input" v-model="newFreeIdea.name"/> 
    </div>

    <div class="tab at-home-ideas" @click="loadIdeas('at-home-idea');atHomeActivities.isActive = !atHomeActivities.isActive">At Home Activities</div>
    <div v-if="atHomeActivities.isActive">
      <div v-for="atHomeIdea in db.atHomeIdeas" :key="atHomeIdea.id">
        <div class="column">
          <button @click="updateStatus(atHomeIdea, 'at-home-idea')" class="button" :class="{'button-checked' : atHomeIdea.isComplete}"/>
          <input class="listItems" v-model="atHomeIdea.name" @change="updateIdea(atHomeIdea, 'at-home-idea')"/>
          <button class="button2" @click="deleteIdea(atHomeIdea, 'at-home-idea')"> X </button>
        </div>
      </div>
      <button class="button add" @click="addIdea(newAtHomeIdea, 'at-home-idea')">+</button>
      <input class="input" v-model="newAtHomeIdea.name"/> 
    </div>
   

  </div>
</template>

<script>

// import clonedeep from "lodash.clonedeep";
import axios from "axios";

export default {
  name: 'App',
  data() {
    return {
      restaurant: {
        isActive: false
      },
      paidActivities: {
        isActive: false
      },
      freeActivities: {
        isActive: false
      },
      atHomeActivities: {
        isActive: false
      },
      newRestaurant: {
        name: "",
        isComplete: false
      },
      newPaidIdea: {
        name: "",
        isComplete: false
      },
      newFreeIdea: {
        name: "",
        isComplete: false
      },
      newAtHomeIdea: {
        name: "",
        isComplete: false
      },
      db: {
        restaurants: [],
        paidIdeas: [],
        freeIdeas: [],
        atHomeIdeas: []
      },
    };
  },
  created() {
    // this.loadRestaurants();
    // this.loadPaidIdeas();
    // this.loadFreeIdeas();
    // this.loadAtHomeIdeas();
  },

  methods: {
    async loadIdeas(ideaString) {
      if (ideaString === 'restaurant') {
        let restaurants = await axios.get(
          `https://us-central1-peytons-projects.cloudfunctions.net/api/${ideaString}s`
        );
        this.db.restaurants = restaurants.data;
        let randomDateIndex = this.getRandomInt(this.db.restaurants.length);
        console.log(this.db.restaurants[randomDateIndex].name);
      }
      if (ideaString === 'paid-idea') {
        let paidIdeas = await axios.get(
          `https://us-central1-peytons-projects.cloudfunctions.net/api/${ideaString}s`
        );
        this.db.paidIdeas = paidIdeas.data;
      }
      if (ideaString === 'free-idea') {
        let freeIdeas = await axios.get(
          `https://us-central1-peytons-projects.cloudfunctions.net/api/${ideaString}s`
        );
        this.db.freeIdeas = freeIdeas.data;
      }
      if (ideaString === 'at-home-idea') {
        let atHomeIdeas = await axios.get(
          `https://us-central1-peytons-projects.cloudfunctions.net/api/${ideaString}s`
        );
        this.db.atHomeIdeas = atHomeIdeas.data;
      }
    },
    async addIdea (newIdeaKey, ideaString) {
      await axios.post(
        `https://us-central1-peytons-projects.cloudfunctions.net/api/${ideaString}`,
        newIdeaKey
      );
      this.loadIdeas(ideaString);
      newIdeaKey.name = '';
    },
    async updateStatus(ideaKey, ideaString) {
      ideaKey.isComplete = !ideaKey.isComplete;
      await axios.patch(
        `https://us-central1-peytons-projects.cloudfunctions.net/api/${ideaString}`,
        ideaKey
      );
      this.loadIdeas(ideaString);
      console.log(ideaKey.isComplete);
    },
    async updateIdea(ideaKey, ideaString) {
      await axios.patch(
        `https://us-central1-peytons-projects.cloudfunctions.net/api/${ideaString}`,
        ideaKey
      );
      this.loadIdeas(ideaString);
    },
    async deleteIdea (ideaKey, ideaString) {
      await axios.delete(
        `https://us-central1-peytons-projects.cloudfunctions.net/api/${ideaString}?id=${ideaKey.id}`);
      this.loadIdeas(ideaString);
    },
    getRandomInt(max) {
      return Math.floor(Math.random() * max);
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 100px;
  max-width: 400px;
  margin: 75px auto;
}
#createNewTask {
  background-color: #13283bb6;
  font-size: 22px;
  color: rgba(255, 255, 255, 0.815);
  margin: 1em auto;
  border-radius: 10px;
  height: 75px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.tab {
  padding-left: 10px;
  margin-top: 20px;
  margin-bottom: 20px;
  border-radius: 5px;
  font-weight: bold;
  color: #ffffff;
  height: 40px;
  display: flex;
  align-items: center;
  cursor: pointer;
}
.tab.restaurants {
  background-color:rgb(119, 207, 169);
}
.tab.paid-ideas {
  background-color:rgb(119, 179, 207);
}
.tab.free-ideas {
  background-color:rgb(172, 127, 236);
}
.tab.at-home-ideas {
  background-color:rgb(223, 111, 176);
}
.button {
  margin-right: 25px;
  margin-left: 30px;
  margin-bottom: 15px;
  background-color: white;
  border-color: #13283bb6;
  border-radius: 2px;
  height: 18px;
  width: 20px;
}
.button-checked {
  background-color: #13283bb6;
  border: none;
}
.add {
  background-color: #13283bb6;
  color: rgba(255, 255, 255, 0.815);
  padding-top: 1px;
  border: none;
}
.button2 {
  align-items: left;
  margin-right: 25px;
  margin-left: 30px;
  margin-bottom: 15px;
  background-color: #8b2e2e60;
  color: rgba(255, 255, 255, 0.815);
  border: none;
  border-radius: 2px;
  height: 18px;
  width: 20px;
}
.input {
  width: 290px;
  border-color: #13283bb6;
  border-style: solid;
  border-radius: 2px;
}
.column {
  width: 80%;
  display: flex;
  width: 400px;
}
.listItems {
  width:245px;
  height: 18px;
  border: none;
  outline: none;
}
/* .header {
  font-weight: bold;
} */
</style>
