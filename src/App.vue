<template>
  <img alt="Vue logo" src="./assets/logo.png">


<head>
    <title>Canavar Avı</title>
    
    <link rel="stylesheet" href="foundation.min.css">
    <link rel="stylesheet" href="canavar.css">
</head>
<body>
<div id="app">
    <section class="row">
        <div class="small-6 columns">
            <h1 class="text-center">SEN</h1>
            <div class="healthbar">
                <div :style="{width:player_heal + '%'}" class="healthbar text-center" style="background-color: green; margin: 0; color: white;">
                   {{player_heal}}%
                </div>
            </div>
        </div>
        <div class="small-6 columns">
            <h1 class="text-center">CANAVAR</h1>
            <div class="healthbar">
                <div :style="{width:monster_heal + '%'}" class="healthbar text-center" style="background-color: green; margin: 0; color: white;">
                    {{monster_heal}}%
                </div>
            </div>
        </div>
    </section>
    <section class="row controls" v-if="!is_game_on">
        <div class="small-12 columns">
            <button id="start-game" @click="start_game">YENİ OYUN</button>
        </div>
    </section>

    <section class="row controls" v-if="is_game_on">
        <div class="small-12 columns">
            <button id="attack" @click="attack" >SALDIRI</button>
            <button id="special-attack" @click="special_attack">ÖZEL SALDIRI</button>
            <button id="monster-attack" @click="monster_attack">KARŞI SALDIRI</button>
            <button id="heal" @click="heal_up">İLK YARDIM</button>
            <button id="give-up" @click="give_up">PES ET!</button>
        </div>
    </section>
    <section class="row log" v-if="logs.length>0">
        <div class="small-12 columns">
            <ul>
                <li
                  :class="{'player-turn':log.turn == 'p','monster-turn':log.turn == 'm'}" 
                  v-for="log in logs" :key="log">{{log.text}}</li>
            </ul>
        </div>
    </section>
</div>

</body>

</template>






<script>

export default {

    data(){
      return{
        player_heal:100,
        monster_heal:100,
        is_game_on:false,
        logs:[]
      }
    },

    methods:{
        start_game:function(){
          this.is_game_on = true
        },
        attack:function(){
          var point =  Math.ceil(Math.random()*10)
          this.monster_heal -= point
          console.log(`M:${this.monster_heal}`)
          this.add_to_logs({turn:"p",text:"Oyuncu Saldırdı("+point+")" })
        },
        special_attack:function(){
          var point =  Math.ceil(Math.random()*22)
          this.monster_heal -= point
          console.log(`M:${this.monster_heal}`)
          this.add_to_logs({turn:"p",text:"Oyuncu Saldırı Kullandı("+point+")" })
        },
        monster_attack:function(){
            var point =  Math.ceil(Math.random()*15)
            this.player_heal -= point
            console.log(`P:${this.player_heal}`)
            this.add_to_logs({turn:"m",text:"Canavar Saldırdı("+point+")" })
          },
        heal_up:function(){
            var point =  Math.ceil(Math.random()*10)
            this.player_heal += point
            console.log(this.player_heal)
            this.add_to_logs({turn:"p",text:"Oyuncu Yardım Aldı("+point+")" })
        },
        give_up:function(){
            this.player_heal = 0
            this.add_to_logs({turn:"p",text:"Oyuncu Pes Etti" })
        },
        add_to_logs:function(log){
          this.logs.push(log)
        }
          
    },
    watch:{
        player_heal:function(value){
          if(value<=0){
            this.player_heal=0
            if(confirm("Oyunu Kaybettin.Tekrar oynamak ister misin?")){
              this.player_heal = 100
              this.monster_heal = 100
              this.log=[]
            }}
          else if(value>=100){this.player_heal=100}
        },
        monster_heal:function(value){
          if(value<=0){
            this.monster_heal=0
            if(confirm("Oyunu Kazandın.Tekrar oynamak ister misin?")){
              this.player_heal = 100
              this.monster_heal = 100
            }}
          else if(value>=100){this.monster_heal=100}
        }
    }

}

</script>




<style>
.text-center {
    text-align: center;
}

.healthbar {
    width: 100%;
    height: 40px;
    background-color: #eee;
    margin: auto;
    transition: width 500ms;
}

.controls, .log {
    margin-top: 30px;
    text-align: center;
    padding: 10px;
    border: 1px solid #ccc;
    box-shadow: 0px 3px 6px #ccc;
}

.turn {
    margin-top: 20px;
    margin-bottom: 20px;
    font-weight: bold;
    font-size: 22px;
}

.log ul {
    list-style: none;
    font-weight: bold;
    text-transform: uppercase;
}

.log ul li {
    margin: 5px;
}

.log ul .player-turn {
    color: blue;
    background-color: #e4e8ff;
}

.log ul .monster-turn {
    color: red;
    background-color: #ffc0c1;
}

button {
    font-size: 20px;
    background-color: #eee;
    padding: 12px;
    box-shadow: 0 1px 1px black;
    margin: 10px;
}

#start-game {
    background-color: #aaffb0;
}

#start-game:hover {
    background-color: #76ff7e;
}

#attack {
    background-color: #ff7367;
}

#attack:hover {
    background-color: #ff3f43;
}

#special-attack {
    background-color: #ffaf4f;
}

#special-attack:hover {
    background-color: #ff9a2b;
}

#monster-attack {
    background-color: #d64fff;
}

#monster-attack:hover {
    background-color: #6e2bff;
}

#heal {
    background-color: #aaffb0;
}

#heal:hover {
    background-color: #76ff7e;
}

#give-up {
    background-color: #ffffff;
}

#give-up:hover {
    background-color: #c7c7c7;
}


</style>
