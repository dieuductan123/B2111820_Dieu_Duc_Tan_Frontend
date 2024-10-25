<template>
  <div v-if="contact" class="page">
    <h4>Hiệu chỉnh Liên hệ</h4>
    <ContactForm :contact="contact" @submit:contact="updateContact" @delete:contact="deleteContact"/>
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
    props: {
      id: { type: String, required: true }, // Nhập ID từ route param
    },
    data() {
      return {
        contact: null, // Lưu thông tin liên hệ cần chỉnh sửa
        message: "", // Thông báo trạng thái
      };
    },
  methods: {
    async getContact(id) {
      try {
        this.contact = await ContactService.get(id);
      } catch (error) {
        console.log(error);
        // Chuyển sang trang NotFound đồng thời giữ cho URL không đổi
        this.$router.push({
          name: "notfound",
          params: {
            pathMatch: this.$route.path.split("/").slice(1)
          },
          query: this.$route.query,
          hash: this.$route.hash,
        });
      }
    },

    async updateContact(data) {
      try {
        await ContactService.update(this.contact._id, data);
        alert('Liên hệ được cập nhật thành công.');
        this.$router.push({ name: "contactbook" });
      } catch (error) {
        console.log(error);
      }
    },

    async deleteContact() {
      if (confirm("Bạn muốn xóa Liên hệ này?")) {
      try {
        await ContactService.delete(this.contact._id);
        this.$router.push({ name: "contactbook" });
      } catch (error) {
        console.log(error);
      }
      }
    },
  },
  created() {
    this.getContact(this.id);
    this.message = "";
  },
};
</script>