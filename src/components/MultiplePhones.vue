<template>
  <div>
    <h1>Multiple Phones Input Component</h1>
    <p>Data props received from parent:</p>
    <input
      type="text"
      name="input"
      id="input"
      class="props-edit-input"
      v-model="phonesInputStr"
      @input="onPropsEditInput"
    />
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
          <option value="">N/A</option>
          <option value="WA">WA</option>
          <option value="Home">Home</option>
          <option value="mobile">mobile</option>
        </select>
        <!-- Input for phone data -->
        <input
          type="text"
          v-model="phone.value"
          class="text-input"
          @input="onPhoneDataInput()"
        />
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
          <option value="">N/A</option>
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
      phonesInputStr: this.phonesInputString,
      phones: [],
      prefix: "",
      value: "",
    };
  },
  methods: {
    onPropsEditInput() {
      // Update the phones array and thus the list items
      console.log("new phonesInputStr", this.phonesInputStr);
      this.updatePhonesArr();
    },
    onPhoneDataInput() {
      // This get called when user type phone no. in list item
      this.updatePhonesInputStr();
    },
    onPrefixChange(phone) {
      // This get called when user selects the prefix from dropdown
      console.log(phone.prefix);
      console.log(this.phones);
      this.updatePhonesInputStr();
    },
    onClickDeleteBtn(id) {
      console.log("Attempt to delete item with id ", id);
      if (confirm("Are you sure?"))
        this.phones = this.phones.filter((phone) => phone.id !== id);
      console.log("New phones array after delete attempt:", this.phones);
      this.updatePhonesInputStr();
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
        this.updatePhonesInputStr();
      }
    },
    updatePhonesArr() {
      this.phones = this.phonesInputStr.split(";");
      this.phones = this.phones.map((phone) => {
        if (phone.includes("WA:")) {
          return {
            id: Math.floor(Math.random() * 100000),
            prefix: "WA",
            value: phone.replace("WA:", "").trim(),
          };
        } else if (phone.includes("Home:")) {
          return {
            id: Math.floor(Math.random() * 100000),
            prefix: "Home",
            value: phone.replace("Home:", "").trim(),
          };
        } else if (phone.includes("mobile:")) {
          return {
            id: Math.floor(Math.random() * 100000),
            prefix: "mobile",
            value: phone.replace("mobile:", "").trim(),
          };
        } else {
          return {
            id: Math.floor(Math.random() * 100000),
            prefix: "",
            value: phone.trim(),
          };
        }
      });
      console.log("Updated phones array:", this.phones);
    },
    updatePhonesInputStr() {
      // Update the model of props editing input (phonesInputStr) based on changes of any list item
      const arrayToJoin = this.phones.map((phone) => {
        if (phone.prefix) {
          return `${phone.prefix}: ${phone.value}`;
        } else {
          return `${phone.value}`;
        }
      });
      console.log("arrayToJoin", arrayToJoin);
      console.log(
        "array string after join by semi-colon:",
        arrayToJoin.join(" ; ")
      );
      this.phonesInputStr = arrayToJoin.join(" ; ");
    },
  },
  mounted() {
    this.updatePhonesArr();
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

.props-edit-input {
  min-width: 40%;
  width: 70%;
  padding: 10px;
  margin-bottom: 15px;
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

.delete-button:hover {
  cursor: pointer;
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
  min-width: 40%;
  margin: 5px;
}
</style>
