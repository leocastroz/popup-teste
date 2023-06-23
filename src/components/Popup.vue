<script>
import "../assets/config/popup.css";
import PopupHeader from "./PopupHeader.vue";
import PopupContainer from "./PopupContainer.vue";
import PopupHelp from "./PopupHelp.vue";
import jsonData from "../data/popupConfig";
import PopupButtons from "./PopupButtons.vue";
import GenderSelect from "./GenderSelect.vue";
import MyModal from "./MyModal.vue";
import PopupHeaderModal from "./PopupHeaderModal.vue";
import PopupHeaderModalTwo from "./PopupHeaderModalTwo.vue";
import VideoSection from "./VideoSection.vue";
import VideoSectionTwo from "./VideoSectionTwo.vue";

export default {
  components: {
    PopupHeader,
    PopupContainer,
    PopupHelp,
    PopupButtons,
    GenderSelect,
    MyModal,
    PopupHeaderModal,
    PopupHeaderModalTwo,
    VideoSection,
    VideoSectionTwo,
  },
  data() {
    return {
      openModals: [],
      formValues: [],
      consentChecked: true,
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
      showSecondFormModal: true,
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
              <PopupHeaderModal
                :config="config"
                @close-in="closeIn"
                v-if="true"
              />
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
                <div class="pt-3">
                  <label
                    for="consentCheckbox"
                    class="text-violet-300 pr-2 text-xs"
                    >{{ config.shareData }}</label
                  >
                  <input type="checkbox" />
                </div>
                <div v-if="config.consentCheckbox" class="my-2">
                  <label
                    for="consentCheckbox"
                    class="text-violet-300 pr-2 text-xs"
                    >{{ config.acceptTerms }}</label
                  >
                  <input
                    type="checkbox"
                    id="consentCheckbox"
                    v-model="consentChecked"
                  />
                </div>
                <button
                  class="my-2 py-2 px-5 rounded text-xs font-extrabold"
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
            <div class="dates py-2 px-10">
              <PopupHeaderModalTwo :config="config" @closeModal="closeModal" />

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
                <div class="pt-3">
                  <label
                    for="consentCheckbox"
                    class="text-violet-300 pr-2 text-xs"
                    >{{ config.shareData }}</label
                  >
                  <input type="checkbox" />
                </div>
                <div v-if="config.consentCheckbox" class="my-2">
                  <label
                    for="consentCheckbox"
                    class="text-violet-300 pr-2 text-xs"
                    >{{ config.acceptTerms }}</label
                  >
                  <input
                    type="checkbox"
                    id="consentCheckbox"
                    v-model="consentChecked"
                  />
                </div>
                <button
                  class="my-2 py-2 px-5 rounded text-xs font-extrabold"
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
        <div v-if="showSecondFormModal && isModalOpen('news')" class="modal">
          <div class="video-modal text-white px-5 max-w-sm">
            <VideoSectionTwo :config="config" @closeModal="closeModal" />
            <div class="text-white">
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
                <div
                  class="my-genders flex items-center flex justify-between text-sm"
                >
                  <p class="text-violet-300 pr-2">{{ config.titleGender }}</p>
                  <GenderSelect
                    :options="config.gender"
                    :selectedGender.sync="selectedGender"
                    class="mr-36 pl-3"
                  />
                </div>
                <div class="pt-3">
                  <label
                    for="consentCheckbox"
                    class="text-violet-300 pr-2 text-xs"
                    >{{ config.shareData }}</label
                  >
                  <input type="checkbox" />
                </div>
                <div v-if="config.consentCheckbox" class="my-2">
                  <label
                    for="consentCheckbox"
                    class="text-violet-300 pr-2 text-xs"
                    >{{ config.acceptTerms }}</label
                  >
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
          <div class="video-modal text-white px-5 max-w-sm">
            <VideoSection :config="config" @closeSecondModal="closeSeconde" />
            <div class="text-black">
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
                <div
                  class="my-genders flex items-center flex justify-between text-sm"
                >
                  <p class="text-violet-300 pr-2">{{ config.titleGender }}</p>
                  <GenderSelect
                    :options="config.gender"
                    :selectedGender.sync="selectedGender"
                    class="mr-36 pl-3"
                  />
                </div>
                <div class="pt-3">
                  <label
                    for="consentCheckbox"
                    class="text-violet-300 pr-2 text-xs"
                    >{{ config.shareData }}</label
                  >
                  <input type="checkbox" />
                </div>
                <div v-if="config.consentCheckbox" class="my-2">
                  <label
                    for="consentCheckbox"
                    class="text-violet-300 pr-2 text-xs"
                    >{{ config.acceptTerms }}</label
                  >
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
