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
            language:'zh_CN',
            height: '600px',
            images_upload_url: '/demo/upimg.php',
            images_upload_base_path: '/demo',

            // plugins: [ 'quickbars' ],
            // toolbar: false,
            // menubar: false,
            // inline: true,

            //菜单栏
            // menubar:'format',
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