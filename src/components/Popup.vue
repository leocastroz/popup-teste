<script>
import PopupHeaderModalTwo from "./PopupHeaderModalTwo.vue";
import PopupHeaderModal from "./PopupHeaderModal.vue";
import VideoSectionTwo from "./VideoSectionTwo.vue";
import PopupContainer from "./PopupContainer.vue";
import VideoSection from "./VideoSection.vue";
import PopupButtons from "./PopupButtons.vue";
import GenderSelect from "./GenderSelect.vue";
import PopupHeader from "./PopupHeader.vue";
import jsonData from "../data/popupConfig";
import CheckInput from "./CheckInput.vue";
import PopupHelp from "./PopupHelp.vue";
import MyModal from "./MyModal.vue";
import "../assets/config/popup.css";
import Button from "./Button.vue";

export default {
  components: {
    PopupHeaderModalTwo,
    PopupHeaderModal,
    VideoSectionTwo,
    PopupContainer,
    PopupButtons,
    VideoSection,
    GenderSelect,
    PopupHeader,
    CheckInput,
    PopupHelp,
    MyModal,
    Button
  },
  data() {
    return {
      cupom: "https://github.com/leocastroz",
      showSecondFormModal: true,
      showSecondModal: false,
      spinerLoading: false,
      consentChecked: true,
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
    setTimeout(() => {
      this.bases = true;
    }, 1000);
  },
  computed: {
    isFormValid() {
      const requiredFields = this.config.formFields.filter(
        (field) => field.required
      );
      const filledFields = this.formValues.filter((value) => value !== "");
      const allRequiredFieldsFilled =
        requiredFields.length === filledFields.length;
      const consentChecked =
        !this.config.consentCheckbox || this.consentChecked;
      return allRequiredFieldsFilled && consentChecked;
    },
  },
  methods: {
    copiarNumero() {
      if (!this.exibirColar) {
        const el = document.createElement("textarea");
        el.value = this.cupom;
        document.body.appendChild(el);
        el.select();
        document.execCommand("copy");
        document.body.removeChild(el);
        this.exibirColar = true;
      }
    },
    closeIn() {
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
    closeNow() {
      this.modalSuccess = false;
      location.reload();
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
    hiddenCut() {
      this.cutCode = false;
      this.pastCode = true;
    },
    submitForm() {
      if (this.isFormValid) {
        this.modalSuccess = true;
        this.bases = false;
        this.showFormModal = false;
        this.showSecondModal = false;
        this.showSecondFormModal = false;
        this.spinerLoading = true;
        setTimeout(() => {
          this.spinerLoading = false;
          this.modalCupom = true;
        }, 3000);
      }
    },
  },
};
</script>

<template>
  <div v-if="config">
    <PopupHeader />
    <PopupContainer />
    <PopupHelp />
    <PopupButtons :openModal="openModal" />
    <div v-if="modalSuccess"
      class="left-0 top-0 w-screen h-screen absolute text-center flex justify-center items-center z-50 bg-black bg-opacity-50">
      <MyModal :config="config" :spiner-loading="spinerLoading" :modal-cupom="modalCupom" :cupom="cupom"
        :cut-code="cutCode" :past-code="pastCode" @closeNow="closeNow" @copiar-numero="copiarNumero"
        @hidden-cut="hiddenCut" />
    </div>
    <transition name="modal-transition">
      <div v-if="bases"
        class="left-0 top-0 w-screen h-screen absolute text-center flex justify-center items-center z-50 bg-black bg-opacity-50">
        <div class="after-modal max-w-sm absolute text-start rounded-xl">
          <div class="py-2 px-5">
            <PopupHeaderModal :config="config" @close-in="closeIn" v-if="true" />
            <form @submit.prevent="submitForm">
              <p class="font-bold text-lg py-3 text-white">
                {{ config.titleForms }}
              </p>
              <div v-for="(field, index) in config.formFields" :key="index"
                class="bg-black-100 flex justify-between items-center">
                <label :for="'field' + index" class="text-sm text-violet-300 pr-3">{{ field.label }}</label>
                <input :type="field.type" :id="'field' + index" :value="field.value"
                  @input="updateFieldValue(index, $event.target.value)" required
                  class="rounded border-none my-2 bg-violet-300 p-1 mr-10" />
              </div>
              <div class="flex items-center flex justify-between">
                <p class="text-violet-300 pr-2">{{ config.titleGender }}</p>
                <GenderSelect :options="config.gender" :selectedGender.sync="selectedGender" class="mr-36" />
              </div>
              <CheckInput v-if="config.consentCheckbox" inputId="consentCheckbox" :label="config.shareData" />
              <CheckInput v-if="config.consentCheckbox" inputId="consentCheckbox" :label="config.acceptTerms"
                :checked="consentChecked" @update:checked="consentChecked = $event" />
              <Button :is-form-valid="isFormValid" button-text="Enviar" />
            </form>
          </div>
        </div>
      </div>
    </transition>
    <transition name="modal-transition">
      <div v-if="showFormModal && isModalOpen('form')"
        class="left-0 top-0 w-screen h-screen absolute text-center flex justify-center items-center z-50 bg-black bg-opacity-50">
        <div class="after-modal max-w-sm absolute text-start rounded-xl">
          <div class="py-2 px-5">
            <PopupHeaderModalTwo :config="config" @closeModal="closeModal" />
            <form @submit.prevent="submitForm">
              <p class="font-bold text-lg py-3 text-white">
                {{ config.titleForms }}
              </p>
              <div v-for="(field, index) in config.formFields" :key="index"
                class="bg-black-100 flex justify-between items-center">
                <label :for="'field' + index" class="text-sm text-violet-300 pr-3">{{ field.label }}</label>
                <input :type="field.type" :id="'field' + index" :value="field.value"
                  @input="updateFieldValue(index, $event.target.value)" required
                  class="rounded border-none my-2 bg-violet-300 p-1 mr-10" />
              </div>
              <div class="flex items-center flex justify-between">
                <p class="text-violet-300 pr-2">{{ config.titleGender }}</p>
                <GenderSelect :options="config.gender" :selectedGender.sync="selectedGender" class="mr-36" />
              </div>
              <CheckInput v-if="config.consentCheckbox" inputId="consentCheckbox" :label="config.shareData" />
              <CheckInput v-if="config.consentCheckbox" inputId="consentCheckbox" :label="config.acceptTerms"
                :checked="consentChecked" @update:checked="consentChecked = $event" />
              <Button :is-form-valid="isFormValid" button-text="Enviar" />
            </form>
          </div>
        </div>
      </div>
    </transition>
    <transition name="modal-transition">
      <div v-if="showSecondFormModal && isModalOpen('news')"
        class="left-0 top-0 w-screen h-screen absolute text-center flex justify-center items-center z-50 bg-black bg-opacity-50">
        <div class="video-modal text-white px-5 max-w-sm absolute text-start rounded-xl">
          <VideoSectionTwo :config="config" @closeModal="closeModal" />
          <form @submit.prevent="submitForm">
            <p class="font-bold text-lg py-3 text-white">
              {{ config.titleForms }}
            </p>
            <div v-for="(field, index) in config.formFields" :key="index"
              class="bg-black-100 flex justify-between items-center">
              <label :for="'field' + index" class="text-sm text-violet-300 pr-3">{{ field.label }}</label>
              <input :type="field.type" :id="'field' + index" :value="field.value"
                @input="updateFieldValue(index, $event.target.value)" required
                class="rounded border-none my-2 bg-violet-300 p-1 mr-10" />
            </div>
            <div class="flex items-center flex justify-between text-sm">
              <p class="text-violet-300 pr-2">{{ config.titleGender }}</p>
              <GenderSelect :options="config.gender" :selectedGender.sync="selectedGender" class="mr-36 pl-3" />
            </div>
            <CheckInput v-if="config.consentCheckbox" inputId="consentCheckbox" :label="config.shareData" />
            <CheckInput v-if="config.consentCheckbox" inputId="consentCheckbox" :label="config.acceptTerms"
              :checked="consentChecked" @update:checked="consentChecked = $event" />
            <Button :is-form-valid="isFormValid" button-text="Enviar" />
          </form>
        </div>
      </div>
    </transition>
    <transition name="modal-transition">
      <div v-if="showSecondModal"
        class="left-0 top-0 w-screen h-screen absolute text-center flex justify-center items-center z-50 bg-black bg-opacity-50">
        <div class="video-modal text-white px-5 max-w-sm absolute text-start rounded-xl">
          <VideoSection :config="config" @closeSecondModal="closeSeconde" />
          <form @submit.prevent="submitForm">
            <p class="font-bold text-lg py-3 text-white">
              {{ config.titleForms }}
            </p>
            <div v-for="(field, index) in config.formFields" :key="index"
              class="bg-black-100 flex justify-between items-center">
              <label :for="'field' + index" class="text-sm text-violet-300 pr-3">{{ field.label }}</label>
              <input :type="field.type" :id="'field' + index" :value="field.value"
                @input="updateFieldValue(index, $event.target.value)" required
                class="rounded border-none my-2 bg-violet-300 p-1 mr-10" />
            </div>
            <div class="flex items-center flex justify-between text-sm">
              <p class="text-violet-300 pr-2">{{ config.titleGender }}</p>
              <GenderSelect :options="config.gender" :selectedGender.sync="selectedGender" class="mr-36 pl-3" />
            </div>
            <CheckInput v-if="config.consentCheckbox" inputId="consentCheckbox" :label="config.shareData" />
            <CheckInput v-if="config.consentCheckbox" inputId="consentCheckbox" :label="config.acceptTerms"
              :checked="consentChecked" @update:checked="consentChecked = $event" />
            <Button :is-form-valid="isFormValid" button-text="Enviar" />
          </form>
        </div>
      </div>
    </transition>
  </div>
</template>
