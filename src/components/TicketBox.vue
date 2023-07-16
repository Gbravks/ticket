<template>
    <div class="container" id="app">
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
          <button type="submit" v-bind:disabled="!formIsValid" v-bind:class="{ active: formIsValid }">Confirm Tickets</button>
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
html {
  font-size: 14px;
  font-family: 'Oxygen', sans-serif;
  background-color: #2b045e;
}

/* Header */

header {
  display: flex;
  align-items: center;
  background-color: #ebebf1;
  color: #ffffff;
}

.logo {
  padding-right: 1.28rem;
}

h1 {
  flex: 1;
  font-family: 'Arial', sans-serif;
  font-size: 1.43rem;
  font-weight: 900;
  letter-spacing: 0.3px;
}

.time {
  padding-right: 4.29rem;
  font-size: 1.28rem;
  font-weight: bold;
}

/* Form Elements */

form {
  display: flex;
  flex-direction: column;
  padding: 1.86rem 20%;
}

input,
textarea,
select {
  padding: .71rem;
  margin-right: 1.21rem;
  background-color: #ffffff;
  border: solid 1px #e0e0e0;
}

textarea:focus, 
input:focus{
    outline: none;
}

textarea {
  height: 5.2rem;
}

select {
  -webkit-appearance: none;
  -moz-appearance: none;
  -webkit-border-radius: 0px;
  background-image: url("https://content.codecademy.com/courses/vue-instances/triangle.svg");
  background-position: 97% 50%;
  background-repeat: no-repeat;
  background-size: 14px;
}

label {
  margin-bottom: .71rem;
  font-weight: bold;
}

legend {
  margin-bottom: .71rem;;
  font-weight: bold;
}

.invalid {
  border: 1px solid #fa4359;
}

.form-row {
  display: flex;
  margin-bottom: 2.14rem;
}

.form-row.vertical {
  flex-direction: column;
}

.form-row > * {
  flex: 1;
}

.form-field {
  display: flex;
  flex-direction: column;
}

.checkbox {
  display: flex;
  flex-direction: column;
}

.checkbox > * {
  margin-bottom: .71rem;
}

.checkbox input {
  font-size: 1.29rem;
}

.checkbox label {
  font-weight: normal;
}

.agreement.form-row {
  margin-bottom: 0;
}

.agreement fieldset > * {
  margin-bottom: .71rem;
}

.signature {
  font-family: 'Petit Formal Script', cursive;
}

button {
  cursor: default;
  padding: .85rem 2.29rem;
  border-radius: 4px;
  margin-right: 1.36rem;
  background-color: gray;
  font-size: 1.14rem;
  color: #ffffff;
  transition: .25s;
}

button.active:hover {
  background-color: #1e5df1;
}

button.reset {
  cursor: pointer;
  background-color: #e85b49;
}

button.reset:hover {
  background-color: #ea2e16;
}
button.active {
  cursor: pointer;
  background-color: #4c7ef3;
}

/* Developer Pane */

#developer-pane {
  border: 1px solid #979797;
  margin: 2rem 20%;
  background-color: #d1fdda;
}

#developer-pane > * {
  padding: 1rem;
  font-weight: 1.2rem;
}

#developer-pane h2 {
  text-align: center;
  font-weight: bold;
  font-size: 1.5rem;
}

#developer-pane em {
  font-weight: bold;
}

</style>
