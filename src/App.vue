<template>
  <div id="app">
    <h1>Ghi chú</h1>
    <el-row :gutter="20">
      <el-col :span="3" class="group-btn">
        <el-button class="open" @click="drawer = true" type="primary">
          Thêm
        </el-button>
        <el-drawer
        title="Thêm ghi chú mới"
        :visible.sync="drawer"
        direction="ltr"
        :before-close="handleClose">
        <el-form class="notice-form" ref="form" :model="form" label-position="top" label-width="50px">
          <el-form-item label="Ghi chú mới">
              <el-input v-model="form.label" placeholder="Nhập chi chú mới"></el-input>
          </el-form-item>
          <el-form-item label="Chọn màu">
            <el-color-picker
              v-model="form.color"
              show-alpha
              :predefine="predefineColors">
            </el-color-picker>
          </el-form-item>
        </el-form>
        <el-button @click="add" type="success">Thêm</el-button>
      </el-drawer>
      </el-col>
      <el-col :span="20">
        <div class="notice-item">
        <draggable
        class="list-group"
        tag="ul"
        v-model="list"
        v-bind="dragOptions"
        @start="drag = true"
        @end="drag = false">
        <transition-group type="transition" :name="!drag ? 'flip-list' : null">
          <li
            class="list-group-item"
            v-for="(element, idx) in list"
            :key="element.label"
            :style="'border-color:' +element.color">
            <i :class=" element.fixed ? 'fa fa-anchor' : 'glyphicon glyphicon-pushpin' "
              @click="element.fixed = !element.fixed"
              aria-hidden="true"></i>
            {{ element.label }}
            <el-button class="el-icon-close close"
            @click="removeAt(idx)"
            size="small"></el-button>
          </li>
        </transition-group>
      </draggable>
    </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import draggable from 'vuedraggable';
import {cloneDeep } from 'lodash'
export default {
  name: 'app',
  
  data() {
    return {
      drawer: false,
      drag: false,
      list: [],
      form: {
        label: '',
        color: ''
      },

      predefineColors: [
          '#ff4500',
          '#ff8c00',
          '#ffd700',
          '#90ee90',
          '#00ced1',
          '#1e90ff',
          '#c71585',
          'rgba(255, 69, 0, 0.68)',
          'rgb(255, 120, 0)',
          'hsv(51, 100, 98)', 
      ]
    }
  },
  components: {
            draggable,
        },
  methods: {
    add() {
      this.list.push(cloneDeep(this.form));
    },
    removeAt(idx) {
      this.list.splice(idx, 1);
    },
    sort() {
      this.list = this.list.sort((a, b) => a.order - b.order);
    },
    checkMove(e) {
      window.console.log("Future index: " + e.draggedContext.futureIndex);
    }
  },
  computed: {
    dragOptions() {
      return {
        animation: 200,
        group: "description",
        disabled: false,
        ghostClass: "ghost"
      };
    }
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap');
* {
  font-family: 'Montserrat', sans-serif !important;
}
#app {
  text-align: center;
  color: #2c3e50;
  padding: 20px;
  background-color: #E4E7ED;
  border-radius: 5px;
}
#app .group-btn {
  .el-button {
  width: 45%;
  margin-top: 20px;
  }
  .open {
  width: 100%;
  margin: 0;
  }
}
.notice-form {
  background-color: #fff;
  padding: 20px;
  border-radius: 5px;
  .el-form-item {
    text-align: left;
  }
}
.notice-item {
  li {
    list-style: none;
    border: dashed ;
    margin: 10px;
    padding: 10px;
    background-color:#fff;
  }
  .close {
  float: right;
  margin-top: -6px;
  background-color: red;
  color: white;
  font-weight: bold;
}
}
</style>
