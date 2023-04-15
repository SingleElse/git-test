<template>
  <q-card-section class="row text-center">
    <q-card-section class="col text-h4 text-primary">
      {{  header || "Hello My Professor!" }}
      <!--add item or modi-->
      <q-btn
        v-if="editing"
        @click="doEdit(false)"
        unelevated
        rounded
        icon-right="clear"
        label="음식 추가 취소"
      ></q-btn>
      <q-btn
      v-else
      @click="doEdit(true)"
      color="primary"
            unelevated
            rounded
            icon-right="send"
            label="음식 추가하기!"
      ></q-btn>
    </q-card-section>
  </q-card-section>


  <q-card-section class="row justify-center">
    <q-card-section v-if="editing" class="col-8 col-md-3">
      <q-input
        v-model="newFood"
        @keyup.enter="saveFood"
        label="음식 추가하기!"
      ></q-input>
    </q-card-section>

    <q-card-section v-if="editing" class="col-4 col-md-3">
      <q-checkbox
        class="q-pa-none"
        :disable="newFood.length == 0 ? true : false"
        v-model="newFoodHighPriority"
        :true-value="true"
        :false-value="false"
        label="가장 먹고 싶은 음식!"
      ></q-checkbox>
    </q-card-section>


     <q-card-section v-if="editing" class="col-12 col-md-3">
      <q-btn outline rounded color="primary" @click="saveFood()" label="음식리스트에 저장!"></q-btn>
    </q-card-section>
  </q-card-section>


  <q-card-section>
    <q-list bordered>
      <q-item
        v-for="food in reversedFoods"
        :key="food.id"
        @click="togglePurchased(food)"
        class="rounded-border"
        :class="{strikeout:food.purchased, priority:food.highPriority}"
        dense
        padding
        clickable
        v-ripple
      >
        <q-item-section>
          {{ food.label }}
        </q-item-section>
      </q-item>
    </q-list>
  </q-card-section>


  <q-item-section class="row text-center">
        <q-card-section>
          <span v-if="foods.length === 0"
            > 혹시 음식이 남아있지 않나요? 축하합니다! 다 드셔봤군요!</span>
        </q-card-section>
  </q-item-section>
</template>

<script>
export default {
  name:"My Food List Check Machine!",
  title:"나의 음식 체크 리스트!",
  data(){
    return {
      header:"My Food List Check Machine!",
      foods : [
      ],
      editing:false,
      newFood:"",
      newFoodHighPriority:false,
    }
  },
  computed : {
    reversedFoods(){
      return [...this.foods].reverse();
    }
  },
  methods :{
    togglePurchased(food){
      food.purchased = !food.purchased;
    },
    doEdit(editing){
      this.editing = editing;
      this.newFood = "";
      this.newFoodHighPriority = false;
    },
    async saveFood(){
      if(this.newFood.length == 0) return;
      this.foods.push({
        id: this.foods.length + 1,
        label: this.newFood,
        highPriority: this.newFoodHighPriority,
      });

      await this.$q.notify({
        color: "primary",
        icon: "done",
        message: ` <span style="color: white"><strong style="color: yellow">${this.newFood}</strong> 음식을 담았어요!</span>`,
        html: true,
      });
      this.newFood = "";
      this.newFoodHighPriority =false;
    }
  }

}
</script>

<style>
.strikeout{
  text-decoration: line-through;
  color: #04edfd;
}
.priority{
  color: #0faa62;
  font-weight: 600;
}
</style>