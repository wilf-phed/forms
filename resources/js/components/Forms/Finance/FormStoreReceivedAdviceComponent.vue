<template>


<div class="container">
    <div class="row justify-content-center" v-show="!componentLoaded">
        <div class="col-md-6 text-center" style="margin-top:10%">
            <rotate-square5 :size="'120px'"></rotate-square5>
        </div>
    </div>
    <div class="row justify-content-center" v-show="componentLoaded">
        <div class="col-md-10">

            <form method="post" class="bg-white px-30 elevation-4 mb-20" id="dlenhanceform" @submit.prevent="verifyDetails">

                <div class="row px-3">
                    <div class="col-md-12 text-center mt-1">
                        <h5 class="text-primary mt-1 mb-0">
                            <img :src="'../../images/PHEDLogo.jpg'" alt="phed logo"  style="width:100px;height:70px"/><br/>
                        </h5>
                        <p id="add">
                            Information Technology Department<br/>
                            Port-Harcourt Electricity Distribution Company<br/>
                            118 Ordinance Junction, Trans Amadi Layout.<br/>
                        </p>
                         <p class="text-primary"><strong>{{title}} FORM</strong></p>
                        <template v-if="valErrors.length > 0">
                            <ul class="mt-3 text-left">
                                <li class="text-danger" v-for="(err, index) in valErrors" :key="index">{{ err }}</li>
                            </ul>
                        </template>
                        <div class="alert alert-danger fade show" role="alert" v-show="formSetupMsg != ''">
                            <p class="bg-danger">
                                {{formSetupMsg}}<br/>
                            </p>
                          <!--  <button type="button" class="close" data-dismiss="alert" aria-label="Close">
                                <span aria-hidden="true">&times;</span>
                            </button> -->
                        </div>
                    </div>
                </div>
                <hr class="divider">
                <div class="row px-3">

                     <!-- Cost Centre -->
                    <div class="col-md-6">
                        <div class="form-group">
                            <label for="designation">Cost Center<span class="text-danger">*</span></label>
                            <input type="text" class="form-control form-control-sm" v-model="details.cost_center" required>
                        </div>
                    </div>

                     <!-- Reference -->
                    <div class="col-md-6">
                        <div class="form-group">
                            <label for="firstName">Reference to other S.R AS<span class="text-danger">*</span></label>
                            <input type="text" class="form-control form-control-sm" id="firstName" v-model="details.ref_to_others" required>
                        </div>
                    </div>

                    <!-- Local Purchase Order No -->
                    <div class="col-md-3">
                        <div class="form-group">
                            <label for="designation">Local Purchase Order No:<span class="text-danger">*</span></label>
                            <input type="text" class="form-control form-control-sm" v-model="details.local_purchase_no" required>
                        </div>
                    </div>

                    <!-- Indent No -->
                    <div class="col-md-3">
                        <div class="form-group">
                            <label for="designation">Indent No:<span class="text-danger">*</span></label>
                            <input type="text" class="form-control form-control-sm" v-model="details.indent_no" required>
                        </div>
                    </div>

                    <!-- Consignment Note No -->
                    <div class="col-md-3">
                        <div class="form-group">
                            <label for="designation">Consignment Note No:<span class="text-danger">*</span></label>
                            <input type="text" class="form-control form-control-sm" v-model="details.consignment_note_no" required>
                        </div>
                    </div>

                    <!-- Item No -->
                    <div class="col-md-3">
                        <div class="form-group">
                            <label for="designation">Item No<span class="text-danger">*</span></label>
                            <input type="text" class="form-control form-control-sm" v-model="details.item_no" required>
                        </div>
                    </div>

                    <!-- Supplier's Name -->
                    <div class="col-md-3">
                        <div class="form-group">
                            <label for="designation">Supplier's Name<span class="text-danger">*</span></label>
                            <input type="text" class="form-control form-control-sm" v-model="details.supplier_name" required>
                        </div>
                    </div>

                    <!-- Supplier's Invoice -->
                    <div class="col-md-3">
                        <div class="form-group">
                            <label for="designation">Supplier's Invoice No<span class="text-danger">*</span></label>
                            <input type="text" class="form-control form-control-sm" v-model="details.supplier_invoice_no" required>
                        </div>
                    </div>

                     <!-- Case Markings -->
                    <div class="col-md-3">
                        <div class="form-group">
                            <label for="designation">Case Markings<span class="text-danger">*</span></label>
                            <input type="text" class="form-control form-control-sm" v-model="details.case_markings" required>
                        </div>
                    </div>

                    <!-- Date Received -->
                    <div class="col-md-3">
                        <div class="form-group">
                            <label for="designation">Date Received<span class="text-danger">*</span></label>
                            <input type="date" class="form-control form-control-sm" v-model="details.date_recieved" required>
                        </div>
                    </div>
                </div>

            <hr class="my-20">

            <div>
                <div class="col-md-12">
                    <!-- dynamic portion -->
                        <div class="table-responsive">
                                <table class="table table-bordered" width="100%">
                                    <thead>
                                        <tr class="text-dark">
                                            <th width="10%"> vocab</th>
                                            <th width="30%">Description</th>
                                            <th width="12%">Qty</th>
                                            <th width="20%">Price</th>
                                            <th width="20%">Value</th>
                                            <th width="4%"></th>
                                        </tr>
                                    </thead>


                                    <tbody >
                                        <tr v-for="(input,k) in p" :key="k">
                                            <td><input type="text" class="form-control" v-model="input.vocab" required></td>

                                            <td>
                                                <textarea class="form-control" v-model="input.desc" required></textarea>
                                            </td>
                                            <td><input type="number" min="0" class="form-control" v-model="input.qty" required></td>
                                            <td><input type="number" min="0" class="form-control" v-model="input.price" required></td>
                                            <td><input type="number" min="0" class="form-control" v-model="input.value" required></td>

                                            <td>
                                                <span>
                                                    <button class="btn btn-danger btn-sm" v-show="k || ( !k && p.length > 1)" @click.prevent="remove(k)">
                                                        <i class="fa fa-minus"></i>
                                                    </button>
                                                    <button class="btn btn-success btn-sm" v-show="k == p.length-1" @click.prevent="add(k)">
                                                    <i class="fa fa-plus"></i>
                                                    </button>
                                                </span>
                                            </td>
                                        </tr>

                                    </tbody>
                                    <tfoot>

                                        <!-- <tr>
                                            <td colspan='7'>
                                                <button class="btn btn-sm btn-danger" @click.prevent="add(k)" >
                                                    <i class="fa fa-plus"></i>
                                                    Add Set Objective
                                                </button>
                                            </td>

                                        </tr> -->
                                    </tfoot>
                                </table>
                            </div>
                    <!-- dynamic portion -->
                </div>
            </div>

            <div class="row">
                <!-- Invoice value -->
                    <div class="col-md-4">
                        <div class="form-group">
                            <label for="firstName">Invoice Value<span class="text-danger">*</span></label>
                            <input type="number" class="form-control form-control-sm" v-model="details.invoice_value" required>
                        </div>
                    </div>

                    <!-- Inward Freight -->
                    <div class="col-md-4">
                        <div class="form-group">
                            <label for="firstName">Inward freight<span class="text-danger">*</span></label>
                            <input type="text" class="form-control form-control-sm" v-model="details.inward_infreight" required>
                        </div>
                    </div>

                    <!-- custom duty -->
                    <div class="col-md-4">
                        <div class="form-group">
                            <label for="firstName">Custom duty<span class="text-danger">*</span></label>
                            <input type="text" class="form-control form-control-sm" v-model="details.custom_duty" required>
                        </div>
                    </div>

                    <!-- VAT(5%)-->
                    <div class="col-md-4">
                        <div class="form-group">
                            <label for="firstName">VAT(5%)<span class="text-danger">*</span></label>
                            <input type="number" min="0" class="form-control form-control-sm" v-model="details.vat" required>
                        </div>
                    </div>


                    <!-- Total-->
                    <div class="col-md-6">
                        <div class="form-group">
                            <label for="firstName">Total<span class="text-danger">*</span></label>
                            <input type="number" min="0" class="form-control form-control-sm" v-model="details.total" required>
                        </div>
                    </div>

                    <!-- brief job description -->
                    <div class="col-md-6">
                        <div class="form-group">
                            <label for="officer_email">Remarks<span class="text-danger">*</span></label>
                            <textarea class="form-control form-control-sm" v-model="details.remarks" required></textarea>
                        </div>
                    </div>
                </div>

                <div class="row justify-content-center">
                    <button name="submit" class="btn btn-round btn-primary mb-10" v-show="formSetupStatus == 'setup_complete'">
                        submit
                    </button>
                </div>

            </form>
        </div>
    </div>



</div>

</template>



<script>
    import {RotateSquare5} from 'vue-loading-spinner'
    import Swal from 'sweetalert2'

    export default {
        created(){
            setTimeout(()=> {
                this.componentLoaded = true
            }, 2000)
        },
        mounted(){
            this.getFormSetupStatus()
        },
        components: {
            RotateSquare5
        },
        props: {
            email: {
                type: String,
                required: true
            },
            staff_id: {
                type: String,
                required: true
            },
            first_name: {
                type: String,
                required: true
            },
            last_name: {
                type: String,
                required: true
            },
            other_name: {
                type: String,
                required: true
            },
            form_type: {
                type: String,
                required: true
            },
            form_id: {
                type: String,
                required: true
            },

        },
        data() {
            return{
               p: [{
                vocab: "",
                desc: "",
                qty: 0,
                price: 0,
                value: 0
                }],
                valErrors: [],
                isLoading: false,
                componentLoaded: false,
                formSetupMsg: '',
                formSetupStatus: '',
                remarks: '',
                details: {},
                modalState: '',
                type: '',
                title: 'STORE RECEIVED ADVICE'
            }
        },
        methods: {
          add(index) {

             this.p.push(
                 {
                  vocab: "",
                  desc: "",
                  qty: 0,
                  price: 0,
                  value: 0
                  }
             )
          },
          remove(index) {
              this.p.splice(index, 1)
          },
          openModal(state){
              this.modalState = state
              $('#terms').modal('show');
          },
          initiateSubmit(){
              //this.loadingAlert()
              $('#dlenhanceform').on('submit')
          },
          verifyDetails(){


              let t = "<p>"
              t +=  "Action cannot be reversed after submission<br/>"
              t +=  "</p> "
              Swal.fire({
                  title: 'Please verify you entered all the information correctly',
                  html: t,
                  icon: 'warning',
                  showCancelButton: true,
                  confirmButtonColor: '#3085d6',
                  cancelButtonColor: '#d33',
                  confirmButtonText: 'Yes, Continue!'
                  }).then((result) => {
                  if (result.value) {
                      this.submit()
                  }
              })
          },
          submit(){

                console.log(this.p)
                console.log(this.details)

                this.details.name = this.first_name + " " + this.last_name + " " + this.other_name;
                this.details.staff_id = this.staff_id
                this.details.form_type = this.form_type
                this.details.form_id = this.form_id
                this.details.stock = this.p

                //this.isLoading = true
                //this.loadingAlert()

                        //this.details.other_name = this.other_name;
                axios.post('../finance_form_request', this.details).then((response)=> {

                      if(response.status == 200 || response.status == 201){
                              console.log(response.data)
                              /*if(response.data.hasOwnProperty('errors')){
                                  this.valErrors = response.data.errors;
                                  //this.isLoading = false;
                                  this.status = "";
                                  setTimeout(()=>{
                                      this.errorAlert("It seems you didn't complete the fields correctly.", "Scroll to top of form for more details")
                                  }, 2000)

                                  //Swal.close();
                              }else{
                                  setTimeout(()=>{
                                      this.successAlert("Request submitted successfully", "Redirecting...please wait")
                                  }, 1000)
                                  setTimeout(()=>{
                                          //window.location.href = "home"
                                      location.reload()
                                  }, 3000)


                              } */
                      }
                      else{
                          this.isLoading = false;
                          this.status = "";
                          this.errorAlert('Something went wrong! We couldn\'t submit your details','<a>please refresh or try again later</a>');
                      }
                }).catch((error)=> {
                    this.isLoading = false;
                    this.status = "";
                    this.errorAlert('Something went wrong! Error connecting to the server');
                })
            },
            getFormSetupStatus(){
                console.log("Get current request");
                axios.get("../get_form_setup_status?form_id="+this.form_id).then( response => {

                      console.log(response.data);
                      this.formSetupStatus = response.data;
                      if(response.data == 'setup_incomplete'){
                          this.formSetupMsg = "This form has not been completely setup. Please contact the Admin for more details";
                      }
                }).catch(error => {
                    //this.infoAlert("Something went wrong", "couldn't retrieve current requests")
                })
            },
          loadingAlert(){
              Swal.fire({
                  html: '<i class="fa fa-circle-o-notch fa-spin text-primary fa-5x"></i>',
                  allowOutsideClick: false,
                  showConfirmButton: false,
                  footer: 'processing request...please wait'
              })
          },
          errorAlert(text, footer){
              Swal.fire({
                  icon: 'error',
                  position: 'center',
                  title: 'Oops...',
                  text: text,
                  showConfirmButton: true,
                  footer: footer
              })
          },
          successAlert(text, footer){
              Swal.fire({
                  position: 'center',
                  icon: 'success',
                  title: "Congrats...",
                  text: text,
                  showConfirmButton: false,
                  allowOutsideClick: false,
                  footer: footer
              })
          },
          infoAlert(text, footer){
                Swal.fire({
                position: 'center',
                icon: 'info',
                title: "Ooop..",
                text: text,
                showConfirmButton: false,
                allowOutsideClick: false,
                footer: footer
            })
          }
        },

        watch: {

          p: {

              // This will let Vue know to look inside the array
              deep: true,

              // We have to move our method to a handler field
              handler(){
                //  console.log('The list of colours has changed!');
                  this.p.forEach((item) => {
                    console.log(Number(item.qty) * Number(item.price))
                    item.value = Number(item.qty) * Number(item.price)
                  })
              }
          }
        }
    }
</script>

<style scoped>
    #add{
        font-size: 12px;
        line-height: 1.2;
    }
    .des{
        font-size: 12px;
        line-height: 1.4;
        margin-bottom: 1px;
    }
    hr{
        margin-top: 2px;
        margin-bottom: 2px;
    }
</style>
