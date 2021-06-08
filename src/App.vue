<template>
  <v-app>
    <v-main>
      <v-container fill-height fluid>
        <v-row align="center" justify="center">
          <v-card
            width="900"
          >
            <v-card-title
              class="primary white--text titulo"
            >
              Conversões de Sistemas Numéricos
            </v-card-title>
            <v-card-text class="pa-6">
              <v-row align="center" justify="center" class="pa-2">
                <v-col cols="12" md="6">
                  <v-select
                    v-model="item"
                    :items="items"
                    label="Converter"
                    outlined
                    @change="limpar"
                  ></v-select>
                </v-col>
                <v-col cols="12" md="6">
                  <v-text-field
                    v-model="valor"
                    label="Valor"
                    outlined
                  ></v-text-field>
                </v-col>
              </v-row>
              <v-row align="center" justify="center" class="pa-2">
                <v-btn
                  color="success"
                  align="center"
                  large
                  @click="calcular"
                >
                  Converter
                </v-btn>
              </v-row>
              <v-row align="center" justify="center" class="pa-2">
                <v-textarea
                  v-model="resultado"
                  label="Resultado"
                  class="mt-5 pa-3"
                  outlined
                  rows="2"
                  readonly
                >
                </v-textarea>
              </v-row>
              <v-row align="center" justify="center" class="pa-2 text-center footer">
                Trabalho desenvolvido na disciplina de Organização e Arquitetura de Computadores
              </v-row>
            </v-card-text>
          </v-card>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: 'Conversoes',

  data: () => ({
    item: null,
    valor: null,
    resultado: null,

    items: [{
      text: 'Decimal para Binário', value: 'decimal_binario'
    }, {
      text: 'Decimal para Octal', value: 'decimal_octal'
    }, {
      text: 'Decimal para Hexadecimal', value: 'decimal_hexadecimal'
    }, {
      text: 'Binário para Decimal', value: 'binario_decimal'
    }, {
      text: 'Octal para Decimal', value: 'octal_decimal'
    }, {
      text: 'Hexadecimal para Decimal', value: 'hexadecimal_decimal'
    }]
  }),

  methods: {
    limpar() {
      this.resultado = null
    },

    calcular() {
      switch (this.item) {
        case 'decimal_binario':
          this.decimalPara(this.valor, 2)
          break
        case 'decimal_octal':
          this.decimalPara(this.valor, 8)
          break
        case 'decimal_hexadecimal':
          this.decimalPara(this.valor, 16)
          break
        case 'binario_decimal':
          this.binarioParaDecimal(this.valor)
          break
        case 'octal_decimal':
          this.octalParaDecimal(this.valor)
          break
        case 'hexadecimal_decimal':
          this.hexadecimalParaDecimal(this.valor)
          break
      }
    },

    decimalPara(num, base) {
      if (num > 0) {
        let resultado = ''
        while (num > 0) {
          let resto = num % base
          if (base === 16) {
            switch(resto) {
              case 10: 
                resto = 'A'
                break
              case 11:
                resto = 'B'
                break
              case 12:
                resto = 'C'
                break
              case 13:
                resto = 'D'
                break
              case 14: 
                resto = 'E'
                break
              case 15: 
                resto = 'F'
                break
            }
          }
          num = parseInt(num / base)
          resultado = resto + resultado
        }
        this.resultado = resultado
      } else {
        this.resultado = 'Valor inválido!'
      }
    },

    binarioParaDecimal(num) {
      if (num >= 0) {
        let numero = num.split('')
        try{
          numero.forEach(n => {
          if (n != 0 && n != 1) {
            this.resultado = 'Valor inválido!'
            throw 'Valor inválido!'
          } else {
            let decimal = 0
            for (let i = 0; i < num.length; i++) {
              if (num[num.length - (i + 1)] === '1') {
              decimal += 2 ** i
              }
            }
            this.resultado = decimal
          }
        }) 
        } catch(e) {
          this.resultado = 'Valor inválido!'
        }
      } else {
        this.resultado = 'Valor inválido!'
      }
    },

    octalParaDecimal(num) {
      if (num > 0) {
        if(!num.includes(8) && !num.includes(9)) {
          let decimal = 0
          for (let i = 0; i < num.length; i++) {
            if (num[num.length - (i + 1)] !== '0') {
              decimal += (8 ** i) * num[num.length - (i + 1)]
            }
          }
          this.resultado = decimal
        } else {
        this.resultado = 'O valor não pode conter 8 ou 9.'
        }
      } else {
        this.resultado = 'Valor inválido!'
      }

    },

    hexadecimalParaDecimal(num) {
      let valor = num.split('')
      let v = ''

      try {
        valor.forEach(n => {
          if (!['A', 'B', 'C', 'D', 'E', 'F', '0', '1', '2', '3', '4', '5', '6', '7', '8', '9'].includes(n)) {
            this.resultado = 'Valor inválido!'
            throw 'Valor inválido!'
          } else {
            v += n
          }
        })

        let numero = v

        numero = numero.replaceAll('A', '10')
                      .replaceAll('B', '11')
                      .replaceAll('C', '12')
                      .replaceAll('D', '13')
                      .replaceAll('E', '14')
                      .replaceAll('F', '15')

        let decimal = 0

        for (let i = 0; i < numero.length; i++) {
          if (numero[numero.length - (i + 1)] !== '0') {
            decimal += (16 ** i) * numero[numero.length - (i + 1)]
          }
        }
        this.resultado = decimal
      } catch (e) {
        this.resultado = 'Valor inválido!'
      }
    }  
  }
}
</script>

<style>
  @media(max-width:500px) {
    .titulo {
      font-size: 1rem !important;
    }

    .footer {
      font-size: 0.85rem !important;
    }
  }
</style>
