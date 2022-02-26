<template>
  <div>
    <DxPopup
      :visible="updateTodoPopupVisibility"
      :drag-enabled="false"
      :close-on-outside-click="true"
      title="Todo Güncelle"
      :width="700"
      height="auto"
      @hiding="updateTodoPopupVisibility = false"
    >
      <div>
        <div class="fieldset">
          <div class="fieldset-header">
            Bilgiler
          </div>
          <div class="field">
            <DxTextBox
              v-model="selectedTodo.title"
              label="İçerik"
              label-mode="floating"
            />
          </div>
          <div class="field">
            <DxButton
              style="float: right;"
              type="success"
              text="Güncelle"
              @click="updateTodo"
            />
          </div>
        </div>
      </div>
    </DxPopup>
    <div v-if="todoLength > 0">
      <v-card
        v-for="(todo, index) in todos"
        :key="todo.id"
        style="margin-bottom: 10pt;"
        :color="todo.completed ? 'green accent-2' : 'red accent-2'"
      >
        <v-card-title>
          Todo #{{ index + 1 }}
        </v-card-title>
        <v-card-text>
          {{ todo.title }}
        </v-card-text>
        <v-card-actions>
          <v-btn
            outlined
            @click="deleteTodoRequest(todo.id)"
          >
            Sil
            <v-icon
              right
            >
              mdi-delete-circle-outline
            </v-icon>
          </v-btn>
          <v-btn
            outlined
            @click="updateTodoRequest(todo)"
          >
            Güncelle
            <v-icon
              right
            >
              mdi-pencil-circle-outline
            </v-icon>
          </v-btn>
          <v-btn
            outlined
            @click="changeTodoStatus(todo)"
          >
            <span v-if="todo.completed">
              Yeniden Aç
              <v-icon
                right
              >
                mdi-rotate-3d-variant
              </v-icon>
            </span>
            <span v-else>
              Tamamla
              <v-icon
                right
              >
                mdi-check-circle-outline
              </v-icon>
            </span>
          </v-btn>
        </v-card-actions>
      </v-card>
    </div>
  </div>
</template>

<script>
import { DxPopup } from 'devextreme-vue/popup'
import { DxTextBox } from 'devextreme-vue/text-box'
import { DxButton } from 'devextreme-vue/button'

export default {
  components: {
    DxPopup,
    DxTextBox,
    DxButton
  },
  data () {
    return {
      todos: [],
      selectedTodo: {},
      updateTodoPopupVisibility: false
    }
  },
  computed: {
    todoLength () {
      return this.todos.length
    }
  },
  created () {
    const userId = this.$route.query.userId
    this.$axios.get(`user/${userId}/todos`)
      .then((response) => {
        this.todos = response.data
      })
  },
  methods: {
    deleteTodoRequest (todoId) {
      this.$axios.delete(`todos/${todoId}`).then((response) => {
        this.todos = this.todos.filter(t => t.id !== todoId)
      })
    },
    updateTodoRequest (todo) {
      this.selectedTodo = todo
      this.updateTodoPopupVisibility = true
    },
    updateTodo () {
      this.updateTodoPopupVisibility = false
    },
    changeTodoStatus (todo) {
      this.$axios.put(`todos/${todo.id}`, {
        userId: todo.userId,
        id: todo.id,
        title: todo.title,
        completed: !todo.completed
      }).then((response) => {
        todo.completed = !todo.completed
      })
    }
  }
}
</script>

<style scoped>
.fieldset {
  display: flex;
  flex-direction: column;
}

.fieldset-header {
  font-weight: 500;
  line-height: 24px;
  font-size: 18px;
  margin-bottom: 20px;
}

.field {
  margin-bottom: 10px;
}
</style>
