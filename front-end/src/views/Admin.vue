<template>
<div class="admin">
  <div class="heading">
      <h2>Add an Assignment</h2>
  </div>
    <div class="add">
      <div class="form">
        <input v-model="title" placeholder="Assignment Title">
        <p></p>
        <div>
          <input type="number" v-model="description" placeholder="Points Allowed" />
          <input v-model="date" placeholder="Due Date" />
          <textarea v-model="instructions" placeholder="Instructions" />
        </div>
        <button @click="upload">Upload</button>
      </div>
      <div class="upload" v-if="addItem">
        <h2>{{addItem.title}} </h2>
        <h2>{{ addItem.description }}</h2>
      </div>
    </div>


    <div class="heading">
      <h2>Enter Grades</h2>
    </div>
    <div class="edit">
      <div class="form">
        <input v-model="findTitle" placeholder="Search">
        <div class="suggestions" v-if="suggestions.length > 0">
          <div class="suggestion" v-for="s in suggestions" :key="s.id" @click="selectItem(s)">{{s.title}}
          </div>
        </div>
      </div>
      <div class="upload" v-if="findItem">
        <input v-model="findItem.title">
        <p> Possible Points <input type="number" v-model="findItem.description" /></p>
        <p> Date <input v-model="findItem.date"></p>
        <p> Instructions <textarea v-model="findItem.instructions"/> </p>
        <div>
          <p> Points <input type="number" v-model="findItem.points" placeholder="Points" /> </p>
        </div>
<img :src="findItem.path" />
      </div>
      <div class="actions" v-if="findItem">
        <button @click="deleteItem(findItem)">Delete Assignment</button>
        <button @click="editPoints(findItem)">Save</button>
      </div>

    </div>
</div>

</template>

<style scoped>
.suggestions {
  width: 200px;
  border: 1px solid #ccc;
}
.suggestion {
  min-height: 20px;
}
.suggestion:hover {
  background-color: #5BDEFF;
  color: #fff;
}
.image h2 {
  font-style: italic;
  font-size: 1em;
}
.heading {

  display: flex;
  margin-bottom: 20px;
  margin-top: 20px;
}
.heading h2 {
  font-size: 30px;
  margin-top: 8px;
  margin-left: 10px;
}
.add,
.edit {
  display: flex;
}
.circle {
  border-radius: 50%;
  width: 18px;
  height: 18px;
  padding: 8px;
  background: #333;
  color: #fff;
  text-align: center
}
/* Form */
input,
textarea,
select,
button {
  font-family: 'Montserrat', sans-serif;
  font-size: 1em;
}
.form {
  margin-right: 50px;
}
/* Uploaded images */
.upload h2 {
  margin: 0px;
}
.upload img {
  
}
</style>

<script>
import axios from 'axios';
export default {
  name: 'Admin',
  data() {
    return {
      title: "",
      description: "",
      file: null,
      addItem: null,
      items: [],
      findTitle: "",
findItem: null,
    }
  },
  computed: {
  suggestions() {
    let items = this.items.filter(item => item.title.toLowerCase().startsWith(this.findTitle.toLowerCase()));
    return items.sort((a, b) => a.title > b.title);
  }
},
created() {
this.getItems();
},
  methods: {
    fileChanged(event) {
      this.file = event.target.files[0]
    },
    async upload() {
        try {
          let r2 = await axios.post('/api/items', {
            title: this.title,
            date: this.date,
            description: this.description,
            instructions: this.instructions,
          });
          this.addItem = r2.data;
        } catch (error) {
          console.log(error);
        }
      },
async getItems() {
  try {
    let response = await axios.get("/api/items");
    this.items = response.data;
    return true;
  } catch (error) {
    console.log(error);
  }
},
selectItem(item) {
  this.findTitle = "";
  this.findItem = item;
},
async deleteItem(item) {
  try {
    await axios.delete("/api/items/" + item._id);
    this.findItem = null;
    this.getItems();
    return true;
  } catch (error) {
    console.log(error);
  }
},
async editItem(item) {
  try {
    await axios.put("/api/items/" + item._id, {
      title: this.findItem.title,
      description: this.findItem.description,
    });
    this.findItem = null;
    this.getItems();
    return true;
  } catch (error) {
    console.log(error);
  }
},
async editPoints(item) {
  try {
    await axios.put("/api/items/" + item._id, {
      points: this.findItem.points,
      title: this.findItem.title,
      date: this.findItem.date,
      description: this.findItem.description,
      instructions: this.findItem.instructions,

    });
    this.findItem = null;
    this.getItems();
    return true;
  } catch (error) {
    console.log(error);
  }
},
  },
}
</script>
