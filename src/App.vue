<script setup lang="ts">
import { ref, type VNodeRef } from "vue";
import QRCode from "qrcode";
import JsBarcode from "jsbarcode";

const inputValue = ref("");
const qrDataUrl = ref<string | null>(null);
const barcode = ref(null);
const downloadLink = ref<VNodeRef | null>(null);

const generateCodes = async () => {
    if (!inputValue.value || String(inputValue.value).trim() === "") {
        alert("Please enter a number");
        return;
    }

    const value = String(inputValue.value);

    qrDataUrl.value = await QRCode.toDataURL(value, {
        width: 200,
        margin: 2,
    });

    JsBarcode(barcode.value, value, {
        format: "CODE128",
        width: 2,
        height: 100,
        displayValue: true,
    });
};

const downloadQR = () => {
    downloadLink.value?.click();
};


</script>

<template>
    <div class="p-6 flex flex-col items-center space-y-4">
        <input v-model="inputValue" type="number" placeholder="Enter number" class="border p-2 rounded"
            @keyup.enter="generateCodes" />

        <button @click="generateCodes" class="bg-blue-500 text-white px-4 py-2 rounded shadow">
            Generate
        </button>

        <div v-if="qrDataUrl" class="flex flex-col items-center">
            <img :src="qrDataUrl" class="border p-2" />

            <a ref="downloadLink" :href="qrDataUrl" download="qrcode.png" class="hidden"></a>

            <button @click="downloadQR" class="mt-2 bg-green-500 text-white px-3 py-1 rounded">
                Download QR
            </button>
        </div>

        <svg ref="barcode"></svg>
    </div>
</template>


