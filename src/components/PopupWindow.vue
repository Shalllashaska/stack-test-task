<template>
  <div class="PopupWindow">
    <div class="popupHeader">Добавить организацию</div>

    <div class="popupWrapper">
      <form action="" class="popupInputs">
        <input
          type="text"
          class="popuInputField compNameInp"
          placeholder="Название организации"
          required
        />
        <input
          type="text"
          class="popuInputField directorNameInp"
          placeholder="ФИО дирекотра"
          required
        />

        <input
          type="text"
          class="popuInputField phoneInp"
          placeholder="Номер телефона"
        />
      </form>
    </div>

    <div class="popupFooter">
      <div class="close">
        <button class="closeBTN" @click="closePopup">Отмена</button>
      </div>
      <div class="done">
        <button class="doneBTN" @click="addNewCompany()">OK</button>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  name: "PopupWindow",
  methods: {
    closePopup() {
      this.$emit("closePopup");
    },
    addNewCompany() {
      let inputComp = document.querySelector(".compNameInp");
      let inputDirector = document.querySelector(".directorNameInp");
      let inputPhone = document.querySelector(".phoneInp");

      let phoneVal = inputPhone.value;
      if (!/[a-zа-яё]/i.test(phoneVal)) {
        let comp = {
          nameCompany: inputComp.value,
          director: inputDirector.value,
          phone: phoneVal,
        };

        if (comp.nameCompany != "" && comp.director != "" && comp.phone != "") {
          this.$emit("addNewCompany", comp);
          this.closePopup();
        }
      } else {
        alert("Введите корректный номер!!!");
      }
    },
  },
};
</script>


<style scoped>
.PopupWindow {
  width: 200px;
  height: 350px;
  background-color: rgb(245, 245, 245);
  display: flex;
  flex-direction: column;
  justify-content: center;
  border: solid 1px grey;
  -webkit-box-shadow: 3px 4px 10px 1px rgba(34, 60, 80, 0.2);
  -moz-box-shadow: 3px 4px 10px 1px rgba(34, 60, 80, 0.2);
  box-shadow: 3px 4px 10px 1px rgba(34, 60, 80, 0.2);
}

.popupHeader {
  padding-top: 8px;
  height: 60px;
  width: inherit;
}

.popupWrapper {
  height: 200px;
  width: inherit;
  margin-top: 10px;
  display: flex;
  flex-direction: column;
}

.popupFooter {
  margin-top: 10px;
  height: 60px;
  width: inherit;
  display: flex;
  justify-content: space-evenly;
  align-items: center;
}

.popupInputs {
  width: inherit;
  height: inherit;
  border: solid 1px grey;
}

.popuInputField {
  box-sizing: border-box;
  width: 180px;
  height: 35px;
  margin-top: 25px;
}

button {
  width: 80px;
  height: 40px;
  background-color: white;
  border-radius: 0;
  border: solid 1px black;
}

button:hover {
  background-color: rgb(211, 211, 211);
}

button:active {
  background-color: rgb(119, 119, 119);
}
</style>