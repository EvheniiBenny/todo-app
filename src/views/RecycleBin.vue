<template>
  <div class="RecycleBin">
    <div class="title is-4 has-text-centered">
      List of Deleted Items
    </div>
    <div class="RecycleBin__content">
      <div
        v-if="deletedItems.length"
        class="RecycleBin__items"
      >
        <recycled-todo-item
          v-for="item in deletedItems"
          :key="item.id"
          :item="item"
          @remove="removeFromBin"
          @restore="restore"
        />
      </div>
      <div
        v-else
        class="has-text-centered"
      >
        No deleted todo items
      </div>
    </div>
  </div>
</template>

<script>
import { ToDoService } from '@/services/ToDoService'
import RecycledTodoItem from '@/components/RecycledTodoItem'

export default {
  name: 'RecycleBin',
  components: { RecycledTodoItem },
  data () {
    return {
      deletedItems: []
    }
  },
  mounted () {
    this.fetchDeletedItems()
  },
  methods: {

    fetchDeletedItems () {
      this.deletedItems = ToDoService.fetchDeleted()
    },

    restore (item) {
      item.deletedAt = null
      ToDoService.updateItem(item.id, item)
      this.fetchDeletedItems()
    },
    removeFromBin (item) {
      ToDoService.removeItem(item.id)
      this.fetchDeletedItems()
    }
  }
}
</script>
