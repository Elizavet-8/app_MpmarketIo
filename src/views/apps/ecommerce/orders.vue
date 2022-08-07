<script>
import Multiselect from "@vueform/multiselect";
import "@vueform/multiselect/themes/default.css";
// import flatPickr from "vue-flatpickr-component";
import "flatpickr/dist/flatpickr.css";
import Swal from "sweetalert2";

import Layout from "../../../layouts/main.vue";
import appConfig from "../../../../app.config";
// import PageHeader from "@/components/page-header";
import axios from 'axios';
// import animationData from "@/components/widgets/msoeawqm.json";
// import Lottie from "@/components/widgets/lottie.vue";
import OrdersTable from "@/views/apps/ecommerce/orders-table.vue";

export default {
  page: {
    title: "Orders",
    meta: [{
      name: "description",
      content: appConfig.description,
    },],
  },
  data() {
    return {
      title: "Orders",
      page: 1,
      perPage: 8,
      pages: [],
      value: null,
      statuscategory: 'Все',
      value1: null,
      searchQuery: null,
      config: {
        wrap: true, // set wrap to true only when using 'input-group'
        altFormat: "M j, Y",
        altInput: true,
        dateFormat: "d M, Y",
        mode: "range",
      },
      timeConfig: {
        enableTime: false,
        dateFormat: "d M, Y",
      },
      date: null,
      date2: null,
      orders: [
        {
          id: 1,
          orderId: "#MP2101",
          order_id: "360160139",
          cabinet: "Деграунд WB",
          img: "https://picsum.photos/200",
          product_name: "Футболка",
          color: "Белый",
          size: "M",
          barcode: "8903338567483",
          amount: "990",
          warehouse: "Склад Поставщик 287289",
          created: "12.07.2022 16:17",
          deliver_during: "48 ч.",
          time_since_order: "1 ч. 40 мин.",
          status: "Отменить"
        },
        {
          id: 2,
          orderId: "#MP2101",
          order_id: "360160143",
          cabinet: "Tekkon WB",
          img: "https://picsum.photos/200",
          product_name: "Костюм спортивный",
          color: "Оранжевый",
          size: "XL",
          barcode: "8903338567481",
          amount: "3990",
          warehouse: "Склад Поставщик 287289",
          created: "12.07.2022 16:17",
          deliver_during: "52 ч.",
          time_since_order: "5 ч. 40 мин.",
          status: "Отменить"
        },
        {
          id: 3,
          orderId: "#MP2101",
          order_id: "360160140",
          cabinet: "Деграунд WB",
          img: "https://picsum.photos/200",
          product_name: "Рубашка",
          color: "Черный",
          size: "44",
          barcode: "8903338567414",
          amount: "1560",
          warehouse: "Склад Поставщик 287289",
          created: "12.07.2022 16:17",
          deliver_during: "49 ч.",
          time_since_order: "2 ч. 40 мин.",
          status: "Отменить"
        }
      ],
      isStatus: null,
      isPayment: null,
    };
  },
  components: {
    Layout,
    // PageHeader,
    // lottie: Lottie,
    Multiselect,
    // flatPickr,
    OrdersTable
  },
  computed: {
    displayedPosts() {
      return this.paginate(this.orders);
    },
    resultQuery() {
      if (this.searchQuery) {
        const search = this.searchQuery.toLowerCase();
        return this.displayedPosts.filter((data) => {
          return (
              data.orderId.toLowerCase().includes(search) ||
              data.cabinet.toLowerCase().includes(search) ||
              data.photo.toLowerCase().includes(search) ||
              data.product_name.toLowerCase().includes(search) ||
              data.color.toLowerCase().includes(search) ||
              data.size.toLowerCase().includes(search) ||
              data.barcode.toLowerCase().includes(search) ||
              data.amount.toLowerCase().includes(search) ||
              data.warehouse.toLowerCase().includes(search) ||
              data.created.toLowerCase().includes(search) ||
              data.deliver_during.toLowerCase().includes(search) ||
              data.time_since_order.toLowerCase().includes(search) ||
              data.status.toLowerCase().includes(search)
          );
        });
      } else if (this.date !== null) {
        if (this.date !== null) {
          var date1 = this.date.split(" to ")[0];
          var date2 = this.date.split(" to ")[1];
        }
        return this.displayedPosts.filter((data) => {
          if (
              new Date(data.orderDate.slice(0, 12)) >= new Date(date1) &&
              new Date(data.orderDate.slice(0, 12)) <= new Date(date2)
          ) {
            return data;
          } else {
            return null;
          }
        });
      } else if (this.value !== null) {
        return this.displayedPosts.filter((data) => {
          if (data.status === this.value) {
            return data;
          } else {
            return null;
          }
        });
      } else if (this.value1 !== null) {
        return this.displayedPosts.filter((data) => {
          if (data.payment === this.value1) {
            return data;
          } else {
            return null;
          }
        });
      } else if (this.date !== null && this.value !== null && this.value1 !== null) {
        return this.displayedPosts.filter((data) => {
          if (
              new Date(data.orderDate.slice(0, 12)) >= new Date(date1) &&
              new Date(data.orderDate.slice(0, 12)) <= new Date(date2) &&
              data.status === this.value &&
              data.payment === this.value1
          ) {
            return data;
          } else {
            return null;
          }
        });
      } else {
        return this.displayedPosts;
      }
    },
  },
  watch: {},
  created() {
    this.setPages();
  },
  filters: {
    trimWords(value) {
      return value.split(" ").splice(0, 20).join(" ") + "...";
    },
  },
  beforeMount() {
    /*axios.get('https://api-node.themesbrand.website/apps/order').then((data) => {
      const monthNames = ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep",
        "Oct", "Nov", "Dec"
      ];
      this.orders = [];
      data.data.data.forEach((row) => {
        var dd = new Date(row.orderDate)
        row.orderDate = dd.getDate() + " " + monthNames[dd.getMonth()] + ", " + dd.getFullYear();
        this.orders.push(row)
      })
    }).catch((er) => {
      console.log(er)
    });*/

  },

  mounted() {
    var checkAll = document.getElementById("checkAll");
    if (checkAll) {
      checkAll.onclick = function () {
        var checkboxes = document.querySelectorAll(
            '.form-check-all input[type="checkbox"]'
        );
        if (checkAll.checked == true) {
          checkboxes.forEach(function (checkbox) {
            checkbox.checked = true;
            checkbox.closest("tr").classList.add("table-active");
          });
        } else {
          checkboxes.forEach(function (checkbox) {
            checkbox.checked = false;
            checkbox.closest("tr").classList.remove("table-active");
          });
        }
      };
    }

    // eslint-disable-next-line no-self-assign
    this.orders = this.orders;
  },
  methods: {
    onChangeStatus(e) {
      this.isStatus = e;
    },
    onChangePayment(e) {
      this.isPayment = e;
    },
    setPages() {
      let numberOfPages = Math.ceil(this.orders.length / this.perPage);
      for (let index = 1; index <= numberOfPages; index++) {
        this.pages.push(index);
      }
    },
    paginate(orders) {
      let page = this.page;
      let perPage = this.perPage;
      let from = page * perPage - perPage;
      let to = page * perPage;
      return orders.slice(from, to);
    },
    editdata(data) {
      let result = this.orders.findIndex(o => o.orderId == data.orderId)
      document.getElementById('edtorderId').value = this.orders[result]._id;
      document.getElementById('edtcustomername').value = this.orders[result].customer;
      document.getElementById('edtproductname').value = this.orders[result].product;
      document.getElementById('edtorderdate').value = this.orders[result].orderDate;
      document.getElementById('edtamount').value = this.orders[result].amount;
      document.getElementById('edtpayment').value = this.orders[result].payment;
      document.getElementById('edtdelivered').value = this.orders[result].status;
    },
    updateorder() {
      let result = this.orders.findIndex(o => o._id == document.getElementById('edtorderId').value)
      this.orders[result].customer = document.getElementById('edtcustomername').value;
      this.orders[result].product = document.getElementById('edtproductname').value;
      this.orders[result].orderDate = document.getElementById('edtorderdate').value;
      this.orders[result].amount = document.getElementById('edtamount').value;
      this.orders[result].payment = document.getElementById('edtpayment').value;
      this.orders[result].status = document.getElementById('edtdelivered').value;

      if (this.orders[result].status == 'Ожидает') {
        this.orders[result].statusClass = 'warning'
      } else if (this.orders[result].status == 'На сборке') {
        this.orders[result].statusClass = 'secondary'
      } else if (this.orders[result].status == 'Архив') {
        this.orders[result].statusClass = 'danger'
      } else if (this.orders[result].status == 'Отменено') {
        this.orders[result].statusClass = 'info'
      } else if (this.orders[result].status == 'Собрано') {
        this.orders[result].statusClass = 'success'
      } else {
        this.orders[result].statusClass = 'danger'
      }
      document.getElementById('edtclosemodal').click();
      axios.patch(`https://api-node.themesbrand.website/apps/order/${document.getElementById('edtorderId').value}`, this.orders[
          result])
          .then(() => {

          }).catch((er) => {
        console.log(er)
      });

    },
    deletedata(event) {
      Swal.fire({
        title: "Вы уверены?",
        text: "Вы не сможете отменить!",
        icon: "warning",
        showCancelButton: true,
        cancelButtonColor: "#f46a6a",
        confirmButtonColor: "#34c38f",
        confirmButtonText: "Да, удалить!",
        cancelButtonText: "Отмена"
      }).then((result) => {
        if (result.value) {
          this.orders.splice(this.orders.indexOf(event), 1);
          axios.delete(`https://api-node.themesbrand.website/apps/order/${event._id}`)
              .then(() => {

              }).catch((er) => {
            console.log(er)
          });
          Swal.fire("Удалено!", "Запись была удалена.", "success");
        }
      });
    },
    deleteMultiple() {
      let ids_array = [];
      var items = document.getElementsByName("chk_child");
      items.forEach(function (ele) {
        if (ele.checked == true) {
          var trNode = ele.parentNode.parentNode.parentNode;
          var id = trNode.querySelector(".id a").innerHTML;
          ids_array.push(id);
        }
      });
      if (typeof ids_array !== "undefined" && ids_array.length > 0) {
        if (confirm("Вы действительно хотите удалить?")) {
          var cusList = this.orders;
          ids_array.forEach(function (id) {
            cusList = cusList.filter(function (orders) {
              return orders.orderId != id;
            });
          });
          this.orders = cusList;
          document.getElementById("checkAll").checked = false;
          var itemss = document.getElementsByName("chk_child");
          itemss.forEach(function (ele) {
            if (ele.checked == true) {
              ele.checked = false
              ele.closest("tr").classList.remove("table-active");
            }
          });
        } else {
          return false;
        }
      } else {
        Swal.fire({
          title: "Пожалуйста, выберите хотя бы один флажок",
          confirmButtonClass: "btn btn-info",
          buttonsStyling: false,
          showCloseButton: true,
        });
      }
    },
    addorder() {
      var orderId = document.getElementById('orderId').value;
      var customername = document.getElementById('customername').value;
      var productname = document.getElementById('productname').value;
      var orderdate = document.getElementById('orderdate').value;
      var amount = document.getElementById('amount').value;
      var payment = document.getElementById('payment').value;
      var delivered = document.getElementById('delivered').value;
      var statuscolor;
      if (delivered == 'Ожидает') {
        statuscolor = 'warning'
      } else if (delivered == 'На сборке') {
        statuscolor = 'secondary'
      } else if (delivered == 'Архив') {
        statuscolor = 'danger'
      } else if (delivered == 'Отменено') {
        statuscolor = 'info'
      } else if (delivered == 'Собрано') {
        statuscolor = 'success'
      } else {
        statuscolor = 'danger'
      }
      if (orderId != null && customername != null && productname != null && orderdate != null && amount != null &&
          payment != null && delivered != null) {
        var data = {
          id: 'x',
          orderId: "#MP2" + orderId,
          customer: customername,
          product: productname,
          orderDate: orderdate,
          amount: amount,
          payment: payment,
          status: delivered,
          statusClass: statuscolor,
        };
        this.orders.push(data)
        axios.post(`https://api-node.themesbrand.website/apps/order`, data)
            .then(() => {

            }).catch((er) => {
          console.log(er)
        });
      }
      document.getElementById('closemodal').click();
      document.getElementById("addform").reset();
    },
    SearchData() {
      this.resultQuery;
      // var isstatus = document.getElementById("idStatus").value;
      //   var payment = document.getElementById("idPayment").value;
    },
    changecategory(data) {
      this.statuscategory = data;
    }
  },
};
</script>

<template>
  <Layout>
    <!--    <PageHeader :title="title" :items="items"/>-->
    <div class="row">
      <div class="col-lg-12">
        <div class="card" id="orderList">
          <div class="card-header border-0">
            <div class="d-flex align-items-center">
              <h5 class="card-title mb-0 flex-grow-1">{{ $t('t-order-history') }}</h5>
              <div class="flex-shrink-0">
                <button v-if="false" class="btn btn-soft-danger me-1" @click="deleteMultiple">
                  <i class="ri-delete-bin-2-line"></i>
                </button>
                <button type="button" class="btn btn-success add-btn" data-bs-toggle="modal" id="create-btn"
                        data-bs-target="#showModal">
                  <i class="ri-add-line align-bottom me-1"></i> {{ $t('t-order-assembly') }}
                </button>
                <button type="button" class="btn btn-danger ms-1">
                  <i class="ri-delete-bin-2-line align-bottom me-1"></i> {{ $t('t-order-cancel') }}
                </button>
                <button v-if="false" type="button" class="btn btn-info ms-1">
                  <i class="ri-file-download-line align-bottom me-1"></i> Import
                </button>
              </div>
            </div>
          </div>
          <div class="card-body border border-dashed border-end-0 border-start-0">
            <form>
              <div class="row g-3">
                <div class="col-xxl-6 col-sm-6">
                  <div class="search-box">
                    <input type="text" class="form-control search" placeholder="Поиск..." v-model="searchQuery"/>
                    <!--Search for order ID, customer, order status or something!-->
                    <i class="ri-search-line search-icon"></i>
                  </div>
                </div>
                <!--end col-->
                <!--                <div class="col-xxl-2 col-sm-6">-->
                <!--                  <div>-->
                <!--                    <flat-pickr placeholder="Select date" v-model="date" :config="config"-->
                <!--                                class="form-control flatpickr-input" id="demo-datepicker"></flat-pickr>-->
                <!--                  </div>-->
                <!--                </div>-->
                <div class="col-xxl-2 col-sm-2">
                  <div>
                    <input type="date"
                           v-model="date" :config="config"
                           class="form-control"
                           id="exampleInputdate">
                  </div>
                </div>
                <!--end col-->
                <div class="col-xxl-2 col-sm-2">
                  <div>
                    <Multiselect class="form-control" v-model="value" :close-on-select="true" :searchable="true"
                                 :create-option="true" @input="onChangePayment" :options="[
                        { value: 'Все', label: 'Все' },
                        { value: 'На сборке', label: 'На сборке' },
                        { value: 'Собрано', label: 'Собрано' },
                        { value: 'Архив', label: 'Архив' },
                      ]"/>
                  </div>
                </div>
                <!--end col-->
                <div class="col-xxl-2 col-sm-4" v-if="false">
                  <div>
                    <Multiselect class="form-control" v-model="value1" :close-on-select="true" :searchable="true"
                                 :create-option="true" @input="onChangeStatus" :options="[
                        { value: '', label: 'Выбрать карту' },
                        { value: 'Все', label: 'Все' },
                        { value: 'Mastercard', label: 'Mastercard' },
                        { value: 'Paypal', label: 'Paypal' },
                        { value: 'Visa', label: 'Visa' },
                        { value: 'COD', label: 'COD' },
                      ]"/>
                  </div>
                </div>
                <!--end col-->
                <div class="col-xxl-2 col-sm-2">
                  <div>
                    <button type="button" class="btn btn-primary w-100" @click="SearchData">
                      <i class="ri-equalizer-fill me-1 align-bottom"></i>
                      {{ $t('t-filter') }}
                    </button>
                  </div>
                </div>
                <!--end col-->
              </div>
              <!--end row-->
            </form>
          </div>
          <OrdersTable></OrdersTable>
        </div>
      </div>
      <!--end col-->
    </div>
    <!--end row-->
  </Layout>
</template>
