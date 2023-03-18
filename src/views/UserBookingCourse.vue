<template>
  <div>
   
    <el-tabs v-model="activeTab" @tab-click="onTabClick">
      <el-tab-pane v-for="day in days" :key="day" :label="day">
        <div class="equipment-list">
          <el-card
            v-for="equipment in equipmentList"
            :key="equipment.id"
            :style="{ backgroundImage: `url(${equipment.img})` }"
            class="equipment-card"
            @mouseenter="hover = true"
            @mouseleave="hover = false"
          >
            <div class="equipment-info" :class="{ hover: hover }">
              <div class="equipment-name">{{ equipment.name }}</div>
              <div class="equipment-duration">
                课程时间： {{ equipment.duration }}分钟
              </div>
              <div class="equipment-description">
                {{ equipment.description }}
              </div>
              <el-button
                type="primary"
                :disabled="equipment.isBooked"
                @click="bookEquipment(equipment.id, day)"
                >预约</el-button
              >
            </div>
          </el-card>
        </div>
      </el-tab-pane>
      <el-tag class="ml-2" type="warning">点击日期开始预约</el-tag>
    </el-tabs>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import request from "@/utils/request";
import {ElMessage} from "element-plus";
import {useUserStore} from "@/stores/user";

const activeTab = ref("");
const equipmentList = ref([]);
const days = ref([]);

const userStore = useUserStore();
const token = userStore.getBearerToken;
const auths = userStore.getAuths;

const hover = ref(false);

const load = () => {
  request.get("/courses").then((res) => {
    if (res.code === "200") {
      equipmentList.value = res.data;
    }
  });
};

onMounted(() => {
  // 设置只能预约最近7天的器材
  const now = new Date();
  for (let i = 0; i < 7; i++) {
    const date = new Date(now.getTime() + i * 24 * 60 * 60 * 1000);
    days.value.push(formatDate(date));
  }
  activeTab.value = days.value[0];

  // 请求器材数据
  load();
});

function onTabClick(tab) {
  activeTab.value = tab.label;
}

function formatDate(date) {
  const year = date.getFullYear();
  const month = (date.getMonth() + 1).toString().padStart(2, "0");
  const day = date.getDate().toString().padStart(2, "0");
  return `${year}-${month}-${day}`;
}
// 预约
async function bookEquipment(equipmentid, day) {
    request.request({
        url:'/courseBookings',
        method: 'post',
        data:{
            memberId:userStore.getUserId,
            courseId:equipmentid,
            bookingTime:day
        }
    }).then(res=>{
        if (res.code === '200'){
            ElMessage.success('预约成功')
        }else{
            ElMessage.success(res.msg)
        }
    })
  console.log(equipmentId, day);
}
</script>

<style scoped>
.equipment-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.equipment-card {
  width: 500px;
  margin: 20px;
  height: 300px;
  border-radius: 8px;
  position: relative;
  overflow: hidden;
  transition: transform 0.3s ease-out;
  cursor: pointer;
}

.equipment-card:hover {
  transform: scale(1.05);
}

.equipment-info {
  position: absolute;
  left: 0;
  backdrop-filter: blur(3px);
  right: 0;
  top: 200px;
  height: 100px;
  padding: 20px;
  background-color: rgba(0, 0, 0, 0.6);
  color: #fff;
}

.equipment-name {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 10px;
}

.equipment-description {
  font-size: 16px;
  margin-bottom: 10px;
  bottom: 13px;
}
.equipment-duration {
  right: 6px;
  top: 20px;
  position: absolute;

}

.el-button {
  position: absolute;
  bottom: 20px;
  right: 20px;
}
</style>
