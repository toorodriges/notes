<template>
  <div class="notes"   >
    <div class="note"
         :class="[{ full: !grid }, note.status]"
         v-model="note.status"
         v-for="(note, index) in notes"
         :key="index">
      <div class="note-header" :class="{ full: !grid }">
        <p
            v-if="!changed"
            @click="changeNoteTitle(note.title)">{{note.title}}</p>
          <input type="text" :placeholder="note.title" v-if="changed" class="change-note-title">
        <p style="cursor: pointer; margin: 0 12px" @click="removeNote(index)">x</p>
      </div>
      <div class="note-body">
        <p>{{note.descr}}</p>
        <span>{{note.date}}</span>
      </div>
    </div>
  </div>
</template>

<script>
    export default {
        name: 'Notes',
        props: {
            notes: {
                type: Array,
                required: true,
            },
            grid: {
                type: Boolean,
                required: true
            },


        },

        data () {
            return {
                changed: false

            }
        },
        methods: {
            removeNote(index) {
                console.log(`note id - ${index} removed`);
                this.$emit('remove', index)
            },

            changeNoteTitle (val) {
                this.changed = true;
                console.log('--$event-', val);
            }
        }
    }
</script>

<style lang="scss" >
  .notes {
    display: flex;
    align-items: center;
    justify-content: space-between;
    flex-wrap: wrap;
    padding: 40px 0;
  }

  .note {
    width: 48%;
    padding: 18px 20px;
    margin-bottom: 20px;
    background-color: #fff;
    transition: all .25s cubic-bezier(.02,.01,.47,1);
    box-shadow: 0 30px 30px rgba(0,0,0,.02);
    border: 2px solid white;
    &:hover {
      box-shadow: 0 30px 30px rgba(0,0,0,.04);
      transform: translate(0, -6px);
      transition-delay: 0s!important;
    }

    &.full {
      width: 100%;
      text-align: center;
    }
    &.attention {
      border: 2px solid  rgba(255, 235, 59,.8);
    }

    &.warning {
      border: 2px solid  tomato;
    }

      .change-note-title {
          color: blue;
          font-size: 22px;
          margin: 0;
          padding: 0;
          border: 0;
      }
  }

  .note-header {
    display: flex;
    align-items: center;
    justify-content: space-between;

    &.full {
      justify-content: center;
    }

    h1 {
      font-size: 22px;
    }

    p {
      color: blue;
      font-size: 22px;
    }

    svg {
      margin-right: 12px;
      color: #999;
      cursor: pointer;

      &.active {
        color: blue;
      }

      &:last-child {
        margin-right: 0;
      }

    }
  }
  .note-body {
   p {
     margin: 20px 0;
   }
    span {
      font-size: 14px;
      color: #999;
    }
  }
</style>