<template>
  <Transition name="scale" >
    <div class="modal" @click="closeModal">
      <div class="modal-content" @click.stop="">
        <h4 class="modal-content__title" v-if="!edit">{{words.modalTitle[lang]}}</h4>
        <h4 class="modal-content__title" v-else>{{words.modalTitleEdit[lang]}}</h4>
        <label class="modal-content__input">
          <span>Title</span>
          <input type="text" placeholder="Title" v-model="title">
        </label>
        <label class="modal-content__input">
          <span>Content</span>
          <input type="text" placeholder="Content" v-model="desc">
        </label>
        <div class="modal-content__btns">
          <button class="modal-btn btn-cancel" @click="closeModal">{{words.cancelBtn[lang]}}</button>
          <button class="modal-btn btn-add" @click="addItem" v-if="!edit">{{words.addBtn[lang]}}</button>
          <button class="modal-btn btn-add" @click="editItem" v-else>{{words.changeBtn[lang]}}</button>
        </div>
      </div>
    </div>
  </Transition>
</template>

<script>
export default {
  props: ['currentId','edit','itemObj', 'lang'],
  inject: ['words'],
  data() {
    return {
      title: '',
      desc: '',
      id: this.currentId,
    }
  },
  methods: {
    closeModal(){
      this.$emit('closeModal', false)
      this.title = ''
      this.desc = ''
    },
    addItem(){
      if(this.title != '' && this.desc != ''){
        localStorage.itemId = this.id
        const item = {
          id: this.id++,
          title: this.title,
          desc: this.desc,
          date: new Date().toLocaleDateString()
        }
        this.$emit('addItem', item)
        this.closeModal()
      }
    },
    editItem(){
      if(this.title != '' && this.desc != ''){
        const itemEditObj = {
          id: this.itemObj.id,
          title: this.title,
          desc: this.desc,
          date: new Date().toLocaleDateString()
        }
        this.$emit('editItem', itemEditObj)
        this.closeModal()
      }
    }
  },
}
</script>