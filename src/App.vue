<template>
  <div class="container">
    <div class="inventory">
      <img src="./assets/images/Img.png" alt="" class="inventory__image">
      <div class="item">
        <div class="item__title"></div>
        <div class="item__descr"></div>
        <div class="item__descr"></div>
        <div class="item__descr"></div>
        <div class="item__descr"></div>
        <div class="item__descr"></div>
      </div>
    </div>
    <div class="cells">
      <div class="cells__droppable"
           v-for="cell in cells"
           :key="cell"
           @drop="onDrop($event, cell)"
           @dragover.prevent
           @dragenter.prevent
           @click="openModalToggle($event)"
      >
        <div class="cells__draggable"
             v-for="item in items.filter(x => x.category === cell)"
             @dragstart="onDragStart($event, item)"
             :key="item.id"
             draggable="true"
        >
          <img class="cells__draggable-image" :src="item.src" alt="s">
          <div class="draggable__id">{{ item.id }}</div>
        </div>
      </div>
      <div
          v-show="openModal"
          class="cells__modal modal"
          v-for="item in items.filter(x => x.id === activeItemId)"
          :key="item.id"
      >
        <div class="modal__image">
          <img :src="item.src" alt="s">
        </div>
        <div class="modal__divider"></div>
        <div class="modal__info item">
          <div class="item__title"></div>
          <div class="item__descr"></div>
          <div class="item__descr"></div>
          <div class="item__descr"></div>
          <div class="item__descr"></div>
          <div class="item__descr"></div>
        </div>
        <div class="modal__divider"></div>
        <button class="modal__btn"
                @click.prevent="deleteItem(item.id)"
        >Удалить предмет</button>
        <button
            class="close"
            @click="closeModal"
        ></button>
      </div>
    </div>
    <div class="bottom-block">
      <div class="bottom-block__item"></div>
      <button class="close"></button>
    </div>
  </div>
</template>

<script setup lang="ts">
import {ref} from "vue";

const openModal = ref(false)
  const cells = ref([1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25])
  const items = ref([
    {
      id: 1,
      category: parseInt(localStorage.getItem('1')) || 1,
      src: '../src/assets/images/svg/itemImage.svg',
    },
    {
      id: 2,
      category: parseInt(localStorage.getItem('2')) || 2,
      src: '../src/assets/images/svg/itemImage2.svg',
    },
    {
      id: 3,
      category: parseInt(localStorage.getItem('3')) || 3,
      src: '../src/assets/images/svg/itemImage3.svg',
    }
  ])
  const activeItemId = ref<number>(0)


  const onDragStart = (event: DragEvent, item) => {
    event.dataTransfer.dropEffect = 'move'
    event.dataTransfer.effectAllowed = 'move'
    event.dataTransfer.setData('itemId', item.id)

  }
  const onDrop = (event: DragEvent, id) => {
    const itemId = parseInt(event.dataTransfer.getData('itemId'))
    const item  = items.value.find(item => item.id === itemId)
    item.category = id
    localStorage.setItem(String(item.id), String(item.category))
  }

  const openModalToggle = (event) => {
    if (event.target.matches('.cells__draggable')) {
      openModal.value = !openModal.value
      activeItemId.value = parseInt(event.target.querySelector('.draggable__id').textContent)
    }
    if (!event.target.matches('.cells__draggable')) {
      openModal.value = false
    }
  }
  const deleteItem = (id) => {
    localStorage.removeItem(id)
    return items.value = items.value.filter(x => x.id !== id)
  }
  const closeModal = () => {
    openModal.value = false
  }
</script>[

<style lang="scss">
@import "assets/style/main";

.container {
  margin: 40px auto;
  max-width: 850px;
  padding: 32px;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}
.inventory {
  width: 236px;
  background-color: #262626;
  padding: 18px 14px 0 14px;
  border: 1px solid #4D4D4D;
  border-radius: 12px;
}
.cells {
  position: relative;
  display: grid;
  width: 525px;
  grid-template-rows: repeat(5, 1fr);
  grid-template-columns: repeat(5, 1fr);
  &__droppable {
    background-color: #262626;

    border: 1px solid #4D4D4D;
    display: flex;
    align-items: center;
    justify-content: center;

    &:nth-child(1) {
      border-radius: 12px 0 0 0;
    }
    &:nth-child(5) {
      border-radius: 0 12px 0 0;
    }
    &:nth-child(21) {
      border-radius: 0 0 0 12px;
    }
    &:nth-child(25) {
      border-radius: 0 0 12px 0;
    }
  }
  &__draggable {
    display: flex;
    position: relative;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 100%;

    &::after {
      position: absolute;
      content: '';
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
    }
  }
}
.draggable__id {
    position: absolute;
    display: flex;
    bottom: -1px;
    right: -1px;
    width: 16px;
    height: 16px;
    border: 1px solid #4D4D4D;
    color: #FFFFFF;
    border-radius: 6px 0 0 0;
    align-items: center;
    justify-content: center;
    font-size: 10px;
}
.bottom-block {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 72px;
  padding: 18px;
  background-color: #262626;
  border: 1px solid #4D4D4D;
  border-radius: 12px;
  margin-top: 24px;

  &__item {
    width: 700px;
    height: 36px;
    border-radius: 12px;
    background: linear-gradient(90deg, rgba(60,60,60,1) 0%, rgba(68,68,68,1) 50%, rgba(51,51,51,1) 100%);
  }
}
.close {
  position: absolute;
  top: 10px;
  right: 10px;
  height: 12px;
  width: 12px;
  cursor: pointer;

  &::before,
  &::after {
    content: '';
    position: absolute;
    top: 10px;
    display: block;
    width: 12px;
    height: 1px;
    background: #FFFFFF;
  }
  &::before {
    transform: rotate(45deg);
  }
  &::after {
    transform: rotate(-45deg);
  }
}
.modal {
  position: absolute;
  top: 0;
  right: 0;
  width: 250px;
  display: flex;
  height: -webkit-fill-available;
  flex-direction: column;
  padding: 42px 15px 15px 18px;
  background-color: #262626;
  border: 1px solid #4D4D4D;
  border-radius: 0 12px 12px 0;

  &__image {
    display: flex;
    justify-content: center;
    align-items: center;
    padding-bottom: 30px;
    & img {
      width: 130px;
    }
  }
  &__divider {
    height: 1px;
    background-color: #4D4D4D;
    margin-bottom: 18px;
  }
  &__info {
    display: flex;
    flex-direction: column;
    margin-bottom: 24px;
  }
  &__btn {
    background-color: #FA7272;
    padding: 11px 55px;
    border-radius: 8px;
    color: #FFFFFF;

    width: 100%;
  }
}
.item {
  &__title {
    height: 30px;
    border-radius: 8px;
    margin-bottom: 24px;
    background: linear-gradient(90deg, rgba(60,60,60,1) 0%, rgba(68,68,68,1) 50%, rgba(51,51,51,1) 100%);
  }
  &__descr {
    height: 10px;
    border-radius: 4px;
    margin-bottom: 16px;
    background: linear-gradient(90deg, rgba(60,60,60,1) 0%, rgba(68,68,68,1) 50%, rgba(51,51,51,1) 100%);
  }
  &__descr:last-child {
    margin-bottom: 24px;
  }
}
.inventory {
  display: flex;
  flex-direction: column;
  justify-content: center;

  &__image {
    margin-bottom: 20px;
  }
}


</style>
