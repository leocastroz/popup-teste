<script>
export default {
  data() {
    return {
      openModals: [],
      formValues: [], 
      consentChecked: false
    }
  },
  props: {
    config: {
      type: Object,
      required: true
    }
  },
  computed: {
    isFormValid() {
      const requiredFields = this.config.formFields.filter(field => field.required);
      const filledFields = this.formValues.filter(value => value !== "");
      const allRequiredFieldsFilled = requiredFields.length === filledFields.length;
      const consentChecked = !this.config.consentCheckbox || this.consentChecked;
      return allRequiredFieldsFilled && consentChecked;
    }
  },
  methods: {
    openModal(name) {
      if (!this.isModalOpen(name)) {
        this.openModals.push(name);
      }
    },
    closeModal(name) {
      const index = this.openModals.indexOf(name);
      if (index !== -1) {
        this.openModals.splice(index, 1);
      }
    },
    isModalOpen(name) {
      return this.openModals.includes(name);
    },
    submitForm() {
      if (this.isFormValid) {
        console.log("Form submitted");
      }
    }
  }
};
</script>
  
<template>
  <div v-if="config" class="popup">
    <div class="container">
      <div class="box">
        <h1>Popup - Popconvert</h1>
      </div>

      <div class="description">
        <h2>Bem-vindo à Popup</h2>
        <p>
          Esse é um componente de popup genérico, 
          que pode ser usado para exibir qualquer 
          conteúdo.
        </p>
      </div>

      <div class="nav-bar">
        <button class="popup-button" @click="openModal('form')">SOBRE O JOGO</button>
        <button class="my-data" @click="openModal('news')">VER VÍDEO</button>
      </div>
      <transition name="modal-transition">
        <div v-if="isModalOpen('form')" class="modal">
          <div class="after-modal"> 
            <div class="dates">
              <div class="header-modal">
                <h2 class="dates-title">{{ config.title }}</h2>
                <img class="close" src="../assets/images/close.svg" alt="" @click="closeModal('form')">
              </div>
             
              <p class="introdution">{{ config.subtitle }}</p>

              <div class="game">
                <img :src="config.videoURL" alt="GIF">
              </div>

              <form @submit.prevent="submitForm">
                <p class="register">{{ config.titleForms }}</p>
                <div v-for="(field, index) in config.formFields" :key="index" class="form-field">
                  <label :for="'field' + index">{{ field.label }}</label>
                  <input :type="field.type" :id="'field' + index" :value="field.value" @input="updateFieldValue(index, $event.target.value)" required />
                </div>
                <div v-if="config.consentCheckbox" class="form-field">
                  <label for="consentCheckbox">Consentimento para coleta de dados</label>
                  <input type="checkbox" id="consentCheckbox" v-model="consentChecked" />
                </div>
                <button class="send" type="submit" :disabled="!isFormValid">Enviar</button>
              </form>
            </div>
            
          </div>
        </div>
      </transition>


      <transition name="modal-transition">
        <div v-if="isModalOpen('news')" class="modal">
          <h2>Modal News</h2>
          <p>Conteúdo da Modal News...</p>
          <button @click="closeModal('news')">Fechar</button>
        </div>
      </transition>

      
    </div>
  </div>
</template>
  
<style scoped>

.send {
  margin-top: 15px;
}

.form-field label{
  font-size: 14px;
  padding: 5px 10px 5px 0;
  background: linear-gradient(270deg,#fff 0%,#a807ff 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.form-field input {
  border-radius: 5px;
  background-color: #8b4fbf;
  border: none;
  padding: 5px 10px 5px 0;
  margin: 5px 0;
}

.my-data {
  background-color: rgb(76, 0, 143);
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  color: #fff;
}

.popup-button {
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  font-size: 12px;
  background-color: #b866ff;
  font-weight: 900;
  color: #ffffff;
  animation: pulse 2s infinite;
  cursor: pointer;
  letter-spacing: 0.5px;
  box-shadow: 0 0 15px 1px #5700a9;
  transition: ease-in-out 0.3s;
  border: 2px solid #b866ff;
}

.popup-button:hover {
  background-color: #ed4dff;
  border: 2px solid #fff;
}


@keyframes pulse {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.1);
  }
  100% {
    transform: scale(1);
  }
}

.description {
  margin: 30px auto;
  width: 300px;
  border-radius: 10px;
  text-align: center;
  padding: 25px;
  border: 2px solid #ffffff76;
}

.description h2 {
  color: #5700a9;
  text-transform: uppercase;
  font-weight: 700;
  font-size: 20px;
  margin: 10px;
}

.description p {
  color: #ffffff6b;
  font-size: 14px;
  letter-spacing: 1px !important;
}

.nav-bar {
  text-align: center;
  padding: 40px;
}
.nav-bar button {
  margin: 10px;
}

.box {
  text-align: center;
  padding: 60px 0 30px 0;
}

h1 {
  text-transform: uppercase;
  background: linear-gradient(270deg,#000000 0%,#a600ff 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  font-weight: 900;
  font-size: 25px;
}

.modal-transition-enter-active,
.modal-transition-leave-active {
  transition: opacity 0.3s;
}

.modal-transition-enter,
.modal-transition-leave-to {
  opacity: 0;
}

.modal-background {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 999;
}

.modal {
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  position: absolute;
  background-color: #00000098;
  text-align: center;
  display: flex;
  justify-content: center;
  align-items: center;
}

.after-modal {
  min-width: 400px;
  min-height: 500px;
  position: absolute;
  background-color: rgb(47, 33, 60);
  border-radius: 10px;
  color: #fff;
  text-align: start;
  border: 3px solid #a501fe;
}

.dates {
  padding: 20px;
}

.dates .dates-title {
  background: linear-gradient(270deg,#d890ff 0%,#9200e0 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  font-weight: 900;
  font-size: 20px;
  margin: 20px 0;
}

.dates .introdution {
  font-size: 13px;
  width: 350px;
  text-align: justify;
  margin-bottom: 20px;
  letter-spacing: 0.2px;
  color: #949494;
}

.game {
  width: 100%;
  display: flex;
  justify-content: center;
}
.game img {
  max-width: 200px;
}

.header-modal {
  display: flex;
  justify-content: space-between;
}

.header-modal .close {
  width: 20px;
  cursor: pointer;
  transition: transform .3s ease;
}

.header-modal .close:hover {
  transform: rotate(45deg);
}

.register {
  background: linear-gradient(270deg,#d890ff 0%,#9200e0 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  font-size: 18px;
  font-weight: 700;
  padding: 20px 0 ;
}

</style>