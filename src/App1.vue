<template>
    <section>
        <div class="max-w-[1140px] mx-auto py-4">
            <h1 class="text-semibold text-2xl text-center">Danh sach user</h1>
            <input v-model="search" class="block border border-solid border-black py-1 px-2" placeholder="Nhap tim kiem">
            <button class="block border border-solid border-black px-2 bg-green-300" @click="searchByName">Tim kiem</button>
            <table id="table-user" class="mt-10">
                <tr>
                    <th>Ten</th>
                    <th>Tuoi</th>
                    <th>Dia chi</th>
                    <th>Gioi tinh</th>
                    <th>Cong viec</th>
                    <th>So thich</th>
                    <th>edit</th>
                    <th>delete</th>
                </tr>
                <tr v-for="(person, index) in persons" :key="`person${index}`" :class="index%2 === 0 ? 'bg-[#f2f2f2]': ''">
                    <td>
                        <span v-if="indexEdit !== index">{{ person.name }}</span>
                        <input v-else v-model="formEdit.name" class="block border border-solid border-black py-1 px-2" placeholder="Nhap ten">
                    </td>
                    <td>
                        <div v-if="indexEdit !== index">
                            {{ person.age }}
                            <span v-if="person.age === maxAge" class="text-blue-300">Gia nhat</span>
                            <span v-if="person.age === minAge" class="text-green-300">Tre nhat</span>
                        </div>
                        <input v-else v-model="formEdit.age" class="block border border-solid border-black py-1 px-2" placeholder="Nhap tuoi">
                    </td>
                    <td>
                        <span v-if="indexEdit !== index">{{ person.address }}</span>
                        <input v-else v-model="formEdit.address" class="block border border-solid border-black py-1 px-2" placeholder="Nhap dia chi">
                    </td>
                    <td>
                        <span v-if="indexEdit !== index">{{ person.gender }}</span>
                        <div v-else class="block mt-4">
                            <input type="radio" id="male" value="male" v-model="formEdit.gender" />
                            <label for="male"  class="mr-4">Nam</label>
                            <input type="radio" id="female" value="female" v-model="formEdit.gender" />
                            <label for="female">Nu</label>
                        </div>
                    </td>
                    <td>
                        <span v-if="indexEdit !== index">{{ person.job }}</span>
                        <select v-else v-model="formEdit.job">
                            <option disabled value="">Please select one job</option>
                            <option v-for="(option, index) in optionsJob" :value="option.value" :key="`job${index}`">
                                {{ option.text }}
                            </option>
                        </select>
                    </td>
                    <td>
                        <div v-if="indexEdit !== index">
                            <span v-for="(hobby, i) in person.hobbies" :key="`hobby${i}`">{{ hobby }}, </span>
                        </div>
                        <div v-else>
                            <div v-for="(option, index) in optionsHobbies" :key="`ohobby${index}`">
                                <p><input type="checkbox" :id="option.value" :value="option.value" v-model="formEdit.hobbies">
                                <label :for="option.value">{{ option.text }}</label></p>
                            </div>
                        </div>
                    </td>
                    <td>
                        <button v-if="indexEdit !== index" class="block border border-solid border-black px-2 bg-green-300" @click="editPerson(person, index)">edit</button>
                        <div v-else class="flex">
                            <button class="block border border-solid border-black px-2 bg-blue-400 mr-4" @click="submitEditPerson">oke</button>
                            <button class="block border border-solid border-black px-2 bg-red-400" @click="indexEdit = null">cancel</button>
                        </div>
                    </td>
                    <td @click="deletePerson(index)">delete</td>
                </tr>
            </table>
            <p class="mt-6 text-2xl font-semibold">Them user vao danh sach</p>
            <form @submit.prevent="submitAddPerson">
                <p class="font-bold mt-4">Ten</p>
                <input v-model="form.name" class="block border border-solid border-black py-1 px-2" placeholder="Nhap ten">
                <p class="font-bold mt-4">Dia chi</p>
                <input v-model="form.address" class="block border border-solid border-black py-1 px-2" placeholder="Nhap ten">
                <p class="font-bold mt-4">Tuoi</p>
                <input v-model="form.age" class="block border border-solid border-black py-1 px-2" placeholder="Nhap tuoi" type="number">
                <div class="block mt-4">
                    <input type="radio" id="male" value="male" v-model="form.gender" />
                    <label for="male"  class="mr-4">Nam</label>
                    <input type="radio" id="female" value="female" v-model="form.gender" />
                    <label for="female">Nu</label>
                </div>
                <div>
                    <p class="font-bold mt-4">Nghe nghiep</p>
                    <select v-model="form.job">
                        <option disabled value="">Please select one</option>
                        <option v-for="(option, index) in optionsJob" :value="option.value" :key="`job${index}`">
                            {{ option.text }}
                        </option>
                    </select>
                </div>
                <div class="block">
                    <div v-for="(option, index) in optionsHobbies" :key="`ohobby${index}`">
                        <p><input type="checkbox" :id="option.value" :value="option.value" v-model="form.hobbies">
                        <label :for="option.value">{{ option.text }}</label></p>
                    </div>
                </div>
                <button class="px-4 py-2 bg-[#f2f2f2] rounded-xl mt-4">Them user</button>
            </form>
        </div>
    </section>
</template>
<script>
const initialForm = {
    name: '',
    address: '',
    age: 0,
    gender: 'male',
    job: '',
    hobbies:[],
}
export default {
    data() {
      return {
        persons: [
        //   {name:'Ngoc', age: '25', gender:'male', address:'ngo lo lon', job:'coder', hobbies: ['Bap Rang Bo', 'Hai Hoa']},
        //   {name:'tu', age:'27', gender:'male', address:'pho kim nguu', job:'teacher', hobbies: ['Page Small']},
        //   {name:'tung', age:'25', gender:'male', address:'pho chua vua', job:'student', hobbies: ['Quynh']}
        ],
        maxAge: 0,
        minAge: 0,
        form : {
            name: '',
            address: '',
            age: 0,
            gender: 'male',
            job: '',
            hobbies:[],
        },
        formEdit : {
            name: '',
            address: '',
            age: 0,
            gender: 'male',
            job: '',
            hobbies:[],
        },
        optionsJob: [
            { text: 'Lap trinh vien', value: 'coder' },
            { text: 'Giao vien', value: 'teacher' },
            { text: 'Hoc sinh', value: 'student' }
        ],
        optionsHobbies: [
            { text: 'Soccer', value: 'soccer' },
            { text: 'Music', value: 'music' },
            { text: 'Travel', value: 'travel' }
        ],
        indexEdit: null,
        search: "",
        listBeforeFilter: []
      }
    },
    mounted() {
        this.persons =  [
          {name:'Ngoc', age: 25, gender:'male', address:'ngo lo lon', job:'coder', hobbies: ['Bap Rang Bo', 'Hai Hoa']},
          {name:'tu', age: 27, gender:'male', address:'pho kim nguu', job:'teacher', hobbies: ['Page Small']},
          {name:'tung', age: 24, gender:'male', address:'pho chua vua', job:'student', hobbies: ['Quynh']}
        ]
        let maxAge = this.persons[0].age
        let minAge = this.persons[0].age
        this.persons.forEach(element => {
            if(maxAge < element.age) {
                maxAge = element.age
            }
            if(minAge > element.age) {
                minAge = element.age
            }
        });
        // let tmpTable = this.persons.map(element => {
        //     if(maxAge === element.age) {
        //         elemnet.oldest = true
        //     }
        //     if(minAge > element.age) {
        //         minAge = element.age
        //     }
        // });
        // this.persons = [...tmpTable]
        this.maxAge = maxAge
        this.minAge = minAge
    },
    methods: {
        submitAddPerson() {
            const vform = Object.assign({}, this.form)
            this.persons.push(vform)
            this.form = Object.assign({}, initialForm)
        },
        deletePerson(index) {
            console.log(index)
            this.persons.splice(index, 1)
            this.indexEdit = null
        },
        editPerson(person, index) {
            this.indexEdit = index 
            this.formEdit = Object.assign({}, person)
        },
        submitEditPerson() {
            this.persons[this.indexEdit] = Object.assign({}, this.formEdit)
            this.indexEdit = null
        },
        // searchByName() {
        //     this.listBeforeFilter = [...this.perons]
        //     const listFilter = this.persons.filter(element => element.name.includes(this.search));
        //     this.persons = [...listFilter]
        // }
    }
}
</script>
<style>
#table-user {
  border-collapse: collapse;
  width: 100%;
}

#table-user td, #table-user th {
  border: 1px solid #ddd;
  padding: 8px;
}

/* #table-user tr:nth-child(even){background-color: #f2f2f2;} */

/* #customers tr:hover {background-color: #ddd;} */
/* #table-user tr:first-child {
    background-color: #04AA6D;
} */
#table-user th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: #04AA6D;
  color: white;
}
</style>