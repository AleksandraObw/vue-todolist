<script>

export default {
data() {
  return {
    taskText: '',
    tasks: [],
  }
},

created() {
    this.tasks = this.tasksFromLocalStorage;
  },
computed: {
    tasksFromLocalStorage() {
      return JSON.parse(localStorage.getItem('tasks') || '[]');
    },
  },
watch: {
    tasks: {
      handler() {
        localStorage.setItem('tasks', JSON.stringify(this.tasks));
      },
      deep: true,
    },
},

methods: {
  addTask() {
    this.tasks.push({
      name: this.taskText.trim(),
      status: 'active'
    });
    this.taskText = '';
  },
  deleteTask(index) {
    this.tasks.splice(index, 1);
  },
  editStatus(index) {
    if (this.tasks[index].status === 'completed') {
      this.tasks[index].status = 'active'
    } else {
      this.tasks[index].status = 'completed'
    }
  },
  deleteCompleted() {
    this.tasks = this.tasks.filter(task => task.status === 'active');
  }
},

}

</script>

<template>
  <div class="body__component component">

    <div class="component__heading heading">
      <h2 class="heading__text heading-text">To Do List</h2>
      <img 
        class="heading__logo logo" 
        src="https://img.icons8.com/nolan/96/list.png" 
        alt="To do list logo"/>
    </div>
    
    <div class="component__input input">
      <input 
        class="input__text-block text-block" 
        v-model="taskText" 
        type="text" 
        placeholder="Type task text here..."
        maxlength="150">
      <button 
        class="input__button input-button button" 
        @click="addTask()" 
        :disabled="!this.taskText.length">Add task
      </button>
    </div>

    <div class="component__todolist todolist">
      <p 
        v-if="!tasks.length" 
        class="component__empty empty">To do list is empty!</p>
      <ul 
        v-else 
        class="component__list list" 
        v-for="(task, index) in tasks" 
        :key="index">
        <li class="list__item item">
          <div class="item__left-part left-part">
              <div class="item__status-button status-button" @click="editStatus(index)">
                <i v-if="task.status === 'completed'" class="far fa-check-circle"></i>
                <i v-else class="far fa-circle"></i>
              </div>
              <p 
                class="item__text item-text"
                :class="{'finished' : task.status === 'completed'}"
                @click="editStatus(index)">{{ task.name }}
              </p>
          </div>
          <button 
              class="buttons__delete button" 
              @click="deleteTask(index)"><i class="fas fa-trash-alt"></i>
          </button>
        </li>
      </ul>
    </div>
    

    <div class="component__footer footer">
      <button class="footer__button button" @click="deleteCompleted()">Delete completed</button>
    </div>

  </div>
</template>

<style scoped>
.body__component {
    width: 50%;
    height: 70%;
}
.component {
    display: flex;
    flex-direction: column;
    justify-content: space-evenly;
    row-gap: 1em;
    margin: 2em auto;
  }
  .heading {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
    padding: .5em;
    border-bottom: 1px solid #BCABAE;
    width: 100%;
  }
  .heading-text {
    color: #2d2e2e;
    font-size: 2em;
    font-weight: 700;
  }
  .logo {
    width: 48px;
    height: 48px;
  }
  .input {
    width: 100%;
    padding: .5em;
    border-bottom: 1px solid #BCABAE;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-between;
  }
  .text-block {
    display: block;
    padding: .5em;
    border: 1px solid #BCABAE;
    width: 80%;
    height: 40px;
    font-size: 1.25em;
  }
  .input-button {
    width: 20%;
    height: 40px;
    font-weight: 700;
  }
  .footer__button {
    font-weight: 700;
    height: 40px;
  }
  .button {
    padding: .5em;
    border: 1px solid #BCABAE;
    background-color: transparent;
  }
  .button:hover {
    cursor: pointer;
  }
  .empty {
    font-size: 1.25em;
    font-weight: 700;
    padding: .5em;
    text-align: center;
  }
  .list {
    width: 100%;
    padding: .5em;
  }  
  .left-part {
    display: flex;
    flex-direction: row;
    column-gap: 1em;
    width: 80%;
  }
  .status-button {
  background-color: transparent;
  border: none;
  }
  .item {
    list-style-type: none;
    width: 100%;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-between;
    column-gap: 1em;
  }
  .item-text {
    text-overflow: ellipsis;
    overflow: hidden;
    font-size: 1.25em;
  }
  .footer {
    width: 100%;
    padding: .5em;
    border-top: 1px solid #BCABAE;
    display: flex;
    flex-direction: row;
    column-gap: 1em;
  }
  label {
    border: 1px solid #FBFBFB;
    padding: .25em;
  }
  input[type="radio"] {
    display: none;
  }
  input[type="radio"]:checked + label {
    border: 1px solid #BCABAE;
    padding: .25em;
  }
  .finished {
    text-decoration: line-through;
  }

@media (max-width: 1200px) {
  .body__component {
    width: 70%;
  }
}

@media (max-width: 800px) {
  .body__component {
    width: 90%;
  }
  .footer__button,
  .input__button,
  .text-block {
    width: 100%;
  }
  .input {
    flex-direction: column;
    row-gap: .5em;
  }
  .text-block {
    height: 40px;
  }
}

</style>
