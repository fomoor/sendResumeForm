<template>
  <div class="form">
    <form>
      <div class="div-input">
        <input v-model.trim="$v.name.$model" :class="{ 'input-error': $v.name.$error && $v.name.$model }" type="text" placeholder="Как вас зовут?">
        <label class="visible-label" :class="{ 'hidden-label': !$v.name.$model }">Как вас зовут?</label>
      </div>
      <div class="div-input">
        <input v-model="$v.email.$model" :class="{ 'input-error': $v.email.$error && $v.email.$model }" type="text" placeholder="Ваш E-mail" >
        <label class="visible-label" :class="{ 'hidden-label': !$v.email.$model }">Ваш E-mail</label>
      </div>
      <div class="div-input vacancy-select" ref="selectVacancy">
        <span @click="showVacancyList = !showVacancyList" class="vacancy-field">
          <span class="choose-vacancy-label" :class="{ 'hidden' : currentVacancy, 'visible' : showVacancyList }"> Выберите вакансию </span>
          <span class="current-vacancy" :class="{ 'hidden' : showVacancyList }"> {{ currentVacancyName }} </span>
          <span class="arrows material-symbols-outlined"> {{ !showVacancyList ? 'expand_more' : 'expand_less' }} </span>
        </span>
        <div class="vacancy-list" :class="{ 'hidden' : !showVacancyList}" @click="setCurrentVacancy($event.target, $event.target.id)">
          <p id="coder">Программист</p>
          <p id="designer">Верстальщик</p>
          <p id="analyst">Аналитик</p>
          <p id="seo">SEO специалист</p>
          <p id="content">Контент менеджер</p>
        </div>
      </div>
      <div class="div-input div-input-about">
        <textarea v-model.trim="$v.about.$model" type="text" placeholder="Напишите о себе кратко"></textarea>
        <label class="visible-label" :class="{ 'hidden-label': !$v.about.$model }">Кратко о себе</label>
      </div>
      <div>
        <label class="attach-file">
          <input type="file" id="file" ref="file" multiple name="input-name" style="display: none;" @change="filesToArray()" />
          <span class="icons material-symbols-outlined">attach_file</span>
          <span class="docs">Прикрепить документы</span>
        </label>
        <div>
          <p class="attach-file-names" v-for="item in files" :key="item" > 
            {{ item }}
            <span class="delete-icon material-symbols-outlined" @click="removeFromArray(item)">delete</span>
          </p>
        </div>
      </div>
      <br />
      <br />
      <div class="privacy">
        <div @click="agreement = !agreement">
          <span class="checked material-symbols-outlined"> {{ agreement ? 'check_box' : 'check_box_outline_blank' }} </span>
        </div>
        <label>Я соглашаюсь с <a href="https://en.wikipedia.org/wiki/Privacy_policy" target="_blank">политикой конфиденциальности</a>, нажимая на кнопку “Откликнуться”</label>
      </div>
      <br />
      <br />
      <button class="send-form" :disabled="$v.$invalid || agreement === false">Откликнуться</button>
    </form>
  </div>
</template>

<script>
import { required, maxLength } from 'vuelidate/lib/validators';

export default {
  name: 'ResumeForm',
  data () {
    return {
      name: null,
      email: null,
      currentVacancy: '',
      currentVacancyName: '',
      showVacancyList: false,
      about: null,
      files: [],
      agreement: false
    }
  },
  validations: {
    name: {
      required,
      maxLength: maxLength(15),
      alpha: val => /^[а-яёa-z- ]*$/i.test(val)
    },
    email: {
      required,
      maxLength: maxLength(30),
      alpha: val => /^[a-z0-9_\-\.\@]*$/i.test(val)
    },
    currentVacancy: {
      required
    },
    about: {
      maxLength: maxLength(100)
    }
  },
  created(){ 
    document.addEventListener('click', this.hideVacancyList)
  },
  destroyed () {
    document.removeEventListener('click', this.hideVacancyList)
  },
  methods: {
    filesToArray() {
      for ( let i = 0; i < this.$refs.file.files.length; i ++ ) {
        if ( this.files.length ) {
          for ( let j = 0; j < this.files.length; j ++ ) {
            if (this.files[j] === this.$refs.file.files[i].name) {
              return false;
            }
          }
        }
        this.files.push(this.$refs.file.files[i].name);
      }
    },
    removeFromArray(index) {
      this.files.splice(this.files.indexOf(index), 1);
    },
    hideVacancyList(e) {
      let el = this.$refs.selectVacancy;
      let target = e.target;
      if (el !== target && !el.contains(target)){
        this.showVacancyList = false;
      }
    },
    setCurrentVacancy(target, id) {
      if ( this.currentVacancy ) {
        document.getElementById(this.currentVacancy).classList.remove("selected");
      }
      
      target.classList.add("selected");

      this.currentVacancy = id;
      this.currentVacancyName = id === 'coder' ? 'Программист' : id === 'designer' ? 'Верстальщик' : id === 'analyst' ? 'Аналитик' : id === 'seo' ? 'SEO специалист' : 'Контент менеджер';
      this.showVacancyList = false;
    }
  }
}
</script>
