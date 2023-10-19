<template>
  <div class="q-pa-md">
    <div class="row">
      <div class="col-12">
        <h3 class="text-center">Masukan NIK</h3>
      </div>
    </div>
    <div class="row">
      <div class="col-12 col-md-8 col-lg-6">
        <q-input v-model="text" filled type="textarea" />
      </div>
    </div>
    <div class="row" style="margin-top: 5px;">
      <div class="col-12 col-md-6 col-lg-3">
        <q-btn label="Cari" color="primary" style="width: 100%;" @click="this.check()" />
      </div>
    </div>
    <div class="row" style="margin-top: 20px;;">
      <div class="col-12">
        <q-table flat bordered title="Hasil Nik yang benar" :rows="rows" :columns="columns" row-key="name">
          <template v-slot:body="props">
            <q-tr :props="props">
              <q-td key="nik" :props="props">
                {{ props.row.nik }}
              </q-td>
              <q-td key="provinsi" :props="props">
                {{ props.row.provinsi }}
              </q-td>
              <q-td key="kabupaten" :props="props">
                {{ props.row.kabupaten }}
              </q-td>
              <q-td key="kecamatan" :props="props">
                {{ props.row.kecamatan }}
              </q-td>
            </q-tr>
          </template>
        </q-table>
      </div>
    </div>
    <div class="row" style="margin-top: 20px;;">
      <div class="col-12">
        <q-table flat bordered title="Hasil Nik tidak benar" :rows="rowsNotGet" :columns="columns" row-key="name">
          <template v-slot:body="props">
            <q-tr :props="props">
              <q-td key="nik" :props="props">
                {{ props.row.nik }}
              </q-td>
              <q-td key="provinsi" :props="props">
                {{ props.row.provinsi }}
              </q-td>
              <q-td key="kabupaten" :props="props">
                {{ props.row.kabupaten }}
              </q-td>
              <q-td key="kecamatan" :props="props">
                {{ props.row.kecamatan }}
              </q-td>
            </q-tr>
          </template>
        </q-table>
      </div>
    </div>
  </div>
</template>

<script>
import { defineComponent, ref } from 'vue';
import provinsiJson from "../store/provinsi.json";
import kabkotaJson from "../store/kabkota.json";
import kecamatanJson from "../store/kecamatan.json";

const columns = [
  {
    name: 'nik',
    required: true,
    label: 'NIK',
    align: 'left',
  },
  { name: 'provinsi', align: 'left', label: 'Provinsi', field: 'provinsi' },
  { name: 'kabupaten', align: 'left', label: 'kabupaten', field: 'kabupaten' },
  { name: 'kecamatan', align: 'left', label: 'kecamatan', field: 'kecamatan' },
]

export default defineComponent({
  name: 'IndexPage',
  setup() {
    const text = ref("");
    let rows = ref([]);
    let rowsNotGet = ref([]);

    return {
      text,
      rows,
      rowsNotGet,
      columns,
      check() {
        rows.value = [];
        rowsNotGet.value = [];
        text.value.split("\n").forEach(async x => {
          let nik = x.replace("'", "");
          let splitted = nik.split("");
          let provinsi = splitted[0] + splitted[1];
          let kabupaten = splitted[0] + splitted[1] + splitted[2] + splitted[3];
          let kecamatan = splitted[0] + splitted[1] + splitted[2] + splitted[3] + splitted[4] + splitted[5];

          if (provinsiJson.find(x => x.kode_dagri == provinsi) == null || kabkotaJson.find(x => x.kode_dagri == kabupaten) == null || kecamatanJson.find(x => x.kode_dagri == kecamatan) == null) {
            rowsNotGet.value.push({
              nik,
              provinsi: provinsiJson.find(x => x.kode_dagri == provinsi) != null ? provinsiJson.find(x => x.kode_dagri == provinsi).nama_dagri : null,
              kabupaten: kabkotaJson.find(x => x.kode_dagri == kabupaten) != null ? kabkotaJson.find(x => x.kode_dagri == kabupaten).nama_dagri : null,
              kecamatan: kecamatanJson.find(x => x.kode_dagri == kecamatan) != null ? kecamatanJson.find(x => x.kode_dagri == kecamatan).nama_dagri : null
            })
          } else {
            rows.value.push({
              nik,
              provinsi: provinsiJson.find(x => x.kode_dagri == provinsi) != null ? provinsiJson.find(x => x.kode_dagri == provinsi).nama_dagri : null,
              kabupaten: kabkotaJson.find(x => x.kode_dagri == kabupaten) != null ? kabkotaJson.find(x => x.kode_dagri == kabupaten).nama_dagri : null,
              kecamatan: kecamatanJson.find(x => x.kode_dagri == kecamatan) != null ? kecamatanJson.find(x => x.kode_dagri == kecamatan).nama_dagri : null
            });
          }
        });
      }
    }
  }
})

</script>
