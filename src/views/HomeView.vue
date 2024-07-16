<script setup>
import { computed } from 'vue';
import { date, Dialog, Notify } from 'quasar';
import { useNotesStore } from '../stores/notes';

const notesStore = useNotesStore();

const searchResult = computed(() => {
  return notesStore.notes.filter((note) => {
    return (
      note.title.toLowerCase().indexOf(notesStore.searchQuery.toLowerCase()) != -1 || note.content.toLowerCase().indexOf(notesStore.searchQuery.toLowerCase()) != -1
    );
  });
});

function confirm(id) {
    Dialog.create({
        dark: true,
        title: 'Xác Nhận!',
        color: 'primary',
        message: 'Bạn có chắc chắn muốn xóa ghi chú này không?',
        cancel: true,
        persistent: true
    }).onOk(() => {
        try {
            notesStore.deleteNote(id)
            Notify.create({
                message: 'Ghi chú đã được xóa thành công!',
                type: "positive",
                actions: [
                    { icon: 'close', color: 'white', round: true, }
                ]
            })
        } catch (error) {
            Notify.create({
                message: error.message,
                type: "negative",
                actions: [
                    { icon: 'close', color: 'white', round: true, }
                ]
            })
        }
    })
};

</script>

<template>
  <q-page class="" style="display: flex; flex-direction: column; ">

    <q-separator />

    <div v-if="notesStore.notes.length > 0">
      <div class="row">
        <div class="col-12 col-sm-6 col-md-4 col-lg-3" v-for="(note, index) in searchResult" :key="note.id">
          <q-card class="note-card q-mt-md" flat bordered>

            <q-card-section>
              <div class="row items-center no-wrap">
                <div class="col">
                  <div class="text-h6">{{ note.title }}</div>
                  <div class="text-subtitle2">
                    {{ date.formatDate(note.dateAdded, 'DD MMMM YYYY') }}
                    <q-badge clickable rounded color="primary" class="q-mx-xs" v-for="tag in note.tags" :key="tag.id">
                      <q-breadcrumbs-el :label="tag.name" :to="{ name: 'tag-detail', params: { id: tag.id } }" />
                    </q-badge>
                  </div>
                </div>

                <div class="col-auto">
                  <q-btn color="grey-7" round flat icon="more_vert">
                    <q-menu cover auto-close>
                      <q-list>
                        <q-item clickable :to="{ name: 'note-detail', params: { id: note.id } }">
                          <q-item-section>Chi Tiết</q-item-section>
                        </q-item>
                        <q-item clickable>
                          <q-item-section color="negative" @click="confirmDelete(note.id)">Xóa</q-item-section>
                        </q-item>
                      </q-list>
                    </q-menu>
                  </q-btn>
                </div>
              </div>
            </q-card-section>

            <q-separator dark inset />

            <q-card-section horizontal>
              <q-card-section>
                {{ note.content.substring(0, 200) }}
              </q-card-section>
            </q-card-section>

            <q-separator />

            <q-card-actions>
              <q-btn color="info" flat :to="{ name: 'note-detail', params: { id: note.id } }">Chi Tiết Ghi Chú</q-btn>
            </q-card-actions>
          </q-card>
        </div>
      </div>
    </div>
    <div v-else>
      <p style="font-weight: bold; font-size: 30px; text-align: center; margin-top: 100px;">Không tìm thấy ghi chú nào, bấm vào dấu cộng (+) để thêm ghi chú mới
        <img style="position: absolute; rotate: 270deg; width: 650px; margin-left: -380px; margin-top: -140px;" src="https://www.icegif.com/wp-content/uploads/2023/10/icegif-755.gif"/>
      </p>
    </div>

    <!-- <div v-if="searchResult.length == 0">
      <p>No Notes Found with the provided title or content, Please try another search query.</p>
    </div> -->

    <q-page-sticky position="bottom-right" :offset="[18, 18]">
      <q-btn fab icon="add" color="primary" :to="{ name: 'add-note' }">
      </q-btn>
    </q-page-sticky>
  </q-page>
</template>


<style lang="sass" scoped>
.search-input
  width: 100%
  max-width: 300px

.note-card
  width: 350px
</style>
