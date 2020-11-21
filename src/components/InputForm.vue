<template>
  <div>
    <form @submit.prevent="submitForm">
      <label for="secret">
        <span>Tapez ou collez votre secret</span>
        <div class="input">
          <input id="secret" :type="secretInputType" v-model="secret" />
          <button class="btn" @click.prevent="toggleSecret" v-text="secretBtnText" />
        </div>
      </label>

      <label for="payload">
        <span>Tapez ou collez votre payload</span>
        <textarea id="payload" cols="60" rows="12" v-model="payload" :placeholder="getPayloadPlaceholder" />
      </label>

      <button type="submit" :disabled="!canSubmitForm">Générer le hash de signature</button>
    </form>

    <div class="signature" v-if="signature">
      <input type="text" class="result" :value="signature" />
      <button @click.prevent="copySignature">📋</button>
    </div>
  </div>
</template>

<script>
import crypto from 'crypto-browserify';
import exemple from '../data/payload.js';

export default {
  data() {
    return {
      secret: '',
      payload: '',
      signature: '',
      secretIsMasked: true,
      exemple,
    };
  },

  computed: {
    canSubmitForm() {
      return this.secret && this.payload;
    },

    secretInputType() {
      return this.secretIsMasked ? 'password' : 'text';
    },

    secretBtnText() {
      return this.secretIsMasked ? '👁️' : '👁';
    },

    getPayloadPlaceholder() {
      return this.exemple;
    },
  },

  methods: {
    submitForm() {
      const hmac = crypto.createHmac('sha256', this.secret);
      this.signature = 'sha256=' + hmac.update(JSON.stringify(JSON.parse(this.payload))).digest('hex');
    },

    toggleSecret() {
      this.secretIsMasked = !this.secretIsMasked;
    },

    copySignature() {
      document.querySelector('.result').select();
      document.execCommand('copy');
    },
  },
};
</script>

<style lang="scss" scoped>
form {
  margin-bottom: 8px;

  label {
    display: flex;
    flex-direction: column;
    align-items: flex-start;

    [type] {
      width: 280px;
      height: 24px;
    }

    * {
      margin-bottom: 10px;
    }
  }

  button {
    cursor: pointer;
    padding: 8px;

    &.btn {
      padding: 4px;
      margin-left: 4px;
      width: 40px;
    }
  }
}

.result {
  width: 500px;
  height: 24px;
  border: none;
}
</style>
