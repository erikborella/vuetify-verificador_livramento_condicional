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
                        v-if="cumpriuDoisTercosDaPena && porcentagemPenaCumprida < 100"
                        transition="scale-transition"
                        text-color="white" 
                        color="red">
                        ⅔ Cumprido
                    </v-chip>
                </v-fade-transition>
                <v-fade-transition>
                    <v-chip 
                        class="mx-1"
                        v-if="cumpriuMetadeDaPena && !cumpriuDoisTercosDaPena"
                        transition="scale-transition"
                        text-color="white" 
                        color="blue">
                        ½ Cumprido
                    </v-chip>
                </v-fade-transition>
                <v-fade-transition>
                    <v-chip 
                        class="mx-1"
                        v-if="cumpriuUmTercoDaPena && !cumpriuMetadeDaPena"
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
                label="Bons Antecedentes"
                v-model="temBonsAntecedentes">
                </v-checkbox>
            </v-col>
            <v-col cols="12" class="py-0">
                <v-checkbox
                class="py-0"
                :label="(ehCrimeHediondo) ? 'É reincidente no crime':'É reincidente em crime doloso'"
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
        <v-row class="text-center mu-5" v-if="aptoAoLivramento != null">
            <v-col cols="12">
                <div class="text-h5">
                    Status:
                </div>
            </v-col>
            <v-col>
                <div :class="(aptoAoLivramento.status ? 'green':'red') + '--text'">
                    {{ aptoAoLivramento.status ? 'Apto ao livramento':'Não apto ao livramento: ' }}
                </div>
                <li v-for="motivo in aptoAoLivramento.motivos" :key="motivo">
                    {{ motivo }}
                </li>
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
        temBonsAntecedentes: true,

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
        
        aptoAoLivramento() {
            if (!this.anosTotais || !this.anosCumpridos)
                return null;

            let motivos = this.verificarMotivosNaoApto();
    
            return {
                'status': motivos.length === 0,
                'motivos': motivos
            }
        },
    },

    methods: {
        setEstaAptoAoLivramento(condicao) {
            this.estaAptoAoLivramento = condicao;
        },

        penaEhInferior2Anos(pena) {
            return pena <= 2;
        },

        verificarMotivosNaoApto() {
            let motivos = [];

            if (this.penaEhInferior2Anos(this.anosTotais)) {
                motivos.push("Pena não é superior a 2 anos");
            }

            else if (this.ehCrimeHediondo) {
                if (!this.cumpriuDoisTercosDaPena)
                    motivos.push("Não cumpriu ⅔ da pena");

                if (this.ehReincidenteEmCrimeDoloso)
                    motivos.push("É reincidente no crime");
            }

            else if (this.ehReincidenteEmCrimeDoloso) {
                if (!this.cumpriuMetadeDaPena)
                    motivos.push("Não cumpriu ½ da pena");
            } 
            
            else {
                if (!this.cumpriuUmTercoDaPena)
                    motivos.push("Não cumpriu ⅓ da pena");
                
                if (!this.temBonsAntecedentes)
                    motivos.push("Não tem bons antecedentes");
            }

            return motivos;
        }
    }

}
</script>