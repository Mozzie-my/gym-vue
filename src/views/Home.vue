<script setup>
import {reactive} from "vue";
import request from "@/utils/request";

const state = reactive({
  notice: []
})

const load = () => {
  request.get("/notice").then(res => {
    state.notice = res.data
  })
}
load()
</script>

<template>
  <div>
    <el-row :gutter="10">
      <el-col :span="12">
        <el-card shadow="hover">
          <template #header>
            <div>
              <span>巨石后台管理系统</span>
            </div>
          </template>
          <div>
            <div style="margin: 10px 0">
              随着社会文明的进步，人们希望身心健康、延年益寿，追求精神满足和享受人生乐趣。体育运动是实现这个目标的重要方式之一，它具有健身性、娱乐性、思想性等多方面的作用，可以充实人类的文化生活、提高文化水准和生命质量，改善人们的生活习惯，培养高尚品格，对社会的良性发展有积极的调节作用。
健身房管理如果采用纯人工的工作流程，会带来很大的不便和不必要的成本投入。如果其中的一步出现差错，就会带来很大的麻烦，从而增加了管理费用。另一方面，健身房的健身器材和工具繁多，其间事物定会大力耗费人力物力，导致资源的浪费，公司的利润达不到最大的优化。如果没有一套完善的管理系统，很难有发展成大型的健身房。由此，鉴于以上种种情况，本系统应运而生。本系统针对健身房业务设计而成，操作人员只需输入一些简单的汉字、数字，即可完成操作，达到自己的目标。可以实现健身的自动化与智能化，方便快捷，节省时间，大大提高了健身房的效率，方便了客户，也给企业带来了效益的最大化实现。所以，健身房急需这样一个管理信息系统。

            </div>
          </div>
        </el-card>
      </el-col>

      <!-- <el-col :span="12">
        <el-card shadow="hover">
          <template #header>
            <div>
              <span>技术栈</span>
            </div>
          </template>
          <div style="display: flex">
            <div style="flex: 1; padding: 0 20px; line-height: 30px">
              <h3 style="font-weight: bold">后端</h3>
              <div>SpringBoot2</div>
              <div>Mybatis-Plus</div>
              <div>Sa-Token</div>
              <div>JWT</div>
              <div>Mysql</div>
              <div>...</div>
            </div>
            <div style="flex: 1; padding: 0 20px;  line-height: 30px">
              <h3 style="font-weight: bold">前端</h3>
              <div>Vue3</div>
              <div>Pinia</div>
              <div>Element-Plus</div>
              <div>Axios</div>
              <div>WangEditor</div>
              <div>...</div>
            </div>
          </div>
        </el-card>
      </el-col> -->


        <el-col :span="12">
        <el-card style="width: 100%;  margin: 0px 0">
          <template #header>
            <div>
              <span>公告列表</span>
            </div>
          </template>
          <el-collapse accordion>
            <el-collapse-item v-for="(item,index) in state.notice" :key="item.id" :name="'' + index">
              <template #title>
                <span style="font-size: 20px;">{{ item.name }}</span>
                <span style="margin-left: 10px">{{ item.createTime }}</span>
              </template>
              <div v-html="item.content"></div>
            </el-collapse-item>
          </el-collapse>
        </el-card>
      </el-col>
    </el-row>

    <el-divider />

    
  </div>
</template>