<template>
  <div>
    <h1>Multiple Phones Input Component</h1>
    <p>Data props received from parent: {{ phonesInputString }}</p>
    <form>
      <!-- Generate rows based on phones array -->
      <div v-for="phone in phones" :key="phone.id" class="row">
        <!-- Prefix Dropdown -->
        <select
          name="prefix"
          id="prefix"
          v-model="phone.prefix"
          class="prefix-select"
          @change="onPrefixChange(phone)"
        >
          <option value="WA">WA</option>
          <option value="Home">Home</option>
          <option value="mobile">mobile</option>
        </select>
        <!-- Input for phone data -->
        <input type="text" v-model="phone.value" class="text-input" />
        <!-- Delete item button -->
        <div class="delete-button" @click="onClickDeleteBtn(phone.id)">
          <img
            class="delete-icon"
            src="../assets/close-icon.png"
            alt="delete"
          />
        </div>
      </div>
      <!-- Add an item -->
      <div class="row">
        <select
          name="prefix"
          id="prefix"
          class="prefix-select"
          v-model="prefix"
        >
          <option value="WA">WA</option>
          <option value="Home">Home</option>
          <option value="mobile">mobile</option>
        </select>
        <!-- Input for phone data -->
        <input
          type="text"
          class="text-input"
          v-model="value"
          @keyup="onPressEnter"
          placeholder="Enter a phone number and press 'Enter'"
        />
        <!-- Delete item button -->
        <div class="delete-button">
          <img
            class="delete-icon"
            src="../assets/close-icon.png"
            alt="delete"
          />
        </div>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: "MultiplePhones",
  props: { phonesInputString: String },
  data() {
    return {
      phones: [],
      prefix: "",
      value: "",
    };
  },
  methods: {
    onPrefixChange(phone) {
      console.log(phone.prefix);
      console.log(this.phones);
    },
    onClickDeleteBtn(id) {
      console.log("Attempt to delete item with id ", id);
      if (confirm("Are you sure?"))
        this.phones = this.phones.filter((phone) => phone.id !== id);
      console.log("New phones array after delete attempt:", this.phones);
    },
    onPressEnter(event) {
      if (event.key === "Enter") {
        if (!this.value) {
          alert("Enter a phone number");
          return;
        }

        //   Create a new phone object
        const newPhoneObject = {
          id: Math.floor(Math.random() * 100000),
          prefix: this.prefix,
          value: this.value.trim(),
        };

        // Clear the input
        (this.prefix = ""), (this.value = "");

        this.phones = [...this.phones, newPhoneObject];
        console.log("New phones array after adding:", this.phones);
      }
    },
  },
  mounted() {
    this.phones = this.phonesInputString.split(";");
    this.phones = this.phones.map((phone) => {
      if (phone.includes("WA:")) {
        return {
          id: Math.floor(Math.random() * 100000),
          prefix: "WA",
          value: phone.replace("WA: ", "").trim(),
        };
      } else if (phone.includes("Home:")) {
        return {
          id: Math.floor(Math.random() * 100000),
          prefix: "Home",
          value: phone.replace("Home: ", "").trim(),
        };
      } else if (phone.includes("mobile:")) {
        return {
          id: Math.floor(Math.random() * 100000),
          prefix: "mobile",
          value: phone.replace("mobile: ", "").trim(),
        };
      } else {
        return {
          id: Math.floor(Math.random() * 100000),
          prefix: "",
          value: phone.trim(),
        };
      }
    });
    console.log("Initialized phones array:", this.phones);
  },
};
</script>

<style scoped>
form {
  display: flex;
  flex-direction: column;
  width: 100%;
  align-items: center;
}

.text-input {
  /* height: 100%;
  width: 30%;
  padding: 0.25rem 1.25rem;
  border-radius: 12px;
  border: 1px solid lightgray; */
  flex-grow: 2;
  /* And hide the input's outline, so the form looks like the outline */
  border: none;
  margin: 2px 10px;
  padding: 5px 10px;
}

.text-input:focus {
  outline: none;
}

.prefix-select {
  height: 100%;
  padding: 0.25rem 0.5rem 0.25rem 0.5rem;
  border-radius: 12px;
}

.delete-button {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 24px;
  height: 24px;
  border-radius: 100%;
  background-color: lightgray;
  color: white;
  margin-right: 10px;
}

.delete-icon {
  width: 12px;
  height: 12px;
}

.row {
  display: flex;
  flex-direction: row;
  align-items: center;
  border: 1px solid grey;
  border-radius: 12px;
  padding: 5px;
  width: 50%;
  margin: 5px;
}
</style>
