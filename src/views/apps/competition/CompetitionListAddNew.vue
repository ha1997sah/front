<template>
  <b-sidebar
    id="add-new-user-sidebar"
    :visible="isAddNewUserSidebarActive"
    bg-variant="white"
    sidebar-class="sidebar-lg"
    shadow
    backdrop
    no-header
    right
    @hidden="resetForm"
    @change="(val) => $emit('update:is-add-new-user-sidebar-active', val)"
  >
    <template #default="{ hide }">
      <!-- Header -->
      <div class="d-flex justify-content-between align-items-center content-sidebar-header px-2 py-1">
        <h5 class="mb-0">
          Ajouter une competition
        </h5>

        <feather-icon
          class="ml-1 cursor-pointer"
          icon="XIcon"
          size="16"
          @click="hide"
        />

      </div>

      <!-- BODY -->
      <validation-observer
        #default="{ handleSubmit }"
        ref="refFormObserver"
      >
        <!-- Form -->
        <b-form
          class="p-2"
          @submit.prevent="handleSubmit(onSubmit)"
          @reset.prevent="resetForm"
        >

          <!-- Full Name -->
          <validation-provider
            #default="validationContext"
            name="titre"
            rules="required"

          >
            <b-form-group
              label="Titre"
              label-for="titre"
            >
              <b-form-input
                id="titre"
                v-model="clubData.name"
                autofocus
                :state="getValidationState(validationContext)"
                trim
              />

              <b-form-invalid-feedback>
                {{ validationContext.errors[0] }}
              </b-form-invalid-feedback>
            </b-form-group>
          </validation-provider>

          <!-- Country -->
          <validation-provider
            #default="validationContext"
            name="Adresse"
            rules="required"

          >
            <b-form-group
              label="Adresse"
              label-for="adresse"
            >
              <b-form-input
                id="adresse"
                v-model="clubData.location"
                :state="getValidationState(validationContext)"
                trim
              />

              <b-form-invalid-feedback>
                {{ validationContext.errors[0] }}
              </b-form-invalid-feedback>
            </b-form-group>
          </validation-provider>

            <validation-provider
            #default="validationContext"
            name="Description"
            rules="required"

          >
            <b-form-group
              label="description"
              label-for="description"
            >
              <b-form-input
                id="descrption"
                v-model="clubData.description"
                :state="getValidationState(validationContext)"
                trim
              />

              <b-form-invalid-feedback>
                {{ validationContext.errors[0] }}
              </b-form-invalid-feedback>
            </b-form-group>
          </validation-provider>

         
        <div class="form-group" v-for="(input,k) in inputs" :key="k">
   <b-row>
      <b-col md="6">
              <validation-provider
              #default="{ errors }"
            name="category"
            rules="required"
          >
             <b-form-group
              label="Catégorie d'age"
              label-for="age"
              :state="input.idCategory === null ? false : true"
            >
            <v-select
      v-model="input.idCategory"
      :options="roleOptions"
         label="label"
      />
        <b-form-invalid-feedback :state="errors.length > 0 ? false:null">
          {{ errors[0] }}
        </b-form-invalid-feedback>
            </b-form-group>
          </validation-provider>

       </b-col>
      <b-col md="6">
         <validation-provider
            #default="{ errors }"
            name="date"
            rules="required"
          >
            <b-form-group
              label="Date debut"
              label-for="date"
              :state="input.dateCategory=== null ? false : true"

            >
 <b-form-datepicker
      id="date-datepicker"
      v-model="input.dateCategory"
      class="mb-1"
    />
         <b-form-invalid-feedback :state="errors.length > 0 ? false:null">
          {{ errors[0] }}
        </b-form-invalid-feedback>
            </b-form-group>
          </validation-provider>
        </b-col>
        </b-row>
    <span>
        <i @click="remove(k)" v-show="k || ( !k && inputs.length > 1)">supprimer</i>
        <i  @click="add(k)" v-show="k == inputs.length-1">ajouter</i>
    </span>
</div>
          <!-- User Role -->
         
        
          <b-form-group>
                <b-form-file
               placeholder="Choisir une photo..."
                 drop-placeholder="Photo..."
                 @change="selectFile"
                   />
             </b-form-group>
      
          <!-- -->
              <validation-provider
            #default="{ errors }"
            name="start"
            rules="required"
          >
            <b-form-group
              label="Date debut"
              label-for="start"
              :state="clubData.start === null ? false : true"

            >
 <b-form-datepicker
      id="start-datepicker"
      v-model="clubData.start"
      class="mb-1"
    />
         <b-form-invalid-feedback :state="errors.length > 0 ? false:null">
          {{ errors[0] }}
        </b-form-invalid-feedback>
            </b-form-group>
          </validation-provider>


    <!---->
        <validation-provider
        #default="{ errors }"
            name="fin"
            rules="required"
          >
            <b-form-group
              label="Date fin"
              label-for="fin"
              :state="clubData.end === null ? false : true"

            >
<b-form-datepicker
      id="end-datepicker"
      v-model="clubData.end"
      class="mb-1"
    />
   <b-form-invalid-feedback :state="errors.length > 0 ? false:null">
          {{ errors[0] }}
        </b-form-invalid-feedback>
            </b-form-group>
          </validation-provider>
              <validation-provider
            #default="{ errors }"
            name="Deadline"
            rules="required"
          >
            <b-form-group
              label="Deadline inscription"
              label-for="deadline"
              :state="clubData.deadline === null ? false : true"

            >
<b-form-datepicker
      id="deadline-datepicker"
      v-model="clubData.deadline"
      class="mb-1"
    />
            <b-form-invalid-feedback :state="errors.length > 0 ? false:null">
          {{ errors[0] }}
        </b-form-invalid-feedback>
            </b-form-group>
          </validation-provider>
    <!---------->
          <!-- Form Actions -->
          <div class="d-flex mt-2">
            <b-button
              v-ripple.400="'rgba(255, 255, 255, 0.15)'"
              variant="primary"
              class="mr-2"
              type="submit"
            >
              Ajouter
            </b-button>
            <b-button
              v-ripple.400="'rgba(186, 191, 199, 0.15)'"
              type="button"
              variant="outline-secondary"
              @click="hide"
            >
              Effacer
            </b-button>
          </div>

        </b-form>
      </validation-observer>
    </template>
  </b-sidebar>
</template>

<script>
import {
  BSidebar, BForm,BFormFile,BFormDatepicker, BFormGroup, BFormInput, BFormInvalidFeedback, BButton,BCol,BRow
} from 'bootstrap-vue'
import { ValidationProvider, ValidationObserver } from 'vee-validate'
import { ref } from '@vue/composition-api'
import { required, alphaNum, email } from '@validations'
import formValidation from '@core/comp-functions/forms/form-validation'
import Ripple from 'vue-ripple-directive'
import vSelect from 'vue-select'
import countries from '@/@fake-db/data/other/countries'
import store from '@/store'
import ToastificationContent from '@core/components/toastification/ToastificationContent.vue'
import { useToast } from 'vue-toastification/composition'
import Multiselect from 'vue-multiselect'
import 'vue-multiselect/dist/vue-multiselect.min.css'

import authentication from '@/services/authentication.js'

export default {

  components: {
    Multiselect,
    BSidebar,
    BFormFile,
    BFormDatepicker,
    BForm,
    BRow,
    BFormGroup,
    BFormInput,
    BFormInvalidFeedback,
    BButton,
    vSelect,
    BCol,

    // Form Validation
    ValidationProvider,
    ValidationObserver,
  },
  directives: {
    Ripple,
  },
  model: {
    prop: 'isAddNewUserSidebarActive',
    event: 'update:is-add-new-user-sidebar-active',
  },
  props: {
    isAddNewUserSidebarActive: {
      type: Boolean,
      required: true,
    },
    	
    roleOptions: {
      type: Array,
      required: true,
    },
    planOptions: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      required,
      alphaNum,
      email,
      location,
      countries,
      error:null,
        options: [
        { name: 'Vue.js', language: 'JavaScript' },
        { name: 'Rails', language: 'Ruby' },
        { name: 'Sinatra', language: 'Ruby' },
        { name: 'Laravel', language: 'PHP' },
        { name: 'Phoenix', language: 'Elixir' }
      ]


    }
  },
      methods:{
       add(index) {
            this.inputs.push({ name: '' });
            console.log(this.inputs)
        },
        remove(index) {
            this.inputs.splice(index, 1);
        },
    },
  setup(props, { emit }) {
        const toast = useToast()
    const selectedCategories= ref(null)

    const blankclubData = {
      name: '',
	    lastname: '',
	    phone: '',
      email: '',
      password: '',
      role:null,
      error:null,
    }
   
           const file = ref(null)
    const  inputs= ref( [
            {   
                idCategory: null,
                dateCategory:null
            }
  ])
    const clubData = ref(JSON.parse(JSON.stringify(blankclubData)))
    const resetclubData = () => {
      clubData.value = JSON.parse(JSON.stringify(blankclubData))
    }
      const selectFile=(e) => {
   file.value=e.target.files[0]
  console.log(file.value)

      }
                 const ch= ref("")

    const onSubmit = () => {
      const formData = new FormData()
      formData.append('name',clubData.value.name)
      formData.append('start',clubData.value.start)
      formData.append('end',clubData.value.end)
      formData.append('location',clubData.value.location)
      formData.append('description',clubData.value.description)
      formData.append('compImage',file.value)
      formData.append('categories',JSON.stringify(inputs.value))
      formData.append('deadline',clubData.value.deadline)
    

      store.dispatch('app-competition/addCompetition',formData)
        .then(() => {
            toast({
          component: ToastificationContent,
          props: {
            title: 'Opération réussi',
            icon: 'AlertTriangleIcon',
            
            variant: 'success',
          },
        })

          emit('refetch-data')
          emit('update:is-add-new-user-sidebar-active', false)
        }).catch({error:error.message})
    }

    const {
      refFormObserver,
      getValidationState,
      resetForm,
    } = formValidation(resetclubData)

    return {
      clubData,
      onSubmit,
      refFormObserver,
      getValidationState,
      resetForm,
      selectFile,
      file,
      selectedCategories,
      inputs,
      
    }
  },
}
</script>

<style lang="scss">
@import '@core/scss/vue/libs/vue-select.scss';

#add-new-user-sidebar {
  .vs__dropdown-menu {
    max-height: 200px !important;
  }
}
</style>
