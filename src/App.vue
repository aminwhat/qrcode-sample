<script setup lang="ts">
import { reactive, ref, type VNodeRef } from "vue";
import QRCode from "qrcode";
import JsBarcode from "jsbarcode";


type State = {
    inputValue: string;
    qrDataUrl: string | null;
    barcode: string;
    downloadLink: VNodeRef | null
}

const state = reactive<State>({
    inputValue: '',
    qrDataUrl:  null,
    barcode: '',
    downloadLink:  null,
})

const generateCodes = async () => {
    if (!state.inputValue || String(state.inputValue).trim() === "") {
        alert("Please enter a number");
        return;
    }

    const value = String(state.inputValue);

    state.qrDataUrl = await QRCode.toDataURL(value, {
        width: 200,
        margin: 2,
    });

    JsBarcode(state.barcode, value, {
        format: "CODE128",
        width: 2,
        height: 100,
        displayValue: true,
    });
};

const downloadQR = () => {
    state.downloadLink?.click();
};


</script>

<template>
    <div class="p-6 flex flex-col items-center space-y-4">
        <input v-model="state.inputValue" type="number" placeholder="Enter number" class="border p-2 rounded"
            @keyup.enter="generateCodes" />

        <button @click="generateCodes" class="bg-blue-500 text-white px-4 py-2 rounded shadow">
            Generate
        </button>

        <div v-if="state.qrDataUrl" class="flex flex-col items-center">
            <img :src="state.qrDataUrl" class="border p-2" />

            <a ref="downloadLink" :href="state.qrDataUrl" download="qrcode.png" class="hidden"></a>

            <button @click="downloadQR" class="mt-2 bg-green-500 text-white px-3 py-1 rounded">
                Download QR
            </button>
        </div>

        <svg ref="barcode"></svg>
    </div>
</template>
