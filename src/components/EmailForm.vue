<template>
    <div class='form-wrapper'>
        <div class='form-title'>Send E-mail</div>
        <input class='recipients' @blur='validateEmails($event.target)' placeholder='To'></input>
        <input class='cc-recipients' @blur='validateEmails($event.target)' placeholder='CC'></input>
        <input class='bcc-recipients' @blur='validateEmails($event.target)' placeholder='BCC'></input>
        <input class='subject' placeholder='Subject' maxlength='100' @blur='checkIfValid()'></input>
        <textarea class='message' rows='10' columns='50' placeholder='Message' @blur='checkIfValid()'></textarea>
        <div class='attachment-wrapper' v-if='hasAttachments'>
            <ul class='attachments'>
                <template v-for='image in images' class='attachment'>
                    <li class='item'>
                        <div class='image-wrapper' @click='removeImage(image)'>
                            <img v-bind:src='image'></img>
                        </div>
                    </li>
                </template>
            </ul>
        </div>
        <button :class='[{enabled: submitButtonEnabled}, "submit-button"]'><div class='line'></div><div class='arrow'></div>Send</button>
        <div class='attach-button'>
            <input type='file' @change='attachImage($event.target.files)'></input>
        </div>
    </div>
</template>

<script>
export default {
  name: 'email-form',

  data () {
    return {
      submitButtonEnabled: false,
      images: []
    }
  },

  mounted () {
    this.subject = document.getElementsByClassName('subject')[0]
    this.message = document.getElementsByClassName('message')[0]
    this.recipients = document.getElementsByClassName('recipients')[0]
  },

  computed: {
    hasAttachments () {
      return this.images.length > 0
    }
  },

  methods: {
    validateEmails (el) {
      const emails = el.value.split(',')
      let result = emails.find((email) => {
        return !this.isEmailAddress(email.trim())
      })

      if (result === void 0 || el.value === '') {
        el.classList.remove('failed-validation')
      } else {
        el.classList.add('failed-validation')
      }

      this.checkIfValid()
    },

    isEmailAddress (stringToTest) {
      // emailregex.com
      // standard regex for 99.99% email validation
      // eslint doesnt understand the regex correctly so disabled for this line
      return /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/.test(stringToTest) // eslint-disable-line
    },

    attachImage (files) {
      if (files.length < 1) {
        return
      }

      if (files.length > 0) {
        for (let index = 0; index < files.length; index++) {
          const reader = new FileReader()
          reader.onloadend = () => {
            this.images.push(reader.result)
          }

          reader.readAsDataURL(files[index])
        }
      }
    },

    checkIfValid () {
      let result = false

      // make sure we have the elements on the DOM before we try to use them
      if (this.subject && this.message && this.recipients) {
        result = this.subject.value.length > 0 &&
          this.message.value.length > 0 &&
          this.recipients.value.length > 0 &&
          !this.recipients.classList.contains('failed-validation')
      }

      this.submitButtonEnabled = result
    },

    removeImage (img) {
      this.images = this.images.filter(e => e !== img)
    }
  }
}
</script>

<style lang='less' scoped>
@regular-font: {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-size: 12px;
};

@center-horizontal: {
    left: 0;
    right: 0;
    margin-left: auto;
    margin-right: auto;
};

.form-wrapper {
    position: relative;
    display: block;
    max-width: 45%;
    min-width: 600px;
    margin: 5rem auto;
    padding-bottom: 5rem;
    @center-horizontal();
    border-radius: 0.3rem;
    background: #FFFFFF;
    box-shadow: 0 0.5rem 3rem grey;
    @regular-font();

    input {
        display: block;
        margin: 0.8rem auto;
        @center-horizontal();
        width: 85%;
        padding: 0.5rem 0.5rem;
        border-radius: 0.2rem;
        border: 1px solid lightgrey;
        @regular-font();
    }

    .form-title {
        display: block;
        text-align: left;
        padding: 1.5rem 3rem;
        background: #094358;
        border-radius: 0.3rem 0.3rem 0 0;
        @regular-font();
        color: white;
        font-size: 1.2rem;
        font-weight: 100;
        margin-bottom: 2rem;
    }

    .message {
        display: block;
        width: 85%;
        @center-horizontal();
        border: 1px solid lightgrey;
        border-radius: 0.3rem;
        padding: 0.5rem;
        resize: none;
        @regular-font();
    }

    .submit-button {
        position: absolute;
        bottom: 2rem;
        right: 2.5rem;
        border-radius: 5rem;
        height: 2rem;
        width: 6rem;
        border: none;
        background: lightgrey;
        outline: none;
        color: grey;

        .arrow {
            border: solid grey;
            border-width: 0 1.5px 1.5px 0;
            transform: rotate(-45deg);
            display: inline-block;
            padding: 0.22rem;
            margin: auto 0.5rem auto auto;
        }

        .line {
            display: inline;
            position: absolute;
            top: 50%;
            transform: translateY(-40%);
            margin: auto auto auto -0.2rem;
            width: 0.8rem;
            height: 2px;
            background: grey;
        }

        &.enabled {
            background: blue;
            color: white;

            .arrow {
                border: solid white;
                border-width: 0 1.5px 1.5px 0;
            }

            .line {
                background: white;
            }
        }
    }

    .attach-button {
        position: absolute;
        bottom: 2rem;
        left: 2.5rem;
        width: 2rem;
        height: 2rem;
        border-radius: 5rem;
        border: 1px solid lightgrey;
        background: white;
        outline: none;
        background-image: url('../assets/paper-clip.png');
        background-position: center;
        background-repeat: no-repeat;
        background-size: 150%;

        input {
            height: 2rem;
            width: 2rem;
            opacity: 0;
        }
    }

    .failed-validation {
        border: 2px solid indianred;
    }

    ul {
        display: flex;
        padding: 1rem;
        flex-flow: row wrap;
        justify-content: left;
        list-style: none;

        li {
            @center-horizontal();
            padding: 1rem;
            max-width: 150px;
            height: auto;
            text-align: left;

            .image-wrapper {
                position: relative;
                width: 100px;
                height: 150px;
            }

            .image-wrapper img {
                width: 100%;
                position: absolute;
                border-radius: 1rem;
                top: 0;
                bottom: 0;
                left: 0;
                right: 0;
                margin: auto;
            }

            .image-wrapper:after {
                content: '';
                display: table;
                clear: both;
                position: absolute;
                width: 100px;
                height: 150px;
                top: 0;
                left: 0;
                background: rgba(0,0,0,0.6);
                background-image: url('../assets/trash.png');
                background-repeat: no-repeat;
                background-size: 60%;
                background-position: center;
                border-radius: 1rem;
                opacity: 0;
                transition: all 0.5s;
            }

            .image-wrapper:hover:after {
                opacity: 1;
            content: '';
            display: table;
            clear: both;
            }
        }

        .clearfix:after {
            content: '';
            display: table;
            clear: both;
        }
    }
}

// fix for I.E 11
@media screen and (-ms-high-contrast: active), screen and (-ms-high-contrast: none) {
    .form-wrapper {
        .submit-button {
            .line {
                margin-left: 0.7rem;
            }
        }
    }
}
</style>