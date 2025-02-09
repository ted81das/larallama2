<script setup>
import { ref } from 'vue';
import { Link, router, useForm } from '@inertiajs/vue3';
import ActionMessage from '@/Components/ActionMessage.vue';
import FormSection from '@/Components/FormSection.vue';
import InputError from '@/Components/InputError.vue';
import InputLabel from '@/Components/InputLabel.vue';
import PrimaryButton from '@/Components/PrimaryButton.vue';
import SecondaryButton from '@/Components/SecondaryButton.vue';
import TextInput from '@/Components/TextInput.vue';
import SecretInput from "@/Components/SecretInput.vue";

const props = defineProps({
    setting: Object,
});

const form = useForm({
    _method: 'PUT',
    api_key: props.setting.secrets?.claude?.api_key,
    api_url: props.setting.secrets?.claude?.api_url ?? 'https://api.anthropic.com/v1',
});


const updateSecrets = () => {

    form.put(route('settings.update.claude', {
        setting: props.setting.id,
    }), {
        errorBag: 'updateClaude',
        preserveScroll: true,
    });
};

</script>

<template>
    <FormSection @submitted="updateSecrets">
        <template #title>
            Claude API Key and Token
        </template>

        <template #description>
            If you want to use this service make sure to setup your API Token.
            You can get your keys
            <a
                class="underline"
                href="https://docs.anthropic.com/en/docs/intro-to-claude" target="_blank">here</a>
        </template>

        <template #form>


            <!-- Name -->
            <div class="col-span-6 sm:col-span-4">
                <InputLabel for="name" value="Api Token" />
                <SecretInput v-model="form.api_key" class="mt-1 block w-full" />
                <InputError :message="form.errors.api_key" class="mt-2" />
            </div>

            <div class="col-span-6 sm:col-span-4">
                <InputLabel for="name" value="Api Url" />
                <TextInput
                    id="name"
                    v-model="form.api_url"
                    type="text"
                    class="mt-1 block w-full"
                    required
                />
                <InputError :message="form.errors.api_url" class="mt-2" />
            </div>

        </template>

        <template #actions>
            <ActionMessage :on="form.recentlySuccessful" class="me-3">
                Saved.
            </ActionMessage>

            <PrimaryButton :class="{ 'opacity-25': form.processing }" :disabled="form.processing">
                Save
            </PrimaryButton>
        </template>
    </FormSection>
</template>
