<template>
  <div class="container">
      <div style="display:flex; flex-wrap:wrap; box-shadow: 2px 2px 15px gray;">
          <div class="py-3" style="background-color:#4daf4e; width:100%;">
                <h3 class="my-auto" style="color:white;">
                    <i class="fa fa-shopping-basket fa-lg" style="color:white; font-size:30px;"></i>
                    Riwayat Transaksi
                </h3>
          </div>
          <div v-for="(transaction,index) in userTransaction" :key="transaction._id" style="width:100%; border-bottom:1px solid gray;">
              <!-- looping -->
              <div v-for="trans in userTransaction[index].products" :key="trans.productId" class="pt-5 px-3" style="display:flex; flex-wrap:wrap; border-bottom:1px solid #e1e1e1;">
                  <div style="width:30%">
                      <img class="zoom" :src="trans.image" alt="" style="width:80%; height:80%; object-fit:cover; border:1px solid #dedede;">
                  </div>
                  <div class="pl-5 mt-4" style="width:60%; text-align:left;">
                        <h2>{{trans.name}} ({{trans.quantity}})</h2>
                        <hr>
                        <h3>Rp. {{trans.price}}</h3>
                  </div>
              </div>
              <!-- total harga -->
              <div class="my-4 mx-4" style="display:flex; flex-wrap:wrap; border:2px dotted #e25822; background-color:#fff6db;">
                  <div class="px-5 py-2" style="width:100%; text-align:left; display:flex; align-items:center;">
                        <div style="width:70%;">
                            <h3>Total Transaksi</h3>
                            <p>
                                Rp. {{ userTransaction[index].totalPrice }}
                                <img src="https://firebasestorage.googleapis.com/v0/b/project-2059615900628317391.appspot.com/o/assets%2Fasset_icon_voucher_xxhdpi.png?alt=media&token=e65dd17a-900a-4aca-8675-94355f2c6b2b" alt="" style="width:30px;">
                            </p>
                        </div>
                        <div style="width:30%; text-align:right;">
                            <h3>
                                Status Pengiriman
                                <img src="https://dev-sayurbox.firebaseapp.com/assets/images/grab-shop-page-delivery.png" alt="" style="width:30px;">
                            </h3>
                            <p v-if="!userTransaction[index].deliverStatus">
                                dalam proses pengiriman
                            </p>
                            <p v-if="userTransaction[index].deliverStatus">
                                barang telah sampai
                            </p>
                        </div>

                  </div>
              </div>
          </div>
      </div>
  </div>
</template>

<script>
import { mapState, mapActions } from 'vuex'
export default {
  name: 'TransactionUser',
  computed: {
    ...mapState([
      'userTransaction'
    ])
  },
  methods: {
    ...mapActions([
      'getUserTrans'
    ])
  },
  created () {
    this.getUserTrans()
  }
}
</script>

<style scoped>
.zoom:hover {
    filter:drop-shadow(8px 8px 10px #28a745);
}
</style>
