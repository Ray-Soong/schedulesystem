<template>
  <div>
    <button @click="showAddWorkstationModal">添加工位</button>

    <!-- 弹框输入工位名称 -->
    <div v-if="showModal" class="modal">
      <div class="modal-content">
        <h3>输入工位名称</h3>
        <input v-model="workstationName" placeholder="工位名称" />
        <button @click="confirmAddWorkstation">确认</button>
        <button @click="closeModal">取消</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "AddWorkstationButton",
  data() {
    return {
      showModal: false,
      workstationName: "",
    };
  },
  methods: {
    showAddWorkstationModal() {
      this.showModal = true;
    },
    closeModal() {
      this.showModal = false;
      this.workstationName = "";
    },
    confirmAddWorkstation() {
      if (this.workstationName.trim()) {
        this.$emit("add-workstation", this.workstationName);
        this.closeModal();
      }
    },
  },
};
</script>

<style>
/* 简单的弹框样式 */
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
</style>
