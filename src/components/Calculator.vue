<template>
    <div class="container my-4">
        <div class="card shadow-lg p-2">
            <div class="card-body">
                <h2 class="card-title text-center mb-4">Calculadora de Resistencias</h2>

                <!-- Selector de bandas -->
                <div class="d-flex justify-content-center flex-wrap gap-2 mb-4">
                    <button
                        @click="selectedBandCount = 4"
                        :class="{ 'btn-primary': selectedBandCount === 4, 'btn-outline-primary': selectedBandCount !== 4 }"
                        class="btn"
                    >
                        4 Bandas
                    </button>
                    <button
                        @click="selectedBandCount = 5"
                        :class="{ 'btn-primary': selectedBandCount === 5, 'btn-outline-primary': selectedBandCount !== 5 }"
                        class="btn"
                    >
                        5 Bandas
                    </button>
                    <button
                        @click="selectedBandCount = 6"
                        :class="{ 'btn-primary': selectedBandCount === 6, 'btn-outline-primary': selectedBandCount !== 6 }"
                        class="btn"
                    >
                        6 Bandas
                    </button>
                </div>

                <!-- Contenedor del resistor y sus bandas -->
                <div class="d-flex justify-content-center align-items-center mb-3 resistor-bg">
                    <!-- Cuerpo del resistor -->
                    <div
                        class="resistor-body bg-beige rounded-pill position-relative d-flex justify-content-around align-items-center mb-4 w-100"
                    >
                        <!-- Patas del resistor -->
                        <div
                            class="resistor-lead position-absolute bg-secondary rounded-pill start-0 translate-middle-x"
                        ></div>
                        <div
                            class="resistor-lead position-absolute bg-secondary rounded-pill end-0 translate-middle-x"
                        ></div>

                        <!-- Bandas de color -->
                        <div
                            class="resistor-band"
                            :style="{ backgroundColor: bands[selectedBand1].colorCode }"
                        ></div>
                        <div
                            class="resistor-band"
                            :style="{ backgroundColor: bands[selectedBand2].colorCode }"
                        ></div>
                        <div
                            v-if="selectedBandCount >= 5"
                            class="resistor-band"
                            :style="{ backgroundColor: bands[selectedBand3].colorCode }"
                        ></div>
                        <div
                            class="resistor-band"
                            :style="{ backgroundColor: multiplierBands[selectedBand4].colorCode }"
                        ></div>
                        <div
                            class="resistor-band"
                            :style="{ backgroundColor: toleranceBands[selectedBand5].colorCode }"
                        ></div>
                        <div
                            v-if="selectedBandCount === 6"
                            class="resistor-band"
                            :style="{ backgroundColor: tempCoeffBands[selectedBand6].colorCode }"
                        ></div>
                    </div>
                </div>

                <!-- Resultado de la calculadora -->
                <div class="p-1 bg-light text-center rounded">
                    <p class="h4">Valor de Resistencia</p>
                    <p class="display-5 fw-bold text-primary mt-2">
                        {{ formattedResistance }}
                    </p>
                </div>

                <!-- Controles para seleccionar las bandas -->
                <div class="row g-3 mt-4">
                    <div class="col-sm-6 col-md-4">
                        <label for="band1" class="form-label">Banda 1</label>
                        <select id="band1" v-model="selectedBand1" class="form-select">
                            <option v-for="(band, index) in bands" :value="index" :key="index">
                                {{ band.name }}
                            </option>
                        </select>
                    </div>
                    <div class="col-sm-6 col-md-4">
                        <label for="band2" class="form-label">Banda 2</label>
                        <select id="band2" v-model="selectedBand2" class="form-select">
                            <option v-for="(band, index) in bands" :value="index" :key="index">
                                {{ band.name }}
                            </option>
                        </select>
                    </div>
                    <div class="col-sm-6 col-md-4" v-if="selectedBandCount >= 5">
                        <label for="band3" class="form-label">Banda 3 (Cifra)</label>
                        <select id="band3" v-model="selectedBand3" class="form-select">
                            <option v-for="(band, index) in bands" :value="index" :key="index">
                                {{ band.name }}
                            </option>
                        </select>
                    </div>
                    <div class="col-sm-6 col-md-4">
                        <label for="band4" class="form-label">Multiplicador</label>
                        <select id="band4" v-model="selectedBand4" class="form-select">
                            <option v-for="(band, index) in multiplierBands" :value="index" :key="index">
                                {{ band.name }}
                            </option>
                        </select>
                    </div>
                    <div class="col-sm-6 col-md-4">
                        <label for="band5" class="form-label">Tolerancia</label>
                        <select id="band5" v-model="selectedBand5" class="form-select">
                            <option v-for="(band, index) in toleranceBands" :value="index" :key="index">
                                {{ band.name }}
                            </option>
                        </select>
                    </div>
                    <div class="col-sm-6 col-md-4" v-if="selectedBandCount === 6">
                        <label for="band6" class="form-label">Coeficiente Temp.</label>
                        <select id="band6" v-model="selectedBand6" class="form-select">
                            <option v-for="(band, index) in tempCoeffBands" :value="index" :key="index">
                                {{ band.name }}
                            </option>
                        </select>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "ResistorCalculator",
    data() {
        return {
            selectedBandCount: 4,
            bands: [
                { name: "Negro", value: 0, colorCode: "black" },
                { name: "Marrón", value: 1, colorCode: "saddlebrown" },
                { name: "Rojo", value: 2, colorCode: "red" },
                { name: "Naranja", value: 3, colorCode: "darkorange" },
                { name: "Amarillo", value: 4, colorCode: "gold" },
                { name: "Verde", value: 5, colorCode: "green" },
                { name: "Azul", value: 6, colorCode: "blue" },
                { name: "Violeta", value: 7, colorCode: "darkviolet" },
                { name: "Gris", value: 8, colorCode: "gray" },
                { name: "Blanco", value: 9, colorCode: "white" },
            ],
            multiplierBands: [
                { name: "Negro", multiplier: 1, colorCode: "black" },
                { name: "Marrón", multiplier: 10, colorCode: "saddlebrown" },
                { name: "Rojo", multiplier: 100, colorCode: "red" },
                { name: "Naranja", multiplier: 1000, colorCode: "darkorange" },
                { name: "Amarillo", multiplier: 10000, colorCode: "gold" },
                { name: "Verde", multiplier: 100000, colorCode: "green" },
                { name: "Azul", multiplier: 1000000, colorCode: "blue" },
                { name: "Violeta", multiplier: 10000000, colorCode: "darkviolet" },
                { name: "Gris", multiplier: 100000000, colorCode: "gray" },
                { name: "Blanco", multiplier: 1000000000, colorCode: "white" },
                { name: "Oro", multiplier: 0.1, colorCode: "#FFD700" },
                { name: "Plata", multiplier: 0.01, colorCode: "#C0C0C0" },
            ],
            toleranceBands: [
                { name: "Marrón (±1%)", tolerance: 1, colorCode: "saddlebrown" },
                { name: "Rojo (±2%)", tolerance: 2, colorCode: "red" },
                { name: "Verde (±0.5%)", tolerance: 0.5, colorCode: "green" },
                { name: "Azul (±0.25%)", tolerance: 0.25, colorCode: "blue" },
                { name: "Violeta (±0.1%)", tolerance: 0.1, colorCode: "darkviolet" },
                { name: "Gris (±0.05%)", tolerance: 0.05, colorCode: "gray" },
                { name: "Oro (±5%)", tolerance: 5, colorCode: "#FFD700" },
                { name: "Plata (±10%)", tolerance: 10, colorCode: "#C0C0C0" },
                { name: "Sin color (±20%)", tolerance: 20, colorCode: "transparent" },
            ],
            tempCoeffBands: [
                { name: "Marrón (100 ppm/°C)", tempCoeff: 100, colorCode: "saddlebrown" },
                { name: "Rojo (50 ppm/°C)", tempCoeff: 50, colorCode: "red" },
                { name: "Naranja (15 ppm/°C)", tempCoeff: 15, colorCode: "darkorange" },
                { name: "Amarillo (25 ppm/°C)", tempCoeff: 25, colorCode: "gold" },
                { name: "Azul (10 ppm/°C)", tempCoeff: 10, colorCode: "blue" },
                { name: "Violeta (5 ppm/°C)", tempCoeff: 5, colorCode: "darkviolet" },
            ],
            selectedBand1: 0,
            selectedBand2: 0,
            selectedBand3: 0,
            selectedBand4: 0,
            selectedBand5: 6,
            selectedBand6: 0,
        };
    },
    computed: {
        resistance() {
            const value1 = this.bands[this.selectedBand1].value;
            const value2 = this.bands[this.selectedBand2].value;
            const multiplier = this.multiplierBands[this.selectedBand4].multiplier;
            let resistanceValue = 0;

            if (this.selectedBandCount === 4) {
                resistanceValue = (value1 * 10 + value2) * multiplier;
            } else if (this.selectedBandCount >= 5) {
                const value3 = this.bands[this.selectedBand3].value;
                resistanceValue = (value1 * 100 + value2 * 10 + value3) * multiplier;
            }
            return resistanceValue;
        },
        formattedResistance() {
            const value = this.resistance;
            const tolerance = this.toleranceBands[this.selectedBand5].tolerance;
            const tempCoeff = this.selectedBandCount === 6 ? this.tempCoeffBands[this.selectedBand6].tempCoeff : null;

            let result = '';
            if (value >= 1000000) {
                result = `${(value / 1000000).toFixed(2)} MΩ`;
            } else if (value >= 1000) {
                result = `${(value / 1000).toFixed(2)} kΩ`;
            } else {
                result = `${value.toFixed(2)} Ω`;
            }

            result += ` ±${tolerance}%`;

            if (tempCoeff !== null) {
                result += ` (${tempCoeff} ppm/°C)`;
            }

            return result;
        },
    },
};
</script>

<style scoped>
.resistor-body {
    width: 20rem;
    height: 3.2rem;
    background-color: #f8bd8f;
    padding: 1rem;
}

.resistor-band {
    width: .9rem;
    height: 3.25rem;
    border-radius: 0.20rem;
}

.resistor-lead {
    width: 0.5rem;
    height: 5rem;
    top: 50%;
}

.resistor-bg {
    padding: 1.5rem;
    background-color: var(--color-bg-primary);
    border-radius: 40px;
    height: 12rem;
}
@media (min-width: 425px) {
    .resistor-bg {
        margin: 0 auto;
        width: 15rem;
    }
}
</style>