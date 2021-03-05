<template>
  <div class="new-note">
    <label> Title</label>
    <div class="select-wrap" >
      <div class="input-note-wrap">
        <input type="text" v-model="note.title">
      </div>
      <select name="select" id="select" v-model="note.status"  >
        <option v-for="option in options" :value="option.value" >
          {{ option.value }}
        </option>
      </select>
    </div>

    <label> Description</label>
    <textarea v-model="note.descr"></textarea>
    <button class="btn btnPrimary" @click="addNote">New note</button>
  </div>
</template>

<script>
    export default {
        name: 'NewNote',
        data() {
            return {selected: 'standard'}
        },

        props: {
            options: {
                type: Array,
                required: true
            },
            note: {
                type: Object,
                required: true
            }
        },
        methods: {
            addNote() {
                this.$emit('addNote', this.note);
            },

        }
    }
</script>

<style lang="scss" scoped>
  .select-wrap {
    display: flex;
  }

  #select {
    height: 55px;
  }

  .input-note-wrap {
    display: flex;
    width: 100%;

    input {
      margin-right: 20px;
    }

    .note-select-wrap {
      select {
        height: 55px;
        -webkit-border-radius: 20px;
        -moz-border-radius: 20px;
        border-radius: 20px;
        padding: 0 10px;
      }
    }
  }
  .new-note {
    text-align: center;
  }

  .btn {
    margin: 10px 0;
  }
</style>