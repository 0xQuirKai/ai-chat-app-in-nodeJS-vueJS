<template>
<div id="app">
 <div class="chat_header">
  <h4>bot</h4>
 </div>
 <div id='azz' class="chat_body">
    <div class="messages" v-for="message in messages" :key="message.id">
     <div class="user messageRow " v-if="message.sender"><div class="message user">{{message.message}}</div> </div>
     <div class="bot messageRow " v-else>
     <div class="message bot">{{message.message}}</div> 
    </div>

    </div>
 </div>

 <div class="chat_footer">

  <form @submit.prevent="sendMessage()" id="createMessage">
      <img id="micro" src="../src/microphone.png" style='max-height:40px; margin-bottom: 10px;' alt="" srcset="">

   <input type="text" class="messageform" v-model="messagecontent">
  <input type="submit">

  </form>
 </div>
</div>
   
</template>

<script >
 import axios from 'axios';
 import {ref} from 'vue';


  
export default {


  name: 'App',

  setup(){
    
window.onload = ()=>{
  let micro = document.querySelector('#micro');
   micro.addEventListener('click',function start() {
    const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;

        const recognition = new SpeechRecognition();
            recognition.start();


recognition.addEventListener('result', function(event) {
                    messagecontent.value = event.results[0][0].transcript ;
           sendMessage()
});
        })
       
}
    const messages = ref([]);
    let messagecontent = ref('');
 


       
        
        
function sendMessage(){
if(messagecontent.value == ''){alert('type something please sir');return;}
createMessage(messagecontent.value);
getres(messagecontent.value);
messagecontent.value = '';
}

function createMessage(message,sender=true){


messages.value.unshift({
  sender : sender ,
  message : message
})

}


async function getres(message){
const postdata = {
  message : message
}
 const {data} =await axios.post('http://localhost:8000/chat',postdata);
const {response} = data;
createMessage(response,false)
}


return {messages,messagecontent,sendMessage}
  },
   
  

}
</script>

<style >
#app {
  background-color: #282c34;
  height: 600px;
  width: 40%;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  position: relative;
 
}
.chat_header {
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  color: white;
  margin-left: 5%;
  text-align: center;
  font-size: 20px;
  text-transform: uppercase;
}
.chat_footer {
  position: absolute;
  bottom: 0px;
  width: 100%;
}
.chat_footer form {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 95%;
  margin: 0 auto;
}
#azz {
  overflow: scroll;
  height: 83%;
 
}
#createMessage {
  width: 80%;
   display: flex;
  justify-content: baseline;
}
input:not(#createMessage) {
  background-color: green;
  border: 0;
  color: white;
  padding: 10px;
  margin-bottom: 12px;
  opacity: 0.8;
}
input:not(#createMessage):hover {
  opacity: 0.5;
}
.messageRow {
  display: flex;
  justify-content: flex-end;
}
.messageRow.bot {
  justify-content: flex-start;
}
.message p {
  color: white;
  padding: 0px 15px 0px 15px;
}
.message {
  border-radius: 50px;
  text-align: center;
  padding: 10px ;
  margin: 10px;
}
.messageRow.user .message {
  background-color: #1982fc;
}
.messageRow.bot .message {
  background-color: #43cc47;
}
.chat_body::-webkit-scrollbar {
  width: 0px;
  height: 100%;
}
</style>