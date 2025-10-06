<template>
  <Form @submit="submitContact" :validation-schema="contactFormSchema">
    <div class="form-group">
      <label for="name">Tên</label>
      <Field name="name" type="text" class="form-control" />
      <ErrorMessage name="name" class="error-feedback" />
    </div>

    <div class="form-group">
      <label for="email">E-mail</label>
      <Field name="email" type="email" class="form-control" />
      <ErrorMessage name="email" class="error-feedback" />
    </div>

    <!-- Nghề nghiệp -->
    <div class="form-group">
      <label>Nghề nghiệp</label>

      <div class="form-check" v-for="job in jobOptions" :key="job.value">
        <Field
          name="job"
          type="checkbox"
          class="form-check-input"
          :value="job.value"
          v-model="selectedJobs"
        />
        <label class="form-check-label">{{ job.label }}</label>
      </div>

      <!-- Checkbox Khác -->
      <div class="form-check">
        <Field
          name="job"
          type="checkbox"
          class="form-check-input"
          value="other"
          v-model="selectedJobs"
        />
        <label class="form-check-label">Khác</label>
      </div>

      <!-- Nếu chọn Khác thì hiện ô nhập -->
      <div v-if="selectedJobs.includes('other')" class="mt-2">
        <Field
          name="otherJob"
          type="text"
          class="form-control"
          placeholder="Nhập nghề nghiệp khác"
        />
        <ErrorMessage name="otherJob" class="error-feedback" />
      </div>

      <ErrorMessage name="job" class="error-feedback" />
    </div>
    <!-- End Nghề nghiệp -->

    <div class="form-group">
      <label for="address">Địa chỉ</label>
      <Field name="address" type="text" class="form-control" />
      <ErrorMessage name="address" class="error-feedback" />
    </div>

    <div class="form-group">
      <label for="phone">Điện thoại</label>
      <Field name="phone" type="tel" class="form-control" />
      <ErrorMessage name="phone" class="error-feedback" />
    </div>

    <div class="form-group form-check">
      <Field name="favorite" type="checkbox" class="form-check-input" />
      <label for="favorite" class="form-check-label">
        <strong>Liên hệ yêu thích</strong>
      </label>
    </div>

    <div class="form-group">
      <button class="btn btn-primary">
        <i class="fas fa-save"></i> Lưu
      </button>
      <button
        v-if="contact._id"
        type="button"
        class="ml-2 btn btn-danger"
        @click="deleteContact"
      >
        <i class="fas fa-trash"></i> Xóa
      </button>
    </div>
  </Form>
</template>

<script>
import * as yup from "yup";
import { Form, Field, ErrorMessage } from "vee-validate";

export default {
  components: { Form, Field, ErrorMessage },
  props: {
    contact: { type: Object, required: true },
  },
  emits: ["submit:contact", "delete:contact"],
  data() {
    const contactFormSchema = yup.object().shape({
      name: yup.string().required("Tên phải có giá trị.").min(2).max(50),
      email: yup.string().email("E-mail không đúng.").max(50),
      job: yup
        .array()
        .of(yup.string())
        .min(1, "Vui lòng chọn ít nhất một nghề nghiệp"),
      otherJob: yup.string().when("job", {
        is: (val) => val && val.includes("other"),
        then: (schema) =>
          schema.required("Vui lòng nhập nghề nghiệp khác").max(50),
        otherwise: (schema) => schema.notRequired(),
      }),
      address: yup.string().max(100),
      phone: yup
        .string()
        .matches(
          /((09|03|07|08|05)+([0-9]{8})\b)/g,
          "Số điện thoại không hợp lệ."
        ),
    });

    return {
      contactFormSchema,
      jobOptions: [
        { value: "Sinh viên", label: "Sinh viên" },
        { value: "Giao viên", label: "Giáo viên" },
        { value: "Kỹ sư", label: "Kỹ sư" },
        { value: "Bác sĩ", label: "Bác sĩ" },
      ],
      selectedJobs: [],
    };
  },
  methods: {
submitContact(values) {
  let jobs = values.job.filter((j) => j !== "other");

  if (values.job.includes("other") && values.otherJob) {
    jobs.push(values.otherJob);
  }
  jobs = jobs.map((v) => {
    const job = this.jobOptions.find((j) => j.value === v);
    return job ? job.label : v;
  });
  const jobString = jobs.join(", ");
  this.$emit("submit:contact", { 
    ...this.contact, 
    ...values, 
    job: jobString 
  });
},
    deleteContact() {
      this.$emit("delete:contact", this.contact._id);
    },
    Cancel() {
      if (window.confirm("Bạn có chắc muốn thoát mà không lưu?")) {
        this.$router.push({ name: "contactbook" });
      }
    },
  },
};
</script>

<style scoped>
@import "@/assets/form.css";
</style>