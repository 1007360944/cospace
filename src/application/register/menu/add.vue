<template>
    <div class="widget flat radius-bordered">
        <div class="widget-header bg-blue">
            <span class="widget-caption">新增菜单信息</span>
        </div>
        <div class="widget-body">
            <div class="row">
                <div class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                    <form class="form-horizontal">                                        
                        <div class="form-group">
                            <label class="col-lg-3 control-label">菜单名称<span class="red margin-left-5">*</span></label>
                            <div class="col-lg-6">
                                <input type="text" class="form-control" v-model="menuData.moduleName" placeholder="请输入菜单名称">
                            </div>
                        </div>                        
                        <div class="form-group">
                            <label class="col-lg-3 control-label">菜单地址<span class="red margin-left-5">*</span></label>
                            <div class="col-lg-6">
                                <input type="text" class="form-control" v-model="menuData.moduleUrl" placeholder="请输入菜单地址">
                            </div>
                        </div>
                        <div class="form-group">
                            <label class="col-lg-3 control-label">ICON<span class="red margin-left-5">*</span></label>
                            <div class="col-lg-6">
                                <input type="text" class="form-control" v-model="menuData.icon" placeholder="请输入ICON">
                            </div>
                        </div> 
                        <div class="form-group">
                            <label class="col-lg-3 control-label">排序</label>
                            <div class="col-lg-6 spinner">
                                <button type="button" class="increase blue" @click="sort(1)"><i class="fa fa-chevron-up"></i></button>
                                <button type="button" class="decrease danger" @click="sort(-1)"><i class="fa fa-chevron-down"></i></button>
                                <input type="text" class="form-control" v-model="menuData.isOrder">
                            </div>                                       
                        </div>
                        <div class="form-group">
                            <label class="col-lg-3 control-label">菜单描述</label>
                            <div class="col-lg-6">
                                <textarea class="form-control" rows="4" v-model="menuData.remark"></textarea>
                            </div>
                        </div>
                        <div class="form-group">
                            <div class="col-lg-offset-3 col-lg-9">
                               <button class="btn btn-info margin-right-10" type="button" @click="confirm()">确定</button>
                                <router-link :to="{path:'/application/register/menu/index',query:{appCode:$route.query.appCode,appId:$route.query.appId}}" class="btn btn-danger">取消</router-link>
                            </div>
                        </div>                        
                    </form> 
                </div>
            </div>             
        </div>        
    </div>   
</template>

<script>
export default{
    data: ()=>({
        menuData: {
            isOrder: 1,
            isUse: 0
        }
    }),  
    methods: { 
        confirm(){
            if(!this.menuData.moduleName){
                $.dialog('请输入菜单名称');
                return false;
            }
            if(!this.menuData.moduleUrl){
                $.dialog('请输入菜单地址');
                return false;            
            } 
            if(!this.menuData.icon){
                $.dialog('请输入ICON');
                return false;            
            }                   
            //新增菜单信息
            let token = localStorage.getItem('token');   
            $.post(window.config.api + "/module/save.do?token="+token,{appCode:this.$route.query.appCode,parentId:this.$route.params.id,moduleName:this.menuData.moduleName,moduleUrl:this.menuData.moduleUrl,icon:this.menuData.icon,isOrder:this.menuData.isOrder,isUse:this.menuData.isUse,remark:this.menuData.remark})
            .then(data=>{  
                if(!data.success){
                    //返回错误状态 
                    $.dialog('数据保存失败！');                   
                    return false;
                }
                $.dialog({
                    content:'数据保存成功！',
                    ok: function(){
                        location.reload();
                    }
                });
            });            
        },        
        sort : function(num){
            if(this.menuData.isOrder+num<0) return false;
            this.menuData.isOrder += num
        }         
    }
}
</script>
