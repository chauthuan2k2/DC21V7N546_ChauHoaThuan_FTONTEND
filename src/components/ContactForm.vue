<template>
<Form @submit="submitContact" :validation-schema="contactFormSchema">
  <div class="form-group">
    <label for="name">Tên</label>
    <Field name="name" type="text" class="form-control" :value="contact.name" />
    <ErrorMessage name="name" class="error-feedback" />
  </div>

  <div class="form-group">
    <label for="email">E-mail</label>
    <Field name="email" type="email" class="form-control" :value="contact.email" />
    <ErrorMessage name="email" class="error-feedback" />
  </div>

  <div class="form-group">
    <label for="job">Nghề nghiệp</label>
    <Field name="job" type="text" class="form-control" :value="contact.job" />
    <ErrorMessage name="job" class="error-feedback" />
  </div>

  <div class="form-group">
    <label for="address">Địa chỉ</label>
    <Field name="address" type="text" class="form-control" :value="contact.address" />
  </div>

  <div class="form-group">
    <label for="phone">Điện thoại</label>
    <Field name="phone" type="tel" class="form-control" :value="contact.phone" />
  </div>

  <div class="form-group form-check">
    <input type="checkbox" v-model="favorite" class="form-check-input" />
    <label class="form-check-label"><strong>Liên hệ yêu thích</strong></label>
  </div>

  <div class="form-group mt-3">
    <button type="submit" class="btn btn-primary">
      <i class="fas fa-save"></i> Lưu
    </button>
    <button
      v-if="contact._id"
      type="button"
      class="ml-2 btn btn-danger"
      @click="$emit('delete:contact', contact._id)"
    >
      <i class="fas fa-trash"></i> Xóa
    </button>
  </div>
</Form>
</template>

<script>
import { Form, Field, ErrorMessage } from "vee-validate";
import * as yup from "yup";

export default {
  components: { Form, Field, ErrorMessage },
  props: {
    contact: { type: Object, required: true },
  },
  emits: ["submit:contact", "delete:contact"],
  data() {
    const schema = yup.object().shape({
      name: yup.string().required("Tên phải có giá trị.").min(2).max(50),
      job: yup.string().max(100, "Nghề nghiệp không được vượt quá 100 ký tự."),
    });

    return {
      favorite: this.contact.favorite || false,
      contactFormSchema: schema,
    };
  },
  methods: {
    submitContact(values) {
      console.log("📤 Gửi dữ liệu form:", values);
      
      this.$emit("submit:contact", {
        ...this.contact,
        ...values,
        favorite: this.favorite,
      });
    },
  },
};
</script>

<style scoped>
@import "@/assets/form.css";
</style>