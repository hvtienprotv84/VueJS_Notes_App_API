<script setup>
import { Dialog, Notify } from 'quasar';
import { useNotesStore } from '../stores/notes';
import { useRouter } from 'vue-router';

const notesStore = useNotesStore();

const router = useRouter();

function confirm(id) {
  Dialog.create({
    dark: true,
    title: 'Xác Nhận!',
    color: 'primary',
    message: 'Xóa thẻ này?, Ghi chú của bạn có thẻ này sẽ không bị xóa',
    cancel: true,
    persistent: true
  }).onOk(() => {
    try {
      notesStore.deleteTag(id)
      router.push('/tags');
      Notify.create({
        message: 'Thẻ đã được xóa thành công!',
        type: "positive",
        actions: [
          { icon: 'close', color: 'white', round: true, }
        ]
      })
    } catch (error) {
      console.log(error)
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
  <q-page class="flex flex-center column">

    <div v-if="notesStore.tags.length > 0">
      <q-card v-for="(tag, index) in notesStore.tags" :key="tag.id" class="tag-card q-mt-md" flat bordered>
        <q-card-section>
          <div class="row items-center no-wrap">
            <div class="col">
              <div class="text-h6">{{ tag.name }}</div>
            </div>

            <div class="col-auto">
              <q-btn color="grey-7" round flat icon="more_vert">
                <q-menu cover auto-close>
                  <q-list>
                    <q-item clickable :to="{ name: 'notes-by-tag', params: { id: tag.id } }">
                      <q-item-section>Notes With This Tag</q-item-section>
                    </q-item>
                    <q-item clickable :to="{ name: 'tag-detail', params: { id: tag.id } }">
                      <q-item-section>Details</q-item-section>
                    </q-item>
                    <q-item clickable>
                      <q-item-section color="negative" @click="confirm(tag.id)">Delete</q-item-section>
                    </q-item>
                  </q-list>
                </q-menu>
              </q-btn>
            </div>
          </div>
        </q-card-section>

        <q-separator />

        <q-card-actions>
          <q-btn color="info" flat :to="{ name: 'tag-detail', params: { id: tag.id } }">Details</q-btn>
          <q-btn color="info" flat :to="{ name: 'notes-by-tag', params: { id: tag.id } }">Notes With This Tag</q-btn>
        </q-card-actions>
      </q-card>
    </div>
    <div v-else>
      <!-- <p>Không tìm thấy thẻ nào, Nhấp vào dấu cộng (+) để thêm thẻ mới</p> -->
      <p style="font-weight: bold; font-size: 30px; text-align: center; margin-top: -250px;">Không tìm thấy thẻ nào, Nhấp vào dấu cộng (+) để thêm thẻ mới
        <img style="position: absolute; rotate: 270deg; width: 650px; margin-left: -380px; margin-top: -140px;" src="https://www.icegif.com/wp-content/uploads/2023/10/icegif-755.gif"/>
      </p>
    </div>

    <q-page-sticky position="bottom-right" :offset="[18, 18]">
      <q-btn fab icon="add" color="primary" :to="{ name: 'add-tag' }">
      </q-btn>
    </q-page-sticky>
  </q-page>
</template>

<style lang="sass" scoped>
.tag-card
  width: 350px
</style>
