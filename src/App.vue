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
            <button class="btn btn-heal" @click="isGameRunning = !isGameRunning">START NEW GAME</button>
        </div>

        <div class="placeholders"  v-if="isGameRunning">
            <button class="btn btn-attack" @click="attackMonster">ATTACK</button>
            <button class="btn btn-special-attack" @click="specialAttackMonster">SPECIAL ATTACK</button>
            <button class="btn btn-heal" @click="heal">HEAL</button>
            <button class="btn" @click="giveUp">GIVE UP</button>
        </div>

        <div class="placeholders hide-content">
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
      userMessage: ''
    }
  },
   methods: {
      attackMonster() {
      let damage = this.calculateDamage(4, 20)
      this.monsterHealth -= damage 
      this.monsterAttack()

      this.turns.unshift({
        hit: 'User hit monster for '+damage
      })

      },
      specialAttackMonster() {
        let damage = this.calculateDamage(8, 15);
        this.monsterHealth -= damage;
        this.monsterAttack()
        this.turns.unshift({
          hit: 'User hit monster harder for '+damage
        })
        
      },
      heal() {
        let userHealth = this.userHealth;
        if(userHealth <= 10 || (userHealth >= 90 && userHealth <= 100)) {
          this.userMessage = 'Sorry, your energy level is too low/high to heal up'
          return
        }
        else{
          this.userMessage = ''
          let heal = this.calculateDamage(9, 15);
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
      
    }
}
</script>

