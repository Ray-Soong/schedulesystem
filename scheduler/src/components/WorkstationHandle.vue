<template>
  <div class="station-mgr">
    <h3>工位管理</h3>
    <div class="buttons">
      <buttonm @click="showAddModal = true">添加工位</buttonm>
      <buttonm @click="showDeleteModal = true">删除工位</buttonm>
    </div>

    <!-- 添加工位弹框 -->
    <div v-if="showAddModal" class="modal">
      <div class="modal-content">
        <h3>输入工位名称</h3>
        <input
          v-model="workstationName"
          :placeholder="`默认: 工位-${workstations.length + 1}`"
        />
        <div class="modal-actions">
          <button @click="confirmAddWorkstation">确认</button>
          <button @click="closeAddModal">取消</button>
        </div>
      </div>
    </div>

    <!-- 删除工位弹框 -->
    <div v-if="showDeleteModal" class="modal">
      <div class="modal-content">
        <h3>输入要删除的工位名称</h3>
        <input
          v-model="deleteWorkstationName"
          :placeholder="`默认: 工位-${workstations.length}`"
        />
        <div class="modal-actions">
          <button @click="confirmDeleteWorkstation">确认</button>
          <button @click="closeDeleteModal">取消</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "WorkstationHandle",
  data() {
    return {
      workstations: [
        { id: 1, name: "工位-1", tasks: [] },
        { id: 2, name: "工位-2", tasks: [] },
        { id: 3, name: "工位-3", tasks: [] },
        { id: 4, name: "工位-4", tasks: [] },
        { id: 5, name: "工位-5", tasks: [] },
        { id: 6, name: "工位-6", tasks: [] },
        { id: 7, name: "工位-7", tasks: [] },
        { id: 8, name: "工位-8", tasks: [] },
      ],
      showAddModal: false,
      showDeleteModal: false,
      workstationName: "",
      deleteWorkstationName: "",
    };
  },
  mounted() {
    // 页面加载时，将默认工位数据传递给父组件
    this.$emit("update-workstations", [...this.workstations]);
  },
  methods: {
    closeAddModal() {
      this.showAddModal = false;
      this.workstationName = "";
    },
    confirmAddWorkstation() {
      const name =
        this.workstationName.trim() || `工位-${this.workstations.length + 1}`;
      const newWorkstation = {
        id: this.workstations.length + 1,
        name,
        tasks: [],
      };
      this.workstations.push(newWorkstation);
      this.$emit("update-workstations", [...this.workstations]); // 向父组件同步数据
      this.closeAddModal();
    },
    closeDeleteModal() {
      this.showDeleteModal = false;
      this.deleteWorkstationName = "";
    },
    confirmDeleteWorkstation() {
      const name =
        this.deleteWorkstationName.trim() || `工位-${this.workstations.length}`;
      this.workstations = this.workstations.filter(
        (workstation) => workstation.name !== name
      );
      this.$emit("update-workstations", [...this.workstations]); // 向父组件同步数据
      this.closeDeleteModal();
    },
  },
};
</script>

<style scoped>
.station-mgr {
  margin-top: 20px;
  border: 1px solid #ccc;
  padding: 10px;
  display: flex;
  flex-direction: column;
}
.buttons {
  display: flex;
  justify-content: space-between;
  margin-bottom: 5px;
  width: 100%;
}
buttonm {
  padding: 10px 50px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
button {
  padding: 10px 20px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
button:hover {
  background-color: #0056b3;
}
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}
.modal-content {
  background: white;
  padding: 20px;
  border-radius: 5px;
}
.modal-actions {
  display: flex;
  justify-content: space-between;
}
</style>
