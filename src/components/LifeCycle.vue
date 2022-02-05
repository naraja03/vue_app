<template>
  <div class="lifeCycle">
    <h1>Vue Js</h1>
    <button @click="fetch">Fetch data</button>
    <button @click="showData">
      <span v-if="show">Show</span><span v-else>Hide</span> data
    </button>
    <button @click="showForm = !showForm">
      <span v-if="showForm">Hide Input</span><span v-else>Show Input</span>
    </button>
    <form @submit.prevent="updateData" v-show="showForm">
      <div>
        <label for="id">User id</label> <br />
        <input type="text" v-model="formData.userId" />
      </div>
      <div>
        <label for="title">Title</label> <br />
        <input type="text" v-model="formData.title" />
      </div>
      <div>
        <label for="body">Body</label> <br />
        <input type="text" v-model="formData.body" />
      </div>
      <button>Update data</button>
    </form>
    <div v-for="datas in values" :key="datas.id" class="data" v-show="!show">
      <h4>User Id: {{ datas.id }}</h4>
      <h2>Title: {{ datas.title }}</h2>
      <p>Body: {{ datas.body }}</p>
      <hr />
    </div>
    <div class="arrayMethod">
    <div class="arrayAt">
        <h3>Array method At( <span><input type="text" class="arrayInput" v-model="arrayInput"></span> )</h3>
        <button @click="arrayAt">click</button>
        <p>{{arrayMethod}}</p>
    </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { ref, reactive } from "vue";
export default {
  setup() {
    let values = ref();
    let show = ref(true);
    let showForm = ref(false);
    let arrayMethod = ref();
    let arrayInput = ref();
    let formData = reactive({
      userId: "",
      title: "",
      body: "",
    });

    let arrayAt = () =>{
        arrayMethod.value = values.value.at(arrayInput.value);
    }
    
    let fetch = () => {
      return axios
        .get("https://jsonplaceholder.typicode.com/posts")
        .then((res) => {
          values.value = res.data.slice(0, 10);
          console.log(res.data);
        })
        .catch((err) => console.log(err, "Problem in get"));
    };

    let updateData = () => {

      axios
        .post("https://jsonplaceholder.typicode.com/posts/1", formData)
        .then((res) => console.log(res.data))
        .catch((err) => console.log(err, "Problem in post"));
    };

    let showData = () => {
      if (show.value) {
        fetch();
        show.value = !show.value;
      } else {
        values.value = null;
        show.value = !show.value;
      }
    };

    return {
      showData,
      show,
      showForm,
      fetch,
      values,
      formData,
      updateData,
      arrayMethod,
      arrayAt,
      arrayInput,
    };
  },
};
</script>

<style scoped>
.data, .arrayMethod {
  text-align: left;
}
div {
  margin: 20px;
}
button {
  margin: 20px;
}
.arrayInput{
    outline: none;
    border: none;
    width: 25px;
}
</style>
