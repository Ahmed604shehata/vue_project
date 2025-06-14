<template>
  <div style="padding: 0 170px">
    <a-row :gutter="[50, 0]">
      <a-col
        :xl="{ span: 12 }"
        :lg="{ span: 12 }"
        :md="{ span: 12 }"
        :xs="{ span: 24 }"
      >
        <h3 class="main-heading">Billing Details</h3>
        <a-form :form="form">
          <a-row :gutter="[15, 0]">
            <a-col :span="24">
              <a-form-item label="Country">
                <a-select
                  size="large"
                  v-decorator="[
                    'country',
                    {
                      rules: [
                        {
                          required: true,
                          message: 'Please select your Country!',
                        },
                      ],
                    },
                  ]"
                  placeholder="Select Country"
                >
                  <a-select-option value="Pakistan">Pakistan</a-select-option>
                  <a-select-option value="India">India</a-select-option>
                </a-select>
              </a-form-item>
            </a-col>
            <a-col :span="24">
              <a-form-item label="Email">
                <a-input
                  size="large"
                  placeholder="Email"
                  v-decorator="[
                    'email',
                    {
                      rules: [
                        { required: true, message: 'Please input your Email!' },
                        { type: 'email', message: 'Please enter a valid Email!' },
                      ],
                    },
                  ]"
                ></a-input>
              </a-form-item>
            </a-col>
            <a-col :span="12">
              <a-form-item label="First Name">
                <a-input
                  size="large"
                  placeholder="First Name"
                  v-decorator="[
                    'first_name',
                    {
                      rules: [
                        {
                          required: true,
                          message: 'Please input your First Name!',
                        },
                      ],
                    },
                  ]"
                />
              </a-form-item>
            </a-col>
            <a-col :span="12">
              <a-form-item label="Last Name">
                <a-input
                  size="large"
                  placeholder="Last Name"
                  v-decorator="[
                    'last_name',
                    {
                      rules: [
                        {
                          required: true,
                          message: 'Please input your Last Name!',
                        },
                      ],
                    },
                  ]"
                />
              </a-form-item>
            </a-col>
            <a-col :span="24">
              <a-form-item label="Address">
                <a-input
                  size="large"
                  placeholder="Address"
                  v-decorator="[
                    'address',
                    {
                      rules: [
                        {
                          required: true,
                          message: 'Please input your Address!',
                        },
                      ],
                    },
                  ]"
                ></a-input>
              </a-form-item>
            </a-col>
            <a-col :span="12">
              <a-form-item label="City">
                <a-input
                  size="large"
                  placeholder="City"
                  v-decorator="[
                    'city',
                    {
                      rules: [
                        {
                          required: true,
                          message: 'Please input your City!',
                        },
                      ],
                    },
                  ]"
                />
              </a-form-item>
            </a-col>
            <a-col :span="12">
              <a-form-item label="State">
                <a-input
                  size="large"
                  placeholder="State"
                  v-decorator="[
                    'state',
                    {
                      rules: [
                        {
                          required: true,
                          message: 'Please input your State!',
                        },
                      ],
                    },
                  ]"
                />
              </a-form-item>
            </a-col>
            <a-col :span="12">
              <a-form-item label="Zip Code">
                <a-input
                  size="large"
                  placeholder="Zip Code"
                  v-decorator="[
                    'zip',
                    {
                      rules: [
                        {
                          required: true,
                          message: 'Please input your Zip Code!',
                        },
                      ],
                    },
                  ]"
                />
              </a-form-item>
            </a-col>
            <a-col :span="12">
              <a-form-item label="Phone">
                <a-input
                  size="large"
                  placeholder="Phone"
                  v-decorator="[
                    'phone',
                    {
                      rules: [
                        {
                          required: true,
                          message: 'Please input your Phone number!',
                        },
                      ],
                    },
                  ]"
                />
              </a-form-item>
            </a-col>
            <a-col :span="24">
              <a-form-item label="Order Notes">
                <a-textarea
                  placeholder="Order Notes"
                  :rows="4"
                  v-decorator="['order_notes']"
                ></a-textarea>
              </a-form-item>
            </a-col>
          </a-row>
        </a-form>
      </a-col>

      <a-col
        :xl="{ span: 12 }"
        :lg="{ span: 12 }"
        :md="{ span: 12 }"
        :xs="{ span: 24 }"
      >
        <div>
          <h3 class="main-heading">Your Order</h3>
          <div class="order-box">
            <a-table
              :columns="columns"
              :data-source="getBillBreakDown"
              :scroll="{ y: 200, x: 200 }"
            >
            </a-table>
            <a-alert :message="'Total Bill: ' + getTotalBill" type="success" />
            <div style="padding: 5px"></div>
            <a-collapse accordion>
              <a-collapse-panel
                key="1"
                header="Direct Bank Transfer"
                :show-arrow="false"
              >
                <p>
                  Make your payment directly into our bank account. Please use
                  your Order ID as the payment reference. Your order won’t be
                  shipped until the funds have cleared in our account.
                </p>
              </a-collapse-panel>
              <a-collapse-panel key="2" header="PayPal" :show-arrow="false">
                <p>
                  Pay via PayPal; you can pay with your credit card if you don’t
                  have a PayPal account.
                </p>
              </a-collapse-panel>
            </a-collapse>
            <a-button
              type="primary"
              block
              :disabled="cart.length === 0"
              style="margin-top: 20px"
              @click="handleSubmit"
            >
              Place Order
            </a-button>
          </div>
        </div>
      </a-col>
    </a-row>
  </div>
</template>

<script>
import { mapState, mapGetters, mapMutations } from "vuex";

const columns = [
  {
    title: "Title",
    dataIndex: "title",
    key: "title",
    width: 120,
  },
  {
    title: "Count",
    dataIndex: "value",
    key: "value",
    width: 80,
  },
  {
    title: "Total/Product",
    dataIndex: "total",
    key: "total",
    width: 80,
    ellipsis: true,
  },
];

export default {
  data() {
    return {
      modalMessage: "",
      columns,
      formLayout: "horizontal",
      form: this.$form.createForm(this, { name: "coordinated" }),
      visible: false,
    };
  },
  computed: {
    ...mapState(["cart"]),
    ...mapGetters(["getTotalBill", "getBillBreakDown"]),
  },
  methods: {
    ...mapMutations(["clearCart"]),
    handleSubmit(e) {
      e.preventDefault();
      this.form.validateFields((err, values) => {
        if (!err) {
          this.modalMessage = `Hi ${
            values?.first_name || ""
          }, your order is placed at the address ${
            values.address
          } with a total bill of ${this.getTotalBill}`;
          this.show();
        }
      });
    },
    show() {
      this.$success({
        title: "Order placed!",
        content: this.modalMessage,
      });
      console.log("Clearing cart");
      this.clearCart();
    },
  },
};
</script>

<style scoped>
.main-heading {
  font-size: 26px;
  font-weight: 600;
  margin: 20px 0;
  padding: 0 0 10px 0;
  text-transform: uppercase;
  border-bottom: 2px solid #eaedef;
  text-align: left;
}
.ant-form {
  text-align: left;
}
.ant-form-item {
  margin-bottom: 12px;
}
.order-box {
  margin-top: 20px;
  padding: 30px 20px;
  border: 2px solid #eaedef;
}
</style>