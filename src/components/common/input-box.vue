<template>
  <div class="input-box">
    <span class="title">{{ title }}</span>
    <input
      type="text"
      @focus="onFocus"
      :placeholder="placeholder"
      :value="inputVal"
      @input="vModel($event.target.value)"
      ref="inputText"
    />
    <div v-if="isShowBtn" class="edit">
      <i class="icon"></i>
      <span @click="edit">修改</span>
    </div>
    <div v-else class="save">
      <span class="save-btn" @click="save">保存</span>
      <span class="cancel-btn" @click="cancel">取消</span>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "input-box",
  props: {
    placeholder: String,
    title: String,
    keyNum: Number,
    value: {
      type: String,
      default: () => ""
    }
  },
  data() {
    return {
      currentIndex: 0,
      isShowBtn: true,
      inputVal: this.value,
      saveVal: ""
    };
  },
  methods: {
    onFocus() {
      // 聚焦事件
      this.isShowBtn = false;
    },
    edit() {
      // 修改按钮
      this.$refs.inputText.focus();
      this.isShowBtn = false;
    },
    cancel() {
      // 取消按钮
      console.log("取消按钮");
      this.inputVal = this.value;
      this.isShowBtn = true;
    },
    save() {
      // 保存按钮
      if (this.inputVal == "") {
        alert("保存信息不能为空");
        return;
      }
      this.inputVal = this.saveVal;
      this.isShowBtn = true;
      this.$emit("save-info", this.inputVal, this.keyNum);
    },
    changeValue() {
      // 把父组件传过来的值，保存到data中
      this.inputVal = this.value;
    },
    vModel(val: string) {
      this.saveVal = val;
      this.inputVal = val;
    }
  },
  created() {
    this.changeValue();
  },
  mounted() {
    console.log(this.value);
  },
  watch: {
    inputVal(val) {
      console.log(val);
    }
  }
});
</script>

<style scoped lang="scss">
$Color: #007fff;
$boderColor: #f5f5f6;
.input-box {
  display: flex;
  .title {
    width: 100px;
    line-height: 30px;
  }
  input {
    width: 60%;
    margin: 0 20px;
    height: 30px;
    color: #666;
    border: 0;
    outline: none;
  }
  .edit {
    display: flex;
    text-align: right;
    width: 100px;
    height: 30px;
    align-items: center;
    justify-content: center;
    .icon {
      cursor: pointer;
      display: block;
      width: 24px;
      height: 24px;
      background: url(../../assets/img/more/edit-icon.6d6382b.svg) no-repeat
        center;
    }
    span {
      cursor: pointer;
      color: $Color;
      font-size: 14px;
    }
  }
  .save {
    display: flex;
    width: 100px;
    height: 30px;
    font-size: 14px;
    text-align: right;
    align-items: center;
    justify-content: center;
    color: $Color;
    span {
      cursor: pointer;
    }
    .save-btn {
      margin-right: 8px;
    }
    .cancel-btn {
      color: #999;
    }
  }
}
</style>
