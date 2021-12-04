<template>
<div class="admin">
    <div class="heading">
    <h2>Submit Assignment</h2>
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
      <p>{{findItem.title}}</p>
      <p> Possible Points {{ findItem.description }}</p>
      <p>Due {{findItem.date}}</p>
      <p>{{findItem.instructions}}</p>
      <div>
      <input type="file" name="photo" @change="fileChanged">
      <button @click="editItem(findItem)">Upload</button>
    </div>
      <img :src="findItem.path" />
    </div>
  </div>
  </div>

</template>


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
          let r1 = await axios.post('/api/items', {
            title: this.title,
            description: this.description,
          });
          this.addItem = r1.data;
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
  const formData = new FormData();
  formData.append('photo', this.file, this.file.name)
  let r1 = await axios.post('/api/photos', formData);
    await axios.put("/api/items/" + item._id, {
      path: r1.data.path,
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
  color: #EAF8FF;
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
  max-width: 1000px;
}
</style>
