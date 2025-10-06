<template>
  <div class="container d-flex justify-content-center">
    <div class="col-md-6">
      <h4 class="mb-3 text-center">
        Thêm Liên hệ mới <i class="fas fa-user-plus"></i>
      </h4>
      <ContactForm :contact="newContact" @submit:contact="addContact" />
    </div>
  </div>
</template>
<script>
import ContactForm from "@/components/ContactForm.vue";
export default {
  name: "ContactAdd",
  components: { ContactForm },
  data() {
    return {
      newContact: {
        name: "",
        email: "",
        address: "",
        phone: "",
        favorite: false,
      },
    };
  },
  methods: {
    addContact(contact) {
      console.log("Liên hệ mới:", contact);
      // gọi API thêm mới ở đây
      // this.$router.push({ name: "contactbook" });
    },
  },
};
</script>
