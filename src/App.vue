<script>
import ImageCarousel from "./components/Carousel/ImageCarousel.vue";
import { imgList } from "./assets/imgList";

export default {
  components: {
    ImageCarousel,
  },
  data() {
    return {
      imgList,
      isShowButtonMore: false,
    };
  },
};
</script>

<template>
  <div>
    <div class="flex flex-col lg:flex-row bg-white rounded-xl p-8 overflow-hidden">
      <div class="flex flex-col gap-8">
        <div>
          <span>Nama Supplier</span>
          <p class="text-2xl font-semibold">
            Guangdong Xuping Jewelry Co., Ltd.
          </p>
        </div>
        <div>
          <span>Alamat Supplier</span>
          <p class="w-[450px] text-base font-semibold">
            Nanyu Building Material Plaza,No.138 Tanwei Rd., Liwan
            District,Guangzhou,China
          </p>
        </div>
        <div>
          <span>Kategori Produk</span>
          <p class="text-sm font-semibold">Accessories</p>
        </div>
        <div>
          <span>Lama Beroperasi</span>
          <p class="text-sm font-semibold">12 apa anj</p>
        </div>
        <div>
          <span>Rating di 1688</span>
          <p class="font-semibold text-2xl">4.9</p>
        </div>
      </div>
      <div class="w-full">
        <ImageCarousel :files="imgList" />
      </div>
    </div>
    <div v-if="isShowButtonMore" class="w-full flex justify-center mt-2">
      <button
        class="bg-gray-200 text-sm py-3 px-6 rounded-lg text-gray-500 hover:bg-gray-300 capitalize"
        @click="onShowButtonMore()"
      >
        Tampilkan Lainnya
      </button>
    </div>
  </div>
</template>
