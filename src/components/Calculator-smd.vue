<template>
  <div class="container my-5">
    <div class="card shadow-lg p-4">
      <div class="card-body">
        <h2 class="card-title text-center mb-4">Calculadora de Resistencias SMD</h2>
        <p class="text-center text-muted mb-4">
          Ingresa el código de 3, 4 dígitos o el código EIA-96 (letras).
        </p>

        <!-- Input para el código SMD -->
        <div class="d-flex justify-content-center mb-4">
          <div class="input-group" style="max-width: 250px;">
            <input 
              type="text" 
              class="form-control form-control-lg text-center bg-dark text-white" 
              v-model="smdCode"
              placeholder="__________"
              maxlength="4"
            >
          </div>
        </div>

        <!-- Resultado de la calculadora -->
        <div class="p-4 bg-light text-center rounded">
          <p class="h4 text-muted">Valor de Resistencia</p>
          <p class="display-4 fw-bold text-primary mt-2">
            {{ formattedResistance.value }}
          </p>
          <p v-if="formattedResistance.tolerance" class="text-muted">
            Tolerancia: {{ formattedResistance.tolerance }}
          </p>
          <p v-if="error" class="text-danger mt-3">
            {{ error }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "SmdResistorCalculator",
  data() {
    return {
      smdCode: '',
      error: '',
    };
  },
  computed: {
    formattedResistance() {
      const code = this.smdCode.trim().toUpperCase();
      this.error = ''; // Limpiar errores previos

      if (!code) {
        return { value: '---', tolerance: '' };
      }

      // Lógica para el estándar EIA-96 (2 dígitos + 1 letra)
      const eia96Regex = /^\d{2}[A-Z]$/;
      if (eia96Regex.test(code)) {
        const valueCode = code.substring(0, 2);
        const multiplierCode = code.substring(2, 3);
        
        const valueTable = {
          '01': 100, '02': 102, '03': 105, '04': 107, '05': 110, '06': 113, '07': 115, '08': 118, '09': 121, '10': 124,
          '11': 127, '12': 130, '13': 133, '14': 137, '15': 140, '16': 143, '17': 147, '18': 150, '19': 154, '20': 158,
          '21': 162, '22': 165, '23': 169, '24': 174, '25': 178, '26': 182, '27': 187, '28': 191, '29': 196, '30': 200,
          '31': 205, '32': 210, '33': 215, '34': 221, '35': 226, '36': 232, '37': 237, '38': 243, '39': 249, '40': 255,
          '41': 261, '42': 267, '43': 274, '44': 280, '45': 287, '46': 294, '47': 301, '48': 309, '49': 316, '50': 324,
          '51': 332, '52': 340, '53': 348, '54': 357, '55': 365, '56': 374, '57': 383, '58': 392, '59': 402, '60': 412,
          '61': 422, '62': 432, '63': 442, '64': 453, '65': 464, '66': 475, '67': 487, '68': 499, '69': 511, '70': 523,
          '71': 536, '72': 549, '73': 562, '74': 576, '75': 590, '76': 604, '77': 619, '78': 634, '79': 649, '80': 665,
          '81': 681, '82': 698, '83': 715, '84': 732, '85': 750, '86': 768, '87': 787, '88': 806, '89': 825, '90': 845,
          '91': 866, '92': 887, '93': 909, '94': 931, '95': 953, '96': 976,
        };
        const multiplierTable = {
          'A': 1e0, 'B': 1e1, 'C': 1e2, 'D': 1e3, 'E': 1e4, 'F': 1e5, 'X': 1e-1, 'Y': 1e-2, 'Z': 1e-3,
          // Códigos de tolerancia para la serie 1% (E96)
          'F': 1e0, 'G': 1e1, 'H': 1e2, 'J': 1e3, 'K': 1e4, 'L': 1e5, 'S': 1e-1, 'T': 1e-2, 'U': 1e-3,
        };
        const toleranceTable = {
          'A': '±1%', 'B': '±1%', 'C': '±1%', 'D': '±1%', 'E': '±1%', 'F': '±1%', 'X': '±1%', 'Y': '±1%', 'Z': '±1%', 
          'G': '±2%', 'H': '±2%', 'J': '±5%', 'K': '±10%', 'M': '±20%',
        };
        
        const numericValue = valueTable[valueCode];
        const multiplier = multiplierTable[multiplierCode];
        const tolerance = toleranceTable[multiplierCode] || '';

        if (numericValue && multiplier) {
          const value = numericValue * multiplier;
          return {
            value: this.formatValue(value),
            tolerance: tolerance
          };
        }
      }

      // Validar que el código solo contenga dígitos para los formatos de 3 y 4
      if (!/^\d+$/.test(code)) {
        this.error = 'Formato inválido. Usa solo números o el formato EIA-96 (Ej: 25A).';
        return { value: 'Inválido', tolerance: '' };
      }

      // Lógica para código de 3 dígitos (ej. 103)
      if (code.length === 3) {
        const d1 = parseInt(code[0]);
        const d2 = parseInt(code[1]);
        const m = parseInt(code[2]);
        const value = (d1 * 10 + d2) * (10 ** m);
        return { value: this.formatValue(value), tolerance: '' };
      } 
      
      // Lógica para código de 4 dígitos (ej. 4701)
      if (code.length === 4) {
        const d1 = parseInt(code[0]);
        const d2 = parseInt(code[1]);
        const d3 = parseInt(code[2]);
        const m = parseInt(code[3]);
        const value = (d1 * 100 + d2 * 10 + d3) * (10 ** m);
        return { value: this.formatValue(value), tolerance: '' };
      } 
      
      this.error = 'El código debe tener 3 o 4 dígitos, o seguir el formato EIA-96 (ej. 25A).';
      return { value: 'Inválido', tolerance: '' };
    },
  },
  methods: {
    formatValue(value) {
      if (value >= 1000000) {
        return `${(value / 1000000).toFixed(2)} MΩ`;
      } else if (value >= 1000) {
        return `${(value / 1000).toFixed(2)} kΩ`;
      } else {
        return `${value.toFixed(2)} Ω`;
      }
    }
  }
};
</script>

<style scoped>
.card {
  border-radius: 1rem;
}

.form-control-lg {
  border-radius: 0.75rem;
  border-color: #dee2e6;
  transition: border-color 0.2s ease-in-out;
  padding: 2rem;
  font-size: 2rem;
}

.form-control-lg:focus {
  border-color: #0d6efd;
  box-shadow: 0 0 0 0.25rem rgba(13, 110, 253, 0.25);
}
.form-control-lg::placeholder{
    color: white;
}
</style>
