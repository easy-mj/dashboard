<template>
  <base-side-page
    @cancel="cancelSidePage"
    :title="$t('table.title.task')"
    icon="res-task"
    :res-name="detailData.name"
    :current-tab="params.windowData.currentTab"
    :tabs="detailTabs"
    :loaded="loaded"
    @tab-change="handleTabChange">
    <template v-slot:actions>
      <actions
        :options="singleActions"
        :row="detailData"
        button-type="link"
        button-size="small" />
    </template>
    <component
      :is="params.windowData.currentTab"
      :res-id="data.id"
      :id="listId"
      :data="detailData"
      :getParams="getParams"
      :on-manager="onManager"
      :columns="columns"
      resource="cloud-phone-tasks"
      :isRoot="false" />
  </base-side-page>
</template>

<script>
import SidePageMixin from '@/mixins/sidePage'
import WindowsMixin from '@/mixins/windows'
import Actions from '@/components/PageList/Actions'
import ChildrenTaskList from '@/sections/TaskList'
import Detail from './Detail'

export default {
  name: 'TaskSidePage',
  components: {
    Detail,
    ChildrenTaskList,
    Actions,
  },
  mixins: [SidePageMixin, WindowsMixin],
  data () {
    return {
      detailTabs: [
        { label: this.$t('scope.cloudphone.detail'), key: 'detail' },
        { label: this.$t('table.title.children_task'), key: 'children-task-list' },
      ],
    }
  },
  computed: {
    getParams () {
      if (this.params.windowData.currentTab === 'children-task-list') {
        return {
          is_root: false,
          parent_id: this.data.id,
        }
      }
      return null
    },
    listId () {
      switch (this.params.windowData.currentTab) {
        case 'children-task-list':
          return 'ChildrenTaskListForTaskSidePage'
        case 'event-drawer':
          return 'EventListForTaskSidePage'
        default:
          return ''
      }
    },
  },
}
</script>
