<template>
  <Navbar
    @getSearch="search = $event"
    :lang="lang"
    @changeLang="changeLang"
  />
  <Contentitems
    :itemList="filterItemList"
    @changeItem="changeItem"
    @delItem="delItem"
    :lang="lang"
  />
  <Modal
    v-show="openOrCloseM"
    @closeModal="closeModal"
    @addItem="addItem"
    :currentId="currentId"
    :edit="edit"
    :itemObj="itemObj"
    @editItem="editItem"
    :lang="lang"
  />
  <AddBtn
    @openModal="openModal"
  />
</template>

<script>
import Navbar from '@/components/Navbar.vue'
import Contentitems from '@/components/Contentitems.vue'
import Modal from '@/components/Modal.vue'
import AddBtn from '@/components/AddBtn.vue'
import langs from '@/lang.js'
export default {
  components: {Navbar, Contentitems, Modal, AddBtn},
  data() {
    return {
      openOrCloseM: false,
      edit: false,
      currentId: 1,
      itemList: [],
      itemObj: {},
      search: '',
      lang: 'ru',
      langWords: {}
    }
  },
  computed: {
    filterItemList(){
      return this.search ? 
        this.itemList.filter(item => item.title.toLowerCase().includes(this.search.toLowerCase())) : 
        this.itemList
    }
  },
  methods: {
    changeLang(value){
      this.lang = localStorage.lang = value
    },
    openModal(bool){
      this.openOrCloseM = bool
      this.edit = false
    },
    closeModal(bool){
      this.edit = this.openOrCloseM = bool
    },
    addItem(itemObj){
      if(itemObj.title != '' && itemObj.desc != ''){
        this.itemList.push(itemObj)
      }
    },
    getItems(){
      const localList = localStorage.getItem('list')
      if(localList){
        this.itemList = JSON.parse(localList)
        this.currentId = JSON.parse(localStorage.itemId)
        this.currentId++
      }
    },
    changeItem(id){
      this.edit = this.openOrCloseM = true
      let obj = this.itemList.find(item => item.id == id)
      this.itemObj = obj
    },
    delItem(id){
      this.itemList = this.itemList.filter((item) => item.id != id)
    },
    editItem(obj){
      this.itemList.forEach(item => {
        if(item.id == obj.id){
          item.title = obj.title
          item.desc = obj.desc
          item.date = obj.date
        }
      })
    }
  },
  created() {
    this.getItems(),
    localStorage.lang = localStorage.lang || 'ru'
    this.lang = localStorage.lang || 'ru'
    this.langWords = langs
    localStorage.words = JSON.stringify(this.langWords)
  },
  watch: {
    itemList: {
      handler(updateList){
        console.log(updateList);
        localStorage.setItem('list', JSON.stringify(this.itemList))
      },
      deep: true
    }
  },
  provide(){
    return {
      words: JSON.parse(localStorage.words)
    }
  }
  
  
}
</script>
