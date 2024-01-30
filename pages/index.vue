<template>
  <div style="overflow-x: hidden;">
      <GameHero :hero="heroState" :style="{ position: 'relative', left: heroState.px + 'px' }" />
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
        height: 30,
        px: 0,
        py: 100,
        vx: 0,
        vy: 0,
        ax: 0,
        ay: 0,
        speed: 1,
        jump: 10,
        isJumping: false,
        gold: 0,
        items: [],
      },
    };
  },
  methods: {
    moveLeft() {
      const updatedHero = { ...this.heroState };
      updatedHero.ax = -0.1;
      this.heroState = updatedHero;
    },
    moveRight() {
      const updatedHero = { ...this.heroState };
      updatedHero.ax = 0.1;
      this.heroState = updatedHero;
    },
    jump() {
      const updatedHero = { ...this.heroState };
      if (!updatedHero.isJumping) {
        updatedHero.isJumping = true;
        updatedHero.vy = -updatedHero.jump;
      }
      this.heroState = updatedHero;
    },
    updateHeroPosition() {
      setInterval(() => {
        // Update velocity based on acceleration
        this.heroState.vx += this.heroState.ax / 10;
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
    window.addEventListener('keydown', (event) => {
      if (event.key === 'ArrowRight') {
        this.moveRight();
      }
      if (event.key === 'ArrowLeft') {
        this.moveLeft();
      }
      if (event.key === 'ArrowUp') {
        this.jump();
      }
      if (event.key === 'ArrowDown') {
        // eslint-disable-next-line no-console
        console.log('TO BE IMPLEMENTED');
      }
    });

    // DECELERATION
    window.addEventListener('keyup', (event) => {
      if (event.key === 'ArrowRight' || event.key === 'ArrowLeft') {
        const updatedHero = { ...this.hero };
        updatedHero.ax = 0;
        this.$emit('heroMoved', updatedHero);
      }
    });
  }
}
</script>
