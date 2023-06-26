<script>
import ButtonsHomePage from "@/components/layout/ButtonsHomePage.vue";
import GenderSelect from "@/components/layout/GenderSelect.vue";
import PopupHeader from "@/components/layout/PopupHeader.vue";
import PopupHeaderModalTwo from "./PopupHeaderModalTwo.vue";
import ButtonSend from "@/components/layout/ButtonSend.vue";
import FormField from "@/components/layout/FormField.vue";
import PopupHeaderModal from "./PopupHeaderModal.vue";
import VideoSectionTwo from "./VideoSectionTwo.vue";
import VideoSection from "./VideoSection.vue";
import jsonData from "../data/popupConfig";
import CheckInput from "./CheckInput.vue";
import MyModal from "./MyModal.vue";
import "../assets/config/popup.css";

export default {
  components: {
    PopupHeaderModalTwo,
    PopupHeaderModal,
    VideoSectionTwo,
    ButtonsHomePage,
    VideoSection,
    GenderSelect,
    PopupHeader,
    CheckInput,
    ButtonSend,
    FormField,
    MyModal,
  },
  data() {
    return {
      cupom: "https://github.com/leocastroz",
      showSecondFormModal: true,
      showSecondModal: false,
      spinerLoading: false,
      consentChecked: true,
      checkedShare: false,
      modalSuccess: false,
      showFormModal: true,
      selectedGender: "",
      modalCupom: false,
      pastCode: false,
      openModals: [],
      formValues: [],
      cutCode: true,
      bases: false,
      jsonData,
    };
  },
  props: {
    config: {
      type: Object,
      required: true,
    },
  },
  mounted() {
    setTimeout(() => { this.bases = true }, 1000)
  },
  computed: {
    isFormValid() {
      const requiredFields = this.config.formsLayout.formFields.filter(
        (field) => field.required
      )
      const filledFields = this.formValues.filter((value) => value !== "")
      const allRequiredFieldsFilled =
        requiredFields.length === filledFields.length
      const consentChecked =
        !this.config.firstModal.consentCheckbox && !this.secondModal.consentCheckbox ||  this.consentChecked
      return allRequiredFieldsFilled && consentChecked
    },
  },
  methods: {
    copyNumber() {
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
      this.bases = false
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
      location.reload()
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
        this.showSecondFormModal = false
        this.showSecondModal = false
        this.showFormModal = false
        this.spinerLoading = true
        this.modalSuccess = true
        this.bases = false
        setTimeout(() => {
          this.spinerLoading = false
          this.modalCupom = true
        }, 3000)
      }
    }
  }
}
</script>

<template>
  <div v-if="config">
    <PopupHeader :config="config" />
    <ButtonsHomePage :config="config" :openModal="openModal" />
    <div v-if="modalSuccess"
      class="left-0 top-0 w-screen h-screen absolute text-center flex justify-center items-center z-50 bg-black bg-opacity-50">
      <MyModal :config="config" :spiner-loading="spinerLoading" :modal-cupom="modalCupom" :cupom="cupom"
        :cut-code="cutCode" :past-code="pastCode" @closeNow="closeNow" @copiar-numero="copyNumber"
        @hidden-cut="hiddenCut" />
    </div>
    <transition name="modal-transition">
      <div v-if="bases"
        class="left-0 top-0 w-screen h-screen absolute text-center flex justify-center items-center z-50 bg-black bg-opacity-50">
        <div class="after-modal max-w-sm absolute text-start rounded-xl pb-2 px-5">
            <PopupHeaderModal :config="config" @close-in="closeIn" v-if="true" />
            <form @submit.prevent="submitForm">
              <FormField :config="config" />
              <div class="flex items-center flex justify-between">
                <p class="text-violet-300">{{ config.formsLayout.titleGender }}</p>
                <GenderSelect :options="config.formsLayout.gender" :selectedGender.sync="selectedGender" class="cursor-pointer" />
              </div>
              <CheckInput v-if="config.firstModal.consentCheckbox" inputId="consentCheckbox" :label="config.firstModal.shareData" :checked="checkedShare"/>
              <CheckInput v-if="config.firstModal.consentCheckbox" inputId="consentCheckbox" :label="config.firstModal.acceptTerms"
                :checked="consentChecked" @update:checked="consentChecked = $event" />
              <ButtonSend :is-form-valid="isFormValid" button-text="Enviar" />
            </form>
        </div>
      </div>
    </transition>
    <transition name="modal-transition">
      <div v-if="showFormModal && isModalOpen('form')"
        class="left-0 top-0 w-screen h-screen absolute text-center flex justify-center items-center z-50 bg-black bg-opacity-50">
        <div class="after-modal max-w-sm absolute text-start rounded-xl pb-2 px-5">
            <PopupHeaderModalTwo :config="config" @closeModal="closeModal" />
            <form @submit.prevent="submitForm">
              <FormField :config="config" />
              <div class="flex items-center flex justify-between">
                <p class="text-violet-300">{{ config.formsLayout.titleGender }}</p>
                <GenderSelect :options="config.formsLayout.gender" :selectedGender.sync="selectedGender" class="cursor-pointer"/>
              </div>
              <CheckInput v-if="config.firstModal.consentCheckbox" inputId="consentCheckbox" :label="config.firstModal.shareData" :checked="checkedShare" />
              <CheckInput v-if="config.firstModal.consentCheckbox" inputId="consentCheckbox" :label="config.firstModal.acceptTerms"
                :checked="consentChecked" @update:checked="consentChecked = $event" />
              <ButtonSend :is-form-valid="isFormValid" button-text="Enviar" />
            </form>
        </div>
      </div>
    </transition>
    <transition name="modal-transition">
      <div v-if="showSecondFormModal && isModalOpen('news')"
        class="left-0 top-0 w-screen h-screen absolute text-center flex justify-center items-center z-50 bg-black bg-opacity-50">
        <div class="video-modal text-white px-5 max-w-sm absolute text-start rounded-xl pb-2">
          <VideoSectionTwo :config="config" @closeModal="closeModal" />
          <form @submit.prevent="submitForm">
            <FormField :config="config" />
            <div class="flex items-center flex justify-between text-sm">
              <p class="text-violet-300">{{ config.formsLayout.titleGender }}</p>
              <GenderSelect :options="config.formsLayout.gender" :selectedGender.sync="selectedGender" class="cursor-pointer" />
            </div>
            <CheckInput v-if="config.secondModal.consentCheckbox" inputId="consentCheckbox" :label="config.secondModal.shareData" :checked="checkedShare" />
            <CheckInput v-if="config.secondModal.consentCheckbox" inputId="consentCheckbox" :label="config.secondModal.acceptTerms"
              :checked="consentChecked" @update:checked="consentChecked = $event" />
            <ButtonSend :is-form-valid="isFormValid" button-text="Enviar" />
          </form>
        </div>
      </div>
    </transition>
    <transition name="modal-transition">
      <div v-if="showSecondModal"
        class="left-0 top-0 w-screen h-screen absolute text-center flex justify-center items-center z-50 bg-black bg-opacity-50">
        <div class="video-modal text-white px-5 max-w-sm absolute text-start rounded-xl pb-2">
          <VideoSection :config="config" @closeSecondModal="closeSeconde" />
          <form @submit.prevent="submitForm">
            <FormField :config="config" />
            <div class="flex items-center flex justify-between text-sm">
              <p class="text-violet-300">{{ config.formsLayout.titleGender }}</p>
              <GenderSelect :options="config.formsLayout.gender" :selectedGender.sync="selectedGender" class="cursor-pointer" />
            </div>
            <CheckInput v-if="config.secondModal.consentCheckbox" inputId="consentCheckbox" :label="config.secondModal.shareData" :checked="checkedShare" />
            <CheckInput v-if="config.secondModal.consentCheckbox" inputId="consentCheckbox" :label="config.secondModal.acceptTerms"
              :checked="consentChecked" @update:checked="consentChecked = $event" />
            <ButtonSend  :is-form-valid="isFormValid" button-text="Enviar" />
          </form>
        </div>
      </div>
    </transition>
  </div>
</template>
