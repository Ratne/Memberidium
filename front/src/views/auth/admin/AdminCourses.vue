<template>
  <div class="home pt-5 mt-5 px-md-5 px-2">
    <div v-if="showAddCourse==false">
      <TitleH1 label="Amministrazione Corsi"></TitleH1>
      <div class="row bgGrayLight containerCard">
        <div class="card mb-sm-2 courseAdminUser" v-for="course of courses" @click="goToCourse(course._id)">
          <img :src="course.image" class="card-img-top mt-2" alt="course image">
          <div class="card-body">
            <h5 class="card-title">{{ course.name }}</h5>
            <p class="card-text" v-html="course.description"></p>
            <p class="badge text-black bg-primary"> Tag: {{ course.requiredTag }}</p>
          </div>
        </div>
      </div>
      <div class="mb-3 mt-3">
        <icon-button label="Aggiungi Corso" icon="bi bi-plus-circle" @clickEvent="showAddCourse=true"/>
      </div>
    </div>


    <div v-if="showAddCourse" class="container pt-5">
      <div class="row">
        <div class="col-12 pt-5">
          <h3 class="text-center">Aggiungi corso</h3>
          <form class="text-start" @submit.prevent="courseAdd">
            <FormGroupCustom name="name" :error="errors['name']" v-model:value="course.name" label="name"
                             type="text"></FormGroupCustom>
            <span class="text-start">Descrizione Corso</span>
            <editor-text-area name="description" :error="errors['description']" label="description"
                              v-model:dataValue="course.description"/>
            <FormGroupCustom name="image" :error="errors['image']" @change="onFileChange" label="image"
                             type="file"></FormGroupCustom>
            <FormGroupCustom name="requiredTag" :error="errors['requiredTag']" v-model:value="course.requiredTag"
                             label="tag" type="number"></FormGroupCustom>
            <button class="btn btn-primary mt-3 mb-3 text" type="submit">Invia</button>
          </form>
          <div class="text-start">
            <icon-button label="Chiudi" icon="bi bi-x-circle" @click="showAddCourse=false"/>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>


import {coursesIndex, coursesStore} from "../../../services/coursesService";
import FormGroupCustom from "../../../components/shared/form/FormGroupCustom";
import {validationMixin} from "../../../mixins/validationMixin";
import {validationTypeName} from "../../../utils/validationType";
import EditorTextArea from "../../../components/shared/form/EditorTextArea";
import {setFormDataWithImage} from "../../../utils/requestUtils";
import IconButton from "../../../components/shared/design/iconButton";
import TitleH1 from "../../../components/shared/design/TitleH1";


export default {
  name: 'AdminCourses',
  components: {TitleH1, IconButton, EditorTextArea, FormGroupCustom},
  data() {
    return {
      courses: [],
      showAddCourse: false,
      course: {},
      validazione: [
        {
          name: 'name',
          validation: {
            type: validationTypeName.required,
          }
        },
        {
          name: 'description',
          validation:
              {type: validationTypeName.required}
        },
        {
          name: 'image',
          validation: {
            type: validationTypeName.required,
          }
        },
        {
          name: 'requiredTag',
          validation: {
            type: validationTypeName.required,
          }
        }
      ]
    }
  },
  mixins: [validationMixin],
  mounted() {
    coursesIndex().then(res => {
      this.courses = res
    })
  },
  methods: {
    courseAdd() {

      this.$store.dispatch('resetErrors');
      let formData = setFormDataWithImage(this.course)
      if (this.isValid(this.course)) {
        coursesStore(formData).then(res => {
          this.courses.push(res)
          this.course = {}
          this.showAddCourse = false
        })
      }
    },
    goToCourse(id) {
      this.$router.push({
        name: 'SingleCourse',
        params: {
          id
        }
      })
    },
    onFileChange(event) {
      this.course.image = event.target.files[0]
    }
  },
  computed: {
    allValidations() {
      return [...this.validazione]

    }
  }
}
</script>
<style scoped lang="scss">
@import "src/sass/variables";
.containerCard{
  display: flex;
  flex-wrap: wrap;
  gap: 16px;
  padding: 16px;

}
.card {
  cursor: pointer;



  &.courseAdminUser {
    display: flex;
    flex-direction: column;
    height: 100%;
    flex-basis: 24%;
    padding: 25px;
    background-color: $white;
    border-radius: 0px;
    margin-bottom: 32px;
    border: none;
    color: $dark;
    min-height: 550px ;
    min-width: 260px;
  }

  .card-text {
    max-height: 200px;
    overflow: auto;
  }

}

</style>
