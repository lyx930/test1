<template>
  <div id="app">
    <h2>考试</h2>
    <button @click="disAll">{{ dis }}</button>
    <br />
    <span>所在城市：</span>
    <select v-model="city">
      <option
        v-for="(item, index) in cityList"
        :key="index"
        :value="item.name"
        :disabled="isTrue"
      >
        {{ item.name }}
      </option>
    </select>
    <br />
    <span>用户名：</span>
    <input type="text" v-model="userName" :disabled="isTrue" />
    <br />
    <span>年龄：</span>
    <input type="text" v-model="age" :disabled="isTrue" />
    <br />
    <button :disabled="isTrue" @click="add">{{ addBtn }}</button>
    <table border="1" style="border-collapse: collapse" width="800">
      <thead>
        <tr>
          <th></th>
          <th>ID</th>
          <th>姓名</th>
          <th>年龄</th>
          <th>地区</th>
          <th>操作</th>
        </tr>
      </thead>
      <tbody>
        <tr align="center" v-for="(item, index) in goodsInfo" :key="index">
          <td>
            <input
              type="checkbox"
              :disabled="isTrue"
              :value="index"
              v-model="checkedArr"
            />
          </td>
          <td>{{ item.id }}</td>
          <td>{{ item.userName }}</td>
          <td>{{ item.age }}</td>
          <td>{{ item.city }}</td>
          <td>
            <button :disabled="isTrue" @click="change(index)">修改</button>
            <button :disabled="isTrue" @click="del(index)">删除</button>
          </td>
        </tr>
      </tbody>
    </table>
    <button :disabled="isTrue" @click="delAll">删除所选</button>
  </div>
</template>

<script>
import cityList from "../assets/data.json";
export default {
  name: "Test",
  data() {
    return {
      dis: "锁定",
      cityList: cityList.data,
      isTrue: false,
      userName: "",
      age: "",
      city: "",
      addBtn: "提交",
      goodsInfo: [],
      num: 1,
      currentIndex: 0,
      checkedArr: [],
      time: 4,
    };
  },
  methods: {
    // 添加方法
    add() {
      let obj = {
        id: this.num,
        userName: this.userName,
        age: this.age,
        city: this.city,
      };

      if (this.addBtn == "提交") {
        this.num++;
        this.goodsInfo.push(obj);
      } else {
        obj.id = this.goodsInfo[this.currentIndex].id;
        this.goodsInfo.splice(this.currentIndex, 1, obj);
        this.addBtn = "提交";
      }
      this.userName = "";
      this.age = "";
      this.city = "";
    },
    // 删除方法
    del(index) {
      this.goodsInfo.splice(index, 1);
    },
    // 修改操作
    change(index) {
      let data = this.goodsInfo[index];
      console.log(data);
      this.userName = data.userName;
      this.age = data.age;
      this.city = data.city;
      (this.addBtn = "确认修改"), (this.currentIndex = index);
    },
    // 删除所选
    delAll() {
      this.checkedArr
        .sort(function (a, b) {
          return b - a;
        })
        .forEach((item) => {
          this.goodsInfo.splice(item, 1);
        });
      this.checkedArr = [];
    },
    // 锁定与解锁
    disAll() {
      if (this.dis == "锁定") {
        let timer = setInterval(() => {
          this.time--;
          this.dis = this.time + "秒后解锁";
          this.isTrue = true;
          if (this.time == 0) {
            clearInterval(timer);
            this.dis = "解锁";
            this.time = 3;
          }
        }, 1000);
      } else {
        this.isTrue = false;
      }
    },
  },
};
</script>

<style>
</style>
