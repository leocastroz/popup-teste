<script>
import PopupHeader from "./PopupHeader.vue";
import PopupContainer from "./PopupContainer.vue";
import PopupHelp from "./PopupHelp.vue";
import jsonData from "../data/popupConfig";
import PopupButtons from "./PopupButtons.vue";
import GenderSelect from "./GenderSelect.vue";
import MyModal from "./MyModal.vue";

export default {
  components: {
    PopupHeader,
    PopupContainer,
    PopupHelp,
    PopupButtons,
    GenderSelect,
    MyModal,
  },
  data() {
    return {
      openModals: [],
      formValues: [],
      consentChecked: true,
      dataChecked: true,
      bases: false,
      jsonData,
      showSecondModal: false,
      selectedGender: "",
      modalSuccess: false,
      spinerLoading: false,
      cupom: "https://github.com/leocastroz",
      cutCode: true,
      pastCode: false,
      modalCupom: false,
      showFormModal: true,
    };
  },
  props: {
    config: {
      type: Object,
      required: true,
    },
  },
  mounted() {
    setTimeout(() => {
      this.bases = true
    }, 1000)
  },
  computed: {
    isFormValid() {
      const requiredFields = this.config.formFields.filter((field) => field.required)
      const filledFields = this.formValues.filter((value) => value !== "")
      const allRequiredFieldsFilled = requiredFields.length === filledFields.length
      const consentChecked = !this.config.consentCheckbox || this.consentChecked
      return allRequiredFieldsFilled && consentChecked
    },
  },
  methods: {
    copiarNumero() {
      if (!this.exibirColar) {
        const el = document.createElement("textarea")
        el.value = this.cupom
        document.body.appendChild(el)
        el.select()
        document.execCommand("copy")
        document.body.removeChild(el)
        this.exibirColar = true
      }
    },
    closeIn() {
      this.bases = false;
      this.showSecondModal = true
    },
    openModal(name) {
      if (!this.isModalOpen(name)) {
        this.openModals.push(name)
      }
    },
    openSecondModal() {
      this.showSecondModal = true
    },
    closeSeconde() {
      this.showSecondModal = false
    },
    closeNow() {
      this.modalSuccess = false
    },
    closeModal(name) {
      const index = this.openModals.indexOf(name)
      if (index !== -1) {
        this.openModals.splice(index, 1)
      }
    },
    isModalOpen(name) {
      return this.openModals.includes(name)
    },
    hiddenCut() {
      this.cutCode = false
      this.pastCode = true
    },
    submitForm() {
      if (this.isFormValid) {
        this.modalSuccess = true
        this.bases = false
        this.showFormModal = false
        this.spinerLoading = true
        setTimeout(() => {
          this.spinerLoading = false
          this.modalCupom = true
        }, 3000)
      }
    },
  },
}
</script>

<template>
  <div v-if="config" class="popup">
    <div class="mx-auto">
      <PopupHeader />
      <PopupContainer />
      <PopupHelp />
      <PopupButtons :openModal="openModal" />

      <div v-if="modalSuccess" class="modal">
        <MyModal
          :config="config"
          :spiner-loading="spinerLoading"
          :modal-cupom="modalCupom"
          :cupom="cupom"
          :cut-code="cutCode"
          :past-code="pastCode"
          @closeNow="closeNow"
          @copiar-numero="copiarNumero"
          @hidden-cut="hiddenCut"
        />
      </div>

      <transition name="modal-transition">
        <div v-if="bases" class="modal">
          <div class="after-modal">
            <div class="dates py-2 px-5">
              <div class="flex justify-between">
                <h2 class="dates-title font-black text-base">
                  {{ config.title }}
                </h2>
                <img
                  class="close cursor-pointer w-5"
                  src="../assets/images/close.svg"
                  alt=""
                  @click="closeIn"
                />
              </div>
              <p class="introdution">{{ config.subtitle }}</p>
              <div class="w-full flex justify-center">
                <img :src="config.videoURL" alt="GIF" class="w-48" />
              </div>
              <form @submit.prevent="submitForm">
                <p class="register font-bold text-lg py-3">
                  {{ config.titleForms }}
                </p>
                <div
                  v-for="(field, index) in config.formFields"
                  :key="index"
                  class="bg-black-100 flex justify-between items-center"
                >
                  <label
                    :for="'field' + index"
                    class="text-sm text-violet-300 pr-3"
                    >{{ field.label }}</label
                  >
                  <input
                    :type="field.type"
                    :id="'field' + index"
                    :value="field.value"
                    @input="updateFieldValue(index, $event.target.value)"
                    required
                    class="rounded border-none my-2 bg-violet-300 p-1 mr-10"
                  />
                </div>
                <div class="my-genders flex items-center flex justify-between">
                  <p class="text-violet-300 pr-2">{{ config.titleGender }}</p>
                  <GenderSelect
                    :options="config.gender"
                    :selectedGender.sync="selectedGender"
                    class="mr-36"
                  />
                </div>
                <div>
                  <label for="consentCheckbox" class="text-violet-300 pr-2">{{
                    config.shareData
                  }}</label>
                  <input type="checkbox" v-model="dataChecked" />
                </div>
                <div v-if="config.consentCheckbox" class="my-2">
                  <label for="consentCheckbox" class="text-violet-300 pr-2">{{
                    config.acceptTerms
                  }}</label>
                  <input
                    type="checkbox"
                    id="consentCheckbox"
                    v-model="consentChecked"
                  />
                </div>
                <button
                  class="my-6 py-2 px-5 rounded text-xs font-extrabold"
                  :class="{
                    'bg-red-400': !isFormValid,
                    'bg-green-400': isFormValid,
                  }"
                  type="submit"
                  :disabled="!isFormValid"
                >
                  {{ config.send }}
                </button>
              </form>
            </div>
          </div>
        </div>
      </transition>

      <transition name="modal-transition">
        <div v-if="showFormModal && isModalOpen('form')" class="modal">
          <div class="after-modal">
            <div class="dates py-2 px-5">
              <div class="flex justify-between">
                <h2 class="dates-title font-black text-base">
                  {{ config.title }}
                </h2>
                <img
                  class="close cursor-pointer w-5"
                  src="../assets/images/close.svg"
                  alt=""
                  @click="closeModal('form')"
                />
              </div>
              <p class="introdution">{{ config.subtitle }}</p>

              <div class="w-full flex justify-center">
                <img :src="config.videoURL" alt="GIF" class="w-48" />
              </div>

              <form @submit.prevent="submitForm">
                <p class="register font-bold text-lg py-3">
                  {{ config.titleForms }}
                </p>
                <div
                  v-for="(field, index) in config.formFields"
                  :key="index"
                  class="bg-black-100 flex justify-between items-center"
                >
                  <label
                    :for="'field' + index"
                    class="text-sm text-violet-300 pr-3"
                    >{{ field.label }}</label
                  >
                  <input
                    :type="field.type"
                    :id="'field' + index"
                    :value="field.value"
                    @input="updateFieldValue(index, $event.target.value)"
                    required
                    class="rounded border-none my-2 bg-violet-300 p-1 mr-10"
                  />
                </div>
                <div class="my-genders flex items-center flex justify-between">
                  <p class="text-violet-300 pr-2">{{ config.titleGender }}</p>
                  <GenderSelect
                    :options="config.gender"
                    :selectedGender.sync="selectedGender"
                    class="mr-36"
                  />
                </div>
                <div>
                  <label for="consentCheckbox" class="text-violet-300 pr-2">{{
                    config.shareData
                  }}</label>
                  <input type="checkbox" v-model="dataChecked" />
                </div>
                <div v-if="config.consentCheckbox" class="my-2">
                  <label for="consentCheckbox" class="text-violet-300 pr-2">{{
                    config.acceptTerms
                  }}</label>
                  <input
                    type="checkbox"
                    id="consentCheckbox"
                    v-model="consentChecked"
                  />
                </div>
                <button
                  class="my-6 py-2 px-5 rounded text-xs font-extrabold"
                  :class="{
                    'bg-red-400': !isFormValid,
                    'bg-green-400': isFormValid,
                  }"
                  type="submit"
                  :disabled="!isFormValid"
                >
                  {{ config.send }}
                </button>
              </form>
            </div>
          </div>
        </div>
      </transition>
      <transition name="modal-transition">
        <div v-if="isModalOpen('news')" class="modal">
          <div class="video-modal text-white px-5 py-8">
            <div class="flex justify-between">
              <h2 class="text-base font-black">{{ config.video.title }}</h2>
              <img
                class="close cursor-pointer w-5"
                src="../assets/images/close.svg"
                alt=""
                @click="closeModal('news')"
              />
            </div>
            <h1>UNITARIO</h1>
            <div class="text-white py-8">
              <p class="mt-3 mb-5 text-center text-purple-400">
                {{ config.video.descriptionVideo }}
              </p>
              <div class="flex items-center justify-center">
                <video controls class="w-11/12 rounded">
                  <source :src="config.video.videoURL" type="video/mp4" />
                </video>
              </div>
              <form @submit.prevent="submitForm">
                <p class="register font-bold text-lg py-3">
                  {{ config.titleForms }}
                </p>
                <div
                  v-for="(field, index) in config.formFields"
                  :key="index"
                  class="bg-black-100 flex justify-between items-center"
                >
                  <label
                    :for="'field' + index"
                    class="text-sm text-violet-300 pr-3"
                    >{{ field.label }}</label
                  >
                  <input
                    :type="field.type"
                    :id="'field' + index"
                    :value="field.value"
                    @input="updateFieldValue(index, $event.target.value)"
                    required
                    class="rounded border-none my-2 bg-violet-300 p-1 mr-10"
                  />
                </div>
                <div class="my-genders flex items-center flex justify-between">
                  <p class="text-violet-300 pr-2">{{ config.titleGender }}</p>
                  <GenderSelect
                    :options="config.gender"
                    :selectedGender.sync="selectedGender"
                    class="mr-36"
                  />
                </div>
                <div>
                  <label for="consentCheckbox" class="text-violet-300 pr-2">{{
                    config.shareData
                  }}</label>
                  <input type="checkbox" v-model="dataChecked" />
                </div>
                <div v-if="config.consentCheckbox" class="my-2">
                  <label for="consentCheckbox" class="text-violet-300 pr-2">{{
                    config.acceptTerms
                  }}</label>
                  <input
                    type="checkbox"
                    id="consentCheckbox"
                    v-model="consentChecked"
                  />
                </div>
                <button
                  class="my-6 py-2 px-5 rounded text-xs font-extrabold"
                  :class="{
                    'bg-red-400': !isFormValid,
                    'bg-green-400': isFormValid,
                  }"
                  type="submit"
                  :disabled="!isFormValid"
                >
                  {{ config.send }}
                </button>
              </form>
            </div>
          </div>
        </div>
      </transition>
      <transition name="modal-transition">
        <div v-if="showSecondModal" class="modal">
          <div class="video-modal text-white px-5 py-8">
            <div class="flex justify-between">
              <h2 class="text-base font-black">{{ config.video.title }}</h2>
              <img
                class="close cursor-pointer w-5"
                src="../assets/images/close.svg"
                alt=""
                @click="closeSeconde"
              />
            </div>
            <div class="text-white py-8">
              <p class="mt-3 mb-5 text-center text-purple-400">
                {{ config.video.descriptionVideo }}
              </p>
              <div class="flex items-center justify-center">
                <video controls class="w-11/12 rounded">
                  <source :src="config.video.videoURL" type="video/mp4" />
                </video>
              </div>
              <form @submit.prevent="submitForm">
                <p class="register font-bold text-lg py-3">
                  {{ config.titleForms }}
                </p>
                <div
                  v-for="(field, index) in config.formFields"
                  :key="index"
                  class="bg-black-100 flex justify-between items-center"
                >
                  <label
                    :for="'field' + index"
                    class="text-sm text-violet-300 pr-3"
                    >{{ field.label }}</label
                  >
                  <input
                    :type="field.type"
                    :id="'field' + index"
                    :value="field.value"
                    @input="updateFieldValue(index, $event.target.value)"
                    required
                    class="rounded border-none my-2 bg-violet-300 p-1 mr-10"
                  />
                </div>
                <div class="my-genders flex items-center flex justify-between">
                  <p class="text-violet-300 pr-2">{{ config.titleGender }}</p>
                  <GenderSelect
                    :options="config.gender"
                    :selectedGender.sync="selectedGender"
                    class="mr-36"
                  />
                </div>
                <div>
                  <label for="consentCheckbox" class="text-violet-300 pr-2">{{
                    config.shareData
                  }}</label>
                  <input type="checkbox" v-model="dataChecked" />
                </div>
                <div v-if="config.consentCheckbox" class="my-2">
                  <label for="consentCheckbox" class="text-violet-300 pr-2">{{
                    config.acceptTerms
                  }}</label>
                  <input
                    type="checkbox"
                    id="consentCheckbox"
                    v-model="consentChecked"
                  />
                </div>
                <button
                  class="my-6 py-2 px-5 rounded text-xs font-extrabold"
                  :class="{
                    'bg-red-400': !isFormValid,
                    'bg-green-400': isFormValid,
                  }"
                  type="submit"
                  :disabled="!isFormValid"
                >
                  {{ config.send }}
                </button>
              </form>
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
  transition: transform 0.3s ease;
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
