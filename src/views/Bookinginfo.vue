<template>
  <div>
    <el-row style="margin-top: 60px">
      <el-col :span="12" style="align-items: center"
        ><el-button color="#626aef" :dark="isDark" style="right: 30px"
          >器材预约信息</el-button
        ></el-col
      >
      <el-col :span="12"
        ><el-button color="#626aef" :dark="isDark"
          >课程预约信息</el-button
        ></el-col
      >
    </el-row>
    <el-row>
      <el-col :span="12">
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
                预约日期： {{      equipment.bookingTime }}
              </div>
              <div class="equipment-description">
                {{ equipment.description }}
              </div>
              <el-button
                type="danger"
                :disabled="equipment.isBooked"
                @click="deleteEquipment(equipment.id)"
                >取消预约</el-button
              >
            </div>
          </el-card>
        </div>
        <el-divider direction="vertical" />
      </el-col>
      <el-col :span="12">
        <div class="equipment-list">
          <el-card
            v-for="course in courseList"
            :key="course.id"
            :style="{ backgroundImage: `url(${course.img})` }"
            class="equipment-card"
            @mouseenter="hover = true"
            @mouseleave="hover = false"
          >
            <div class="equipment-info" :class="{ hover: hover }">
              <div class="equipment-name">{{ course.courseName }}</div>
              <div class="equipment-duration">
                课程时间： {{ course.duration }}分钟
                <br/>
                预约日期：{{ course.bookingTime}}
              </div>
              
              <div class="equipment-description">
                {{ course.courseDescription }}
              </div>
              <el-button
                type="danger"
                :disabled="course.isBooked"
                @click="deleteCourse(course.id)"
                >取消预约</el-button
              >
            </div>
          </el-card>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import request from "@/utils/request";
import {ElMessage} from "element-plus";
import {useUserStore} from "@/stores/user";

const activeTab = ref("");
const equipmentList = ref([]);
const courseList = ref([]);
const days = ref([]);

const userStore = useUserStore();
const token = userStore.getBearerToken;
const auths = userStore.getAuths;

const hover = ref(false);

const load = () => {
  request.get("/equipmentBookings/findByUserId/"+userStore.getUserId).then((res) => {
    if (res.code === "200") {
      equipmentList.value = res.data;
    }
  });
  request.get("/courseBookings/findByUserId/"+userStore.getUserId).then((res) => {
    if (res.code === "200") {
      courseList.value = res.data;
    }
  });
};

onMounted(() => {

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
async function deleteCourse(equipmentid) {

  request.delete('/courseBookings/' + equipmentid).then(res => {
    if (res.code === '200') {
      ElMessage.success('操作成功')
      load()  // 刷新表格数据
    } else {
      ElMessage.error(res.msg)
    }
  })

  load();
}

async function deleteEquipment(equipmentid) {

request.delete('/equipmentBookings/' + equipmentid).then(res => {
  if (res.code === '200') {
    ElMessage.success('操作成功')
    load()  // 刷新表格数据
  } else {
    ElMessage.error(res.msg)
  }
})

load();
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
  height: 200px;
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
  top: 100px;
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
  right: 7px;
  top: 6px;
  position: absolute;
}

.el-button {
  position: absolute;
  bottom: 20px;
  right: 20px;
}
</style>
