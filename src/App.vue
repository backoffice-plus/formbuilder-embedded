<template>

  <header>
    <h1 class="text-2xl p-4 text-center">Formbuilder Demo</h1>
  </header>

  <main class="container mx-auto flex flex-col gap-4">

    <h2 class="text-xl">1. As Formbuilder Component</h2>
    <section>
        <FormBuilder
            :jsonFormsRenderers="jsonFormsRenderers"
            :tools="tools"
            class="flex-grow"
            @schemaUpdated="(e)=>resultFormBuilder=e"
        />
        <aside>
          <FormPreview :jsonform="resultFormBuilder" />
        </aside>
    </section>



    <h2 class="text-xl">2. At JsonForms as FormbuilderRenderer (schemaOnly)</h2>
    <section>
      <div class="styleA flex-grow">
        <JsonForms
            :schema="formJson.schema"
            :uischema="formJson.uischema"
            :data="{}"
            :renderers="jsonFormsRenderers"
            @change="(e)=>resultJsonForms=e?.data"
        />
      </div>

      <aside>
        <FormPreview :jsonform="resultJsonForms" />
      </aside>
    </section>

  </main>

</template>

<style>
body {
  @apply bg-gray-50
}
main > section {
  @apply
  bg-white shadow p-4 mb-8
  flex flex-row
}
main > section > aside {
  @apply w-4/12 ml-4 flex flex-col
}
</style>


<script setup lang="ts">
import {provide, ref} from "vue";
import {JsonForms} from "@jsonforms/vue";
import {vanillaRenderers} from "@jsonforms/vue-vanilla";
import {FormBuilder, defaultTools, boplusVueVanillaRenderers, formbuilderRenderers } from '@backoffice-plus/formbuilder'
import '@backoffice-plus/formbuilder/style.css'
import './style.css'
import './form.stylea.css'
import FormPreview from "@/components/FormPreview.vue";


const tools = [
  ...defaultTools,
]

const jsonFormsRenderers = Object.freeze([
  ...vanillaRenderers,
  ...boplusVueVanillaRenderers,
  ...formbuilderRenderers,
]);

const formJson = {
  "schema": {
    "type": "object",
    "properties": {
      "name": {
        "type": "string"
      },
      "schema": {
        "type": "object"
      }
    }
  },
  "uischema":  {
    "type": "VerticalLayout",
    "elements": [
      {
        "type": "Control",
        "scope": "#/properties/name"
      },
      {
        "type": "Disclosure",
        "label": "Schema",
        "elements": [
          {
            "type": "Formbuilder",
            "scope": "#/properties/schema",
            "options": {
              "schemaOnly": true
            }
          }
        ]
      }
    ]
  }
}

provide('formbuilder-tools', tools)

const resultFormBuilder = ref();
const resultJsonForms = ref();
</script>

