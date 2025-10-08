<template>
  <div class="card shadow-sm mb-3">
    <div class="card-body">
      <h5 class="card-title">{{ contact.name }}</h5>
      <p class="card-text"><strong>Email:</strong> {{ contact.email }}</p>
      <p class="card-text"><strong>Điện thoại:</strong> {{ contact.phone }}</p>
      <p class="card-text"><strong>Địa chỉ:</strong> {{ contact.address }}</p>
      <p class="card-text"><strong>Nghề nghiệp:</strong> {{ contact.job || "Chưa có" }}</p>
      <p v-if="contact.favorite" class="text-warning fw-bold">★ Liên hệ yêu thích</p>

    </div>
  </div>
</template>

<script>
export default {
  props: { contact: { type: Object, required: true } },
};
</script>
