<template>
  <base-modal v-if="inputInvalid" title="Invalid Input" @closeModal="confirmError">
    <template #default>
        <p>One of your input values is invalid.</p>
        <p>Please check all input fields and make sure they are not empty before submitting the form.</p>
    </template>
    <template #actions>
        <base-button @click="confirmError">Okay</base-button>
    </template>
  </base-modal>
  <base-card>
    <form @submit.prevent="submit">
        <div class="form-control">
            <label for="title">Title</label>
            <input id="title" name="title" type="text" ref="resourceTitle">
        </div>
        <div class="form-control">
            <label for="title">Description</label>
            <textarea id="description" name="description" rows="4" ref="resourceDesc"></textarea>
        </div>
        <div class="form-control">
            <label for="link">Link</label>
            <input id="link" name="link" type="url" ref="resourceLink">
        </div>
        <div>
            <base-button type="submit">Add Resource</base-button>
        </div>
    </form>
  </base-card>
</template>

<script>
export default {
    inject: ['addResource'],
    data() {
        return{
            inputInvalid: false
        }
    },
    methods: {
        submit() {
            const title = this.$refs.resourceTitle.value
            const description = this.$refs.resourceDesc.value
            const link = this.$refs.resourceLink.value

            if (
                title.trim() === '' || 
                description.trim() === '' || 
                link.trim() === ''
            ){
                this.inputInvalid = true
                return;
            }

            this.addResource(title, description, link)
        },
        confirmError() {
            this.inputInvalid = false;
        }
    }
}
</script>

<style scoped>
label {
  font-weight: bold;
  display: block;
  margin-bottom: 0.5rem;
}

input,
textarea {
  display: block;
  width: 100%;
  font: inherit;
  padding: 0.25rem;
  border: 1px solid #ccc;
}

input:focus,
textarea:focus {
  outline: none;
  border-color: #3a0061;
  background-color: #f7ebff;
}

.form-control {
  margin: 1rem 0;
}
</style>