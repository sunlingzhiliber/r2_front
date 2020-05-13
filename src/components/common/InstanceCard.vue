<template>
  <el-card style="height:150px;margin:10px -15px">
    <div>
      <div
        v-if="cardData.type === 'model'"
        style="float:right; margin-top:-10px;"
      >
        <i class="iconfont icon-model" color="#daa520" />
        <span style="font-size:10px;color:#daa520">Model</span>
      </div>
      <div
        v-if="cardData.type === 'process'"
        style="float:right; margin-top:-10px;"
      >
        <i class="iconfont icon-process" color="#d2691e" />
        <span style="font-size:10px;color:#d2691e">Data Process</span>
      </div>
      <div
        v-if="cardData.type === 'evaluation'"
        style="float:right; margin-top:-10px;"
      >
        <i class="iconfont icon-dataanalysis" color="#d2691e" />
        <span style="font-size:10px;color:#d2691e">Evaluation</span>
      </div>
      <div class="title">
        <a href="#" @click.prevent="reTry(cardData)">{{ cardData.name }}</a>
      </div>
      <p class="cmpItemDesc">{{ cardData.description }}</p>
      <div id="bottom-info">
        <div class="info">
          <i class="el-icon-user" :size="15" />
          <span style="margin-left:10px; color:#2b85e4">{{
            cardData.creator ? cardData.creator : "sunlingzhi"
          }}</span>
        </div>

        <div class="info">
          <i class="el-icon-alarm-clock" :size="15" />
          <span style="margin-left:10px">{{ cardData.createTime }}</span>
        </div>
      </div>
    </div>
  </el-card>
</template>

<script>
export default {
  props: {
    cardData: {
      type: Object
    }
  },
  methods: {
    reTry({ type, id, serviceId }) {
      this.$router.push({
        path: `/resource/${serviceId}/${type}/invoke?instanceId=${id}`
      });
    }
  }
};
</script>

<style>
.title {
  /* height: 3em; */
  overflow: hidden;
  /* text-overflow: ellipsis; */
  display: -webkit-box;
  -webkit-line-clamp: 1;
  -webkit-box-orient: vertical;
}
.title a {
  color: #2d8cf0;
  text-decoration: none;
  font-weight: 600;
  font-size: 16px;
  line-height: 1em;
}
#bottom-info {
  display: flex;
  margin-top: 5px;
  justify-content: space-between;
}

.info {
  display: flex;
  align-items: center;
  margin-right: 10px;
}
</style>
