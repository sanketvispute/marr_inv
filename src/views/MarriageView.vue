<template>
  <div class="marriage-template">
    <div class="header">
      <h1>Wedding Invitation</h1>
      <p>You're Invited!</p>
    </div>

    <transition name="fade">
      <div class="photo-section">
        <img src="../assets/bride.jpeg" alt="Bride" class="photo" />
        <img src="../assets/groom.jpg" alt="Groom" class="photo" />
      </div>
    </transition>

    <transition name="slide-up">
      <div class="details">
        <p><strong>Bride:</strong> {{ brideName }}</p>
        <p><strong>Groom:</strong> {{ groomName }}</p>
        <p><strong>Wedding Date:</strong> {{ weddingDate }}</p>
        <p><strong>Venue:</strong> {{ venue }}</p>
      </div>
    </transition>

    <transition name="bounce">
      <div class="rsvp">
        <h2>RSVP</h2>
        <form @submit.prevent="submitRSVP">
          <input
            v-model="guestName"
            type="text"
            placeholder="Your Name"
            required
          />
          <button type="submit">Submit</button>
        </form>
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  data() {
    return {
      brideName: "Alice",
      groomName: "Bob",
      weddingDate: "2024-12-31",
      venue: "Sunset Beach Resort",
      bridePhoto: "../assets/bride.jpeg",
      groomPhoto: "../assets/groom.jpg",
      guestName: "",
    };
  },
  methods: {
    submitRSVP() {
      alert(`Thank you for your RSVP, ${this.guestName}!`);
      this.guestName = ""; // Reset the form
    },
  },
};
</script>

<style scoped>
.marriage-template {
  max-width: 600px;
  margin: 0 auto;
  text-align: center;
  background-color: white;
  padding: 15px;
  border-radius: 10px;
  box-shadow: 0 4px 4px rgba(0, 0, 0, 0.1);
  color: #000000; /* Black text for a classic look */
  margin-top: 20px;
}

.header {
  padding: 10px;
  border-radius: 8px 8px 0 0;
}

.photo-section {
  display: flex;
  justify-content: center;
  margin: 20px 0;
}

.photo {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  margin: 0 10px;
  border: 3px solid #9c27b0; /* Violet border for a subtle highlight */
  object-fit: cover;
}

.details p {
  font-size: 1.2em;
  margin: 5px 0;
  color: #000000; /* Ensure text is black */
}

.rsvp form {
  margin-top: 20px;
}

.rsvp input {
  padding: 10px;
  margin-right: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.rsvp button {
  padding: 10px 15px;
  background-color: #9c27b0; /* Violet background for button */
  border: none;
  color: white;
  border-radius: 4px;
  cursor: pointer;
}

.rsvp button:hover {
  background-color: #7b1fa2; /* Darker violet on hover */
}

/* Animations */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}

.slide-up-enter-active {
  transition: transform 0.5s ease;
}
.slide-up-enter {
  transform: translateY(20px);
}

.bounce-enter-active {
  animation: bounce 0.5s;
}
@keyframes bounce {
  0%,
  100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-10px);
  }
}
</style>
