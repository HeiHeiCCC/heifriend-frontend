<template>
  <user-card-list :user-list="userList"/>
  <van-empty v-if="!userList || userList.length < 1" description="数据为空" />

</template>

<script setup>
import {onMounted, ref} from "vue";
import {useRoute} from "vue-router";
import myAxios from "../plugins/myAxios.ts";
import Qs from "qs";
import UserCardList from "../components/UserCardList.vue";
import {showFailToast, showSuccessToast, Toast} from "vant";

const route = useRoute();

const userList = ref([])

const {tags} = route.query;

onMounted(async () => {
  const  userListData = await myAxios.get('/user/recommend', {
    params: {
      pageSize: 8,
      pageNum: 1,
    },

  })
      .then(function (response) {
        // 处理成功情况
        console.log('/user/recommend success', response);
        showSuccessToast('加载成功');

        return response?.data?.records;
      })
      .catch(function (error) {
        // 处理错误情况
        console.log('/user/recommend error', error);
        showFailToast('加载失败');
      })

  if (userListData) {
    userListData.forEach(user => {
      if (user.tags) {
        user.tags = JSON.parse(user.tags)
      }
    })
    userList.value = userListData;
  }
})
</script>

<style scoped>

</style>