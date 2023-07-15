<template>
    <div id="app">
      <header>
        <img class="logo" src="https://content.codecademy.com/courses/vue-instances/ticket_logo.svg" alt="logo">
        <h1>TICKETBOX</h1>
      </header>
      <form v-on:reset="resetFields" v-on:submit.prevent="submitForm">
        <div class="form-row">
          <div class="form-field">
            <label for="first-name">First Name</label>
            <input type="text" id="first-name" v-model.trim="firstName" />
          </div>
          <div class="form-field">
            <label for="last-name">Last Name</label>
            <input type="text" id="last-name" v-model.trim="lastName" />
          </div>
        </div>
        <div class="form-row">
          <div class="form-field">
            <label for="email">Email</label>
            <input type="text" id="email" v-model.trim="email" />
          </div>
        </div>
        <div class="form-row">
          <div class="form-field">
            <label for="ticket-quantity">Ticket Quantity</label>
            <select id="ticket-quantity" v-model.number="ticketQuantity">
              <option>1</option>
              <option>2</option>
              <option>3</option>
              <option>4</option>
            </select>
          </div>
          <div class="form-field">
            <legend>Ticket Type</legend>
            <div>
              <input type="radio" id="general" value="general" v-model="ticketType">
              <label for="general">General Admission</label>
            </div>
            <div>
              <input type="radio" id="vip" value="vip" v-model="ticketType">
              <label for="vip">VIP</label>
            </div>
          </div>
        </div>
        <div class="form-row checkbox">
          <fieldset>
            <legend>How did you hear about this event?</legend>
            <div>
              <input type="checkbox" id="friend" value="friend" v-model="referrals">
              <label for="friend">Friend</label>
            </div>
            <div>
              <input type="checkbox" id="publication" value="publication" v-model="referrals">
              <label for="publication">Publication</label>
            </div>
            <div>
              <input type="checkbox" id="social-media" value="social" v-model="referrals">
              <label for="social-media">Social Media</label>
            </div>
          </fieldset>
        </div>
        <div class="form-row vertical">
          <label for="requests">Special requests</label>
          <textarea id="requests" v-model.trim="specialRequests"></textarea>
        </div>
        <div class="form-row vertical agreement checkbox">
          <fieldset>
            <legend>Purchase Agreement</legend>
            <p>I, {{ fullName }}, wish to buy {{ ticketDescription }}. I understand that all ticket sales are final.</p>
            <input type="checkbox" name="agreement" id="agree" v-model="purchaseAgreementSigned" />
            <label for="agree">I Agree</label>
          </fieldset>
        </div>
        <div class="form-row vertical">
          <label for="signature">Signature</label>
          <input id="signature" class="signature" />
        </div>
        <div>
          <button type="reset" class="reset">Reset</button>
          <button type="submit" v-bind:disabled="!formIsValid" v-bind:style="submitButtonStyles">Confirm Tickets</button>
        </div>
      </form>
  
    </div>

</template>

<script>
export default {
  name: 'TicketBox',
  data() {
    return {
      firstName: '',
      lastName: '',
      email: '',
      ticketQuantity: 1,
      ticketType: 'general',
      referrals: [],
      specialRequests: '',
      purchaseAgreementSigned: false
    }
  },
  computed: {
    fullName: {
      get() {
        if (this.firstName && this.lastName) {
          return this.firstName + ' ' + this.lastName;
        } else {
          return this.firstName || this.lastName;
        }
      },
      set(newFullName) {
        const names = newFullName.split(' ');

        if (names.length === 2) {
          this.firstName = names[0];
          this.lastName = names[1];
        }

        if (names.length <= 1) {
          this.firstName = names[0] || '';
          this.lastName = '';
        }
      }
    },
    ticketDescription: function () {
      let readableTicketType = 'General Admission';
      if (this.ticketType === 'vip') {
        readableTicketType = 'VIP';
      }

      let ticketPluralization = 'tickets';
      if (this.ticketQuantity === 1) {
        ticketPluralization = 'ticket';
      }

      return this.ticketQuantity + ' ' + readableTicketType + ' ' + ticketPluralization;
    },
    emailIsValid: function () {
      return this.email.includes('@');
    },
    formIsValid: function () {
      return this.firstName && this.lastName && this.emailIsValid && this.purchaseAgreementSigned;
    },
    submitButtonStyles: function () {
      if (this.formIsValid) {
        return {
          'background-color': '#4c7ef3',
          cursor: 'pointer'
        }
      } else {
        return {
          'background-color': 'gray',
          cursor: 'default'
        }
      }
    },
    touchedEmailStyles: function () {
      if (this.email) {
        return {
          'border-color': '#bdbcbc',
          'border-width': '2px'
        }
      } else {
        return {
          'border-color': '#e0e0e0',
          'border-width': '2px'
        }
      }
    },
    invalidEmailStyles() {
      if (this.email && !this.emailIsValid) {
        return {
          backgroundColor: '#ffeded',
          borderColor: '#da5252'
        };
      } else {
        return {};
      }
    },

  },

    watch: {
      // eslint-disable-next-line no-unused-vars
      specialRequests(newRequests, oldRequests) {
        if (newRequests.toLowerCase().includes('meet and greet') ||
          newRequests.toLowerCase().includes('meet-and-greet')) {
          this.ticketType = 'vip';
        }
      }
    },
    methods: {
      resetFields: function () {
        this.firstName = '';
        this.lastName = '';
        this.email = '';
        this.ticketQuantity = 1;
        this.ticketType = 'general';
        this.referrals = [];
        this.specialRequests = '';
        this.purchaseAgreementSigned = false;
      },

      getSignature() {
        return this.firstName + ' ' + this.lastName;
      }
    }
  
};

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
