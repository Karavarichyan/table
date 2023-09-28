<template>
  <div class="container mt-4">
    <div class="card">
      <div class="card-body">
        <div v-for="(item, index) in form" :key="index">
          <h3>Item {{ index + 1 }}</h3>
          <div class="form-row">
            <div class="form-group col-md-4">
              <label for="name">Name</label>
              <input type="text" class="form-control" v-model="item.name" id="name">
            </div>
            <div class="form-group col-md-4">
              <label for="surname">Surname</label>
              <input type="text" class="form-control" v-model="item.surname" id="surname">
            </div>
            <div class="form-group col-md-3">
              <label for="qualification">Qualification</label>
              <input type="text" class="form-control" v-model="item.qualification" id="qualification">
            </div>
            <div class="form-group col-md-1">
              <div class="add-remove-buttons">
                <button type="button" class="btn btn-danger btn-sm" @click="removeRow(index)">x</button>&nbsp;
                <button type="button" class="btn btn-success btn-sm" @click="addRow">+</button>
              </div>
            </div>
          </div>
        </div>
        <button type="button" class="btn btn-success mt-3" @click="saveItem">Save</button>
      </div>
    </div>
  </div>

  <div class="container mt-4">
    <h2>List of Previously Entered Forms:</h2>
    <table class="table table-bordered">
      <thead>
        <tr>
          <th>Name</th>
          <th>Surname</th>
          <th>Qualification</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in savedForms" :key="index">
          <td>{{ item.name }}</td>
          <td>{{ item.surname }}</td>
          <td>{{ item.qualification }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';

export default {
  name: 'dynamic-form',
  setup() {
    const form = ref([
      { name: '', surname: '', qualification: '' }
    ]);

    const savedForms = ref([]);

    onMounted(() => {
      const savedData = localStorage.getItem('formDataList');
      if (savedData) {
        savedForms.value = JSON.parse(savedData);
      }
    });

    const addRow = () => {
      form.value.push({ name: '', surname: '', qualification: '' });
    }

    const removeRow = (index) => {
      if (form.value.length > 1) {
        form.value.splice(index, 1);
      }
    }

    const saveItem = () => {
      savedForms.value.push(...form.value);
      localStorage.setItem('formDataList', JSON.stringify(savedForms.value));
      form.value = [{ name: '', surname: '', qualification: '' }];
    }

    return {
      form,
      savedForms,
      addRow,
      removeRow,
      saveItem
    }
  }
}
</script>

<style scoped>
.container {
  margin-top: 20px;
}

.card {
  max-width: 800px;
  margin: 0 auto;
  border: 1px solid #ccc;
  border-radius: 5px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  background-color: #fff;
  padding: 20px;
}

.card-title {
  font-size: 24px;
  margin-bottom: 20px;
  color: #333;
}

.form-control {
  width: 100%;
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 4px;
  margin-bottom: 10px;
  background-color: #f9f9f9;
  color: #333;
}

.btn {
  margin-top: 10px;
  font-size: 16px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.btn:hover {
  background-color: #0056b3;
}

.table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

.table th {
  background-color: #f0f0f0;
  border: 1px solid #ddd;
  padding: 8px;
  text-align: center;
  color: #333;
}

.table td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: center;
  color: #333;
}


.add-remove-buttons {
  display: flex;
  justify-content: center;
  align-items: center;
}

.add-remove-buttons button {
  margin: 0 5px;
}
/* vvvv */
.error-message {
  color: red;
  font-size: 14px;
  margin-top: 5px;
}


@media (max-width: 767px) {
  .form-group {
    margin-bottom: 10px;
  }

  .add-remove-buttons {
    justify-content: space-between;
  }
}
</style>
