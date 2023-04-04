v-model을 이용한 퍼센테이지 계산기
<template>
      <div class="title text-center">
        <img src="./assets/logo_v2.png" alt="logo" class="w-[150px] lg:w-[180px] bg-contain m-auto mt-5">
        <h1 class="font-extrabold text-2xl mt-1 lg:mt-4 lg:text-3xl darkMode dark:text-white">Calculator</h1>
        <p class="mt-1 mb-10 text-sm lg:text-base darkMode dark:text-white">퍼센트 계산기</p>
      </div>
      <!-- 전체 영역 -->
      <!--  

        왼쪽 영역

      -->
      <div class="w-full lg:flex dark:transition-all">
        <div class="leftArea lg:w-2/4 lg:border-r-2 lg:border-dotted lg:h-[600px]">
          <!--
            1. 전체값에서 일정 비율에 해당하는 값을 계산
          -->
          <div class="pb-10 text-center sm:border-t-2 sm:border-dotted lg:border-none">
            <h3 class="mb-5 font-semibold text-lg text-center sm:mt-4 lg:text-2xl lg:mb-10 darkMode dark:text-white">
              <span class="hidden lg:inline-block">&bull;</span>&nbsp;퍼센트 비율값 계산&nbsp;<span class="hidden lg:inline-block">&bull;</span>
            </h3>
        
            <div class="inline lg:block relative">
              <input @input="bindNumber" @change="UpdateCalc" v-model="calc1" type="text" placeholder="전체값 100" class="border border-dashed rounded p-2 text-left text-xs w-full h-8 sm:w-[40%] sm:h-10 sm:text-sm sm:mx-2 lg:text-base darkMode dark:bg-[#555555] dark:caret-[#eee] dark:text-white">
              <span class="hidden lg:inline-block lg:absolute lg:top-2 darkMode dark:text-white">의</span>
            </div>
            
            <div class="inline lg:block relative">
              <input v-model="calc2" type="text" placeholder="비율값 20" class="border border-dashed rounded p-2 text-left text-xs mt-4 w-full h-8 sm:w-[40%] sm:h-10 sm:text-sm sm:mx-2 lg:text-base lg:my-5 dark:bg-[#555] dark:caret-[#eee] dark:text-white">
              <span class="hidden lg:inline-block lg:absolute lg:top-7 darkMode dark:text-white">%는</span>
            </div>
            
            <div class="inline lg:block relative">
              <input :value="ResultCalcA" readonly type="text" placeholder="일부값 20" class="border border-gray-300 rounded p-2 text-left text-xs mt-4 w-full h-8 sm:w-[40%] sm:h-10 sm:text-sm lg:text-base lg:mb-10 lg:mt-0 dark:bg-[#555555] cursor-not-allowed dark:text-white">
              <span class="hidden lg:inline-block mx-2 lg:absolute lg:top-2 darkMode dark:text-white">입니다.</span>
            </div>
          </div>
          <!--
          2. 전체값의 일정 값에 해당하는 비율을 계산
          -->
          <div class="pb-10 text-center sm:border-t-2 sm:border-dotted lg:border-none">
            <h3 class="mb-5 font-semibold text-lg text-center sm:mt-4 lg:text-2xl lg:mb-10 darkMode dark:text-white"><span class="hidden lg:inline-block">&bull;</span>&nbsp;일정값 비율 계산&nbsp;<span class="hidden lg:inline-block">&bull;</span></h3>
        
            <div class="inline lg:block relative">
              <input v-model="calc3" type="text" placeholder="전체값 100" class="border border-dashed rounded p-2 text-left text-xs w-full h-8 sm:w-[40%] sm:h-10 sm:text-sm sm:mx-2 lg:text-base dark:bg-[#555555] dark:caret-[#eee] dark:text-white">
              <span class="hidden lg:inline-block lg:absolute lg:top-2 darkMode dark:text-white">의</span>
            </div>
        
            <div class="inline lg:block relative">
              <input v-model="calc4" type="text" placeholder="일부값 20" class="border border-dashed rounded p-2 text-left text-xs mt-4 w-full h-8 sm:w-[40%] sm:h-10 sm:text-sm sm:mx-2 lg:text-base lg:my-5 dark:bg-[#555555] dark:caret-[#eee] dark:text-white">
              <span class="hidden lg:inline-block lg:absolute lg:top-7 darkMode dark:text-white">은&#40;는&#41;</span>
            </div>
        
            <div class="inline lg:block relative">
              <input :value="ResultCalcB" readonly type="text" placeholder="비율값 20" class="border border-gray-300 rounded p-2 text-left text-xs mt-4 mb-2 w-full h-8 sm:w-[40%] sm:h-10 sm:text-sm lg:text-base lg:mt-0 dark:bg-[#555555] cursor-not-allowed dark:text-white">
              <span class="hidden lg:inline-block mx-2 lg:absolute lg:top-2 darkMode dark:text-white">입니다.</span>
              <span disabled class="mx-2 text-xs text-gray-400 lg:absolute lg:top-3 lg:right-6 xl:absolute xl:right-12 2xl:right-36">소수점 자리</span>
              <select v-model="decimalPoint" class="text-xs rounded border lg:absolute lg:top-3 lg:right-0 xl:absolute xl:right-6 2xl:right-[115px] dark:bg-[#bdbdbd]">
                <option v-for="e in 5" :key="e" :value="e">{{ e }}</option>
              </select>
            </div>
          </div>
      </div>
      <!--  

        오른쪽 영역

      -->
        <div class="rightArea lg:w-2/4">
          <!--
            3. 기준값이 변경값으로 변화시, 얼만큼 증가/감소 했는지 계산
          -->
          <div class="pb-10 text-center sm:border-t-2 sm:border-dotted lg:border-none">
            <h3 class="mb-5 font-semibold text-lg text-center sm:mt-4 lg:text-2xl lg:mb-10 darkMode dark:text-white"><span class="hidden lg:inline-block">&bull;</span>&nbsp; 기준 > 변경값 비율 계산 &nbsp;<span class="hidden lg:inline-block">&bull;</span></h3>
            
            <div class="inline lg:block relative">
              <input v-model="calc5" type="text" placeholder="기준값 100" class="border border-dashed rounded p-2 text-left text-xs w-full h-8 sm:w-[40%] sm:h-10 sm:text-sm sm:mx-2 lg:text-base dark:bg-[#555555] dark:caret-[#eee] dark:text-white">
              <span class="hidden lg:inline-block lg:absolute lg:top-2 darkMode dark:text-white">이&#40;가&#41;</span>
            </div>
            
            <div class="inline lg:block relative">
              <input v-model="calc6" type="text" placeholder="변경값 150" class="border border-dashed rounded p-2 text-left text-xs mt-4 w-full h-8 sm:w-[40%] sm:h-10 sm:text-sm sm:mx-2 lg:text-base lg:my-5 dark:bg-[#555555] dark:caret-[#eee] dark:text-white">
              <span class="hidden lg:inline-block lg:absolute lg:top-7 darkMode dark:text-white">으&#40;로&#41; 바뀌면 </span>
            </div>
            
            <div class="inline lg:block relative">
              <input  v-model="ResultCalcC" readonly type="text" placeholder="증가값 50" class="border border-gray-300 rounded p-2 text-left text-xs mt-4 w-full h-8 sm:w-[40%] sm:h-10 sm:text-sm lg:text-base lg:mb-10 lg:mt-0 dark:bg-[#555555] cursor-not-allowed dark:text-white">
              <span class="hidden lg:inline-block mx-2 lg:absolute lg:top-2 darkMode dark:text-white">입니다.</span>
            </div>
          </div>
          <!--
            4. 기준값에서 일정 비율로 증가/감소한 결과를 계산
          -->
          <div class=" pb-10 text-center sm:border-t-2 sm:border-dotted lg:border-none">
            <h3 class="mb-5 font-semibold text-lg text-center sm:mt-4 lg:text-2xl lg:mb-10 darkMode dark:text-white"><span class="hidden lg:inline-block">&bull;</span>&nbsp; 기준 > 변경값 비율 증가 / 감소 계산 &nbsp;<span class="hidden lg:inline-block">&bull;</span></h3>
            
            <div class="inline lg:block relative">
              <input v-model="calc7" type="text" placeholder="기준값 100" class="border border-dashed rounded p-2 text-left text-xs w-full h-8 sm:w-[40%] sm:h-10 sm:text-sm sm:mx-2 lg:text-base dark:bg-[#555555] dark:caret-[#eee] dark:text-white">
              <span class="hidden lg:inline-block lg:absolute lg:top-2 darkMode dark:text-white">이&#40;가&#41;</span>
            </div>
            
            <div class="inline lg:block relative">
              <input v-model="calc8" type="text" placeholder="비율값 20" class="border border-dashed rounded p-2 text-left text-xs mt-4 w-full h-8 sm:w-[40%] sm:h-10 sm:text-sm sm:mx-2 lg:text-base lg:my-5 dark:bg-[#555555] dark:caret-[#eee] dark:text-white">
              <span class="hidden lg:inline-block lg:absolute lg:top-7 darkMode dark:text-white">라면</span>
            </div>
            
            <div class="inline lg:block relative">
              <input v-model="ResultCalcD" readonly type="text" placeholder="증가값 120" class="border border-gray-300 rounded p-2 text-left text-xs mt-4 w-[50%] h-8 sm:w-[40%] sm:h-10 sm:text-sm sm:mx-2 lg:text-base lg:w-[30%] lg:mt-0 dark:bg-[#555555] cursor-not-allowed dark:text-white">
              
              <input v-model="ResultCalcE" readonly type="text" placeholder="감소값 80" class="border border-gray-300 rounded p-2 text-left text-xs mt-4 w-[50%] h-8 sm:w-[40%] sm:h-10 sm:text-sm sm:mx-2 lg:text-base lg:w-[30%] lg:mt-0 dark:bg-[#555555] cursor-not-allowed dark:text-white">
              <span class="hidden lg:inline-block mx-1 lg:absolute lg:top-2 darkMode dark:text-white">입니다.</span>
            </div>
          </div>
        </div>
      </div>
    <p class="upDate text-center text-xs darkMode dark:text-white">23.03.22</p>
    <NavMenu :isDark="isDark" @dark="toggleDark()" />
</template>

<script>
import NavMenu from './components/Nav.vue'
import {useDark, useToggle} from '@vueuse/core'

export default {
  name: 'App',
  components:{
    NavMenu
  },
  data() {
    return {
      calc1: '',
      calc2: '',
      calc3: '',
      calc4: '',
      calc5: '',
      calc6: '',
      calc7: '',
      calc8: '',
      decimalPoint: '2',
      isDark: useDark(),
      toggleDark: useToggle(useDark()),
    }
  },
  methods: {
    UpdateCalc(){
      console.log("11")
    },
    bindNumber(e){
      this.calc1 = e.target.value;
    },
    DarkMode(){
      document.querySelector("html").classList.toggle("dark")
    },
  },
  computed: {
    ResultCalcA(){
      return this.calc2 === '' || this.calc1 === ''? '' :(Number(this.calc1) *Number((this.calc2/100))).toFixed(3)
    },
    ResultCalcB(){
      let b = ((100 * Number(this.calc4)) / Number(this.calc3)).toFixed(this.sosu)
      return this.calc4 === '' || this.calc3 === '' ? '' : b
    },
    ResultCalcC(){
      let a = (this.calc6 - this.calc5) / this.calc5 * 100;
      return this.calc6 === '' || this.calc5 === '' ? '' : a > 0 ? a.toFixed(0)+"% 증가" : a.toFixed(0)+"% 감소"
    },
    ResultCalcD(){
      return this.calc7 === '' || this.calc8 === '' ? '' : (Number(this.calc7) +  Number(this.calc7) * Number(this.calc8 / 100)).toFixed(0)
    },
    ResultCalcE(){
      return this.calc7 === '' || this.calc8 === '' ? '' : (Number(this.calc7) -  Number(this.calc7) * Number(this.calc8 / 100)).toFixed(0)
    },
  },
  // 역할을 감시해줌 / (/[^0-9]/g, '') 
  watch:{
    calc1: function(){
      return this.calc1 = this.calc1.replace(/[^0-9]/g, '');
    },
    calc2: function(){
      return this.calc2 = this.calc2.replace(/[^0-9]/g, '');

    },
    calc3: function(){
      return this.calc3 = this.calc3.replace(/[^0-9]/g, '');

    },
    calc4: function(){
      return this.calc4 = this.calc4.replace(/[^0-9]/g, '');

    },
    calc5: function(){
      return this.calc5 = this.calc5.replace(/[^0-9]/g, '');

    },
    calc6: function(){
      return this.calc6 = this.calc6.replace(/[^0-9]/g, '');

    },
    calc7: function(){
      return this.calc7 = this.calc7.replace(/[^0-9]/g, '');

    },
    calc8: function(){
      return this.calc8 = this.calc8.replace(/[^0-9]/g, '');

    },
  },
}
</script>

<style>
  *{
    font-family: 'PyeongChangPeace-Light';
  }
</style>
