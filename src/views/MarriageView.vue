<template>
  <div class="marriage-template">
    <div class="header">
      <h1>Wedding Invitation</h1>
      <p class="subtitle">You're Invited to Celebrate Love!</p>
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
          <div class="form-group">
            <label for="name">Name:</label>
            <input type="text" id="name" v-model="formData.name" required />
          </div>
          <div class="form-group">
            <label for="email">Email:</label>
            <input type="email" id="email" v-model="formData.email" required />
          </div>
          <div class="form-group">
            <label for="contact">Contact Number:</label>
            <input
              type="tel"
              id="contact"
              v-model="formData.contact"
              required
            />
          </div>
          <div class="form-group">
            <label for="meal">Meal Preference:</label>
            <select id="meal" v-model="formData.meal" required>
              <option value="vegetarian">Vegetarian</option>
              <option value="non-vegetarian">Non-Vegetarian</option>
              <option value="vegan">Vegan</option>
            </select>
          </div>

          <!-- reCAPTCHA Widget -->
          <div
            id="recaptcha"
            class="g-recaptcha"
            data-sitekey="6LfpoJ4qAAAAACHaEsBVg_BHGUOws0Ql2bNNYzUP"
          ></div>

          <button type="submit">Submit RSVP</button>
        </form>
      </div>
    </transition>
  </div>
</template>

<script>
import VueRecaptcha from "vue-recaptcha";
export default {
  components: {
    VueRecaptcha,
  },
  data() {
    return {
      brideName: "Sanjana",
      groomName: "Max",
      weddingDate: "2024-12-31",
      venue: "Sunset Beach Resort",
      formData: {
        name: "",
        email: "",
        contact: "",
        meal: "vegetarian",
      },
      captchaResponse: "",
    };
  },
  mounted() {
    this.loadRecaptcha();
  },
  // mounted() {
  //   this.getRSVP();
  // },
  methods: {
    loadRecaptcha() {
      if (window.grecaptcha) {
        // Render the reCAPTCHA widget
        grecaptcha.render("recaptcha", {
          sitekey: "6LfpoJ4qAAAAACHaEsBVg_BHGUOws0Ql2bNNYzUP",
          callback: this.onCaptchaSuccess, // Called when CAPTCHA is solved
          "expired-callback": this.onCaptchaExpired, // Optional callback
        });
      } else {
        console.error("reCAPTCHA script not loaded");
      }
    },
    onCaptchaSuccess(token) {
      console.log("CAPTCHA solved, token:", token);
      if (!token) {
        console.error("CAPTCHA token is empty.");
      }
      this.captchaResponse = token; // Assign token
    },
    onCaptchaExpired() {
      console.log("CAPTCHA expired");
      this.captchaResponse = ""; // Reset token
      grecaptcha.reset(); // Reset the CAPTCHA widget
      alert("The CAPTCHA has expired. Please solve it again.");
    },
    async submitRSVP() {
      try {
        const captchaResponse = grecaptcha.getResponse();
        if (!captchaResponse) {
          alert("Please complete the CAPTCHA");
          return;
        }
        const response = await fetch("https://eventapi-hjb8.onrender.com/", {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify({
            ...this.formData,
            captchaResponse: this.captchaResponse,
          }),
        });

        if (response.ok) {
          alert("RSVP submitted successfully!");
          this.resetForm();
          grecaptcha.reset(); // Reset CAPTCHA after successful submission
        } else {
          alert("Failed to submit RSVP. Please try again.");
        }
      } catch (error) {
        console.error("Error submitting RSVP:", error);
        alert("An error occurred while submitting the RSVP.");
      }
    },

    // async getRSVP() {
    //   try {
    //     const response = await fetch("http://localhost:5000/");
    //     if (!response.ok) {
    //       throw new Error("Something went wrong with fetch RSPV");
    //     } else {
    //       const data = await response.json();
    //       console.log("data", data);
    //     }
    //   } catch (err) {
    //     console.log("error", err);
    //   }
    // },
    resetForm() {
      this.formData = {
        name: "",
        email: "",
        contact: "",
        meal: "vegetarian",
      };
    },
  },
};
</script>

<style scoped>
#recaptcha {
  display: flex;
  justify-content: center; /* Center the widget horizontally */
  margin: 15px 0; /* Add spacing above and below */
  transform: scale(0.9); /* Scale down the widget */
  transform-origin: center; /* Keep the scaling centered */
}
.marriage-template {
  max-width: 700px;
  margin: 20px auto;
  background-color: #ffffff;
  padding: 20px;
  border-radius: 15px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  color: #333333;
}

.header {
  text-align: center;
  margin-bottom: 20px;
}

.header h1 {
  font-size: 1.7em;
  font-weight: 600;
  margin: 0;
}

.subtitle {
  font-size: 1em;
  color: #9c27b0;
}

.photo-section {
  display: flex;
  justify-content: center;
  margin: 20px 0;
}

.photo {
  width: 120px;
  height: 120px;
  border-radius: 50%;
  margin: 0 15px;
  border: 5px solid #9c27b0;
  object-fit: cover;
}

.details {
  text-align: center;
  font-size: 1.1em;
  color: #555555;
}

.details p {
  margin: 10px 0;
}

.rsvp {
  margin-top: 30px;
}

.rsvp h2 {
  text-align: center;
  color: #9c27b0;
  font-size: 1.8em;
  margin-bottom: 20px;
}

.form-group {
  margin-bottom: 15px;
}

.form-group label {
  display: block;
  font-size: 1em;
  margin-bottom: 5px;
  color: #333333;
}

.form-group input,
.form-group select {
  width: 100%;
  padding: 10px;
  border: 1px solid #cccccc;
  border-radius: 5px;
  font-size: 1em;
}

.form-group input:focus,
.form-group select:focus {
  outline-color: #9c27b0;
}

button {
  width: 100%;
  padding: 10px;
  font-size: 1.2em;
  background-color: #9c27b0;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #7b1fa2;
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
