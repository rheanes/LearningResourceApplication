<template>
    <base-card>
        <base-button @click="setSelectedTab('stored-resources')" :mode="storedResButtonMode">Stored Resources</base-button>
        <base-button @click="setSelectedTab('add-resources')" :mode="addResButtonMode">Add Resources</base-button>
    </base-card>
    <keep-alive>
        <component :is="selectedTab"></component>
    </keep-alive>
</template>

<script>
import StoredResources from './StoredResources.vue';
import AddResources from './AddResources.vue';
export default {
    components:{
        StoredResources,
        AddResources
    },
    data(){
        return{
            selectedTab: 'stored-resources',
            storedResources: [
                {
                    id: 'official-guide',
                    title: 'Official Guide',
                    description: 'The official vue.js doccumentation.',
                    link: 'https://vuejs.org'
                },
                {
                    id: 'google',
                    title: 'Google',
                    description: 'A link to the most popular search engine.',
                    link: 'https://www.google.com/'
                }
            ],
        };
    },
    methods: {
        setSelectedTab(tab){
            this.selectedTab = tab;
        },
        addResource(title, description, URL){
            const newResource = {
                id: new Date().toISOString,
                title: title,
                description: description,
                link: URL

            }
            this.storedResources.push(newResource);
            this.selectedTab = 'stored-resources';
        },
        removeResource(resId){
            const resIndex = this.storedResources.findIndex(
                (res)=> res.id === resId
            );
            this.storedResources.splice(resIndex, 1);
        }
    },
    provide(){
        return{
            resources: this.storedResources,
            addResource: this.addResource,
            removeResource: this.removeResource,
        };
    },
    computed: {
        storedResButtonMode(){
            return this.selectedTab === 'stored-resources' ? null : 'flat';
        },
        addResButtonMode(){
            return this.selectedTab === 'add-resources' ? null : 'flat';
        }
    }
}

</script>