<script>
import { remote } from 'electron';
const mainProcess = remote.require('./main.js');

export default {
    name: 'post-editors-common',
    mounted () {
        this.sidebarVisible = localStorage.getItem('publii-' + this.$options.editorType + '-editor-sidebar') === 'opened';
    },
    methods: {
        slugUpdated () {
            this.postSlugEdited = true;
        },
        updateSlug () {
            if(this.isEdit || this.postSlugEdited) {
                return;
            }

            let slugValue = mainProcess.slug(this.postData.title);
            this.postData.slug = slugValue;
        },
        toggleSidebar () {
            this.sidebarVisible = !this.sidebarVisible;

            if (this.sidebarVisible) {
                this.sidebarVisible = true;
                localStorage.setItem('publii-' + this.$options.editorType + '-editor-sidebar', 'opened');
            } else {
                this.sidebarVisible = false;
                localStorage.setItem('publii-' + this.$options.editorType + '-editor-sidebar', 'closed');
            }
        },
        closeEditor () {
            let siteName = this.$route.params.name;
            this.$router.push('/site/' + siteName + '/posts/');
        }
    }
}
</script>
