<script>
import Swal from "sweetalert2";
import appConfig from "../../../../../app.config.json";
import "@vueform/multiselect/themes/default.css";
import axios from 'axios';
import Lottie from "@/components/widgets/lottie.vue";

export default {
  page: {
    title: "Products",
    meta: [{
      name: "description",
      content: appConfig.description,
    },],
  },
  data() {
    return {
      title: "Products",
      searchQuery: null,
      productsData: [
        {
          id: 1,
          image: require("@/assets/images/products/img-1.png"),
          title: "Half Sleeve Round Neck T-Shirts",
          brand: "Banzaron",
          item: "Футболки-поло",
          size_code: "125748800",
          supplier_article: "ban-futb-poloбелый",
          wb_article: "74499707",
          color: "Белый",
          size: "48",
          barcode: "2028360865624",
          retail_price: "1870",
          purchase_price: "350",
          supplier: " ",
          remainder: " ",
        },
        {
          id: 2,
          image: require("@/assets/images/products/img-1.png"),
          title: "Half Sleeve Round Neck T-Shirts",
          brand: "Banzaron",
          item: "Футболки-поло",
          size_code: "125748801",
          supplier_article: "ban-futb-poloбелый",
          wb_article: "74499707",
          color: "Белый",
          size: "50",
          barcode: "2028360865631",
          retail_price: "1870",
          purchase_price: "350",
          supplier: " ",
          remainder: " ",
        },
        {
          id: 3,
          image: require("@/assets/images/products/img-1.png"),
          title: "Half Sleeve Round Neck T-Shirts",
          brand: "Banzaron",
          item: "Футболки-поло",
          size_code: "125748802",
          supplier_article: "ban-futb-poloбелый",
          wb_article: "74499707",
          color: "Белый",
          size: "52",
          barcode: "2028360865648",
          retail_price: "1870",
          purchase_price: "350",
          supplier: " ",
          remainder: " ",
        },
      ],
      pages: [],
      page: 1,
      perPage: 10,
      value: ["Watches", "Headset"],
    };
  },
  computed: {
    displayedPosts() {
      return this.paginate(this.productsData);
    },
    resultQuery() {
      if (this.searchQuery) {
        const search = this.searchQuery.toLowerCase();
        return this.displayedPosts.filter((data) => {
          return (
              data.title.toLowerCase().includes(search) ||
              data.brand.includes(search) ||
              data.item.includes(search) ||
              data.size_code.includes(search) ||
              data.supplier_article.includes(search) ||
              data.wb_article.includes(search) ||
              data.color.toLowerCase().includes(search) ||
              data.size.toLowerCase().includes(search) ||
              data.barcode.toLowerCase().includes(search) ||
              data.retail_price.toLowerCase().includes(search) ||
              data.purchase_price.toLowerCase().includes(search) ||
              data.supplier.toLowerCase().includes(search) ||
              data.remainder.toLowerCase().includes(search)
          );
        });
      } else {
        return this.displayedPosts;
      }
    },
  },
  watch: {
    productsData(newValue) {
      this.setPages(newValue);
    },
    resultQuery() {
      if (this.searchQuery) this.setPages(this.displayedPosts);
      else this.setPages(this.productsData);
    },
  },
  created() {
    this.setPages(this.productsData);

  },
  filters: {
    trimWords(value) {
      return value.split(" ").splice(0, 20).join(" ") + "...";
    },
  },
  beforeMount() {
    // axios.get('https://api-node.themesbrand.website/apps/product').then((data) => {
    //   this.productsData = [];
    //   const monthNames = ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep",
    //     "Oct", "Nov", "Dec"
    //   ];
    //   data.data.data.forEach(row => {
    //     var dd = new Date(row.publishedDate)
    //     row.publishedDate = dd.getDate() + " " + monthNames[dd.getMonth()] + ", " + dd.getFullYear();
    //     var hours = dd.getHours();
    //     var minutes = dd.getMinutes();
    //     var ampm = hours >= 12 ? 'PM' : 'AM';
    //     hours = hours % 12;
    //     hours = hours ? hours : 12; // the hour '0' should be '12'
    //     hours = hours < 10 ? '0' + hours : hours;
    //     minutes = minutes < 10 ? '0' + minutes : minutes;
    //     var strTime = hours + ':' + minutes + ' ' + ampm;
    //     row.publishedtime = strTime;
    //     row.image_src = 'https://api-node.themesbrand.website/fileupload/product_bucket/' + row.image;
    //     // row.image_src = `@/assets/images/products/img-8.png`;
    //     this.productsData.push(row);
    //   })
    // }).catch((er) => {
    //   console.log(er)
    // });

  },

  methods: {
    deleteMultiple() {
      let ids_array = [];
      var items = document.getElementsByName("chk_child");
      items.forEach(function (ele) {
        if (ele.checked == true) {
          var trNode = ele.parentNode.parentNode;
          var id = trNode.querySelector(".id a").innerHTML;
          ids_array.push(id);
        }
      });
      if (typeof ids_array !== "undefined" && ids_array.length > 0) {
        if (confirm("Are you sure you want to delete this?")) {
          var cusList = this.productsData;
          ids_array.forEach(function (id) {
            cusList = cusList.filter(function (orders) {
              return orders.id != id;
            });
          });
          this.productsData = cusList;
          document.getElementById("checkAll").checked = false;
          var itemss = document.getElementsByName("chk_child");
          itemss.forEach(function (ele) {
            if (ele.checked == true) {
              ele.checked = false
              ele.closest("tr").classList.remove("table-active");
              document.getElementById('selection-element').style.display = 'none'
            }
          });
        } else {
          return false;
        }
      } else {
        Swal.fire({
          title: "Please select at least one checkbox",
          confirmButtonClass: "btn btn-info",
          buttonsStyling: false,
          showCloseButton: true,
        });
      }
    },
    setPages(data) {
      this.pages = [];
      let numberOfPages = Math.ceil(data.length / this.perPage);
      this.pages = [];
      for (let index = 1; index <= numberOfPages; index++) {
        this.pages.push(index);
      }
    },
    deletedata(event) {
      Swal.fire({
        title: "Are you sure?",
        text: "You won't be able to revert this!",
        icon: "warning",
        showCancelButton: true,
        cancelButtonColor: "#f46a6a",
        confirmButtonColor: "#34c38f",
        confirmButtonText: "Yes, delete it!",
      }).then((result) => {
        if (result.value) {
          this.productsData.splice(this.productsData.indexOf(event), 1);
          axios.delete(`https://api-node.themesbrand.website/apps/product/${event._id}`)
              .then(() => {

              }).catch((er) => {
            console.log(er)
          });
          Swal.fire("Deleted!", "Your file has been deleted.", "success");
        }
      });

    },
    paginate(productsData) {
      let page = this.page;
      let perPage = this.perPage;
      let from = page * perPage - perPage;
      let to = page * perPage;
      return productsData.slice(from, to);
    },
  },
  mounted() {
    var total = 0;
    var checkchild = document.querySelectorAll('.form-check-all input[type="checkbox"]');
    checkchild.forEach(function (checkbox) {
      checkbox.onclick = function () {
        total = 0
        checkchild.forEach(function (box) {
          if (box.checked == true) {
            total++;
          }
        })
        if (checkbox.checked == true) {
          checkbox.closest("tr").classList.add("table-active");
          document.getElementById('selection-element').style.display = 'block'
          document.getElementById('select-content').innerHTML = total;
        } else {
          if (total <= 0) {
            document.getElementById('selection-element').style.display = 'none'
          }
          checkbox.closest("tr").classList.remove("table-active");
          document.getElementById('select-content').innerHTML = total;
        }
      }
    })
    var checkAll = document.getElementById("checkAll");
    if (checkAll) {
      checkAll.onclick = function () {
        var checkboxes = document.querySelectorAll(
            '.form-check-all input[type="checkbox"]'
        );
        if (checkAll.checked == true) {
          checkboxes.forEach(function (checkbox) {
            checkbox.checked = true;
            document.getElementById('selection-element').style.display = 'block'
            document.getElementById('select-content').innerHTML = checkboxes.length;
            checkbox.closest("tr").classList.add("table-active");
          });
        } else {
          checkboxes.forEach(function (checkbox) {
            checkbox.checked = false;
            document.getElementById('selection-element').style.display = 'none'
            checkbox.closest("tr").classList.remove("table-active");
          });
        }
      };
    }
  },

  components: {
    lottie: Lottie,
  },
};
</script>
<template>
  <div class="card-body">
    <div class="tab-content text-muted">
      <div class="tab-pane active" id="productnav-all" role="tabpanel">
        <div id="table-product-list-all" class="table-card gridjs-border-none table-responsive">
          <table class="table align-middle table-nowrap mb-0" id="customerTable">
            <thead class="table-light">
            <tr class="text-muted">
              <th scope="col" style="width: 50px">
                <input class="form-check-input" type="checkbox" id="checkAll" value="option"/>
              </th>
              <th>{{ $t('t-table-products-sort.title') }}</th>
              <th>{{ $t('t-table-products-sort.brand') }}</th>
              <th>{{ $t('t-table-products-sort.item') }}</th>
              <th>{{ $t('t-table-products-sort.size-code') }}</th>
              <th>{{ $t('t-table-products-sort.supplier-article') }}</th>
              <th>{{ $t('t-table-products-sort.wb-article') }}</th>
              <th>{{ $t('t-table-products-sort.color') }}</th>
              <th>{{ $t('t-table-products-sort.size') }}</th>
              <th>{{ $t('t-table-products-sort.barcode') }}</th>
              <th>{{ $t('t-table-products-sort.retail-price') }}</th>
              <th>{{ $t('t-table-products-sort.purchase-price') }}</th>
              <th>{{ $t('t-table-products-sort.supplier') }}</th>
              <th>{{ $t('t-table-products-sort.remainder') }}</th>
              <th scope="col">{{ $t('t-table-products-sort.action') }}</th>
            </tr>
            </thead>
            <tbody class="list form-check-all">
            <tr class="gridjs-tr" v-for="(data, index) of resultQuery" :key="index">
              <td data-column-id="productListAllCheckbox" class="gridjs-td">
                <input type="checkbox" name="chk_child" class="form-check-input"/>
              </td>
              <td class="id" hidden>
                <a href="">{{ data.id }}</a>
              </td>
              <td data-column-id="product" class="gridjs-td">
                            <span>
                              <div class="d-flex align-items-center">
                                <div class="flex-shrink-0 me-3">
                                  <div class="avatar-sm bg-light rounded p-1">
                                    <img :src="data.image" alt="" class="img-fluid d-block"/>
                                  </div>
                                </div>
                                <div class="flex-grow-1">
                                  <h5 class="fs-14 mb-1">
                                    <router-link to="/ecommerce/product-details" class="text-dark">{{ data.title }}
                                    </router-link>
                                  </h5>
                                </div>
                              </div>
                            </span>
              </td>
              <td data-column-id="brand" class="gridjs-td">{{ data.brand }}</td>
              <td data-column-id="item" class="gridjs-td">{{ data.item }}</td>
              <td data-column-id="size_code" class="gridjs-td">{{ data.size_code }}</td>
              <td data-column-id="supplier_article" class="gridjs-td">{{ data.supplier_article }}</td>
              <td data-column-id="wb_article" class="gridjs-td">{{ data.wb_article }}</td>
              <td data-column-id="color" class="gridjs-td">{{ data.color }}</td>
              <td data-column-id="size" class="gridjs-td">{{ data.size }}</td>
              <td data-column-id="barcode" class="gridjs-td">{{ data.barcode }}</td>
              <td data-column-id="retail_price" class="gridjs-td">{{ data.retail_price }}</td>
              <td data-column-id="purchase_price" class="gridjs-td">{{ data.purchase_price }}</td>
              <td data-column-id="supplier" class="gridjs-td">{{ data.supplier }}</td>
              <td data-column-id="remainder" class="gridjs-td">{{ data.remainder }}</td>
              <td data-column-id="action" class="gridjs-td">
                    <span>
                      <div class="dropdown">
                        <button class="btn btn-soft-secondary btn-sm dropdown" type="button"
                                data-bs-toggle="dropdown" aria-expanded="false">
                          <i class="ri-more-fill"></i>
                        </button>
                        <ul class="dropdown-menu dropdown-menu-end">
                          <li>
                            <router-link class="dropdown-item" to="/ecommerce/product-details"><i
                                class="ri-eye-fill align-bottom me-2 text-muted"></i>
                              {{ $t("t-action.view") }}</router-link>
                          </li>
                          <li>
                            <router-link class="dropdown-item" to="/edit-product"><i
                                class="ri-pencil-fill align-bottom me-2 text-muted"></i>
                              {{ $t("t-action.edit") }}</router-link>
                          </li>
                          <li class="dropdown-divider"></li>
                          <li>
                            <a class="dropdown-item" @click="deletedata(data)">
                              <i class="ri-delete-bin-fill align-bottom me-2 text-muted"></i>
                              {{ $t("t-action.delete") }}</a>
                          </li>
                        </ul>
                      </div>
                    </span>
              </td>
            </tr>
            </tbody>
          </table>
          <div class="d-flex justify-content-end m-3">
            <div class="pagination-wrap hstack gap-2">
              <a class="page-item pagination-prev disabled" href="#" v-if="page != 1" @click="page--">
                {{ $t('t-prev') }}
              </a>
              <ul class="pagination listjs-pagination mb-0">
                <li v-for="(pageNumber, index) in pages.slice(
                              page - 1,
                              page + 5
                            )" :key="index" @click="page = pageNumber" :class="{
                              active: pageNumber == page,
                              disabled: pageNumber == '...',
                            }">
                  <a class="page" href="#">{{ pageNumber }}</a>
                </li>
              </ul>
              <a class="page-item pagination-next" href="#" @click="page++" v-if="page < pages.length">
                {{ $t('t-next') }}
              </a>
            </div>
          </div>
        </div>
      </div>
      <!-- end tab pane -->
      <div id="removeItemModal" class="modal fade zoomIn" tabindex="-1" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered">
          <div class="modal-content">
            <div class="modal-header">
              <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"
                      id="btn-close"></button>
            </div>
            <div class="modal-body">
              <div class="mt-2 text-center">
                <lord-icon src="https://cdn.lordicon.com/gsqxdxog.json" trigger="loop"
                           colors="primary:#f7b84b,secondary:#f06548"
                           style="width:100px;height:100px"></lord-icon>
                <div class="mt-4 pt-2 fs-15 mx-4 mx-sm-5">
                  <h4>Are you Sure ?</h4>
                  <p class="text-muted mx-4 mb-0">Are you Sure You want to Remove this Product ?</p>
                </div>
              </div>
              <div class="d-flex gap-2 justify-content-center mt-4 mb-2">
                <button type="button" class="btn w-sm btn-light" data-bs-dismiss="modal"
                        id="btnNo">Close
                </button>
                <button type="button" class="btn w-sm btn-danger " id="delete-record">Yes, Delete It!</button>
              </div>
            </div>

          </div><!-- /.modal-content -->
        </div><!-- /.modal-dialog -->
      </div><!-- /.modal -->
      <div class="tab-pane" id="productnav-published" role="tabpanel">
        <div id="table-product-list-published" class="table-card gridjs-border-none"></div>
      </div>
      <!-- end tab pane -->

      <div class="tab-pane" id="productnav-draft" role="tabpanel">
        <div class="py-4 text-center">
          <div>
            <lottie class="avatar-xl" colors="primary:#121331,secondary:#08a88a" :options="defaultOptions"
                    :height="75" :width="75"/>
          </div>

          <div class="mt-4">
            <h5>Sorry! No Result Found</h5>
          </div>
        </div>
      </div>
      <!-- end tab pane -->
    </div>
    <!-- end tab content -->
  </div>
</template>