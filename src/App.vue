<template>
  <div id="app">
    
    <div class="container">
        <div class="user content">
            <h2>User</h2>
            <div class="content__shade">
                <div class="power-strength" :style="{ width: userHealth+'%' }">{{ userHealth }}</div>
            </div>
        </div>
        <div class="monster content">
            <h2>Monster</h2>
            <div class="content__shade">
                <div class="power-strength" :style="{ width: monsterHealth+'%'}">{{ monsterHealth }}</div>
            </div>
        </div>

        <div class="placeholders" v-if="!isGameRunning">
            <button class="btn btn-heal" @click="startGame">START NEW GAME</button>
        </div>

        <div class="placeholders"  v-if="isGameRunning">
            <button class="btn btn-attack" @click="attackMonster">ATTACK</button>
            <button class="btn btn-special-attack" @click="specialAttackMonster">SPECIAL ATTACK</button>
            <button class="btn btn-heal" @click="heal">HEAL</button>
            <button class="btn" @click="giveUp">GIVE UP</button>
        </div>

        <div class="placeholders hide-content" v-if="turns.length > 1 || userMessage">
            <h2 v-if="winStatus">{{ winMessage }}</h2>
            <h2>{{ userMessage }}</h2>
            <ul>
              <li v-for="(name, index) in turns" :key="index" :class="{evenClass: index % 2==0, oddClass: index % 2 != 0}">
                  <span :style="{ float: 'left', color: '#000'}">{{index+=1}}</span>{{ name.hit }}
              </li>
            </ul>
        </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'app',
  data() {
    return {
      isGameRunning: false,
      monsterHealth: 100,
      userHealth: 100,
      turns: [],
      userMessage: '',
      winStatus: false,
      winMessage: ''
    }
  },
   methods: {
      startGame() {
        this.isGameRunning = !this.isGameRunning,
        this.monsterHealth = 100
        this.userHealth = 100
        this.turns = [];
        this.winMessage = ''
      },
      attackMonster() {
      let damage = this.calculateDamage(3, 10)
      this.monsterHealth -= damage 
      this.monsterAttack()

      this.turns.unshift({
        hit: 'User hit monster for '+damage
      })

      this.userMessage = ''

      this.checkWin();

      },
      specialAttackMonster() {
        let damage = this.calculateDamage(8, 15);
        this.monsterHealth -= damage;
        this.monsterAttack()
        this.turns.unshift({
          hit: 'User hit monster harder for '+damage
        })

        this.userMessage = ''

        this.checkWin()
        
      },
      heal() {
        let userHealth = this.userHealth;
        if(userHealth <= 10 || (userHealth >= 90 && userHealth <= 100)) {
          this.userMessage = 'Sorry, your energy level is too low/high to heal up'
          return
        }
        else{
          this.userMessage = ''
          let heal = this.calculateDamage(5, 12);
          this.userHealth += heal;
          this.turns.unshift({
            hit: 'User heal for '+heal
          })
          this.monsterAttack();
        }
      },
      giveUp() {
        this.isGameRunning = false;
      },
      calculateDamage(min, max) {
        return Math.floor(Math.random() * (max - min)) + min
      },
      monsterAttack() {
        var monsterDamage = this.calculateDamage(5, 12)
        this.userHealth -= monsterDamage
        this.turns.unshift({
          hit: 'Monster hit user for '+monsterDamage
        })
      },
      checkWin() {
        if(this.userHealth <= 10){
          this.winMessage = 'Sorry, you just lost to the monster'
          this.winStatus = !this.winStatus;
          this.isGameRunning = false;
          return 
        } else {
          if(this.monsterHealth <= 10) {
            this.winMessage = 'Congratulations! You win.'
            this.winStatus = !this.winStatus;
            this.isGameRunning = false;
            return
          }
        }
      }
      
    }
}
</script>

