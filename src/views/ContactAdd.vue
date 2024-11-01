<template>
  <div class="page">
    <h4>Thêm mới Liên hệ</h4>
    <ContactForm :contact="contact" @submit:contact="addContact" />
    <p>{{ message }}</p>
  </div>
</template>

<script>
import ContactForm from "@/components/ContactForm.vue";
import ContactService from "@/services/contact.service";

export default {
  components: {
    ContactForm,
  },
  data() {
    return {
      contact: {
        name: "",
        emil: "",
        address: "",
        phone: "",
        favorite: false,
      },
    };
  },
  methods: {
    async addContact(data) {
      try {
        // Gọi dịch vụ để thêm mới liên hệ
        await ContactService.create(data);
        alert("Liên hệ được thêm thành công.");
        this.$router.push({ name: "contactbook" }); // Điều hướng về trang danh bạ sau khi thêm
      } catch (error) {
        console.log("Lỗi khi thêm liên hệ:", error);
      }
    },
  },
  created() {
    this.message = "";
  },
};
</script>
