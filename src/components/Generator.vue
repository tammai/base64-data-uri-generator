<template>
  <div>
    <div class="dropbox">
      <input type="file" single class="input-file" @change="fileUpdate($event.target.files)">
      <p class="message">
        <span v-if="isInitial" class="info-message">Click or drop your file here!</span>
        <span v-if="isFileChanged || isError">
          <strong class="file-name">{{ selectedFile.name }}</strong>
          <span> selected. <span class="inline-block">(Click to change)</span></span>
        </span>
        <span v-if="isSaving">Uploading ...</span>
      </p>
      <p v-if="errorMess" class="error-message">{{ errorMess }}</p>
    </div>
    <div v-if="isFileChanged">
      <textarea class="output-string" v-model="base64" readonly />
      <div class="actions-container">
        <button class="btn-primary" @click.prevent="copyCode" :disabled="!isFileChanged">Copy to clipboard</button>
        <button class="btn-secondary" @click.prevent="resetToInitialState">Clear</button>
      </div>
    </div>
  </div>
</template>

<script>
const STATUS_INITIAL = 0
const STATUS_FILE_CHANGED = 1
const STATUS_SAVING = 2
const STATUS_ERROR = 3
const reader = new FileReader()

export default {
  name: 'Generator',
  data () {
    return {
      selectedFile: null,
      currentStatus: 0,
      errorMess: null,
      base64: null
    }
  },
  computed: {
    isInitial () {
      return this.currentStatus === STATUS_INITIAL
    },
    isFileChanged () {
      return this.currentStatus === STATUS_FILE_CHANGED
    },
    isSaving () {
      return this.currentStatus === STATUS_SAVING
    },
    isError () {
      return this.currentStatus === STATUS_ERROR
    }
  },
  mounted () {
    this.resetToInitialState()
  },
  methods: {
    resetToInitialState () {
      this.currentStatus = STATUS_INITIAL
      this.selectedFile = null
      this.errorMess = null
      this.base64 = null
    },
    fileUpdate (file) {
      if (file.length) {
        this.selectedFile = file[0]
        this.base64 = null
        this.currentStatus = STATUS_SAVING

        if ((this.selectedFile.size / 1024) >= 1024) {
          this.errorMess = 'Please select a file less than 1024 KB'
          this.currentStatus = STATUS_ERROR
        } else {
          this.errorMess = null
          this.currentStatus = STATUS_FILE_CHANGED
          reader.readAsDataURL(this.selectedFile)
          reader.onload = (e) => {
            this.base64 = e.target.result
          }
        }
      }
    },
    copyCode () {
      const temp = document.createElement('textarea')
      document.body.appendChild(temp)
      temp.value = this.base64
      temp.select()
      document.execCommand('copy')
      temp.parentNode.removeChild(temp)
      window.alert('Copied to clipboard!')
    }
  }
}
</script>

<style lang="scss" scoped>

.dropbox {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;

  min-height: 8rem;

  border: 1px dashed #DDD;
  border-radius: 4px;

  transition: all .5s;

  .message {
    max-width: calc(100% - 3.2rem);
    margin: 0;

    font-size: 1.4rem;
    font-weight: 300;
    line-height: 1.5;
    text-align: center;
    text-overflow: ellipsis;
    overflow: hidden;

    color: #666;
  }

  .info-message {
    color: #41B883;
    font-weight: 400;

    transition: all .5s;
  }

  .error-message {
    margin: .4rem 0 0;
    font-size: 1.2rem;
    color: #c0392b;
  }

  &:hover {
    border-color: #41B883;

    .info-message {
      letter-spacing: .5px;
    }
  }
}

.input-file {
  position: absolute;
  width: 100%;
  height: 100%;

  cursor: pointer;

  opacity: 0;
}

.output-string {
  width: 100%;
  height: 16rem;
  margin-top: 2.4rem;
  padding: .8rem;

  color: #666;

  border: 1px solid #DDD;
  border-radius: .4rem;

  resize: none;

  &:focus {
    outline: none;
    border-color: #41B883;
    box-shadow: 0 0 0 .2rem rgba(#41B883, .25);
  }
}

.actions-container {
  display: flex;
  justify-content: space-between;
}

.btn-primary,
.btn-secondary {
  margin-top: 1.6rem;
  padding: 0;
  font-size: 1.2rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: .5px;

  
  background-color: transparent;
  border: none;

  &:hover,
  &:focus {
    cursor: pointer;
  }

  &:focus {
    outline: none;
  }
}

.btn-primary {
  color: #41B883;

  &:hover,
  &:focus {
    color: darken(#41B883, 10%);
  }

  &:disabled {
    opacity: .5;
    pointer-events: none;
  }
}

.btn-secondary {
  color: #999;

  &:hover,
  &:focus {
    color: darken(#999, 10%);
  }
}

.inline-block {
  display: inline-block;
}
</style>
