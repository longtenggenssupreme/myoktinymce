<template>
  <section id='tinymceeditor'>这是tinymceeditor富文本编辑器</section>
</template>

<script>
import tinymce  from  '../tinymce_editor/tinymce.min.js'
import './themes/silver/theme.min.js'
// import './skins/ui/oxide/skin.min.css'
import './langs/zh_CN.js'
import './plugins/preview/plugin.min.js'
import './plugins/colorpicker/plugin.min.js'
import './plugins/advlist/plugin.min.js'
import './plugins/autolink/plugin.min.js'
import './plugins/link/plugin.min.js'
import './plugins/image/plugin.min.js'
import './plugins/lists/plugin.min.js' 
import './plugins/textcolor/plugin.min.js' 
import './plugins/fullscreen/plugin.min.js'
import './plugins/wordcount/plugin.min.js'
import './plugins/textcolor/plugin.min.js'
import './plugins/print/plugin.min.js'
import './plugins/image/plugin.min.js'
import './plugins/imagetools/plugin.min.js'

export default {
    name:'tinymceeditor',
    props:{
        value:{
            type:String,
            default:'编辑器默认值'
        }
    },
    mounted(){
        tinymce.init({
            selector:'#tinymceeditor',
            language:'zh_CN',
            // width: 600,//width默认是100%，。
            height: '600px',//height通常为200px
            // content_css: 'css/content.css',
            images_upload_url: '/demo/upimg.php',
            images_upload_base_path: '/demo',
            // image_uploadtab: false,

            plugins: 'image',
            // a11y_advanced_options: true,

            file_picker_callback: function(callback, value, meta) {
                // Provide file and text for the link dialog
                if (meta.filetype == 'file') {
                    callback('mypage.html', {text: 'My text'});
                    }
                // Provide image and alt text for the image dialog
                if (meta.filetype == 'image') {
                    callback('myimage.jpg', {alt: 'My alt text'});
                    }
                // Provide alternative source and posted for the media dialog
                if (meta.filetype == 'media') {
                    callback('movie.mp4', {source2: 'alt.ogg', poster: 'image.jpg'});
                    }
            },
            
            //菜单蓝菜单和工具栏图片上传
            // plugins: "image",
            // menubar: "insert",
            // toolbar: "image",
            // image_list: [
            //     {title: 'My image 1', value: 'https://www.example.com/my1.gif'},
            //     {title: 'My image 2', value: 'http://www.moxiecode.com/my2.gif'}
            //     ],

            //可以快速插入一个富文本编辑框，有很多快捷键
            // plugins: [ 'quickbars' ],
            // toolbar: false,
            // menubar: false,
            // inline: true,
            
            //菜单栏
            // menubar:'format',
            // menubar: 'file edit print',
            // menubar:'bar1 bar2 format',
            // menu:{
            //     bar1:{title:'菜单12',items:'copy paste' },
            //     bar2:{title:'菜单2',items:'cut italic forecolor backcolor' }
            //     },
            //
            //工具栏
            toolbar:['bold italic underline strikethrough formats wordcount print image imagetools| styleselect formatselect fontselect fontsizeselect',
            'numlist bullist outdent indent blockquote | subscript superscript | alignleft aligncenter alignright alignjustify  | undo redo removeformat preview fullscreen ',
             ],
            // newdocument（新文档）bold（加粗）italic（斜体）underline（下划线）strikethrough（删除线）
            // alignleft（左对齐）aligncenter（居中对齐）alignright（右对齐）alignjustify（两端对齐）
            // styleselect（格式设置）formatselect（段落格式）fontselect（字体选择）fontsizeselect（字号选择）
            // cut（剪切）copy（复制）paste（粘贴）bullist（项目列表UL）numlist（编号列表OL）
            // outdent（减少缩进）indent（增加缩进）
            // blockquote（引用）undo（撤销）redo（重做/重复）
            // removeformat（清除格式）
            // subscript（下角标）superscript（上角标）
            //插件
            plugins:'preview fullscreen wordcount print image imagetools',
             // plugins:'preview fullscreen wordcount print image advlist table',
            //插件
            // plugins:'preview colorpicker',
            // plugins : 'advlist autolink link image lists preview', //字符串方式
            //plugins : ['advlist','autolink','link'], //数组方式
            //通过属性初始化
            setup:(editor)=>{
                editor.on('init',(e)=>{
                    editor.setContent(this.value);
                    })
                // // 定义按钮
                // editor.addButton('mybutton', {
                //     text: 'My button',
                //     icon: false,
                //     onclick: function () {
                //         editor.insertContent('&nbsp;<b>It\'s my button!</b>&nbsp;');
                //         }
                //     })
                },
            //input和change事件
            init_instance_callback:(editor)=>{
                editor.on('input',(e)=>{
                    this.$emit('input',e.target.innerHTML);
                    }),
                editor.on('change',(e)=>{
                    this.$emit('input',e.level.content);
                    })
                },
            //状态栏指的是编辑器最底下、左侧显示dom信息、右侧显示Tiny版权链接和调整大小的那一条。默认是显示的，设为false可将其隐藏    
            statusbar: false
    });
}
}
</script>

<style lang="less" scoped>
@import url('./skins/ui/oxide/skin.min.css');
</style>