<template>
    <v-text-field variant="outlined" density="compact" :label="props.label" prepend-inner-icon="$calendar"
        @click="dialog = true" v-model="fecha" @input="$emit('update:modelValue', $event)" ></v-text-field>

    <v-dialog v-model="dialog" width="auto">
        <v-card max-width="400" prepend-icon="$calendar" :title="props.title" :style="props.mode == 'light' ? styleCardLight : styleCardDark">
            <v-tabs v-model="tab" align-tabs="center">
                <v-tab value="date"><v-icon icon="$calendar"></v-icon></v-tab>
                <v-tab value="time" :disabled="!date"><v-icon icon="mdi-clock-outline"></v-icon></v-tab>
            </v-tabs>
            <v-card-text>
                <v-tabs-window v-model="tab">
                    <v-tabs-window-item value="date">
                        <v-date-picker :hide-header="true" :show-adjacent-months="true" v-model="date"
                            @change="emitValue" :style="props.mode == 'light' ? styleDateLight : styleDateDark"></v-date-picker>
                    </v-tabs-window-item>
                    <v-tabs-window-item value="time">
                        <VTimePicker format="24hr" v-model="time" @change="emitValue" :style="props.mode == 'light' ? styleTimeLight : styleTimeDark"></VTimePicker>
                    </v-tabs-window-item>
                </v-tabs-window>
            </v-card-text>
            <v-card-actions>
                <v-btn @click="clear">Cancelar</v-btn>
                <v-btn :color="props.color" variant="elevated" @click="emitValue(); dialog = false" :disabled="!date || !time">OK</v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>
</template>

<script setup>
import { ref, defineProps, defineEmits } from 'vue';
import { VDatePicker } from 'vuetify/components/VDatePicker';
import { VTimePicker } from 'vuetify/labs/VTimePicker';

const props = defineProps({
    modelValue: String,
    label: String,
    title: String,
    color: String,
    mode: String
});

const date = ref(null);
const time = ref(null);
const dialog = ref(false);
const tab = ref('date');

const fecha = ref(null);

const emit = defineEmits(['update:modelValue']);

function clear() {
    date.value = null;
    time.value = null;
    dialog.value = false;
}

function emitValue() {
    if (date.value && time.value) {
        const formattedDate = `${date.value.getFullYear()}-${('0' + (date.value.getMonth() + 1)).slice(-2)}-${('0' + date.value.getDate()).slice(-2)}`;
        const formattedTime = `${time.value}`;
        fecha.value = `${formattedDate} ${formattedTime}`;
        emit('update:modelValue', fecha.value);
    }
}

const styleCardDark = 'background-color: #212121; color: #fff';
const styleDateDark = 'background-color: #151515; color: #fff';
const styleTimeDark = 'background-color: #151515; color: #fff';

const styleCardLight = 'background-color: #fff; color: #000';
const styleDateLight = 'background-color: #fff; color: #000';
const styleTimeLight = 'background-color: #fff; color: #000';
</script>
