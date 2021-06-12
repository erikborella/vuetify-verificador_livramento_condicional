<template>
    <v-container>
        <v-row class="text-center">
            <v-col>
                <v-fade-transition>
                    <v-chip 
                        class="mx-1"
                        v-if="porcentagemPenaCumprida"
                        transition="scale-transition"
                        >
                        {{ porcentagemPenaCumprida }}% Cumprido
                    </v-chip>
                </v-fade-transition>
                <v-fade-transition>
                    <v-chip 
                        class="mx-1"
                        v-if="cumpriuDoisTercosDaPena() && porcentagemPenaCumprida < 100"
                        transition="scale-transition"
                        text-color="white" 
                        color="red">
                        ⅔ Cumprido
                    </v-chip>
                </v-fade-transition>
                <v-fade-transition>
                    <v-chip 
                        class="mx-1"
                        v-if="cumpriuMetadeDaPena() && !cumpriuDoisTercosDaPena()"
                        transition="scale-transition"
                        text-color="white" 
                        color="blue">
                        ½ Cumprido
                    </v-chip>
                </v-fade-transition>
                <v-fade-transition>
                    <v-chip 
                        class="mx-1"
                        v-if="cumpriuUmTercoDaPena() && !cumpriuMetadeDaPena()"
                        transition="scale-transition"
                        text-color="white" 
                        color="green">
                        ⅓ Cumprido
                    </v-chip>
                </v-fade-transition>
            </v-col>
        </v-row>
        <v-row class="text-center">
            <v-col>
                <v-text-field
                    label="Pena total"
                    type="number"
                    suffix="Anos"
                    :rules="regrasValidacao.validacaoAnosTotais"
                    v-model="anosTotais"
                ></v-text-field>
            </v-col>
            <v-col>
                <v-text-field
                    label="Pena cumprida"
                    type="number"
                    suffix="Anos"
                    :rules="regrasValidacao.validacaoAnosCumpridos"
                    v-model="anosCumpridos"
                ></v-text-field>
            </v-col>
        </v-row>
        <v-row >
            <v-col cols="12" class="py-0">
                <v-checkbox
                class="py-0"
                label="Bons Comportamentos"
                v-model="temBonsComportamentos">
                </v-checkbox>
            </v-col>
            <v-col cols="12" class="py-0">
                <v-checkbox
                class="py-0"
                label="Reincidente em Crime Doloso"
                v-model="ehReincidenteEmCrimeDoloso">
                </v-checkbox>
            </v-col>
            <v-col cols="12" class="py-0">
                <v-checkbox
                class="py-0"
                label="Crime é considerado Hediondo (lei 8.079/90)"
                v-model="ehCrimeHediondo">
                </v-checkbox>
            </v-col>
        </v-row>
    </v-container>
</template>

<script>
export default {
    name: 'VerificadorLivramentoCondicional',
    
    data: () => ({
        anosTotais: null,
        anosCumpridos: null,

        ehReincidenteEmCrimeDoloso: false,
        ehCrimeHediondo: false,
        temBonsComportamentos: true,

        regrasValidacao: {
            validacaoAnosTotais: [
                v => !!v || "Informe a quantidade de anos!"
            ],
            validacaoAnosCumpridos: [
                v => !!v || "Informe a quantidade de anos!",
            ]
        }
    }),

    computed: {
        porcentagemPenaCumprida() {
            if (!this.anosTotais || !this.anosCumpridos)
                return 0;

            let porcentagem = (this.anosCumpridos / this.anosTotais) * 100;
            return parseInt(porcentagem);
        },
    },

    methods: {
        cumpriuUmTercoDaPena() {
            if (!this.anosTotais || !this.anosCumpridos)
                return false;

            let umTercoDaPena = this.anosTotais / 3;
            return this.anosCumpridos >= umTercoDaPena;
        },
        cumpriuMetadeDaPena() {
            if (!this.anosTotais || !this.anosCumpridos)
                return false;

            let metadeDaPena = this.anosTotais / 2;
            return this.anosCumpridos >= metadeDaPena;
        },
        cumpriuDoisTercosDaPena() {
            if (!this.anosTotais || !this.anosCumpridos)
                return false;

            let doisTercosDaPena = this.anosTotais * (2/3);
            return this.anosCumpridos >= doisTercosDaPena;
        },
    }

}
</script>