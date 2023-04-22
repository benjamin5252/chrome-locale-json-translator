<template>
  <header>

  </header>
  <div>
    <div class="flex w-full">
      <div class="left_panel h-[80vh] p-[20px] flex-1">

        <div class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">JSON Input:</div>
        <textarea v-model="inputText" id="message" rows="4"
          class=" overflow-scroll h-full resize-none  block p-2.5 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
          placeholder="Write your language in JSON here..."></textarea>
          <span class="p-[0px] m-[20px] " id="translate"></span>
      </div>
      <div class="right_panel p-[20px] flex-1">
        <div class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Output: </div>
        <div id="message" rows="4"
          class=" overflow-scroll h-full resize-none  block p-2.5 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
          placeholder="Write your language in JSON here...">
        <div>
          {
        </div>
        <div v-for="item in outputArray">

          <span>
            {{ item[0] + ':' }}
          </span>
          <span class="translate-words">
            {{ item[1]  }}
          </span>
          <span>
            ,
          </span>

        </div>
        <div>
          }

        </div>
      </div>
      </div>
    </div>


  </div>
</template>
<script setup>
const wordList = reactive([['table_pencil', 'Pencil'], ['table_hello', '你好']])
onMounted(() => {
  if(localStorage.getItem('inputText')){
    inputText.value = localStorage.getItem('inputText');
  }
  let recaptchaScript = document.createElement('script')
  recaptchaScript.setAttribute('src', 'js/translate.js')
  document.head.appendChild(recaptchaScript)

  setTimeout(() => {
    console.log(translate)
    translate.setUseVersion2(); //Set to use v2.x version
    //Set the local language (the language of the current page). If not set, the language of the text displayed on the current page will be automatically recognized by default. Can be filled in, such as 'english'、'chinese_simplified' , please refer to the description below the document.
    var documents = [];

    documents.push(...document.getElementsByClassName('translate-words'));
    translate.setDocuments(documents);
    translate.language.setLocal('english');
    translate.execute(); // Translate

    translate.listener.renderTaskFinish = function (task) {
      console.log('Execute once');
      console.log(document.getElementsByClassName('translate-words'))
      for (const item of document.getElementsByClassName('translate-words')) {
        console.log(item.innerText)
      }

    }
  }, 200)

})


const japanese = () => {
  translate.changeLanguage('japanese')
}

/* --------------------------- process input text --------------------------- */
const inputText = ref('')
watch(inputText, ()=>{
  localStorage.setItem("inputText", inputText.value);
  
})
const parseText = (str)=>{
  let strTemp = '';
  strTemp = str.replace('{', '').replace('}', '')
  const strArray =  strTemp.split(',')
  let output = []
  for(const subStr of strArray){
    output.push(subStr.split(':'))
    
  }
  return output
}
const outputArray = computed(()=>{
  return parseText(inputText.value)
})
</script>
<style lang="scss"></style>

