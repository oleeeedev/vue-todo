<template>
  <div class="d-flex">
    <div class="nav flex-column nav-pills me-3 m-1 align-self-center" id="v-pills-tab" role="tablist" aria-orientation="vertical">
      <button class="nav-link active" id="v-pills-home-tab" data-bs-toggle="pill" data-bs-target="#v-pills-home"
              type="button" role="tab" aria-controls="v-pills-home" aria-selected="true">Home</button>
      <button class="nav-link" id="v-pills-todos-tab" data-bs-toggle="pill" data-bs-target="#v-pills-todos"
              type="button" role="tab" aria-controls="v-pills-todos" aria-selected="false">Todos</button>
      <button class="nav-link" id="v-pills-disabled-tab" data-bs-toggle="pill" data-bs-target="#v-pills-disabled"
              type="button" role="tab" aria-controls="v-pills-disabled" aria-selected="false" disabled>Notes</button>
      <button class="nav-link" id="v-pills-messages-tab" data-bs-toggle="pill" data-bs-target="#v-pills-messages"
              type="button" role="tab" aria-controls="v-pills-messages" aria-selected="false" disabled>Messages</button>
      <button class="nav-link" id="v-pills-settings-tab" data-bs-toggle="pill" data-bs-target="#v-pills-settings"
              type="button" role="tab" aria-controls="v-pills-settings" aria-selected="false" disabled>Settings</button>
    </div>
    <div class="tab-content align-items-start bg-light" id="v-pills-tabContent">
      <div class="tab-pane fade show active" id="v-pills-home" role="tabpanel" aria-labelledby="v-pills-home-tab">
        <slot name="home"></slot>
      </div>
      <div class="tab-pane fade" id="v-pills-todos" role="tabpanel" aria-labelledby="v-pills-todos-tab">
        <slot name="todos"></slot>
      </div>
      <div class="tab-pane fade" id="v-pills-messages" role="tabpanel" aria-labelledby="v-pills-messages-tab">
        <slot name="notes"></slot>
      </div>
      <div class="tab-pane fade" id="v-pills-settings" role="tabpanel" aria-labelledby="v-pills-settings-tab">
        <slot name="settings"></slot>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    tabs: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      activeTab: 'notes'
    };
  },
  methods: {
    changeTab(tab) {
      this.activeTab = tab;

      this.$emit('changeTab', tab);
    }
  }
};
</script>

<style>
.navbar {
  display: flex;
  justify-content: center;
  background-color: #f2f2f2;
  padding: 10px;
}

.navbar-items {
  display: flex;
  list-style: none;
  padding: 0;
}

.navbar-items li {
  margin: 0 10px;
  cursor: pointer;
  transition: color 0.3s;
}

.navbar-items li.active {
  color: blue;
}

.navbar-items li:hover {
  color: blue;
}</style>