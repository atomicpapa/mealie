<template>
  <div>
    <v-app-bar color="primary">
      <v-spacer></v-spacer>
      <v-btn href="https://github.com/sponsors/hay-kot" target="_blank" class="mx-1" color="secondary">
        <v-icon left>
          {{ $globals.icons.heart }}
        </v-icon>
        {{ $t("about.support") }}
      </v-btn>
      <v-btn href="https://github.com/hay-kot" target="_blank" class="mx-1" color="secondary">
        <v-icon left>
          {{ $globals.icons.github }}
        </v-icon>
        {{ $t("about.github") }}
      </v-btn>
      <v-btn href="https://hay-kot.dev" target="_blank" class="mx-1" color="secondary">
        <v-icon left>
          {{ $globals.icons.account }}
        </v-icon>
        {{ $t("about.portfolio") }}
      </v-btn>
      <v-btn href="https://hay-kot.github.io/mealie/" target="_blank" class="mx-1" color="secondary">
        <v-icon left>
          {{ $globals.icons.folderOutline }}
        </v-icon>
        {{ $t("about.docs") }}
      </v-btn>
      <v-spacer></v-spacer>
    </v-app-bar>
    <v-card class="mt-3">
      <v-card-title class="headline">
        {{ $t("about.about-mealie") }}
      </v-card-title>
      <v-divider></v-divider>
      <v-card-text>
        <v-list-item-group color="primary">
          <v-list-item v-for="property in prettyInfo" :key="property.name">
            <v-list-item-icon>
              <v-icon> {{ property.icon || $globals.icons.user }} </v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title class="pl-4 flex row justify-space-between">
                <div>{{ property.name }}</div>
                <div>{{ property.value }}</div>
              </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list-item-group>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <TheDownloadBtn download-url="/api/debug/last-recipe-json">
          <template v-slot:default="{ downloadFile }">
            <v-btn color="primary" @click="downloadFile">
              <v-icon left> {{ $globals.icons.codeBraces }} </v-icon> {{ $t("about.download-recipe-json") }}
            </v-btn>
          </template>
        </TheDownloadBtn>
      </v-card-actions>
      <v-divider></v-divider>
    </v-card>
    <LogCard />
  </div>
</template>

<script>
import { api } from "@/api";
import TheDownloadBtn from "@/components/UI/Buttons/TheDownloadBtn";
import LogCard from "@/components/UI/LogCard.vue";
export default {
  components: { TheDownloadBtn, LogCard },
  data() {
    return {
      prettyInfo: [],
    };
  },
  async created() {
    await this.getInfo();
  },
  methods: {
    async getInfo() {
      const debugInfo = await api.meta.getDebugInfo();

      this.prettyInfo = [
        {
          name: this.$t("about.version"),
          icon: this.$globals.icons.information,
          value: debugInfo.version,
        },
        {
          name: this.$t("about.application-mode"),
          icon: this.$globals.icons.devTo,
          value: debugInfo.production ? this.$t("about.production") : this.$t("about.development"),
        },
        {
          name: this.$t("about.demo-status"),
          icon: this.$globals.icons.testTube,
          value: debugInfo.demoStatus ? this.$t("about.demo") : this.$t("about.not-demo"),
        },
        {
          name: this.$t("about.api-port"),
          icon: this.$globals.icons.api,
          value: debugInfo.apiPort,
        },
        {
          name: this.$t("about.api-docs"),
          icon: this.$globals.icons.file,
          value: debugInfo.apiDocs ? this.$t("general.enabled") : this.$t("general.disabled"),
        },
        {
          name: this.$t("about.database-type"),
          icon: this.$globals.icons.database,
          value: debugInfo.dbType,
        },
        {
          name: this.$t("about.database-url"),
          icon: this.$globals.icons.database,
          value: debugInfo.dbUrl,
        },
        {
          name: this.$t("about.default-group"),
          icon: this.$globals.icons.group,
          value: debugInfo.defaultGroup,
        },
      ];
    },
  },
};
</script>
