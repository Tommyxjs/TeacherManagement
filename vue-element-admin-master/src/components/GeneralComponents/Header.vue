<template>
<div>
  <div  class="selfstyle">
      <ul style="position:absolute; width: 100%" :class="{'activeSwitch':isSwitchOne}" @click="isSwitchOne = !isSwitchOne">
       <div class="titlestyle"> <i class="el-icon-reading"></i>
         <i> Intelligent invigilator system</i> </div>
          <li class="changeSwitch">Home</li>
          <li style="width:16%" class="changeSwitch">Intelligent invigilator</li>
          <li class="changeSwitch">Institution</li>
          <li class="changeSwitch">About</li>
      </ul>


  </div>
</div>
</template>


<script>
  export default {
    name: 'HomeHeader',
    data: function() {
      return {
          isSwitchOne:true,

      }
    },
    methods: {

    }
  }
</script>
<style>
  li,ul,ol {
  	list-style:none;
  }
  .titlestyle{
    margin-left:15%;
    margin-top:1.5%;
    font-size: 28px;
    color: rgba(255, 255, 255, 1);
    text-shadow: 0 0 0.2px #fff,0 0 0.5px #fff,0 0 0.5px #fff;
    width: 25%;
  }
  /* 还原效果
   --------------------------------------------------*/
   .selfstyle {
   		 background: rgba(0, 0, 10, 0.55);
   }
   .selfstyle ul {
   	width:100%;
   	margin:0 auto;
 	  display:flex;
   }
   .changeSwitch {
   	position:relative;
   	/* 重点 */
   	width:12%;
    border-top:1px solid lemonchiffon;
    margin-top: 1%;
    font-family:"Apple LiSung Light";
    font-weight:800;
   	text-align:center;
   	line-height:70px;
   	font-size:20px;
   	color:#fff;
   	cursor:pointer;
   	/* transition:color .5s;
   	*/
   }

    .changeSwitch .activeSwitch {
   	position:relative;
   	/* 重点 */
   	width:12%;
    border-bottom:1px solid lemonchiffon;
    margin-top: 1%;
    font-family:"Apple LiSung Light";
    font-weight:800;
   	text-align:center;
   	line-height:70px;
   	font-size:20px;
   	color:#fff;
   	cursor:pointer;
   	/* transition:color .5s;
   	*/
   }

   .selfstyle ul li::after {
   	content:"";
   	position:absolute;
   	/* 重点 */
   	left:0;
   	/* 重点 */
   	bottom:0;
   	/* 重点 */
   	width:0;
   	/* 重点 */
   	height:2px;
   	background:#FFE300;
   	transition:width .5s;
   }
   .selfstyle ul li:hover::after {
   	width:100%
   }



</style>
