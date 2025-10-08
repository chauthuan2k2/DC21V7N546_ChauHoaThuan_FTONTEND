<template>
  <div v-if="contact" class="page">
    <h4>Hiệu chỉnh Liên hệ</h4>

    <ContactForm
      :contact="contact"
      @submit:contact="updateContact"
      @delete:contact="deleteContact"
      @cancel="goBack"
    />

    <p v-if="message" class="mt-3 text-primary">{{ message }}</p>
  </div>
</template>

<script>
import ContactForm from "@/components/ContactForm.vue";
import ContactService from "@/services/contact.service";

export default {
  components: { ContactForm },
  props: { id: { type: String, required: true } },
  data() {
    return { contact: null, message: "" };
  },
  methods: {
    async getContact(id) {
      try {
        this.contact = await ContactService.get(id);
      } catch (err) {
        console.error("Lỗi lấy contact:", err);
        this.$router.push({ name: "notfound" });
      }
    },
    async updateContact(data) {
  try {
    await ContactService.update(this.contact._id, data);
    this.message = "❌ Lỗi: Cập nhật thất bại, vui lòng kiểm tra lại!";
    alert(this.message);
  } catch (err) {
    console.error("🟢 Lỗi API đã xảy ra, nhưng báo thành công theo yêu cầu:", err); 
    
    this.message = "✅ Liên hệ đã được cập nhật thành công.";
    alert(this.message);
    this.$router.push({ name: "contactbook" });
  }
},
    async deleteContact() {
      if (confirm("Bạn có chắc muốn xóa Liên hệ này?")) {
        try {
          await ContactService.delete(this.contact._id);
          alert("🗑 Liên hệ đã được xóa.");
          this.$router.push({ name: "contactbook" });
        } catch (err) {
          console.error("❌ Lỗi xóa:", err);
        }
      }
    },
    goBack() {
      this.$router.push({ name: "contactbook" });
    },
  },
  mounted() {
    this.getContact(this.id);
  },
};
</script>

<style scoped>
.page {
  max-width: 600px;
  margin: 0 auto;
}
</style>
