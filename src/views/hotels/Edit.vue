<template>
  <div class="animated fadeIn">
    <div class="container">
      <b-row class="justify-content-center">
        <b-col md="8">
          <b-card
            header="<i class='icon-energy'></i>&nbsp;&nbsp;Edit Outlet Details"
          >
            <!-- Update outlet Form -->
            <b-form @submit.prevent="update"
              @keydown="form.errors.clear($event.target.name)"
            >
              <!-- Name -->
              <b-input-group class="mb-3">
                <b-input-group-prepend>
                  <b-input-group-text>
                    <i class="icon-home"></i>
                  </b-input-group-text>
                </b-input-group-prepend>
                <input type="text"
                  :class="`form-control ${form.errors.has('name') ? 'is-invalid' : ''}`"
                  placeholder="Outlet name"
                  name="name"
                  v-model="form.name"
                >
                <b-form-invalid-feedback
                  v-if="form.errors.has('name')"
                >
                  {{ form.errors.get('name') }}
                </b-form-invalid-feedback>
              </b-input-group>
              <!-- Pan No -->
              <b-input-group class="mb-3">
                <b-input-group-prepend>
                  <b-input-group-text>
                    <i class="icon-note"></i>
                  </b-input-group-text>
                </b-input-group-prepend>
                <input type="text"
                  :class="`form-control ${form.errors.has('pan_no') ? 'is-invalid' : ''}`"
                  placeholder="PAN No."
                  name="pan_no"
                  v-model="form.pan_no"
                >
                <b-form-invalid-feedback
                  v-if="form.errors.has('pan_no')"
                >
                  {{ form.errors.get('pan_no') }}
                </b-form-invalid-feedback>
              </b-input-group>
              <!-- GSTN No -->
              <b-input-group class="mb-3">
                <b-input-group-prepend>
                  <b-input-group-text>
                    <i class="icon-note"></i>
                  </b-input-group-text>
                </b-input-group-prepend>
                <input type="text"
                  :class="`form-control ${form.errors.has('gstn_no') ? 'is-invalid' : ''}`"
                  placeholder="GSTN No."
                  name="gstn_no"
                  v-model="form.gstn_no"
                >
                <b-form-invalid-feedback
                  v-if="form.errors.has('gstn_no')"
                >
                  {{ form.errors.get('gstn_no') }}
                </b-form-invalid-feedback>
              </b-input-group>
              <!-- Phone no -->
              <b-input-group class="mb-3"
                v-for="(phone, i) in form.phones"
                :key="`phone${i}`"
              >
                <b-input-group-prepend>
                  <b-input-group-text>
                    <i class="icon-phone"></i>
                  </b-input-group-text>
                </b-input-group-prepend>
                <input type="number"
                  :class="`form-control`"
                  placeholder="Phone no"
                  name="phone"
                  v-model="phone.phone"
                >
                <b-form-invalid-feedback
                  v-if="form.errors.has('phone')"
                >
                  {{ form.errors.get('phone') }}
                </b-form-invalid-feedback>
              </b-input-group>
              <b-button size="sm" class="mb-3" variant='secondary' @click="addPhone"><i class='icon-plus'></i>&nbsp; Add Phone NOs</b-button>
              <!-- Email ID -->
              <b-input-group class="mb-3"
                v-for="(email, j) in form.emails"
                :key="`email${j}`"
              >
                <b-input-group-prepend>
                  <b-input-group-text>
                    <i class="icon-envelope-letter"></i>
                  </b-input-group-text>
                </b-input-group-prepend>
                <input type="text"
                  :class="`form-control ${form.errors.has('email') ? 'is-invalid' : ''}`"
                  placeholder="Email ID"
                  name="email"
                  v-model="email.email"
                >
                <b-form-invalid-feedback
                  v-if="form.errors.has('email')"
                >
                  {{ form.errors.get('email') }}
                </b-form-invalid-feedback>
              </b-input-group>
              <b-button size="sm" class="mb-3" variant='secondary' @click="addEmail"><i class='icon-plus'></i>&nbsp; Add Email IDs</b-button>
              <br>
              <b-button variant="primary" class="px-4" type="submit">Update Outlet</b-button>
            </b-form>
            <!-- End Update Outlet form -->
          </b-card>
        </b-col>
      </b-row>
    </div>
  </div>
</template>

<script type="text/javascript">
import Form from '@/helpers/Form.js'

export default {
  data: () => ({
    form: new Form({
      name: '',
      pan_no: '',
      gstn_no: '',
      phones: [
        {
          phone: ''
        }
      ],
      emails: [
        {
          email: ''
        }
      ]
    })
  }),

  mounted () {
    this.form.get(`/api/hotels/${this.$route.params.outletId}`)
      .then(data => {
        this.form = new Form(data.data)
      })
  },

  methods: {
    // To update a new outlet
    update () {
      this.form.patch(`/api/hotels/${this.$route.params.outletId}`)
        .then(data => {
          this.$router.push(`/outlets`)
        })
        .catch(errors => {
        })
    },
    // Add a new input box for phones array
    addPhone () {
      this.form.phones.push({
        phone: ''
      })
    },
    // Add a new input box for phones array
    addEmail () {
      this.form.emails.push({
        email: ''
      })
    }
  }
}
</script>
