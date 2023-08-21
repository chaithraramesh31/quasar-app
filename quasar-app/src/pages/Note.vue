<script>
/* eslint-disable */
import Container from 'src/components/Container.vue';
import { useLocalNotes } from 'src/helper';
import { useRoute, useRouter } from 'vue-router';

export default {
    components: { Container },
    name: 'NotePage',
    setup() {
        const notes = useLocalNotes();
        const router = useRouter();
        const route = useRoute();
        const noteId = computed(() => parseInt(route.params.idx))
        const note = computed(() => notes.value[noteId.value])

        const remove = () => {
            notes.value.splice(noteId.value, 1)
            router.push('/')
        }

        const editing = ref(false)

        return { note, remove, editing }
    }
}
</script>

<template>
    <q-page padding>
        <Container>

        </Container>
    </q-page>
</template>
