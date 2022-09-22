<template>
  <base-card>
    <base-button  
    @click="setSelectedTab('stored-resources')" 
    :mode="storedResourcesButtonMode"
    >View Stored Resources</base-button>
    <base-button
    :mode="addResourceButtonMode" 
    @click="setSelectedTab('add-resource')"
    >Add New Resource</base-button>
  </base-card>
  <keep-alive>
        <component :is="selectedTab"></component>
    </keep-alive>
</template>

<script>
import StoredResources from './StoredResources.vue';
import AddResource from './AddResource.vue';

export default {
    components: {
        StoredResources, 
        AddResource
    },
    data() {
        return {
            selectedTab: 'stored-resources',
            storedResources: [
                {
                    id: 'official-guide', 
                    title: 'Official Guide', 
                    description: 'The official Vue.js documentation.',
                    link: 'https://vuejs.org'
                },
                {
                    id: 'guide', 
                    title: 'Google', 
                    description: 'Learn how to google',
                    link: 'https://google.com'
                }
            ]
        };
    },
    provide() {
        return{
            resources: this.storedResources,
            addResource: this.addResource,
            deleteResource: this.deleteResource
        }
    },
    computed: {
        storedResourcesButtonMode() {
            return this.selectedTab  === 'stored-resources' ? '' : 'flat'
        },
        addResourceButtonMode() {
            return this.selectedTab  === 'add-resource' ? '' : 'flat'
        }
    },
    methods: {
        setSelectedTab(tab) {
            this.selectedTab = tab;
        },
        addResource(title, description, url) {
            const newResource = {
                id: new Date().toISOString(),
                title: title,
                description: description,
                link: url
            }
            this.storedResources.unshift(newResource)
            this.selectedTab = 'stored-resources'
        },
        deleteResource(id) {
            const resourceIndex = this.storedResources.findIndex(resource => resource.id === id)
            this.storedResources.splice(resourceIndex,1)
        }
    }
}
</script>

<style>

</style>