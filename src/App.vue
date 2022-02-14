<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="SNOMED International Logo"
          class="shrink mr-2"
          contain
          src="https://www.snomed.org/SNOMED/media/SNOMED/other/brand-mark.png"
          transition="scale-transition"
          width="200"
          height="60"
        />
        <h1>SNOMED CT HEDIS Measures Coding Guide</h1>
      </div>
      <v-spacer></v-spacer>
      <v-btn color="purple lighten-1">
          <vue-excel-xlsx
            :data="bindings"
            :columns="columns"
            :file-type="'xlsx'"
            :file-name="'ecl-export'"
            :sheet-name="'sheet1'"
            >
            <v-icon right dark class="mr-4">mdi-cloud-download</v-icon>EXPORT BINDINGS
          </vue-excel-xlsx>
      </v-btn>
      <v-menu offset-y>
        <template v-slot:activator="{ on, attrs }">
          <v-btn
            color="purple lighten-1"
            dark
            v-bind="attrs"
            v-on="on"
            class="ml-4"
          >
            <v-icon dark>
              mdi-information
            </v-icon>
          </v-btn>
        </template>
        <v-list>
          <v-list-item-group>
            <v-list-item>
              <v-list-item-icon>
                <v-icon>mdi-github</v-icon>
              </v-list-item-icon>
              <v-list-item-title @click="goToGithub()">
                Source code on GitHub
              </v-list-item-title>
            </v-list-item>
          </v-list-item-group>
          <v-subheader>Bound to SNOMED CT Release:</v-subheader>
          <v-list-item>
            <v-list-item-title>
              Edition
            </v-list-item-title>
            <v-list-item-subtitle>
              Int. Edition
            </v-list-item-subtitle>
          </v-list-item>
          <v-list-item>
            <v-list-item-title>
              Version
            </v-list-item-title>
            <v-list-item-subtitle v-text="codeSystemVersionDisplay">
            </v-list-item-subtitle>
          </v-list-item>
        </v-list>
      </v-menu>
    </v-app-bar>

    <v-main>
      <MainTabs :sections="sections"/>
    </v-main>
  </v-app>
</template>
<style scoped>
b {
    font-weight: 900;
}
</style>
<script>
import MainTabs from './components/MainTabs.vue';

import VueExcelXlsx from "vue-excel-xlsx";
import Vue from "vue";

Vue.use(VueExcelXlsx);

export default {
  name: 'App',

  components: {
    MainTabs,
  },
  mounted() {
    this.bindings = [];
    for (const section in this.sections) {
      for (const binding in this.sections[section].bindings) {
        this.bindings.push({ section: this.sections[section].title, title: this.sections[section].bindings[binding].title, ecl: this.sections[section].bindings[binding].ecl.replace(/\s\s+/g, ' ') })
      }
    }
    Vue.prototype.$snowstormBase = 'https://snowstorm.ihtsdotools.org/snowstorm/snomed-ct';
    Vue.prototype.$codeSystemVersion = '2022-01-31';
    Vue.prototype.$snowstormBranch = `MAIN/${this.$codeSystemVersion}`;
    this.codeSystemVersionDisplay = this.$codeSystemVersion;
  },
  methods: {
    goToGithub() {
      window.open('https://github.com/alopezo/coding-demonstrator', '_blank');
    }
  },
  data: () => ({
    codeSystemVersionDisplay: '',
    bindings: [],
    columns : [
                    {
                        label: "Section",
                        field: "section",
                    },
                    {
                        label: "Title",
                        field: "title",
                    },
                    {
                        label: "ECL",
                        field: "ecl",
                    }
                ],
    sections: {
        'HEDIS-PREV': {
          title: 'Prevention and Screening',
          note: 'HEDIS® measures performance in health care where improvements can make a meaningful difference in people’s lives.',
          bindings: {
            'HEDIS-PREV-ABA' : {
              title: 'Adult BMI Assessment (ABA)',
              fieldTitle: 'BMI Observables',
              type: 'dropdown',
              ecl: `<< 60621009 |Body mass index (observable entity)|`,
              value: '',
              note: 'The percentage of members 18–74 years of age who had an outpatient visit and whose body mass index (BMI) was documented during the measurement year or the year prior to the measurement year.'
            },
            'HEDIS-PREV-ABA2' : {
              title: '',
              fieldTitle: 'BMI Findings',
              type: 'dropdown',
              ecl: `( << 404684003 |Clinical finding (finding)| : 363714003 |Interprets (attribute)| = << 60621009 |Body mass index (observable entity)| )`,
              value: '',
              note: ''
            },
            'HEDIS-PREV-COL' : {
              title: 'Colorectal Cancer Screening (COL)',
              fieldTitle: 'Fecal occult blood testing',
              type: 'dropdown',
              ecl: `<< 104435004 |Screening for occult blood in feces (procedure)| OR 
              << 59614000 |Occult blood in stools (finding)| OR
              ( << 404684003 |Clinical finding (finding)| : 363714003 |Interprets (attribute)| = 104435004 |Screening for occult blood in feces (procedure)| )`,
              value: '',
              note: 'Assesses adults 50–75 who had appropriate screening for colorectal cancer with any of the following tests: annual fecal occult blood test, flexible sigmoidoscopy every 5 years, colonoscopy every 10 years, computed tomography colonography every 5 years, stool DNA test every 3 years.'
            },
            'HEDIS-PREV-COL2' : {
              title: '',
              fieldTitle: 'Flexible sigmoidoscopy',
              type: 'dropdown',
              ecl: `<< 44441009 |Flexible fiberoptic sigmoidoscopy (procedure)| OR 
              ( << 404684003 |Clinical finding (finding)| : 363714003 |Interprets (attribute)| = << 44441009 |Flexible fiberoptic sigmoidoscopy (procedure)| )`,
              value: '',
              note: ''
            },
            'HEDIS-PREV-COL3' : {
              title: '',
              fieldTitle: 'Colonoscopy',
              type: 'dropdown',
              ecl: `<< 73761001 |Colonoscopy (procedure)| OR 
              ( << 404684003 |Clinical finding (finding)| : 363714003 |Interprets (attribute)| = << 73761001 |Colonoscopy (procedure)| )`,
              value: '',
              note: ''
            },
            'HEDIS-PREV-COL4' : {
              title: '',
              fieldTitle: 'Stool DNA',
              type: 'dropdown',
              ecl: `<< 708699002 |Stool DNA-based colorectal cancer screening positive (finding)|`,
              value: '',
              note: ''
            }
          }
        },
        'HEDIS-RESP': {
          title: 'Respiratory Conditions',
          note: 'HEDIS® measures performance in health care where improvements can make a meaningful difference in people’s lives.',
          bindings: {
            'HEDIS-RESP-CWPC' : {
              title: 'Appropriate Testing for Children with Pharyngitis (CWP) - Diagnosis',
              fieldTitle: 'Diagnosis',
              type: 'autocomplete',
              ecl: `<< 405737000 |Pharyngitis (disorder)|
              `,
              value: '',
              note: `Assesses the percentage of episodes for members 3 years of age and older with a <b>diagnosis of pharyngitis</b>, dispensed <b>an antibiotic</b> and received 
              <b>a group A streptococcus test</b> for the episode. A higher rate indicates completion of the appropriate testing required to merit antibiotic treatment for pharyngitis.`
            },
            'HEDIS-RESP-CWPT' : {
              title: '',
              fieldTitle: 'Streptococcus Tests',
              type: 'dropdown',
              ecl: `<< 71388002 |Procedure (procedure)| : 
              246093002 |Component (attribute)| = (<< 58800005 |Streptococcus| OR << 304058009 |Antigen of Streptococcus (substance)|)
              `,
              value: '',
              note: ``
            },
            'HEDIS-RESP-CWPA' : {
              title: '',
              fieldTitle: 'Antibiotics',
              type: 'autocomplete',
              ecl: `<< 373873005 |Pharmaceutical / biologic product (product)| : 
              766939001 |Plays role (attribute)| = 787994008 |Antibacterial therapeutic role (role)|
              `,
              value: '',
              note: ``
            }
          }
        },
        'HEDIS-CARD': {
          title: 'Cardiovascular Conditions',
          note: 'HEDIS® measures performance in health care where improvements can make a meaningful difference in people’s lives.',
          bindings: {
            'HEDIS-CARD-PBH' : {
              title: 'Persistence of Beta-Blocker Treatment After a Heart Attack (PBH)- Discharge diagnosis',
              fieldTitle: 'Diagnosis',
              type: 'dropdown',
              ecl: `<< 57054005 |Acute myocardial infarction (disorder)|
              `,
              value: '',
              note: `Assesses adults 18 years of age and older during the measurement year who were hospitalized and discharged alive 
              with a <b>diagnosis of acute myocardial infarction</b> and who received 
              persistent <b>beta-blocker treatment</b> for six months after discharge.`
            },
            'HEDIS-CARD-CWPT' : {
              title: '',
              fieldTitle: 'Medication',
              type: 'autocomplete',
              ecl: `<< 33252009 |Product containing beta adrenergic receptor antagonist (product)|
              `,
              value: '',
              note: ``
            }
          }
        }
      }
  }),
};
</script>
