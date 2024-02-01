<template>
  <div style="overflow-x: hidden; height: 100px; width: 100%;">
    <GameHero :hero="heroState" :style="{
      position: 'fixed',
      left: heroState.px + 'px',
      bottom: heroState.py + 'px',
      transform: isFacingLeft ? `scale(-${heroState.scale}, ${heroState.scale})` : `scale(${heroState.scale}, ${heroState.scale})`,
    }" @heroMoved="handleHeroMove" />
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      heroState: {
        name: 'Nathan',
        scale: 0.2,
        currentFrameIndex: 0,
        isFacingLeft: false,
        px: 0,
        py: 0,
        vx: 0,
        vy: 0,
        ax: 0,
        ay: 0,
        friction: 0,
        speed: 1.5,
        jump: 1,
        isMoving: false,
        isJumping: false,
        askToJump: false,
        isFalling: false,
        gold: 0,
        items: [],
      },
      gravity: 0.01,
      floor: 0,
      timeInterval: 1,
      spriteTimeInterval: 200,
    };
  },
  methods: {

    handleHeroMove(hero) {
      this.heroState = hero;
    },

    calculateAnimationInterval() {
      this.spriteTimeInterval -= 1;
    },

    updateHeroPosition() {
      setInterval(() => {
        // VELOCITY BASED ON ACCELERATION
        this.heroState.vx += this.heroState.ax / 10;

        // LIMITES DE VITESSE
        if (Math.abs(this.heroState.vx) > this.heroState.speed) {
          this.heroState.vx =
            (this.heroState.vx / Math.abs(this.heroState.vx)) *
            this.heroState.speed;
        }

        // HANDLE JUMPING
        if (this.heroState.askToJump) {
          this.heroState.askToJump = false;
          this.heroState.isJumping = true;
          this.heroState.ay = this.heroState.jump;
        }

        // JUMPING
        if (this.heroState.isJumping) {
          if (this.heroState.isFalling === true) {
            this.heroState.vy += this.heroState.ay; // Update vertical velocity
            console.log("FALL");
            this.heroState.ay -= this.gravity / 4; // Apply gravity to reduce acceleration

            // Update position based on vertical velocity
            this.heroState.py += this.heroState.vy;
          } else {
            this.heroState.vy += this.heroState.ay; // Update vertical velocity
            this.heroState.ay -= this.gravity; // Apply gravity to reduce acceleration

            // Update position based on vertical velocity
            this.heroState.py += this.heroState.vy;
          }

          // Check if the hero has reached its peak height or hit the ground
          if (this.heroState.py < this.floor) {
            this.heroState.py = this.floor;
            this.heroState.vy = 0;
            this.heroState.isJumping = false;
          }
        }

        // APPLY FRICTION
        if (this.heroState.isMoving) {
          this.heroState.vx *= this.heroState.friction;
        }

        // STOP MOVING IF SPEED IS LOW
        if (Math.abs(this.heroState.vx) < 0.05) {
          this.heroState.vx = 0;
        }

        // DETECT FALLING
        if (this.heroState.vy < 0) {
          this.heroState.isFalling = true;
        } else {
          this.heroState.isFalling = false;
        }

        // DETECT MOVING
        if (this.heroState.vx !== 0) {
          this.heroState.isMoving = true;
        } else {
          this.heroState.isMoving = false;
        }

        // UPDATE POSITION  
        this.heroState.px += this.heroState.vx;
        this.heroState.py += this.heroState.vy;

      }, this.timeInterval);
    },
    updateHeroAppearance() {
      setInterval(() => {
        this.calculateAnimationInterval();

        // DIRECTION
        if (this.heroState.vx >= 0) {
          this.isFacingLeft = false;
        } else {
          this.isFacingLeft = true;
        }

        // FRAME
        if (this.heroState.isMoving) {
          this.heroState.currentFrameIndex++;
          if (this.heroState.currentFrameIndex > 2) {
            this.heroState.currentFrameIndex = 1;
          }
        } else {
          this.heroState.currentFrameIndex = 0;
        }
      }, this.spriteTimeInterval);
    },
  },
  mounted() {
    this.updateHeroPosition();
    this.updateHeroAppearance();
  }
}
</script>
