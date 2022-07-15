<template>
  <div id="app">
    <div class="wrapper">
      <PopupWindow
        v-if="isPopupVisible"
        @closePopup="closePopupWindow"
        @addNewCompany="addNewCompany"
      />
      <div class="input">
        <div class="inputActions">
          <input
            type="search"
            v-model="search"
            class="inputForm"
            placeholder="Найти по ФИО..."
            required
          />
        </div>
        <button class="inputActions addBTN" @click="showPopupWindow">
          Добавить
        </button>
      </div>
      <div class="table">
        <div class="tableColumnNames">
          <div
            class="tableColumnName companiesNames"
            @click="ChangeArrow('.companiesNames')"
          >
            <div>Название</div>
          </div>
          <div
            class="tableColumnName directorsNames"
            @click="ChangeArrow('.directorsNames')"
          >
            <div>ФИО директора</div>
          </div>
          <div class="tableColumnName phones">
            <div>Номер телефона</div>
          </div>
          <div class="tableColumnName"></div>
        </div>

        <div class="tableWrapper">
          <TableItem
            v-for="comp in searchHandler"
            v-bind:comp="comp"
            v-bind:key="comp.id"
            @deleteItem="deleteItemI"
          />
        </div>
      </div>

      <div class="footer">
        <div class="pages">
          <button class="left pageBTN" @click="changePageDown()">←</button>
          <div class="pagesWord">
            Страница <span class="numOfPage">{{ currentPage }}</span>
          </div>
          <button class="rigth pageBTN" @click="changePageUp()">→</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import TableItem from "@/components/TableItem.vue";
import PopupWindow from "@/components/PopupWindow.vue";
import {data} from "./data.js";

export default {
  name: "App",
  data() {
    return {
      isPopupVisible: false,
      currId: 4,
      currentPage: 1,
      companiesOnPage: 5,
      currentLength: 1,
      search: "",
      companies: [],
    };
  },
  created() {
    this.companies = data;
    this.currId = this.companies.length;
  },
  methods: {
    showPopupWindow() {
      this.isPopupVisible = true;
    },
    closePopupWindow() {
      this.isPopupVisible = false;
    },
    addNewCompany(comp) {
      this.currId++;
      comp.id = this.currId;
      this.companies.push(comp);
    },
    deleteItemI(id) {
      for (let i = 0; i < this.companies.length; i++) {
        if (id == this.companies[i].id) {
          let leftArr = this.companies.slice(0, i);
          let rigthArr = this.companies.slice(i + 1, this.companies.length);
          this.companies = leftArr.concat(...rigthArr);
          break;
        }
      }
    
    },
    ChangeArrow(elClass) {
      this.clearFilters(elClass);
      let El = document.querySelector(elClass);
      let str = El.textContent.toString();
      if (str[str.length - 1] != "↓" && str[str.length - 1] != "↑") {
        El.textContent = str + "↓";
      } else if (str[str.length - 1] == "↓") {
        El.textContent = str.slice(0, str.length - 1) + "↑";
      } else if (str[str.length - 1] == "↑") {
        El.textContent = str.slice(0, str.length - 1) + "↓";
      }
      let str2 = El.textContent.toString();
      this.SortComps(elClass, str2[str2.length - 1]);
    },
    SortComps(curEl, arr) {
      let mult = 1;
      if (arr == "↓") {
        mult = -1;
      }
      if (curEl == ".companiesNames") {
        this.companies.sort(function (a, b) {
          if (a.nameCompany > b.nameCompany) {
            return mult * 1;
          }
          if (a.nameCompany < b.nameCompany) {
            return mult * -1;
          }
          return 0;
        });
      } else if (curEl == ".directorsNames") {
        this.companies.sort(function (a, b) {
          if (a.director > b.director) {
            return mult * 1;
          }
          if (a.director < b.director) {
            return mult * -1;
          }
          return 0;
        });
      }
    },
    clearFilters(curEl) {
      let compsEl = document.querySelector(".companiesNames");
      let directorsEl = document.querySelector(".directorsNames");
      if (curEl == ".companiesNames") {
        this.clearElement(directorsEl);
      } else if (curEl == ".directorsNames") {
        this.clearElement(compsEl);
      }
    },
    clearElement(el) {
      let str = el.textContent.toString();
      if (str[str.length - 1] == "↓" || str[str.length - 1] == "↑") {
        el.textContent = str.slice(0, str.length - 1);
      }
    },
    changePageDown() {
      if (this.currentPage > 1) {
        this.currentPage--;
        
      }
    },
    changePageUp() {
      if (this.currentPage * this.companiesOnPage < this.companies.length) {
        this.currentPage++;
      }
    },
  },
  computed: {
    searchHandler(){
      let arr = this.companies.filter(elem =>{
        return elem.director.toLowerCase().includes(this.search.toLowerCase());
      });
      let newArr = [];
      for (
        let i = (this.currentPage - 1) * this.companiesOnPage;
        i <
        this.companiesOnPage + (this.currentPage - 1) * this.companiesOnPage;
        i++
      ) {
        if (i >= arr.length) {
          break;
        }
        newArr.push(arr[i]);
      }
      return newArr;
    }
  },
  components: {
    TableItem,
    PopupWindow,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.wrapper {
  width: 700px;
  height: 600px;
}

.input {
  width: inherit;
  height: 60px;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

.inputActions {
  width: 110px;
  height: inherit;
}

.inputForm {
  box-sizing: border-box;
  width: inherit;
  height: inherit;
}

.table {
  width: inherit;
  height: 400px;
  margin-top: 25px;
}

.tableColumnNames {
  box-sizing: border-box;
  width: inherit;
  height: 60px;
  display: grid;
  grid-template-columns: 200px 200px 200px 100px;
}

.tableColumnName {
  border: 1px solid black;
  display: flex;
  align-items: center;
  justify-content: center;
}

.PopupWindow {
  position: fixed;
  top: 100px;
  left: 200px;
}

.tableWrapper {
  width: inherit;
  height: 340px;
}

.footer {
  width: inherit;
  height: 40px;
  display: flex;
  justify-content: flex-end;
}

.pages {
  height: inherit;
  display: flex;
  align-items: center;
}

.pagesWord {
  margin: 0px 10px 0px 10px;
}

.pageBTN {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  border: none;
  -webkit-box-shadow: 3px 4px 10px 1px rgba(34, 60, 80, 0.2);
  -moz-box-shadow: 3px 4px 10px 1px rgba(34, 60, 80, 0.2);
  box-shadow: 3px 4px 10px 1px rgba(34, 60, 80, 0.2);
}

.addBTN {
  border: solid 1px black;
  background-color: white;
}

.addBTN:hover {
  background-color: rgb(211, 211, 211);
}

.addBTN:active {
  background-color: rgb(119, 119, 119);
}
</style>
