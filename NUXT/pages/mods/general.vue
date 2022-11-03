<template>
  <div class="mainContainer pt-1">
    <!--   Language Picker   -->
    <v-card
      flat
      class="pb-5 background"
      :class="$vuetify.theme.dark ? 'lighten-1' : 'darken-1'"
      :style="{ borderRadius: `${roundTweak / 2}rem` }"
    >
      <v-card-title>{{ lang.language }}</v-card-title>
      <v-card-text>
        <language />
      </v-card-text>
    </v-card>

    <!--   Backup   -->
    <v-card
      flat
      class="pb-5 background"
      :class="$vuetify.theme.dark ? 'lighten-1' : 'darken-1'"
      :style="{ borderRadius: `${roundTweak / 2}rem` }"
    >
      <v-card-title>{{ lang.backup }}</v-card-title>
      <v-card-text>
        <p>{{ lang.backupinfo }}</p>
      </v-card-text>
      <v-card-actions>
        <v-btn rounded depressed class="background--text ml-2" color="primary" @click="registryBackup">{{ lang.backup }}</v-btn>
        <v-btn rounded @click="registryRestore">{{ lang.restore }}</v-btn>
      </v-card-actions>
    </v-card>

        <v-card
      flat
      class="pb-5 background"
      :class="$vuetify.theme.dark ? 'lighten-1' : 'darken-1'"
      :style="{ borderRadius: `${roundTweak / 2}rem` }"
    >
      <v-card-title>{{ lang.bedtime }}</v-card-title>
      <input type="text" class="time-pickable" readonly>
      <v-card-text>
          <v-select
    class="fixed"      
    v-model="selectedLang"
    background-color="background"
    :items="hour"
    label="App Language"
    solo
    rounded
  />
            <v-select
            class="fixed2"
    v-model="selectedLang"
    background-color="background"
    :items="hour"
    label="App Language"
    solo
    rounded
  />
      </v-card-text>

      <v-card-text>
        <p>{{ lang.backupinfo }}</p>
      </v-card-text>
      <v-card-actions>
        <v-btn rounded depressed class="background--text ml-2" color="primary" @click="registryConfrim">{{ lang.confirm }}</v-btn>
      </v-card-actions>
    </v-card>


  </div>
</template>

<script>
import language from "~/components/Settings/language.vue";
export default {
  components: {
    language,
  },
  data() {
    return {
      lang: {},
      hour: ['01','02','03','04','06','07','08','09','10','11','12'],
    };
  },
  computed: {
    roundTweak() {
      return this.$store.state.tweaks.roundTweak;
    },
  },
  mounted() {
    const lang = this.$lang();
    this.lang = lang.mods.general;
  },
  methods: {
    download(filename, text) {
      var element = document.createElement('a');
      element.setAttribute('href', 'data:text/plain;charset=utf-8,' + encodeURIComponent(text));
      element.setAttribute('download', filename);
    
      element.style.display = 'none';
      document.body.appendChild(element);
    
      element.click();
    
      document.body.removeChild(element);
    },

    getRegistry() {
      let keys = [];
      const localStorageKeys = Object.keys(localStorage);
      for (const i in localStorageKeys) {
        const key = localStorageKeys[i];
        const keyValue = localStorage.getItem(key);
        keys.push({ key: key, value: keyValue });
      }
      return keys;
    },

    registryBackup() {
      const file = JSON.stringify({
        scheme: "VueTube Backup",
        version: process.env.version,
        channel: process.env.channel,
        date: Date.now(),
        registry: this.getRegistry()
      });
      this.download("vuetube-backup.json",file);
    },
    registryRestore() {

    },
    registryConfrim() {
      
    }
  }
};
</script>

<style scoped>
.v-card {
  margin: 1em;
}

section {
  padding: 0 1em 1em 1em;
}
.fixed {
    position: fixed;
    width: 150px;
    height: 200px;
}
.fixed2{
  position: fixed;
    width: 60px;
    height: 200px;
}
</style>
