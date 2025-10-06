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
import ContactService from "@/services/contact.service";  

export default {
  name: "ContactAdd",
  components: { ContactForm },
  data() {
    return {
      newContact: {
        name: "",
        email: "",
        job:"",
        address: "",
        phone: "",
        favorite: false,
      },
    };
  },
  methods: {
    async addContact(contact) {
      try {
        await ContactService.create(contact);   
        alert("Liên hệ được thêm thành công!");
        this.$router.push({ name: "contactbook" });
      } catch (error) {
        console.error("Lỗi khi thêm liên hệ:", error);
        alert("Không thể thêm liên hệ. Vui lòng thử lại.");
      }
    },
  },
};

</script>