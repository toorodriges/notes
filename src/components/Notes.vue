<template>
  <div class="notes"   >
    <div class="note"
         :class="[{ full: !grid }, note.status]"
         v-model="note.status"
         v-for="(note, index) in notes"
         :key="index">
      <div class="note-header" :class="{ full: !grid }">
        <p
            v-if="!note.changed"
            @click="changedStatus(note, index)"
            ref="noteTitle">
            {{note.title}}
        </p>



          <input type="text" v-if="note.changed" class="change-note-title"
                 ref="changeNote"
                 @focus="updateNotes($event, index)"
                 @blur="handleBlur($event, index)"
                 @keyup="setNewTitle($event, index)"
                 autofocus/>
          <span class="error">title cannot be empty</span>
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
                changedNotes: '',
                tempNoteTitle: ''
            }
        },

        methods: {
            removeNote(index) {
                this.$emit('remove', index)
            },

            changedStatus (note, index) {

                let targetIndex = index;
                let title = '';
                this.tempNoteTitle = note.title

                this.notes.map( function (note, index, notes) {
                    if (index === targetIndex) {
                        note.changed = true;
                        title = note.title
                        note.date = new Date(Date.now()).toLocaleString()
                        notes.splice(targetIndex, 1, note)
                    }
                })
                this.tempNoteTitle = title;
            },

            updateNotes(targetIndex, value) {
                if (value !== '') {
                    this.$nextTick(() => {
                        this.notes.map( function (note, index, notes) {
                            if (index === targetIndex) {
                                note.changed = false;
                                note.title = value
                                note.date = new Date(Date.now()).toLocaleString()
                                notes.splice(targetIndex, 1, note)
                            }
                        })
                    });
                }

                if (value === '') event.target.nextElementSibling.style.display = 'block'

            },

            setNewTitle(event,index) {

                let title;
                let targetIndex = index;

                event.target.value === '' ? event.target.nextElementSibling.style.display = 'block' : event.target.nextElementSibling.style.display = 'none'

                if(event.key === "Enter") {
                    if (this.tempNoteTitle.length || event.target.value !== '') {
                        title = event.target.value;
                        this.updateNotes(targetIndex, title)
                        this.tempNoteTitle = '';
                    }
                }

                if (event.key === 'Escape') {
                    title = this.tempNoteTitle
                    this.updateNotes(targetIndex, title)
                    this.tempNoteTitle = '';
                }
            },

            handleBlur(event,index) {
                if(event.target.value !== '') {
                    this.tempNoteTitle =  event.target.value;
                    let targetIndex = index;
                    this.$refs.noteTitle.text = this.tempNoteTitle;
                    this.$nextTick(() => {
                        let title = event.target.value;
                        this.notes.map( function (note, index, notes) {
                            if (index === targetIndex) {
                                note.changed = false;
                                note.title = title
                                notes.splice(targetIndex, 1, note)
                            }
                        })
                    });
                }

                event.target.value === '' ? event.target.nextElementSibling.style.display = 'block' : event.target.nextElementSibling.style.display = 'none'
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
          border-bottom: 1px solid blue;
          border-radius: 0;
      }
  }

  .note-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    position: relative;

    span.error {
        display: none;
        position: absolute;
        bottom: -22px;
        color: tomato;
        font-size: 12px;
    }



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