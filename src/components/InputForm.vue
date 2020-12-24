<template>
  <v-card
  class="my-10"
  :elevation="formElevation"
  :rounded="formRounded"
  width="100%"
  >
    <v-container
      class="mx-10 tex-center"
    >
      <v-form
        ref="form"
      >
        <v-text-field
          label="YouTube動画ID"
          v-model="movieUrl"
          :error-messages="movieErrorMessages"
          placeholder="動画URLの【v= 】の後に続く英数字"
          style="width:300px"
        />
        <v-text-field
          label="コメント"
          v-model="comment"
          :rules="commentRules"
          placeholder="動画紹介のための任意のコメント"
          class="mb-5"
          style="width: 500px"
        />
        <v-btn
            @click="formClick"
            color="blue"
            type="button"
            x-large
        >
            CLICK!
        </v-btn>
      </v-form>
    </v-container>
	</v-card>
</template>

<script>
	export default {
      name: "InputForm",

			data () {
					return {
						movieUrl: "",
						movieErrorMessages: [],
						comment: "",
						commentRules:[
							value => !!value || '入力して下さい',
							value => value.length <= 16 || '16文字以内で入力して下さい'
						],
					}
      },
      
      props:{
        formRounded:{
          type: String,
          required: true,
          default:'xl',
        },
        formElevation: {
        type: String,
        required: false,
        default: '10',
        },
      },

			watch:{
				movieUrl: function(value){
					this.validateMovieUrl(value)
				}
      },
      
      methods: {
      formClick () {
        // バリデーションを強制実行
        this.validateMovieUrl(this.movieUrl)
        const validateComment = this.$refs.form.validate()
        // フォームにエラーがなければ、動画保存処理を実行
        if (this.movieErrorMessages.length === 0 && validateComment) {
          this.$emit('storeMovie', this.movieUrl, this.comment)
        }
      },
      validateMovieUrl (value) {
        const stringValidation = /^[a-zA-Z0-9!-/:-@¥[-`{-~]+$/.test(value)
        if (!value) {
          this.movieErrorMessages = [
            '入力してください',
          ]
        } else if (value.length !== 11) { 
          this.movieErrorMessages = [
            '11文字で入力してください',
          ]
        } else if (!stringValidation) { 
          this.movieErrorMessages = [
            '半角英数記号で入力してください',
          ]
        } else {
          this.movieErrorMessages = []
        }
        },
      }
	}
</script> 