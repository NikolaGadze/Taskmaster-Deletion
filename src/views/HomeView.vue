<template>
  <fragment> 
    <div class="background"></div>
    <v-main class="d-flex justify-center align-center">
      <v-col cols="10" lg="4" class="mx-auto">

        <v-card class="pa-4">

          <div class="text-center">
            
            <v-avatar size="100" color="primary lighten-5">

              <v-icon size="40" color="primary"> mdi-mail </v-icon>
            </v-avatar>

            <h2 class="primary--text"> Contact us to request for your account to be deleted!</h2>
          </div>

          <v-form @submit.prevent="submitHandler" ref="form" class="d-flex justify-center">
              <v-card-text>
                <v-text-field v-model="to" :rules="toRules" label="Email" append-icon="mdi-mail" required></v-text-field>
                <v-text-field v-model="subject" :rules="subjectRules" label="Subject" append-icon="mdi-text-long" required style="margin-bottom: 8%"></v-text-field>
                <v-textarea v-model="message" :rules="messageRules" label="Message" append-icon="mdi-pencil" outlined required></v-textarea>
              </v-card-text>

            <v-card-actions class="justify-center">
              <v-btn type="submit" color="blue">Send</v-btn>
            </v-card-actions>

          </v-form>
        </v-card>
      </v-col>
    </v-main>

    <v-snackbar
      v-model="snackbar"
      color="success"
    >
      {{ text }}

      <template v-slot:action="{ attrs }">
        <v-btn
          color="white"
          text
          v-bind="attrs"
          @click="snackbar = false"
        >
          Zatvori
        </v-btn>
      </template>
    </v-snackbar>

    <v-snackbar
      v-model="error_snackbar"
      color="red"
    >
      {{ text2 }}

      <template v-slot:action="{ attrs }">
        <v-btn
          color="blue-darken-1"
          text
          v-bind="attrs"
          @click="error_snackbar = false"
        >
          Zatvori
        </v-btn>
      </template>
    </v-snackbar>

  </fragment> 
</template>

<script>

export default {
  name: 'ContactUs',
  data: () => ({
    to: '',
    subject: '',
    message: '',
    snackbar : false,
    error_snackbar: false,
    text: "Email successfully sent!",
    text2: "Error while sending the email!",

    toRules: [
      v => !!v || 'Email is required',
      v => /.+@.+\..+/.test(v) || 'Wrong format of email address!',
    ],

    subjectRules: [
      v => !!v || 'Subject of the email is required',
      v => (v && v.length >= 3) || 'The email subject must have at least 3 or more letters!',
    ],

    messageRules: [
      v => !!v || 'Message of the email is required',
      v => (v && v.length >= 3) || 'The message must have at least 3 or more letters!',
    ],
    
  }),

  methods: {
    async sendEmail() {
      try {
        const response = await fetch('https://formspree.io/f/xrgnejyz', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({
            _replyto: this.to,
            subject: this.subject,
            message: this.message
          })
        })
        if (response.ok) {
          this.snackbar = true
          this.to = ''
          this.subject = ''
          this.message = ''
        } else {
          this.error_snackbar = true

        }
      } catch (error) {
        this.error_snackbar = true
        console.error(error)
      }
    },

    submitHandler() {
      if (this.$refs.form.validate()){
  
        this.sendEmail()
        
        }
      }

  
  }

    
       
  }
  

</script>