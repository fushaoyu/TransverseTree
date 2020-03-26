<template>
    <table>
      <tr>
        <td :colspan="treeData.childObjectiveList ? treeData.childObjectiveList.length * 2 : 1" :class="{parentLevel: treeData.children, extend: treeData.children && treeData.extend}">
          <div :class="{node: true, hasMate: treeData.mate}">
            <div class="person" @click="$emit('click-node', treeData)">
              <div class="content">
                <div class="describe">{{treeData.describe}}</div>
                <div class="schedule">
                  <div class="text">{{treeData.progressRate}}%</div>
                  
                </div>
              </div>
            </div>
          </div>
          <div class="extend_handle" v-if="treeData.childObjectiveList" ></div> 
          <!-- v-if="treeData.childObjectiveList" @click="toggleExtend(treeData)" -->
        </td>
      </tr>
      <tr v-if="treeData.childObjectiveList">
        <!--  && treeData.extend -->
        <td v-for="(children, index) in treeData.childObjectiveList" :key="index" colspan="2" class="childLevel">
          <TreeChart :json="children" @click-node="$emit('click-node', $event)"/>
        </td>
      </tr>
    </table>
</template>

<script>
export default {
  name: "TreeChart",
  props: ["json"],
  data() {
    return {
      treeData: {}
    };
  },
  mounted() {
  },
  updated() {},
  watch: {
    json: {
      handler: function(Props) {
        if (Props) {
          this.treeData = Props;
        }
      },
      immediate: true
    }
  },
  methods: {
    toggleExtend: function(treeData) {
      treeData.extend = !treeData.extend;
      this.$forceUpdate(); //迫使 Vue 实例重新渲染。注意它仅仅影响实例本身和插入插槽内容的子组件，而不是所有子组件
    },
  }
};
</script>

<style lang="less" scoped>
@rem: 750/10rem;
table {
  border-collapse: separate !important;
  border-spacing: 0 !important;
}
td {
  position: relative;
  vertical-align: top;
  padding: 0 0 50px 0;
  text-align: center;
}
.extend_handle {
  position: absolute;
  left: 50%;
  bottom: 30px;
  width: 25px;
  height: 25px;
  padding: 10px;
  transform: translate3d(-15px, 0, 0);
  cursor: pointer;
}
.extend_handle:before {
  content: "";
  display: block;
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  border: 5px solid;
  border-color: #000000 #000000 transparent transparent;
  transform: rotateZ(135deg);
  transform-origin: 50% 50% 0;
  transition: transform ease 300ms;
}
.extend_handle:hover:before {
  border-color: #333 #333 transparent transparent;
}
.extend .extend_handle:before {
  transform: rotateZ(-45deg);
}
.extend::after {
  content: "";
  position: absolute;
  left: 50%;
  bottom: 15px;
  height: 15px;
  border-left: 5px solid #000000;
  transform: translate3d(-1px, 0, 0);
}
.childLevel::before {
  content: "";
  position: absolute;
  left: 50%;
  bottom: 100%;
  height: 15px;
  border-left: 5px solid #000000;
  transform: translate3d(-1px, 0, 0);
}
.childLevel::after {
  content: "";
  position: absolute;
  left: 0;
  right: 0;
  top: -15px;
  border-top: 5px solid #000000;
}
.childLevel:first-child:before,
.childLevel:last-child:before {
  display: none;
}
.childLevel:first-child:after {
  left: 50%;
  height: 15px;
  border: 5px solid;
  border-color: #000000 transparent transparent #000000;
  border-radius: 18px 0 0 0;
  transform: translate3d(1px, 0, 0);
}
.childLevel:last-child:after {
  right: 50%;
  height: 15px;
  border: 5px solid;
  border-color: #000000 #000000 transparent transparent;
  border-radius: 0 18px 0 0;
  transform: translate3d(-1px, 0, 0);
}
.childLevel:first-child.childLevel:last-child::after {
  left: auto;
  border-radius: 0;
  border-color: transparent #000000 transparent transparent;
  transform: translate3d(1px, 0, 0);
}
.node {
  position: relative;
  display: inline-block;
  width: 13em;
  box-sizing: border-box;
  text-align: center;
}
.node .person {
  position: relative;
  display: inline-block;
  z-index: 2;
  width: 6em;
  overflow: hidden;
}
.node .person .avat {
  display: block;
  width: 4em;
  height: 4em;
  margin: auto;
  overflow: hidden;
  background: #fff;
  border: 3px solid #000000;
  box-sizing: border-box;
}
.node .person .avat img {
  width: 100%;
  height: 100%;
}
.node .person .name {
  height: 2em;
  line-height: 2em;
  overflow: hidden;
  width: 100%;
}
.node.hasMate::after {
  content: "";
  position: absolute;
  left: 2em;
  right: 2em;
  top: 2em;
  border-top: 5px solid #000000;
  z-index: 1;
}
.node.hasMate .person:last-child {
  margin-left: 1em;
}
.landscape {
  transform: translate(-100%, 0) rotate(-90deg);
  transform-origin: 100% 0;
}
.landscape .node {
  text-align: left;
  height: 8em;
  width: 8em;
}
.landscape .person {
  position: relative;
  transform: rotate(90deg);
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  top: 1em;
  left: 1em;
}
.landscape .person .avat {
  position: absolute;
  left: 0;
}
.landscape .person .name {
  height: 4em;
  line-height: 4em;
}
.landscape .hasMate {
  position: relative;
}
.landscape .hasMate .person {
  position: absolute;
}
.landscape .hasMate .person:first-child {
  left: auto;
  right: -4em;
}
.landscape .hasMate .person:last-child {
  left: -4em;
  margin-left: 0;
}

.content {
  border-radius: 5px;
  background: #6095ff80;
  .describe {
    font-size: 16px;
    color: #ffffff;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    box-sizing: border-box;
    padding: 10px;
  }
  .schedule {
    box-sizing: border-box;
    padding: 20px;
    .text {
      font-size: 14px;
      color: #ffffff;
      text-align: center;
    }
  }
}
</style>