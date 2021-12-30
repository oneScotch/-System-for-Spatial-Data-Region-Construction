<template #activator="{ on, attrs }">
  <v-dialog v-model="dialog" persistent max-width="600px">
    <template #activator="{ on, attrs }">
      <v-btn
        class="ma-2"
        color="#938427"
        outlined
        v-bind="attrs"
        v-on="on"
        :right="true"
        :absolute="true"
      >
        Edit your POI set
      </v-btn>
    </template>
    <v-card>
      <v-card-title>
        <span class="headline">POIs Details</span>
      </v-card-title>
      <v-form ref="form" v-model="valid" lazy-validation>
        <v-card-text>
          <v-container>
            <v-row>
              <v-col cols="12" sm="6" md="4">
                <v-text-field v-model="firstName" label="First name*" />
              </v-col>
              <v-col cols="12" sm="6" md="4">
                <v-text-field
                  v-model="middleName"
                  label="Middle name"
                  hint="leave blank if not applicable"
                />
              </v-col>
              <v-col cols="12" sm="6" md="4">
                <v-text-field v-model="lastName" label="Last name*" />
              </v-col>
              <!-- <v-col
                cols="12"
                sm="6"
              >
                <v-text-field
                  label="Password"
                  type="password"
                />
              </v-col> -->
              <v-col cols="12" sm="6">
                <v-select
                  :items="['Male', 'Female', 'Others']"
                  v-model="gender"
                  label="Gender*"
                />
              </v-col>
              <v-col cols="12">
                <v-text-field v-model="address" label="Address" />
              </v-col>
              <v-col cols="12" sm="6">
                <v-text-field v-model="postalCode" label="Postal Code" />
              </v-col>
              <v-col cols="12" sm="6">
                <v-select v-model="area" :items="allCountries" label="Area" />
              </v-col>
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="phoneNumber"
                  hint="Please insert country code"
                />
              </v-col>
            </v-row>
          </v-container>
          <small>*indicates required field</small>
        </v-card-text>
      </v-form>
      <v-card-actions>
        <v-spacer />
        <v-btn color="blue darken-1" text @click="dialog = false">
          Close
        </v-btn>
        <v-btn color="blue darken-1" text @click="submitForm">
          Save
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
import { pickBy, identity } from 'lodash'

export default {
  data() {
    return {
      dialog: false,
      firstName: '',
      middleName: '',
      lastName: '',
      gender: null,
      area: null,
      address: '',
      postalCode: '',
      phoneNumber: '',
      //emergencyContactPhoneNumber: '',
      genderOptions: ['Male', 'Female', 'Other'],
      bloodTypeOptions: ['A+', 'A-', 'B+', 'B-', 'AB+', 'AB-', 'O+', 'O-'],
      valid: true,
      nameRules: [
        v => !!v || 'Name is required',
        v => v.length <= 30 || 'Name must be less than 30 characters',
      ],
      genderRules: [v => !!v || 'Gender is required'],
    }
  },
  async fetch() {
    await this.$axios.$get('/v1/profile')
    const profileDetails = await this.$axios.$get('/v1/profile')

    /********** Card 1 **********/
    this.firstName = profileDetails.firstName
    this.middleName = profileDetails.middleName
    this.lastName = profileDetails.lastName
    this.gender = profileDetails.gender
    this.dob = profileDetails.dateOfBirth
    this.address = profileDetails.address
    this.postalCode = profileDetails.postalCode
    this.area = profileDetails.area
    this.phoneNumber = profileDetails.phoneNumber
    // this.email = 'placeholder@email.com' //TODO: profileDetails.email
  },
  computed: {
    allCountries() {
      let countryList = []
      for (const key in require('country-list').getNameList()) {
        const country = key[0].toUpperCase() + key.slice(1)
        countryList.push(country)
      }
      return countryList
    },
  },
  methods: {
    async submitForm() {
      // console.log('middleName: ', this.middleName)

      if (!this.$refs.form.validate()) {
        return
      }

      try {
        const input = {
          firstName: this.firstName,
          middleName: this.middleName,
          lastName: this.lastName,
          gender: this.gender,
          area: this.area,
          address: this.address,
          postalCode: this.postalCode,
          // dateOfBirth: this.dateOfBirth,
          // bloodType: this.bloodType,
          // email: this.email,
          phoneNumber: this.phoneNumber,
          //emergencyContact: this.emergencyContactPhoneNumber,
        }
        await this.$axios.$put('/v1/profile', pickBy(input, identity))
        await this.$store.dispatch('profile/refresh')
        this.$router.push({ path: '/' })

        if (this.middleName == null || this.middleName == '') {
          //await this.$axios.$delete('/v1/profile', { data: { middleName: this.middleName}})
        } else {
          // do nothing
        }
      } catch (err) {
        // TODO: show error in page
        alert(err.message)
      }
    },
  },
}
</script>
