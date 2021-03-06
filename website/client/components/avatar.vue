<template lang="pug">
.avatar(:style="{width, height, paddingTop}", :class="backgroundClass")
  .character-sprites
    template(v-if="!avatarOnly" v-once)
      // Mount
      span(v-if="user.items.currentMount", :class="'Mount_Body_' + user.items.currentMount")

    // Buffs that cause visual changes to avatar: Snowman, Ghost, Flower, etc
    template(v-for="(klass, item) in visualBuffs")
      span(v-if="user.stats.buffs[item]", :class="klass")

    // Show flower ALL THE TIME!!!
    // See https://github.com/HabitRPG/habitrpg/issues/7133
    span(:class="'hair_flower_' + user.preferences.hair.flower")

    // Show avatar only if not currently affected by visual buff
    template(v-if!="!user.stats.buffs.snowball && !user.stats.buffs.spookySparkles && !user.stats.buffs.shinySeed && !user.stats.buffs.seafoam")
      span(:class="'chair_' + user.preferences.chair")
      span(:class="user.items.gear[costumeClass].back")
      span(:class="skinClass")
      span(:class="user.preferences.size + '_shirt_' + user.preferences.shirt")
      span(:class="user.preferences.size + '_' + user.items.gear[costumeClass].armor")
      span(:class="user.items.gear[costumeClass].back_collar")
      span(:class="user.items.gear[costumeClass].body")
      span.head_0
      template(v-for="type in ['base', 'bangs', 'mustache', 'beard']")
        span(:class="'hair_' + type + '_' + user.preferences.hair[type] + '_' + user.preferences.hair.color")
      span(:class="user.items.gear[costumeClass].eyewear")
      span(:class="user.items.gear[costumeClass].head")
      span(:class="user.items.gear[costumeClass].headAccessory")
      span(:class="'hair_flower_' + user.preferences.hair.flower")
      span(:class="user.items.gear[costumeClass].shield")
      span(:class="user.items.gear[costumeClass].weapon")

    // Resting
    span.zzz(v-if="user.preferences.sleep")

    template(v-if="!avatarOnly" v-once)
      // Mount Head
      span(v-if="user.items.currentMount", :class="'Mount_Head_' + user.items.currentMount")
      // Pet
      span.current-pet(v-if="user.items.currentPet", :class="'Pet-' + user.items.currentPet")
</template>

<style scoped>
.avatar {
  width: 140px;
  height: 147px;
  image-rendering: pixelated;
}

.character-sprites {
  margin: 0 auto;
  width: 90px;
  height: 90px;
}

.character-sprites span {
  position: absolute;
}

.current-pet {
  bottom: 21px;
  left: 20px;
}
</style>

<script>
export default {
  props: {
    user: {
      type: Object,
      required: true,
    },
    avatarOnly: {
      type: Boolean,
      default: false,
    },
    width: {
      type: Number,
      default: 140,
    },
    height: {
      type: Number,
      default: 147,
    },
  },
  computed: {
    paddingTop () {
      let val = '28px';

      if (!this.avatarOnly) {
        if (this.user.items.currentPet) val = '24.5px';
        if (this.user.items.currentMount) val = '0px';
      }

      return val;
    },
    backgroundClass () {
      let background = this.user.preferences.background;

      if (background && !this.avatarOnly) {
        return `background_${this.user.preferences.background}`;
      }

      return '';
    },
    visualBuffs () {
      return {
        snowball: 'snowman',
        spookySparkles: 'ghost',
        shinySeed: `avatar_floral_${this.user.stats.class}`,
        seafoam: 'seafoam_star',
      };
    },
    skinClass () {
      let baseClass = `skin_${this.user.preferences.skin}`;

      return `${baseClass}${this.user.preferences.sleep ? '_sleep' : ''}`;
    },
    costumeClass () {
      return this.user.preferences.costume ? 'costume' : 'equipped';
    },
  },
};
</script>