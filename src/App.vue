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

        <div class="placeholders">
            <h2>Winner Message</h2>
            <ul>
              <li v-for="(name, index) in turns" :key="index" :class="{evenClass: index % 2==0, oddClass: index % 2 != 0}">{{ name.hit }}</li>
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
          hit: 'user hit monster harder for '+damage
        })
        
      },
      heal() {

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
          hit: 'monster hit user for '+monsterDamage
        })

        

      }
    }
}
</script>

