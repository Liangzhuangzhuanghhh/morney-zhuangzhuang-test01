<template>
    <div>
        <Layout class-prefix="layout">
            <!--            标签-->
            <Tags @selectTags="onUpdateSelectedTags"/>
            <!--            备注-->
            <div class="notes">
                <FormItem field-name="备注" placeholder="在这里输入备注" @update:value="onUpdateNotes"/>
            </div>
            <!--            收支选择-->
            <Tabs :data-source="recordTypeList" :value.sync="record.type"/>
            <!--            数字键盘-->
            <NumberPad :value.sync="record.amount" @submit="saveRecord"/>
        </Layout>
    </div>
</template>

<script lang="ts">
    import Vue from 'vue';
    import Tags from '@/components/Money/Tags.vue';
    import FormItem from '@/components/Money/FormItem.vue';
    import NumberPad from '@/components/Money/NumberPad.vue';
    import {Component} from 'vue-property-decorator';
    import Tabs from '@/components/Tabs.vue';
    import recordTypeList from '@/constants/recordTypeList';

    @Component({
        components: {
            Tabs,
            NumberPad,
            FormItem,
            Tags,
        }
    })
    export default class Money extends Vue{
        recordTypeList = recordTypeList;
        get recordList(){
            return this.$store.state.recordList;
        }
        record: RecordItem = {
            tags:[], notes:'', type:'-', amount:0
        };

        created(){
            this.$store.commit('fetchRecords');
        }

        onUpdateSelectedTags(tagList: Tag[]){
            this.record.tags = tagList.map((tag) => {
                return tag.name;
            })
        }

        onUpdateNotes(value: string){
            this.record.notes = value;
        }

        saveRecord(){
            this.$store.commit('createRecord', this.record);
        }

    }

</script>

<style lang="scss" scoped>
    ::v-deep .layout-content {
        display: flex;
        flex-direction: column;
        justify-content: flex-end;
    }
    .notes{
        padding: 12px 0;
    }
</style>

