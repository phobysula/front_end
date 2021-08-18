<template>
<q-page padding>
    <div class="q-pa-md">
      <q-table
        title="Data Nilai"
        :data="data"
        :columns="columns"
        row-key="name"
      >
        <template v-slot:body="props">
          <q-tr :props="props">
           <q-td key="nama_kegiatan" :props="props">
              {{ props.row.nama_kegiatan}}
            </q-td>
            <q-td key="Nilai_skill" :props="props">
              {{ props.row.Nilai_skill}}
            </q-td>
            <q-td key="Nilai_matakuliah" :props="props">
              {{ props.row.Nilai_matakuliah}}
            </q-td>
          </q-tr>
        </template>
      </q-table>

    </div>
  </q-page>
  </template>
<script>
export default {
  data () {
    return {
      columns: [
        { name: 'nama_kegiatan', align: 'left', label: 'Nama Kegiatan', field: 'nama_kegiatan', sortable: true },
        { name: 'Nilai_skill', align: 'left', label: 'Nilai Skill', field: 'Nilai_skill', sortable: true },
        { name: 'Nilai_matakuliah', align: 'left', label: 'Nilai Mata Kuliah', field: 'aksi' }
      ],

      data: []
    }
  },
  created () {
    this.getdata()
  },
  methods: {
    getdata () {
      const dataMhs = this.$q.localStorage.getItem('dataUser')
      this.$axios.get('inputnilai/getbynpm/' + dataMhs.username)
        .then(res => {
          this.data = res.data.data
        })
    },
    confirm (id) {
      this.$q.dialog({
        title: 'Confirm',
        message: 'Would you like to delete this data?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.$axios.delete('akhir/delete/' + id)
          .then(res => {
            if (res.data.sukses) {
              this.$showNotif(res.data.massage, 'positive')
              this.getdata()
            } else {
              this.$showNotif(res.data.massage, 'negative')
            }
          })
      })
    }
  }
}
</script>
