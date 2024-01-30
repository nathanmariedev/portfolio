<template>
  <div style="overflow-x: hidden; height: 100px; width: 100%;">
      <GameHero :hero="heroState" :style="{ position: 'fixed', left: heroState.px + 'px', bottom: '0px' }" @heroMoved="handleHeroMove" />
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      heroState: {
        name: 'Nathan',
        width: 10,
        height: 100,
        px: 0,
        py: 0,
        vx: 0,
        vy: 0,
        ax: 0,
        ay: 0,
        speed: 1,
        jump: 5,
        isJumping: false,
        gold: 0,
        items: [],
      },
    };
  },
  methods: {
    
    handleHeroMove(hero) {
      this.heroState = hero;
      // eslint-disable-next-line no-console
      console.log(hero);
    },
    updateHeroPosition(){
      setInterval(() => {
        // Update velocity based on acceleration
        this.heroState.vx += this.heroState.ax/10;
        this.heroState.vy += this.heroState.ay;

        // Limit velocity to the maximum speed
        if (Math.abs(this.heroState.vx) > this.heroState.speed) {
          this.heroState.vx =
            (this.heroState.vx / Math.abs(this.heroState.vx)) *
            this.heroState.speed;
        }
        if (Math.abs(this.heroState.vy) > this.heroState.speed) {
          this.heroState.vy =
            (this.heroState.vy / Math.abs(this.heroState.vy)) *
            this.heroState.speed;
        }

        // Update position based on velocity
        this.heroState.px += this.heroState.vx;
        this.heroState.py += this.heroState.vy;

        // Apply gravity to simulate realistic jumping
        if (this.heroState.isJumping) {
          this.heroState.vy += 0.2; // Adjust gravity value as needed
        }

        // Check for ground and stop jumping when landing
        if (this.heroState.py >= 200) {
          this.heroState.py = 200;
          this.heroState.vy = 0;
          this.heroState.isJumping = false;
        }
      }, this.timeInterval);
    }
  },
  mounted() {
    this.updateHeroPosition();
  }
}
</script>
