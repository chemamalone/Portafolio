<script lang="ts" setup>
import { ref, onMounted } from "vue";
import emailjs from "@emailjs/browser";
import gsap from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
import SectionTitle from "~/components/SectionTitle.vue";

gsap.registerPlugin(ScrollTrigger);

// Define the types for the contact items and social links

// Reactive form data and button state
const formData = ref({
  name: "",
  email: "",
  message: "",
});

const submitBtn = ref("Submit");
const config = useRuntimeConfig();
// Handle form submission
const form = ref<HTMLFormElement | null>(null);
const handleSubmit = async () => {
  submitBtn.value = "Sending...";
  try {
    await emailjs.sendForm(config.public.SERVICE_ID as string, config.public.TEMPLATE_ID as string, form.value!, { publicKey: config.public.PUBLIC_KEY as string });
    console.log("SUCCESS!");
    formData.value = { name: "", email: "", message: "" };
    submitBtn.value = "Success";
    setTimeout(() => {
      submitBtn.value = "Submit";
    }, 3000);
  } catch (error) {
    console.log("FAILED...", error);
    submitBtn.value = "Submit";
  }
};

// GSAP animations on mounted
onMounted(() => {
  gsap.fromTo(
    ".section-title-overlay-text",
    { y: "50%" },
    {
      y: "-50%",
      scrollTrigger: {
        trigger: ".contact",
        start: "top bottom",
        end: "bottom top",
        scrub: true,
      },
    }
  );

  gsap.from(".submit-btn", {
    scale: 0,
    duration: 3.5,
    ease: "elastic",
    delay: 0.2,
    scrollTrigger: {
      trigger: ".submit-btn",
    },
  });

  gsap.from(".contact-item", {
    scale: 0,
    duration: 0.8,
    ease: "back",
    scrollTrigger: {
      trigger: ".contact-items",
    },
  });

  gsap.from(".contact-input", {
    opacity: 0,
    scale: 0,
    duration: 0.8,
    scrollTrigger: {
      trigger: ".contact-input",
    },
  });
});
</script>
<template>
  <span class="section-title-overlay-text">contact</span>
  <SectionTitle subtitle="Let's Get in touch" title="Contact me" />

  <div class="row pb-120 contact-items">
    <div class="row">
      <div class="col-sm-6 col-xl-4 col-xxl-3">
        <div class="contact-item">
          <div class="icon-box">
            <PhosphorIconPhoneCall :size="28" />
          </div>
          <p>+420 652 887 351</p>
        </div>
      </div>
      <div class="col-sm-6 col-xl-4 col-xxl-3">
        <div class="contact-item">
          <div class="icon-box">
            <PhosphorIconEnvelopeOpen :size="28" />
          </div>
          <p>emily@devis.com</p>
        </div>
      </div>
      <div class="col-sm-6 col-xl-4 col-xxl-3">
        <div class="contact-item">
          <div class="icon-box">
            <PhosphorIconMapPin :size="28" />
          </div>
          <p>4730 Crystal Springs Dr, Los Angeles, CA</p>
        </div>
      </div>
      <div class="col-sm-6 col-xl-4 col-xxl-3 d-flex align-items-center">
        <div class="d-flex gap-3 social-icons">
          <a href="#">
            <PhosphorIconFacebookLogo :size="24" />
          </a>
          <a href="#">
            <PhosphorIconLinkedinLogo :size="24" />
          </a>
          <a href="#">
            <PhosphorIconXLogo :size="24" />
          </a>
        </div>
      </div>
    </div>
  </div>

  <form ref="form" @submit.prevent="handleSubmit" id="contact-form" class="contact-form">
    <h4>Leave a Message</h4>
    <div class="row g-4 g-xl-5">
      <div class="col-sm-6 contact-input">
        <label for="name">Name</label>
        <input type="text" id="user_name" name="name" v-model="formData.name" placeholder="Your name" required />
      </div>
      <div class="col-sm-6 contact-input">
        <label for="email">Email</label>
        <input type="email" id="user_email" name="email" v-model="formData.email" placeholder="Your e-mail" required />
      </div>
      <div class="col-12 contact-input">
        <label for="message">Message</label>
        <textarea id="message" name="message" v-model="formData.message" placeholder="Your message"></textarea>
      </div>
      <div class="col-12">
        <button type="submit" class="submit-btn position-relative">
          <div class="waves-top-md">
            <span></span>
            <span></span>
            <span></span>
            <span></span>
          </div>
          {{ submitBtn }}
          <div class="waves-bottom-md">
            <span></span>
            <span></span>
            <span></span>
            <span></span>
          </div>
        </button>
      </div>
    </div>
  </form>

  <div class="col mt-5 pt-5 next-chapter">
    <span class="page">11/11</span>
  </div>
</template>

<style lang="scss">
// contact section
.contact {
  padding-top: 60px 0;
  @media (min-width: 1200px) {
    padding-top: 120px 0;
  }
  padding-bottom: 40px;
  .contact-item {
    display: flex;
    align-items: center;
    gap: 16px;
    @media (min-width: 992px) {
      gap: 24px;
    }
    .icon-box {
      height: 60px;
      width: 60px;
      @media (min-width: 992px) {
        width: 80px;
        height: 80px;
        svg {
          font-size: 2.2rem;
        }
      }
      border-radius: 50%;
      border: 1px solid rgb(var(--black));
      display: flex;
      align-items: center;
      justify-content: center;
      flex-shrink: 0;
      svg {
        font-size: 1.6rem;
      }
    }
    p {
      @media (min-width: 992px) {
        font-size: 20px;
      }
      font-weight: 500;
    }
  }
  .social-icons {
    a {
      width: 42px;
      height: 42px;
      @media (min-width: 992px) {
        width: 60px;
        height: 60px;
        svg {
          font-size: 32px;
        }
      }
      border-radius: 50%;
      border: 1px solid rgb(var(--black));
      display: flex;
      align-items: center;
      justify-content: center;
      transition-duration: 400ms;
      &:hover {
        background-color: #000;
        color: #fff;
      }
      svg {
        font-size: 24px;
      }
    }
  }
}
.contact-form {
  max-width: 856px;
  h4 {
    font-weight: 600;
    font-size: 28px;
    margin-bottom: 50px;
    @media (min-width: 992px) {
      font-weight: 700;
      font-size: 40px;
      margin-bottom: 80px;
    }
  }
  label {
    font-size: 20px;
    font-weight: 500;
    color: rgb(var(--black));
  }
  input,
  textarea {
    margin-top: 20px;
    display: block;
    background-color: transparent;
    border: none;
    padding-bottom: 16px;
    width: 100%;
    border-bottom: 1px solid rgb(var(--black));
    &:focus {
      outline: none;
    }
  }
  textarea {
    height: auto;
    margin-bottom: 40px;
    @media (min-width: 992px) {
      margin-bottom: 60px;
    }
  }
  .submit-btn {
    width: 150px;
    height: 150px;
    border-radius: 50%;
    border: 1px solid rgb(var(--black));
    display: flex;
    background-color: transparent;
    justify-content: center;
    align-items: center;
    font-size: 24px;
    font-weight: 500;
    &:hover {
      background-color: #000;
      color: white;
      svg {
        filter: brightness(0) invert(1);
      }
      .waves-top-md,
      .waves-bottom-md {
        opacity: 0;
      }
    }
  }
}
</style>
