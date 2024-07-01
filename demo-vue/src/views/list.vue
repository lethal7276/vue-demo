  <template>
    <div class="list-comp">
      <h1>同学列表</h1>
      <div class="tool-bar">
        <button class="add-btn" type="primary" @click="openDialog()">新增</button>
      </div>

      <ul>
        <li>
          <span class="No">序号</span>
          <span class="study-code">学号</span>
          <span class="name">姓名</span>
          <span class="year">年龄</span>
          <div class="operation">操作</div>
        </li>
        <li v-for="(item, index) in list" :key="item.id">
          <span class="No">{{ index + 1 }}.</span>
          <span class="study-code">{{ item.id }}</span>
          <span class="name">{{ item.userName }}</span>
          <span class="year">{{ item.age }}</span>
          <div class="operation">
            <button @click="deleteUser(index)">删除</button>
            <button @click="openDialog(item, index)">编辑</button>
            <button @click="getYourName(item.id)">问名字</button>
          </div>
        </li>
      </ul>

      <div class="pop-blank" v-if="showFlag">
        <h3>{{ isEditing ? '编辑同学' : '新增同学' }}</h3>
        <div class="input-group">
          <span>学号</span>
          <input type="text" v-model="currentStudent.id" @input="validateId">
        </div>
        <div class="input-group">
          <span>姓名</span>
          <input type="text" v-model="currentStudent.userName">
        </div>
        <div class="input-group">
          <span>年龄</span>
          <input type="text" v-model="currentStudent.age">
        </div>
        <div class="footer">
          <button type="primary" @click="showFlag = false">取消</button>
          <button type="primary" @click="saveStudent">{{ isEditing ? '保存' : '确定' }}</button>
        </div>
      </div>
    </div>
  </template>

  <script setup>
  import { reactive, ref } from 'vue';

  const list = reactive([
    {
      id: 220812022,
      userName: '刘淑文',
      age: 18
    },
    {
      id: 220812008,
      userName: '蔡欣怡',
      age: 18
    },
    {
      id: 220812030,
      userName: '龙依',
      age: 18
    }
  ]);

  const showFlag = ref(false);
  const isEditing = ref(false);
  const currentStudent = reactive({
    id: '',
    userName: '',
    age: ''
  });
  const currentIndex = ref(-1);

  const openDialog = (student = null, index = -1) => {
    if (student) {
      currentStudent.id = student.id.toString();
      currentStudent.userName = student.userName;
      currentStudent.age = student.age;
      isEditing.value = true;
      currentIndex.value = index;
    } else {
      currentStudent.id = '';
      currentStudent.userName = '';
      currentStudent.age = '';
      isEditing.value = false;
      currentIndex.value = -1;
    }
    showFlag.value = true;
  };

  const deleteUser = index => {
    list.splice(index, 1);
  };

  const getYourName = id => {
    const student = list.find(item => item.id === id);
    alert(student.userName);
  };

  const saveStudent = () => {
    if (!validateUniqueId(currentStudent.id)) {
      alert('学号已存在，请输入唯一的学号');
      return;
    }
    if (!validateUniqueName(currentStudent.userName)) {
      alert('姓名已存在，请输入唯一的姓名');
      return;
    }
  
    if (isEditing.value) {
      list[currentIndex.value] = { ...currentStudent };
    } else {
      list.unshift({ ...currentStudent });
    }
    currentStudent.id = '';
    currentStudent.userName = '';
    currentStudent.age = '';
    showFlag.value = false;
  };

  const validateUniqueId = id => {
    return !list.some((item, index) => item.id.toString() === id && index !== currentIndex.value);
  };

  const validateUniqueName = name => {
    return !list.some((item, index) => item.userName === name && index !== currentIndex.value);
  };

  const validateId = event => {
    const value = event.target.value;
    if (!/^\d*$/.test(value)) {
      alert('学号必须是数字');
      event.target.value = currentStudent.id;
    } else {
      currentStudent.id = value;
    }
  };


</script>
<style lang="less" scoped>
.list-comp {
  text-align: left;
}
h1 {
  text-align: center;
}
.tool-bar {
  display: flex;
  justify-content: flex-end;
  .add-btn {
    background-color: rgba(42, 46, 54, 0.48);
    color: #ffffff;
    width: 90px;
    margin-right: 24px;
  }
}
ul {
  padding-left: 0;
  padding: 0 24px;
  text-align: center;
}
.pop-blank {
  position: absolute;
  background-color: #ffffff;
  border-radius: 8px;
  left: 50%;
  top: 50%;
  width: 50%;
  height: 50vh;
  transform: translate(-50%, -50%);
  padding: 24px;
  border: 1px solid rgba(0, 0, 0, 0.4);
    display: flex;
    flex-direction: column;
  h2 {
    text-align: center;
  }
  .blank-body {
    flex-grow: 1;
    flex-shrink: 1;
    display: flex;
    flex-direction: column;
    justify-content: center;
    .blank-item {
      height: 72px;
      font-size: 24px;
      display: flex;
      span {
        margin-right: 12px;
        width: 60px;
      }
      input {
        height: 36px;
        flex-grow: 1;
        flex-shrink: 1;
        font-size: 16px;
        // width: 100%;
      }
    }
  }
  .footer {
    // position: absolute;
    display: flex;
    justify-content: flex-end;
    width: 100%;
    button {
      background-color: #1677FF;
      color: #ffffff;
    }
    & > button:nth-child(1) {
      margin-right: 12px;
      background-color: #ffffff;
      border: 1px solid #1677FF;
      color: #1677FF;
    }
  }
}
li {
  list-style: none;
  display: flex;
  justify-content: flex-start;
  align-items: center;
  // margin-bottom: 24px;
  height: 64px;
  border-bottom: 1px solid rgba(0, 0, 0, 0.4);
  &:nth-child(1) {
    color: aliceblue;
    background-color: rgba(42, 46, 54, 0.48);
  }
  .No {
    width: 18%;
  }
  .study-code {
    width: 18%;
  }
  .name {
    width: 18%;
  }
  .year {
    width: 18%;
  }
  .operation {  
    flex-grow: 1;  
    display: flex;  
    justify-content: space-around;  
}  
  
.operation > button {  
    background-color: #314659;  
    color: #ffffff;  
    width: 90px;  
    white-space: nowrap; /* 阻止文本换行 */
  }
}
</style>
