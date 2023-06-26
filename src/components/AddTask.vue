<template>
  <form @submit="onSubmit" class="add-form">
    <div class="form-control">
      <label>ID</label>
      <input type="number" v-model="Id" name="Id" placeholder="Add Id" />
    </div>
    <div class="form-control">
      <label>Title</label>
      <input type="text" v-model="Title" name="Title" placeholder="Add Title" />
    </div>
    <div class="form-control">
      <label>Description</label>
      <input type="text" v-model="Description" name="Description" placeholder="Add Description" />
    </div>
    <div class="form-control form-control-check">
      <label>Set Reminder</label>
      <input type="checkbox" v-model="IsCompleted" name="IsCompleted" />
    </div>

    <input type="submit" value="Save Task" class="btn btn-block" />
  </form>
</template>

<script>
export default {
  name: 'AddTask',
  data() {
    return {
      Id: '',
      Title: '',
      Description:'',
      IsCompleted: false,
    }
  },
  methods: {
    onSubmit(e) {
      e.preventDefault()

      if (!this.Title) {
        alert('Please add a title')
        return
      }

      const newTask = {
        // id: Math.floor(Math.random() * 100000),
        Id: this.Id,
        Title: this.Title,
        Description: this.Description,
        IsCompleted: this.IsCompleted,
      }

      this.$emit('add-task', newTask)
      this.Id= Math.floor(Math.random() * 100000);
      this.Title = ''
      this.Description = ''
      this.IsCompleted = false
    },
  },
}
</script>

<style scoped>
.add-form {
  margin-bottom: 40px;
}

.form-control {
  margin: 20px 0;
}

.form-control label {
  display: block;
}

.form-control input {
  width: 100%;
  height: 40px;
  margin: 5px;
  padding: 3px 7px;
  font-size: 17px;
}

.form-control-check {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.form-control-check label {
  flex: 1;
}

.form-control-check input {
  flex: 2;
  height: 20px;
}
</style>
