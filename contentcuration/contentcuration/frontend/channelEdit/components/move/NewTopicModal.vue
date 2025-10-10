<template>

  <KModal
    v-model="dialog"
    :title="$tr('createTopic')"
    :cancelText="$tr('cancel')"
    :submitText="$tr('create')"
    @cancel="close"
    @submit="create"
  >
    <form
      novalidate
      @submit.prevent="create"
    >
      <KTextbox
        v-model="title"
        :maxlength="200"
        :label="$tr('topicTitle')"
        :invalid="!!errors.title"
        :invalidText="$tr('topicTitleRequired')"
        showInvalidText
      />
    </form>
  </KModal>

</template>


<script>

  import { generateFormMixin } from 'shared/mixins';

  const formMixin = generateFormMixin({
    title: {
      required: true,
      validator: v => v && v.trim().length > 0,
    },
  });

  export default {
    name: 'NewTopicModal',
    mixins: [formMixin],
    props: {
      value: {
        type: Boolean,
        default: false,
      },
    },
    data() {
      return {
        title: '',
      };
    },
    computed: {
      dialog: {
        get() {
          return this.value;
        },
        set(value) {
          this.$emit('input', value);
        },
      },
    },
    methods: {
      create() {
        const formData = this.clean();
        if (!this.validate(formData)) {
          return;
        }
        this.$emit('createTopic', formData.title);
        this.dialog = false;
        this.resetForm();
      },

      close() {
        this.dialog = false;
        this.resetForm();
      },
    },
    $trs: {
      topicTitle: 'Folder title',
      topicTitleRequired: 'Folder title is required',
      createTopic: 'Create new folder',
      cancel: 'Cancel',
      create: 'Create',
    },
  };

</script>


<style lang="scss" scoped></style>
