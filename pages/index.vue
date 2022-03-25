<template>
  <div>
    <Header :is-supported="ethereumSupported" @connect="contractConnect" />

    <!-- Main content here -->
    <section class="body container">
      <div class="holder">
        <div class="text-div">
          <h1>Decentralized Fund Me,</h1>
          <p class="text">
            A new way to transfer <span class="funds">funds</span> to friends
            and the public. To get started click the button below.
          </p>

          <!-- Button -->
          <div class="form-group">
            <input
              type="text"
              placeholder="Enter Address"
              v-model="transferAddress"
            />
            <button @click="handleTransfer" :disabled="!transferAddress">
              Transfer funds
            </button>
          </div>
          <p class="rise">we rise by lifting others.</p>
        </div>

        <!-- Image Here -->
        <div class="image">
          <img src="~/assets/card.png" alt="card" />
        </div>
      </div>

      <!-- Top Funding -->
      <div class="top-funding">
        <img src="~/assets/podium.svg" alt="poidum" class="podium" />
        <h1>Top Funding</h1>
      </div>
    </section>

    <Footer />
  </div>
</template>

<script>
import { abi } from "./abi.js";
import Web3 from "web3";

export default {
  name: "IndexPage",

  data: () => ({
    contractAddress: "0x86C12A724340f3F4F6142789808874d0A55Bd01f",
    ethereumSupported: false,
    response: null,
    transferAddress: "0x32Be343B94f860124dC4fEe278FDCBD38C102D88",
    test: "0x89903264edAD5cEc3abE5C48554e7e23e91Fe378",
  }),

  async mounted() {
    this.ethereumSupported = await this.isEthereumSupported();

    console.log("Ethereum supported:", window.web3);
  },

  methods: {
    async contractConnect() {
      const response = await new window.web3.eth.Contract(
        abi,
        this.contractAddress
      );
      this.response = response;
      console.log("response", this.response);
    },
    async isEthereumSupported() {
      if (window.ethereum) {
        window.web3 = new Web3(window.ethereum);
        try {
          // Request account access
          await window.ethereum.enable();
          this.contractConnect();
          console.log("Ethereum enabled");
          this.handleSuccess();

          return true;
        } catch (error) {
          console.log("error", error);
          return false;
        }
      }
      // Non-decentralized app browsers...
      else {
        this.handleError();
      }
    },

    handleTransfer() {
      const unit = 1;

      this.response.methods
        .transfer(this.contractAddress, unit)
        .call((err, res) => {
          if (err) {
            console.log("error", err);
          } else {
            console.log("response", res);
          }
        });
    },

    handleSuccess(message) {
      const successMessage =
        message || "This browser is supported for ethereum";

      this.$toast.success(successMessage);
    },

    handleError(error) {
      const errorMessage =
        error ||
        "Non-Ethereum browser detected. You should consider trying MetaMask!";

      this.$toast.error(errorMessage);
    },
  },
};
</script>

<style scoped>
.body {
  width: 100%;
  overflow: hidden;
}
.holder {
  display: flex;
  align-items: center;
}
.text-div {
  width: 60%;
  color: #424242;
}
.text {
  color: #94a5c5;
  font-weight: 400;
  padding-top: 1rem;
  line-height: 24px;
}
span.funds {
  color: #1c4298;
}
.image {
  width: 100%;
}

.image > img {
  width: inherit;
  height: inherit;
  object-fit: contain;
}

.form-group {
  margin-top: 3rem;
  display: flex;
  flex-direction: column;
}

input {
  border: 1px solid #e7eaf3;
  border-radius: 4px;
  border: none;
  margin-bottom: 1.25rem;
  padding: 0 12px;
  height: 52px;
  border-radius: 8px;
  font-size: 0.875rem;
}

input:focus {
  outline: none;
  border: 1px solid #1c4298;
}

.rise {
  color: #94a5c5;
  font-size: 13px;
  padding-top: 12px;
}
.top-funding {
  margin: 3rem 0 7rem 0;
  display: flex;
  color: #94a5c5;
}

.podium {
  width: 40px;
  margin-right: 10px;
}
</style>
