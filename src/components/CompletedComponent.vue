<template>
  <div>
    <h1 class="box-title">Completed</h1>
    <ul>
      <draggable class="list-group"  group="tasks">
        <draggable class="list-group" :list="completed" group="tasks">
      <li class="task" v-for="(task,row) in completed" v-bind:key="row" v-bind:style="{backgroundColor:task.color}"
          @click.self="ModalOpen(task)">
        <p>{{ task.title }}</p>
        <span @click="DeleteTask(task)">
          <font-awesome-icon icon="trash-alt"/>
        </span>
      </li>
      </draggable>
      </draggable>
      <li id="drop-zone" @click="changeTextInput" v-if="isHidden">
        + Add New Card
      </li>
      <li class="drop-zone-input" v-if="!isHidden">
        <input type="text" v-model="taskAdd" style="border: none;width: 100%" autofocus>
        <button @click="SaveTask" class="btn btn-primary-outline">Add Card</button>
        <button @click="isHidden = !isHidden " class="btn btn-primary-outline">Cancel</button>
      </li>

    </ul>
    <modal name="completed-modal">

      <div class="modal-body">
        <p style="font-weight: bold;text-align: left;">{{ activeTask.title }}</p>
        <slot name="body">
          <label style="float: left">Description</label>
          <textarea class="textarea" v-model="activeTask.description"
                    placeholder="Enter the description for the card"></textarea>
          <label style="float: left">Colors</label>
          <div v-for="item in colors" :key="item.id" style="float: left;
          padding-left: 10px;">
            <div class="color-picker" v-bind:style="{backgroundColor:item.code}" v-on:click="setColor(item)">
              <span v-if="item.selected" class="checkmark">&#10004;</span>
            </div>
          </div>
        </slot>
      </div>

      <div class="modal-footer">
        <button class="btn btn-primary" @click="submitDescription()">
          Submit
        </button>
      </div>
    </modal>
  </div>
</template>
<script>
import draggable from "vuedraggable";
export default {
  name: 'CompletedComponent',
  components: {
    draggable,
  },
  watch: {
    completed: {
      handler() {
        console.log('Tasks completed changed')
        localStorage.setItem('completed', JSON.stringify(this.completed));
      },
      deep: true,
    },
  },
  mounted() {
    if (localStorage.getItem('completed')) this.completed = JSON.parse(localStorage.getItem('completed'));
  },
  data() {
    return {
      isHidden: true,
      taskAdd: '',
      completed: [],
      changedDescription: '',
      activeTask: {},
      selectedColor: {},
      colors: [
        {id: 1, code: "#DCDCDC", selected: true},
        {id: 2, code: "#D3D3D3", selected: false},
        {id: 3, code: "#FFC0CB", selected: false},
        {id: 4, code: "#D8BFD8", selected: false},
        {id: 5, code: "#FFFACD", selected: false},
        {id: 6, code: "#FFEFD5", selected: false},
      ]
    }
  },
  created() {

    //this.tasks.push(localStorage.getItem('storedDataTask'))
    //console.log(localStorage.getItem('storedDataTask'))
  },
  methods: {
    changeTextInput: function () {
      //item.active = !item.active;
      this.isHidden = false
    },
    SaveTask() {
      let inputObject = {title: this.taskAdd, color: '#DCDCDC', description: ''}
      this.completed.push(inputObject)
      this.isHidden = !this.isHidden
      this.taskAdd = ''
    },
    setColor(color) {
      this.colors.map((item) => {
        if (item.id == color.id) {
          item.selected = true;
        } else {
          item.selected = false;
        }
      })
      this.activeTask.color = color.code;
    },
    ModalOpen(task) {
      this.$modal.show('completed-modal')
      this.activeTask = task
    },
    submitDescription() {
      this.$modal.hide('completed-modal')
      console.log(this.activeTask)
    },
     DeleteTask(task) {
      this.completed.splice(this.completed.indexOf(task), 1);
    }

  }
}
</script>
<style scoped>

#drop-zone {
  width: 100%;
  height: 150px;
  list-style-type: none;
  border: 2px dashed rgba(0, 0, 0, .3);
  border-radius: 20px;
  font-family: Arial;
  text-align: center;
  position: relative;
  line-height: 130px;
  font-size: 0.8rem;
  font-weight: bold;
  color: #000000;
}

.drop-zone-input {
  width: 100%;
  height: 150px;
  list-style-type: none;
  border: 2px dashed rgba(0, 0, 0, .3);
  border-radius: 20px;
  font-family: Arial;
  text-align: center;
  position: relative;
  line-height: 75px;
  font-size: 0.8rem;
  font-weight: bold;
  color: #000000;
}

*:focus {
  outline: none;
}

.textarea {
  display: block;
  width: 100%;
  padding: .375rem .75rem;
  font-size: 1rem;
  line-height: 1.5;
  color: #495057;
  background-color: #fff;
  background-clip: padding-box;
  border: 1px solid #ced4da;
  border-radius: .25rem;
}

.color-picker {
  height: 50px;
  width: 50px;
  margin-top: 27px;
  border-radius: 10px;
}

.checkmark {
  font-size: 40px;
  color: #ffff;
}

.modal-container {
  width: 300px;
  margin: 0px auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
  font-family: Helvetica, Arial, sans-serif;
}

.modal-header h3 {
  margin-top: 0;
  color: #42b983;
}

.modal-body {
  margin: 20px 0;
}

.modal-default-button {
  float: right;
  margin-right: 2px;
}

</style>
