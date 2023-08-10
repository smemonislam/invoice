<script setup>
import { reactive } from "vue";
const data = reactive({
  sender: "",
  billTo: "",
  shipTo: "",
  invoicNumber: "",
  date: "",
  dueDate: "",
  additionalDate: "",
  items: [
    {
      description: "",
      quantity: "",
      rate: "",
      amount: "",
      discount: "",
      tax1: "",
    },
  ],
  notes: "",
  terms: "",
  subtotal: "",
  tax: "",
  total: "",
  payment: "",
  blanceDue: "",
});

function addMoreItem() {
  data.items.push({
    description: "",
    quantity: "",
    rate: "",
    amount: "",
    discount: "",
    tax1: "",
  });
}

function getSubTotal() {
  let subtotal = 0;
  data.items.forEach((item) => (subtotal += item.amount));
  data.subtotal = subtotal;
  return subtotal;
}

function getTotal() {
  let tax = (data.subtotal * data.tax) / 100;
  let total = data.subtotal + tax;
  data.total = total;
  return total;
}

function getAmount(item) {
  item.amount = item.quantity * item.rate;
  item.amount -= item.discount;
  item.amount += (item.amount * item.tax1) / 100;

  return item.amount;
}

function BalanceDue() {
  data.total = data.total - data.payment;
  data.blanceDue = data.total;

  return data.blanceDue;
}

function ItemDelete(index) {
  data.items.splice(index, 1);
}
</script>

<template>
  <section
    class="mx-auto container bg-white border border-gray-400 min-h-screen p-12"
  >
    <div class="flex justify-between">
      <div class="flex flex-col space-y-5 w-1/2s">
        <div class=" ">
          <img
            class="w-40"
            src="https://www.shutterstock.com/image-vector/invoice-typographic-stamp-sign-badge-600w-1027820257.jpg"
            alt=""
          />
        </div>
        <p class="mt-5">Sender</p>
        <textarea
          name=""
          id=""
          cols="30"
          rows="2"
          v-model="data.sender"
        ></textarea>
        <div class="flex space-x-2">
          <div class="flex flex-col">
            <span>Bill to</span>
            <textarea
              name=""
              id=""
              cols="30"
              rows="2"
              v-model="data.billTo"
            ></textarea>
          </div>
          <div class="flex flex-col">
            <span>Ship to</span>
            <textarea
              name=""
              id=""
              cols="30"
              rows="2"
              v-model="data.shipTo"
            ></textarea>
          </div>
        </div>
      </div>
      <div class="flex flex-col w-1/2 items-end">
        <h1 class="mt-12 text-4xl uppercase text-right mb-5">Invoice</h1>
        <input
          class="w-[200px] text-right"
          type="text"
          :placeholder="data.invoicNumber"
          v-model="data.invoicNumber"
        />
        <div class="mt-10 flex-y-5 text-right space-y-3 w-full">
          <p>
            <span>Date</span>
            <input class="ml-2 w-[200px]" v-model="data.date" />
          </p>
          <p>
            <span>Due Date</span>
            <input class="ml-2 w-[200px]" v-model="data.dueDate" />
          </p>
          <p>
            <span>Additional Note</span>
            <input
              class="ml-2 w-[200px]"
              type="text"
              v-model="data.additionalDate"
            />
          </p>
        </div>
      </div>
    </div>
    <div class="mt-20">
      <table class="table-auto w-full">
        <tr class="bg-gray-800 text-left text-white">
          <th class="p-2 pl-5 w-1/3">Item</th>
          <th class="p-2">Quantity</th>
          <th class="p-2">Rate</th>
          <th class="p-2">Dicount</th>
          <th class="p-2">Tax</th>
          <th class="p-2 w-[150px] text-right pr-5">Amount</th>
          <th class="p-2 w-[50px]">Action</th>
        </tr>
        <tr v-for="(item, index) in data.items" :key="index">
          <td class="py-1">
            <input
              class="w-full pl-5"
              type="text"
              placeholder="Description"
              v-model="item.description"
            />
          </td>
          <td class="">
            <input
              class="w-full"
              type="number"
              placeholder="Quantity"
              v-model="item.quantity"
            />
          </td>
          <td class="">
            <input
              class="w-full"
              type="number"
              placeholder="Rate"
              v-model="item.rate"
            />
          </td>

          <td class="">
            <input
              class="w-full"
              type="number"
              placeholder="Discount"
              v-model="item.discount"
            />
          </td>
          <td class="">
            <input
              class="w-full"
              type="number"
              placeholder="Tax"
              v-model="item.tax1"
            />
          </td>
          <td class="py-1 pr-5 text-right text-gray-800">
            $
            {{ getAmount(item) }}
          </td>
          <td class="py-1 text-right">
            <button
              v-show="data.items.length > 1"
              class="border py-2 px-3 bg-white text-red-500"
              @click="ItemDelete(index)"
            >
              <i class="fa-solid fa-trash"></i>
            </button>
          </td>
        </tr>
      </table>
      <button
        class="mt-5 bg-green-600 hover:bg-green-700 text-white font-bold py-2 px-4 rounded"
        @click="addMoreItem"
      >
        Add More
      </button>

      <!-- <p>{{ data }}</p> -->
    </div>
    <div class="mt-[200px]">
      <div class="flex justify-between">
        <div class="flex flex-col space-y-5 w-1/2">
          <span>Notes</span>
          <textarea
            name=""
            id=""
            cols="30"
            rows="2"
            v-model="data.notes"
          ></textarea>
          <span>Terms</span>
          <textarea
            name=""
            id=""
            cols="30"
            rows="2"
            v-model="data.terms"
          ></textarea>
        </div>
        <div class="flex flex-col w-1/2 items-end">
          <div class="mt-10 flex-y-5 text-right space-y-3 w-full">
            <p>
              <span>Subtotal</span>
              <input
                readonly
                class="focus:ring-0 focus:ring-offset-0 text-right ml-2 pr-4 w-[200px] border-0"
                :value="getSubTotal()"
              />
            </p>
            <p>
              <span>Tax</span>
              <input
                type="number"
                class="tax text-right w-[200px] ml-2"
                v-model="data.tax"
              />
            </p>
            <p>
              <span>Total</span>
              <input
                readonly
                class="focus:ring-0 focus:ring-offset-0 text-right ml-2 pr-4 w-[200px] border-0"
                :value="getTotal()"
              />
            </p>
            <p>
              <span>Payment</span>
              <input
                type="number"
                class="tax text-right w-[200px] ml-2"
                v-model="data.payment"
              />
            </p>
            <p>
              <span>Balace Due</span>
              <input
                readonly
                class="focus:ring-0 focus:ring-offset-0 text-right ml-2 pr-4 w-[200px] border-0"
                :placeholder="BalanceDue()"
              />
            </p>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
input,
textarea {
  border: 1px solid #ddd !important;
  padding: 5px;
  border-radius: 5px;
}

input.tax::-webkit-outer-spin-button,
input.tax::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
</style>
