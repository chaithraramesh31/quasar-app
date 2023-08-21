<script>
/* eslint-disable */
import { defineComponent, reactive } from 'vue';
import Container from 'src/components/Container.vue';
import { useLocalNotes } from 'src/helper';
import { useRouter } from 'vue-router';
export default defineComponent({
    components: {Container},
    name: 'PageNew',
    setup() {
        const notes = useLocalNotes();
        const router = useRouter();
        const note = reactive({
            title: '',
            description: '',
            content: ''
        })
        const submit = () => {
            notes.value.unshift({
                ...note,
                createdAt: Date.now(),
                updatedAt: Date.now()
            })
            router.push('/')
            note.title = ''
            note.description = ''
            note.content = ''
        }
        return {note, submit}
    }
})
</script>

<template>
    <q-page padding>
        <Container>
            <h3>New Note</h3>
            <form @submit="submit">
                <q-input class="q-mt-sm" outlined label="Title" v-model="note.title"/>
                <q-input class="q-mt-sm" outlined label="Description" v-model="note.description" dense/>
                <q-card flat bordered class="q-mt-sm">
                    <q-editor v-model="note.content" min-height="5rem"/>
                </q-card>
                <div class="q-mt-md">
                    <q-btn to="/" type="reset" color="grey">Cancel</q-btn>
                    <q-btn type="submit" class="q-ml-md" color="positive">Create</q-btn>
                </div>
            </form>
        </Container>
    </q-page>
</template>
