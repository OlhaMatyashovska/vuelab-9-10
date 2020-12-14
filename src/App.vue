<template>
<div>
  <section class="control">
    Вибрано книгу {{ selected }}
    <input type="button" value="Додати нову книгу" v-on:click="showForm" />
    <input type="button" value="Редагувати книгу" v-on:click="showEditForm" />
    <input type="button" value="Вилучити" v-on:click="deleteCompany"/>
    <input type="number" placeholder="min" v-model.number="minWorkerCount">
          <input type="number" placeholder="max" v-model.number="maxWorkerCount">
          <input type="button" value="знайти" v-on:click="Find()">
    
  </section>
  <div>
    <new-book-form 
      v-model = "newCompany"
      @submit.prevent="addNewCompany"
      ref="newBookForm"> </new-book-form>
      </div>
      <div>
    <new-book-form
      v-model = "editCompany"
      ref="editBookForm">
    </new-book-form>
    </div>
    <ul>
      <book-template
        v-for="c in companies"
        :key="c.Id"
        class="companyvie"
        v-on:click="selectCompany(c.Id)"
        v-bind:company="c">
      </book-template>
    </ul>
  </div>
</template>

<script>
import BookTemplate from './components/BookTemplate.vue';
import NewBookForm from './components/NewBookForm.vue';
export default {
  name: "App",
  components:{
    BookTemplate,
    NewBookForm
  },
  data() {
    return {
      
      selected: -1,
      minWorkerCount:0,
      maxWorkerCount:0,
      companies:
      [
        {
          Id:1346,
          Name:"Neuralink",
          Countries:["USA","Canada","Germany"],
          NumberOfWorkers:100,
          TypeOfProducts:"biological engineering"
        },
        {
          Id:1346456,
          Name:"Tesla",
          Countries:["USA","Germany","China"],
          NumberOfWorkers:1000,
          TypeOfProducts:"car production, battery construction"
       },
        {
          Id:13461098,
          Name:"Google",
          Countries:["USA","Great Britain"],
          NumberOfWorkers:100000,
          TypeOfProducts:"search engine studies, hardware and software production"
        }
      ],
      newCompany:
      {
          Name:"",
          Countries:"",
          NumberOfWorkers:0,
          TypeOfProducts:""
      },
      editCompany: {},
    };
  },
  methods: {
    addNewCompany() {
      console.log(this.newBook);
      let newCompanyCopy = Object.assign({}, this.newCompany);
      newCompanyCopy.Id = parseInt(Date.now());
      newCompanyCopy.Countries=newCompanyCopy.Countries.split(",");
      this.companies.push(newCompanyCopy);
      this.showNewCompanyForm = false;
    },
    showForm() {
      this.$refs.newBookForm.show();
    },
    selectCompany(id) {
      this.selected = id;
    },
    showEditForm() {
      if (this.selected >= 0) {
        let index = this.companies.findIndex(book => book.Id == this.selected);
        this.editCompany = this.companies[index];
        console.log(this.editCompany);
        this.$refs.editBookForm.show();
      } else alert("Виберіть компанію");
    },
    deleteCompany() {
      let index = this.companies.findIndex(company => company.Id == this.selected);
      if (this.selected >= 0) this.companies.splice(index, 1);
    },
    closeForm(){
      this.showNewCompanyForm = false;
    },
    Find(){
        var z=this.minWorkerCount;
        var m=this.maxWorkerCount;
        this.companies=this.companies.filter(x=>Number(x.NumberOfWorkers)>=z && Number(x.NumberOfWorkers)<=m);
    }
  },
  
};
</script>

<style scoped>
ul {
  list-style: none;
  position: relative;
}
.wrap {
  position: relative;
}
</style>

