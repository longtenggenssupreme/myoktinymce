<template>
  <section id='tinymceeditor'>这是tinymceeditor富文本编辑器</section>
</template>

<script>
import '../tinymce_editor/tinymce.min.js'
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
import './plugins/fullscreen/plugin.min.js'
import './plugins/wordcount/plugin.min.js'
import './plugins/print/plugin.min.js'
import './plugins/image/plugin.min.js'
import './plugins/imagetools/plugin.min.js'
import './plugins/table/plugin.min.js'
import './plugins/quickbars/plugin.min.js' //显示一个文本框，可以进行各种操作

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
            language:'zh_CN',//this.$i18n.locale
            height: '600px',
            // images_upload_url: '/demo/upimg.php',
            // images_upload_base_path: '/demo',
            // image_uploadtab: false,

            // plugins: 'image',
            // a11y_advanced_options: true,
            images_upload_handler: function (blobInfo, success, failure) {
                var xhr, formData;
                xhr = new XMLHttpRequest();
                xhr.withCredentials = false;
                xhr.open('POST', 'postAcceptor.php');
                xhr.onload = function() {
                    var json;
                    if (xhr.status != 200) {
                        failure('HTTP Error: ' + xhr.status);
                        return;
                    }
                    json = JSON.parse(xhr.responseText);
                    if (!json || typeof json.location != 'string') {
                        failure('Invalid JSON: ' + xhr.responseText);
                        return;
                    }
                    success(json.location);
                };
                formData = new FormData();
                formData.append('file', blobInfo.blob(), blobInfo.filename());
                xhr.send(formData);
            },

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

            // plugins: "image",
            // menubar: "insert",
            // toolbar: "image",
            // image_list: [
            //     {title: 'My image 1', value: 'https://www.example.com/my1.gif'},
            //     {title: 'My image 2', value: 'http://www.moxiecode.com/my2.gif'}
            //     ],

            // plugins: [ 'quickbars' ],
            // toolbar: false,
            // menubar: false,
            // inline: true,
            
            //菜单栏
            // menubar: 'file edit print',
            menubar:'bar1 bar2 format',
            menu:{
                bar1:{title:'菜单12',items:'copy paste' },
                bar2:{title:'菜单2',items:'cut italic forecolor backcolor' }
                },
            //工具栏
            toolbar:['bold italic underline strikethrough wordcount print image imagetools table forecolor backcolor| styleselect formatselect fontselect fontsizeselect',
            'numlist bullist outdent indent blockquote | subscript superscript | alignleft aligncenter alignright alignjustify  | undo redo removeformat preview fullscreen ',
             ],          
            // //插件
            plugins:'preview fullscreen wordcount print image advlist table',            
            //通过属性初始化
            setup:(editor)=>{
                editor.on('init',(e)=>{
                    editor.setContent(this.value);
                    })
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