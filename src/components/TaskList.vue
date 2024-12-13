<script setup>
import { reactive, computed, onMounted } from 'vue';


const state = reactive({
  tasks: [],
  newTask: '',
  showForm: false, 
});

// Add a new task
const addTask = () => {
  if (state.newTask.trim().length >= 3) {
    state.tasks.push({
      id: Date.now(),
      name: state.newTask,
      completed: false,
    });
    state.newTask = '';
    saveTasks();
  } else {
    alert('Task name must be at least 3 characters long');
  }
};

// Toggle task completion
const toggleComplete = (task) => {
  task.completed = !task.completed;
  saveTasks();
};

// Delete a task
const deleteTask = (id) => {
  state.tasks = state.tasks.filter((task) => task.id !== id);
  saveTasks();
};

// Save tasks to localStorage
const saveTasks = () => {
  localStorage.setItem('tasks', JSON.stringify(state.tasks));
};


const loadTasks = () => {
  const storedTasks = localStorage.getItem('tasks');
  if (storedTasks) {
    state.tasks = JSON.parse(storedTasks);
  }
};


onMounted(() => {
  loadTasks();
});

// Computed property to filter completed tasks
const completedTasks = computed(() => {
  return state.tasks.filter((task) => task.completed);
});
</script>

<template>
  <div>
    <!-- Step 8: Toggle Form -->
    <button @click="state.showForm = !state.showForm" class="form-toggle-btn">
      {{ state.showForm ? 'HIDE ADD TASK FORM' : 'SHOW ADD TASK FORM' }}
    </button>

    <!-- Step 8: Conditional Form Display -->
    <div v-if="state.showForm" class="task-form">
      <input 
        v-model="state.newTask" 
        placeholder="Enter a task name" 
        class="task-input"
      />
      <button @click="addTask" class="add-btn">Add Task</button>
    </div>


    <p v-if="state.tasks.length === 0" class="no-tasks">No tasks available</p>
    <div v-else class="task-list">
      <!-- Step 6: Task List Rendering -->
      <div
        v-for="task in state.tasks"
        :key="task.id"
        :style="{
          backgroundColor: task.completed ? 'lightgreen' : '#333',
          border: task.completed ? '2px solid red' : '1px solid #555',
        }"
        class="task-item"
      >
        <span 
          @click="toggleComplete(task)" 
          :class="{ completed: task.completed }"
        >
          {{ task.name }}
        </span>
        <button @click="toggleComplete(task)" class="complete-btn">
          {{ task.completed ? 'Undo' : 'Complete' }}
        </button>
        <button @click="deleteTask(task.id)" class="delete-btn">Delete</button>
      </div>
    </div>

   
  </div>
</template>

<style scoped>
/* Main Wrapper */
.task-list {
  margin-top: 20px;
}

/* Task Items */
.task-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px;
  margin: 5px 0;
  border-radius: 4px;
  transition: all 0.3s ease;
  color: #e0e0e0;
}

/* Completed Task Style */
.task-item span {
  flex-grow: 1;
  text-align: left;
  cursor: pointer;
}
.task-item span.completed {
  text-decoration: line-through;
  font-weight: bold;
  color: #444;
}

/* Buttons */
.form-toggle-btn,
.add-btn,
.complete-btn,
.delete-btn {
  padding: 10px 15px;
  border: none;
  border-radius: 4px;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.form-toggle-btn {
  margin: 10px 0;
  background-color: #6c757d;
  color: white;
}

.task-input {
  padding: 10px;
  margin-right: 10px;
  border: 1px solid #555;
  border-radius: 4px;
  background-color: #222;
  color: white;
}

.add-btn {
  background-color: #28a745; 
  color: white;
}

.add-btn:hover {
  background-color: #218838;
}

.complete-btn {
  background-color: #6f42c1; 
  color: white;
}

.complete-btn:hover {
  background-color: #59369c;
}

.delete-btn {
  background-color: #dc3545; 
  color: white;
}

.delete-btn:hover {
  background-color: #c82333;
}


.no-tasks {
  text-align: center;
  color: #aaa;
  margin: 20px 0;
}

/* Completed Section */
.completed-section {
  margin-top: 20px;
}
.completed-section h3 {
  color: #28a745;
}
</style>
