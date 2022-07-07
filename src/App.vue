<template> 
  <div class="max-w-[1140px] mx-auto p-6 ">
    <h1 class="text-center font-bold text-4xl mb-8">Information</h1>
    <table class="table-user mx-auto">
      <th class="text-2xl">Name</th>
      <th class="text-2xl">Age</th>
      <th class="text-2xl">Gender</th>
      <th class="text-2xl">Adress</th>
      <th class="text-2xl">Job</th>
      <th class="text-2xl">Hobbies</th>
      <tr class="text-center text-xl" v-for="(item, index) in person" :key="`${index}`" :class="(index%2)===0 ? 'bg-gray-200':'bg-white'">
        <td>{{item.name}}</td>
        <td>{{item.age}}</td>
        <td>{{item.gender}}</td>
        <td>{{item.address}}</td>
        <td>{{item.job}}</td>
        <td>{{item.hobbies}}</td>
      </tr>
    </table>
    <form class="mx-auto flex p-6" @submit.prevent="onSubmit">
      <div class="text-center text-xl">
        Name
        <p><input type="text" v-model="form.name"></p>
      </div>
      <div class="text-center text-xl">
        Age
        <input type="text" v-model="form.age">
      </div>
      <div class="text-center text-xl">
        Gender
        <p clalss="text-center mb-4 mt-6 "><input type="radio" id="male" value="male" v-model="form.gender" />
        <label for="male">Nam</label></p>
        <p class="text-center mr-4 mt-4"><input type="radio" id="female" value="female" v-model="form.gender" />
        <label for="female">Nu</label></p>
      </div>
      <div class="text-center text-xl">
        Address
        <input type="text" v-model="form.address">
      </div>
      <div class="text-center text-xl">
        Job
        <select v-model="selected">
          <option :value="{ job: dev }">Dev</option>
          <option :value="{ job: coder }">Coder</option>
          <option :value="{ job: manager }">Manager</option>
          <option :value="{ job: teacher }">Teacher</option>
        </select>
      </div>
      <div class="text-center text-xl ml-4">
        Hobbies
        <p><input type="checkbox" id="soccer block" value="soccer" v-model="form.hobbies">
        <label for="soccer">Soccer</label></p>

        <p class="mt-4 mb-4"><input type="checkbox" id="music" value="music" v-model="form.hobbies">
        <label for="music">Music</label></p>

        <p><input type="checkbox" id="travel" value="travel" v-model="form.hobbies">
        <label for="travel">Travel</label></p>       
      </div>
      <button class="mx-auto ml-4">Submit</button>
    </form>
    
  </div>
</template>

<script>
  let initialForm = {
    name: "",
    age: 0,
    gender: "male",
    address: "",
    job: "",
    hobbies: []
  }
  export default{
    data() {
      return {
        person:[
          {name:'Ngoc', age: '25', gender:'male', address:'ngo lo lon', job:'coder', hobbies:'Bap Rang Bo, Hai Hoa'},
          {name:'tu', age:'27', gender:'male', address:'pho kim nguu', job:'teacher', hobbies:'Page Small'},
          {name:'tung', age:'25', gender:'male', address:'pho chua vua', job:'student', hobbies:'Quynh'}
        ],
        form: Object.assign({}, initialForm)
      }
    },
    methods: {
      onSubmit() {
        console.log(this.form)
      }
    }
  }  
</script>

<style>
.table-user {
  font-family: Arial, Helvetica, sans-serif;
  border-collapse: collapse;
  width: 100%;
}
.table-user td, .table-user th {
  border: 1px solid #ddd;
  padding: 8px;
}
.table-user th {
  padding-top: 12px;
  padding-bottom: 12px;
  background-color: #04AA6D;
  color: white;
}


</style>
