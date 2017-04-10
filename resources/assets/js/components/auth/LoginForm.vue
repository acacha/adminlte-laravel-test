<template>
 <form method="post" @submit.prevent="submit" @keydown="form.errors.clear($event.target.name)")">

  <login-input-field name="name" domain="domain"></login-input-field>

  <div class="form-group has-feedback" :class="{ 'has-error': form.errors.has('name') }">
   <input type="password" class="form-control" :placeholder="trans('adminlte_lang::message.password')" name="password" v-model="form.password"/>
   <span class="glyphicon glyphicon-lock form-control-feedback"></span>
   <transition name="fade">
    <span class="help-block" v-if="form.errors.has('name')" v-text="form.errors.get('password')"></span>
   </transition>
  </div>
  <div class="row">
   <div class="col-xs-8">
    <div class="checkbox icheck">
     <label>
      <input style="display:none;" type="checkbox" name="remember" v-model="form.remember"> {{ trans('adminlte_lang::message.remember') }}
     </label>
    </div>
   </div>
   <div class="col-xs-4">
    <button type="submit" class="btn btn-primary btn-block btn-flat" v-text="trans('adminlte_lang::message.buttonsign')" :disabled="form.errors.any()"><i v-if="form.submitting" class="fa fa-refresh fa-spin"></i></button>
   </div>
  </div>
 </form>
</template>

<style src="./face.css"></style>

<script>

import Form from 'acacha-forms'
import initialitzeIcheck from './InitializeIcheck'
import redirect from './redirect'

export default {
  mixins: [initialitzeIcheck,redirect],
  data: function () {
    let form = new Form({ name: '', password: '', remember: '' })
    if (this.name === 'email') {
      form = new Form({ email: '', password: '', remember: '' })
    }
    return {
      form : form
    }
  },
  props: {
    name: {
      type: String,
      required: true
    },
    domain: {
      type: String,
      required: false
    }
  },
  watch: {
    'form.remember': function (value) {
      if (value) {
        $('input').iCheck('check')
      } else {
        $('input').iCheck('uncheck')
      }
    }
  },
  methods: {
    submit () {
      this.form.post('/login')
       .then(response => {
         this.redirect(response)
       })
       .catch(error => {
         console.log(trans('adminlte_lang_message.loginerror') + ':' + error)
       })
    }
  }
}

</script>
