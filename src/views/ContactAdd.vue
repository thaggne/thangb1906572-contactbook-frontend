<template>
  <div class="page">
    <h4>Thêm liên hệ mới</h4>
    <Form @submit="getContactNew" :validation-schema="contactFormSchema">
      <div class="form-group">
        <label for="name">Tên</label>
        <Field
          name="name"
          type="text"
          class="form-control"
          v-model="contactnew.name"
        />
        <ErrorMessage name="name" class="error-feedback" />
      </div>
      <div class="form-group">
        <label for="email">E-mail</label>
        <Field
          name="email"
          type="email"
          class="form-control"
          v-model="contactnew.email"
        />
        <ErrorMessage name="email" class="error-feedback" />
      </div>
      <div class="form-group">
        <label for="address">Địa chỉ</label>
        <Field
          name="address"
          type="text"
          class="form-control"
          v-model="contactnew.address"
        />
        <ErrorMessage name="address" class="error-feedback" />
      </div>
      <div class="form-group">
        <label for="phone">Điện thoại</label>
        <Field
          name="phone"
          type="tel"
          class="form-control"
          v-model="contactnew.phone"
        />
        <ErrorMessage name="phone" class="error-feedback" />
      </div>
      <div class="form-group form-check">
        <input
          name="favorite"
          type="checkbox"
          class="form-check-input"
          v-model="contactnew.favorite"
        />
        <label for="favorite" class="form-check-label">
          <strong>Liên hệ yêu thích</strong>
        </label>
      </div>
      <div class="form-group">
        <button class="btn btn-primary">
          <i class="fa-solid fa-floppy-disk"></i>Lưu
        </button>
      </div>
    </Form>
  </div>
</template>
<script>
import * as yup from "yup";
import { Form, Field, ErrorMessage } from "vee-validate";
import ContactService from "@/services/contact.service";
export default {
  components: {
    Form,
    Field,
    ErrorMessage,
  },
  data() {
    const contactFormSchema = yup.object().shape({
      name: yup
        .string()
        .required("Tên phải có giá trị.")
        .min(2, "Tên phải ít nhất 2 ký tự.")
        .max(50, "Tên có nhiều nhất 50 ký tự."),
      email: yup
        .string()
        .required("Email phải có giá trị.")
        .email("E-mail không đúng.")
        .max(50, "E-mail tối đa 50 ký tự."),
      address: yup
        .string()
        .max(100, "Địa chỉ tối đa 100 ký tự.")
        .required("Địa chỉ phải có giá trị."),
      phone: yup
        .string()
        .required("Số điện thoại phải có giá trị.")
        .matches(
          /((09|03|07|08|05)+([0-9]{8})\b)/g,
          "Số điện thoại không hợp lệ."
        ),
    });
    return {
      contactFormSchema,
      contactnew: {
        name: "",
        phone: "",
        address: "",
        favorite: false,
      },
    };
  },
  methods: {
    getContactNew() {
      if (confirm("Bạn có chắc chắn thêm !")) {
        this.$router.push({ name: "ContactBook" });
        return ContactService.create(this.contactnew);
      }
    },
  },
};
</script>
