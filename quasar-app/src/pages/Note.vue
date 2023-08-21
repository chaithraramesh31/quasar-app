<script>
/* eslint-disable */
import { computed, ref } from 'vue';
import Container from 'src/components/Container.vue';
import { useLocalNotes } from 'src/helper';
import { useRoute, useRouter } from 'vue-router';

export default {
    components: { Container },
    name: 'PageNote',
    setup() {
        const notes = useLocalNotes();
        const router = useRouter();
        const route = useRoute();
        const noteId = computed(() => parseInt(route.params.id))
        const note = computed(() => notes.value[noteId.value])

        const remove = () => {
            notes.value.splice(noteId.value, 1)
            router.push('/')
        }

        const editing = ref(false)

        return { noteId, note, remove, editing }
    }
}
</script>

<template>
    <q-page padding>
        <Container>
            <div v-if="editing">
                <form @submit="editing = false">
                    <q-input filled label="Title" v-model="note.title"/>
                <q-input class="q-mt-sm" filled label="Description" v-model="note.description" dense/>
                <q-card flat bordered class="q-mt-sm">
                    <q-editor v-model="note.content" min-height="5rem"/>
                </q-card>
                <div class="q-mt-md">
                    <q-btn type="submit" class="q-ml-sm" color="positive">Done</q-btn>
                </div>
                </form>
            </div>

            <div v-else>
                <div class="row items-center justify-between">
                    <h3 class="q-mb-md q-mt-md">{{ note.title }}</h3>
                    <div>
                        <q-btn round color="secondary" icon="edit" @click="editing = true"/>
                        <q-btn class="q-ml-sm" round color="red" icon="delete" @click="remove"/>
                    </div>
                </div>
                <div>{{ note.description }}</div>
                <div class="q-mt-md" v-html="note.content"/>
                <div>
                    <q-btn class="q-mt-md" color="primary" to="/">Back</q-btn>
                </div>
            </div>
        </Container>
    </q-page>
</template>
