<script>
import PopupHeader from './PopupHeader.vue';
import PopupContainer from './PopupContainer.vue';
import PopupHelp from './PopupHelp.vue';
import jsonData from '../data/popupConfig';
import PopupButtons from './PopupButtons.vue';

export default {
  components: {
    PopupHeader,
    PopupContainer,
    PopupHelp,
    PopupButtons
  },
  data() {
    return {
      openModals: [],
      formValues: [],
      consentChecked: false,
      bases: false,
      jsonData: jsonData,
      showSecondModal: false
    }
  },
  props: {
    config: {
      type: Object,
      required: true
    }
  },
  mounted() {
    setTimeout(() => {
      this.bases = true;
    }, 1000);
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
    closess() {
      this.bases = false;
      this.showSecondModal = true;
    },
    openModal(name) {
      if (!this.isModalOpen(name)) {
        this.openModals.push(name);
      }
    },
    openSecondModal() {
      this.showSecondModal = true;
    },
    closeSeconde() {
      this.showSecondModal = false;
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
    <div class="mx-auto">
      <PopupHeader />
      <PopupContainer />
      <PopupHelp />
      <PopupButtons :openModal="openModal" />

      <transition name="modal-transition">
        <div v-if="bases" class="modal">
          <div class="after-modal">
            <div class="dates py-2 px-5">
              <div class="flex justify-between">
                <h2 class="dates-title text-base">{{ config.title }}</h2>
                <img class="close cursor-pointer w-5" src="../assets/images/close.svg" alt="" @click="closess">
              </div>
              <p class="introdution">{{ config.subtitle }}</p>
              <div class="w-full flex justify-center">
                <img :src="config.videoURL" alt="GIF" class="w-48">
              </div>
              <form @submit.prevent="submitForm">
                <p class="register">{{ config.titleForms }}</p>
                <div v-for="(field, index) in config.formFields" :key="index" class=" bg-black-100">
                  <label :for="'field' + index" class="text-sm text-violet-300 pr-3">{{ field.label }}</label>
                  <input :type="field.type" :id="'field' + index" :value="field.value"
                    @input="updateFieldValue(index, $event.target.value)" required class="rounded border-none my-2 bg-violet-300 p-1" />
                </div>
                <div v-if="config.consentCheckbox" class="form-field my-2">
                  <label for="consentCheckbox" class="text-violet-300 pr-2">Consentimento para coleta de dados</label>
                  <input type="checkbox" id="consentCheckbox" v-model="consentChecked" />
                </div>
                <button class="my-6 bg-green-400 py-2 px-5 rounded text-xs text-lime-700 font-extrabold" type="submit" :disabled="!isFormValid">Enviar</button>
              </form>
            </div>
          </div>
        </div>
      </transition>

      <transition name="modal-transition">
        <div v-if="isModalOpen('form')" class="modal">
          <div class="after-modal">
            <div class="dates py-2 px-5">
              <div class="flex justify-between">
                <h2 class="dates-title font-black text-base">{{ config.title }}</h2>
                <img class="close cursor-pointer w-5" src="../assets/images/close.svg" alt="" @click="closeModal('form')">
              </div>
              <p class="introdution">{{ config.subtitle }}</p>

              <div class="w-full flex justify-center">
                <img :src="config.videoURL" alt="GIF" class="w-48">
              </div>

              <form @submit.prevent="submitForm">
                <p class="register font-bold text-lg py-3">{{ config.titleForms }}</p>
                <div v-for="(field, index) in config.formFields" :key="index" class="form-field">
                  <label :for="'field' + index" class="text-sm text-violet-300 pr-3">{{ field.label }}</label>
                  <input :type="field.type" :id="'field' + index" :value="field.value"
                    @input="updateFieldValue(index, $event.target.value)" required class="rounded border-none my-2 bg-violet-300 p-1" />
                </div>
                <div v-if="config.consentCheckbox" class="form-field my-2">
                  <label for="consentCheckbox" class="text-violet-300 pr-2">Gostaria de compartilhar seus dados ?</label>
                  <input type="checkbox" id="consentCheckbox" v-model="consentChecked" class="text-black" />
                </div>
                <button class="my-6 bg-green-400 py-2 px-5 rounded text-xs text-lime-700 font-extrabold" type="submit"
                  :disabled="!isFormValid">Enviar</button>
              </form>
            </div>
          </div>
        </div>
      </transition>
      <transition name="modal-transition">
        <div v-if="isModalOpen('news')" class="modal">
          <div class="video-modal text-white px-5 py-8">
            <div class="flex justify-between">
              <h2 class="text-base font-black">CONFIRA O VÍDEO</h2>
              <img class="close cursor-pointer w-5" src="../assets/images/close.svg" alt="" @click="closeModal('news')">
            </div>
            <div class="text-white py-8">
              <p class="mt-3 mb-5 text-center text-purple-400">Confira as melhores jogadas 🎆</p>
              <div class="flex items-center justify-center">
                <video controls class="w-11/12 rounded">
                  <source :src="config.videoBaseURL" type="video/mp4">
                </video>
              </div>
            </div>
          </div>
        </div>
      </transition>
      <transition name="modal-transition">
        <div v-if="showSecondModal" class="modal">
          <div class="video-modal text-white px-5 py-8">
            <div class="flex justify-between">
              <h2 class="text-base font-black">CONFIRA O VÍDEO</h2>
              <img class="close cursor-pointer w-5" src="../assets/images/close.svg" alt="" @click="closeSeconde">
            </div>
            <div class="text-white py-8">
              <p class="mt-3 mb-5 text-center text-purple-400">Confira as melhores jogadas 🎆</p>
              <div class="flex items-center justify-center">
                <video controls class="w-11/12 rounded">
                  <source :src="config.videoBaseURL" type="video/mp4">
                </video>
              </div>
            </div>
          </div>
        </div>
      </transition>
    </div>
  </div>
</template>
  
<style scoped>

.my-data {
  transition: ease-in-out 0.3s;
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
  z-index: 9999;
}

.video-modal {
  max-width: 400px;
  min-height: 500px;
  position: absolute;
  background-color: rgb(47, 33, 60);
  border-radius: 10px;
  text-align: start;
  border: 3px solid #a501fe;
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


.dates .dates-title {
  background: linear-gradient(270deg, #d890ff 0%, #9200e0 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
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

.close {
  transition: transform .3s ease;
}

.close:hover {
  transform: rotate(45deg);
}

.register {
  background: linear-gradient(270deg, #d890ff 0%, #9200e0 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

</style>
