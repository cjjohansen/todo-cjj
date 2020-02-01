<template>
  <v-dialog max-width="600px">
      <template v-slot:activator="{ on }">
    <v-btn text v-on="on" class="success">Add New Project</v-btn>
    </template>
    <v-card>
      <v-card-title>
        <h2>Add a New Project</h2>
      </v-card-title>
       <v-card-text>
        <v-form class="px-3" ref="form">
          <v-text-field v-model="title" label="Title" prepend-icon="folder" :rules="inputRules"></v-text-field>
          <v-textarea v-model="content" label="Information" prepend-icon="edit" :rules="inputRules"></v-textarea>

          <v-menu v-model="menu" :close-on-content-click="false">
              <template v-slot:activator="{ on }">
                <v-text-field v-on="on" 
                  v-model="due" clearable label="Due date" prepend-icon="date_range" :rules="inputRules">
                </v-text-field>
              </template>
            <v-date-picker v-model="due" @change="menu = false"></v-date-picker>
          </v-menu>

          <v-spacer></v-spacer>

          <v-btn text @click="submit" class="success mx-0 mt-3">Add Project</v-btn>
        </v-form>
      </v-card-text>
    </v-card>
   
  </v-dialog>
</template>

<script>
import format from 'date-fns/format';
import { parseISO } from 'date-fns'; 
export default {
  data() {
    return {
      title: '',
      content: '',
      due: new Date().toISOString().substr(0, 10),
      menu: false,
           inputRules: [
        v => !!v || 'This field is required',
        v => v?.length >= 3 || 'Minimum length is 3 characters'
      ]
    };
  },
  methods: {
     submit() {
      if(this.$refs.form.validate()) {
        console.log(this.title, this.content)
      }
    }
 },
  computed: {
    formattedDate () {
      console.log(this.due);
      return this.due ? format(this.due, 'yyyy-mm-dd') : ''
    },
    isoDate () {
      console.log(this.due);
      return this.due ? parseISO( this.due, 'yyyy-LL-dd') : ''
    }
  }
}
</script>
