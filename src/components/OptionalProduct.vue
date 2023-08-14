<template>
  <section>
    <div class="h-screen flex justify-center items-center">
      <div class="md:w-3/4 md:h-2/3 flex flex-col md:flex-row border-2 border-dashed">
        <!-- left -->
        <div class="md:w-1/3 flex flex-col border-r-2 border-dashed">
          <!-- top -->
          <div 
            class="flex p-4 border-b-2 border-dashed"
          >
            <ColorButton 
              v-for="item in bagList.category" 
              :key="item.name"
              :text="item.text"
              :isChecked="item.name === bagType"
              class="mr-2"
              @handleClick="handleBagtype(item.name)"
            />
          </div>
          <!-- bottom -->
          <div class="md:grow">
            <ProductCard
              v-for="item in bagList[bagType]?.color"
              :key="`${bagType}-${item}`"
              :type="bagType"
              :color="item"
              class="m-2 lg:m-4"
              draggable
              @dragstart="startDrag($event, item)"
            />
          </div>
        </div>
        <!-- right -->
        <div 
          class="grow flex flex-col"
          @drop="onDrop($event)"
          @dragover.prevent
          @dragenter.prevent
        >
          <section class="p-4 w-full flex flex-col sm:flex-row items-center justify-between">
            <!-- information -->
            <div class="text-gray-500">拖曳左邊圖像至此區，完成客製化商品</div>
            <div 
              class="px-3 py-2 bg-rose-900 text-white rounded cursor-pointer"
              @click="clearShowArea"
            >
              <p>全部清除</p>
            </div> 
          </section>
          <!-- custom product -->
          <section class="grow relative flex justify-center items-center">
            <div :key="showAreaChanged" class="relative w-40 h-40">
              <div v-if="showArea.outer" :class="['absolute top-0 left-0']">
                <i class="fa-solid fa-10x fa-suitcase" :style="`color: ${showArea.outer};`"></i>
              </div>
              <div 
                v-if="showArea.inner" 
                :class="['absolute', hasShowAreaOnlyOne ? 'top-0 left-0' : 'top-8 left-12']">
                <i 
                  :class="['fa-solid fa-bag-shopping', hasShowAreaOnlyOne ? 'fa-10x' : 'fa-4x']" 
                  :style="`color: ${showArea.inner};`"
                >
                </i>
              </div>
              <div 
                v-if="showArea.packet" 
                :class="['absolute', hasShowAreaOnlyOne ? 'top-0 left-0' : 'top-28 left-16']"
              >
                <i 
                  :class="['fa-solid fa-wallet', hasShowAreaOnlyOne ? 'fa-10x' : 'fa-2x']" 
                  :style="`color: ${showArea.packet};`"
                >
                </i>
              </div>
            </div>
          </section>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import ColorButton from '@/components/common/ColorButton.vue'
import ProductCard from '@/components/ProductCard.vue'
export default {
  components: {
    ColorButton,
    ProductCard 
  },
  data() {
    return {
      drag: false,
      bagType: '',
      bagList: {
        'category': [
          { name: 'outer', text: '背包' },
          { name: 'inner', text: '小包' },
          { name: 'packet', text: '口袋包' }
        ],
        'outer': {
          color: ['#b3b3b3', '#000', '#658677', '#ECD4A7']
        },
        'inner': {
          color: ['#b3b3b3', '#000', '#658677', '#ECD4A7']
        },
        'packet': {
          color: ['#b3b3b3', '#000', '#D4971E', '#667EB3']
        }
      },
      showAreaChanged: 1,
      showArea: {
        outer: '',
        inner: '',
        packet: ''
      }
    }
  },
  mounted() {
    this.bagType = this.bagList.category[0]?.name ?? '';
  },
  computed: {
    currentBagType() {
      return this.bagList[this.bagType];
    },
    hasShowAreaOnlyOne() {
      const chooseList = Object.values(this.showArea).filter(item => item !== '');
      return chooseList.length <= 1;
    }
  },
  methods: {
    handleBagtype(name) {
      this.bagType = name;
    },
    onDrop(event) {
      const color = event.dataTransfer.getData('color');
      this.showArea[this.bagType] = color;
      this.showAreaChanged ++;
    },
    clearShowArea() {
      const typeList = ['outer', 'inner', 'packet'];
      typeList.map(item => {
        this.showArea[item] = '';
      })
    }
  }
}
</script>