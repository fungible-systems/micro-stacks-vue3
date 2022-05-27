<template>
  <button @click="handleAuth">
    {{ isSignedIn ? session?.addresses?.mainnet : "Sign in with Stacks" }}
  </button>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { authenticate, AuthOptions, StacksSessionState } from "micro-stacks/connect";


export default defineComponent({
  name: "WalletConnectButton",
  setup() {
    const session = typeof document !== "undefined" ? localStorage.getItem("stacks-session") : null;
    const isSignedIn = !!session;
    const authOptions: AuthOptions = {
      appDetails: {
        name: "Vue3 app",
        icon: "/"
      }, onFinish(payload) {
        localStorage.setItem("stacks-session", JSON.stringify(payload));
      }
    };
    return {
      session: isSignedIn ? JSON.parse(session) as StacksSessionState : null, isSignedIn,
      handleAuth: () => {
        authenticate(authOptions);
      }
    };
  }
});
</script>

