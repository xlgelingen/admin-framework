<script setup>
import { ref,watch } from 'vue'
import { useRoute} from 'vue-router'
import { useStore } from '@/stores/index'
import { RotateLeftOutlined } from '@ant-design/icons-vue'
import routesAll from '@/router/routes'
import { formatRoutes } from '@/utils/permission'
const route = useRoute()
//获取用户拥有的权限列表
const { permissions } = useStore()

const isCollapse = ref(false)

//获取有权限的具有导航信息的路由
const navRoutes = ref(formatRoutes(routesAll, permissions))

//激活的路由name
const activeIndex = ref(route.name)
watch(route, (to, from) => {
    activeIndex.value = to.name
})
</script>

<template>
    <div class="sider-section" :style="[{ width: isCollapse ? '80px' : '243px' }]">
        <div class="logo-section">
            <div class="header-logo-container">
                <img v-show="!isCollapse" class="header-logo-element" alt="斗西家计划"
                    src="@/assets/images/header-logo.svg" />
                <div class="header-logo-collapse-btn" @click="isCollapse = !isCollapse">
                    <RotateLeftOutlined />
                </div>
            </div>
        </div>
        <div class="menu-section">
            <el-scrollbar>
                <!-- :router是否启用vue-router模式。启用该模式会在激活导航时以:index作为path进行路由跳转，使用default-active来设置加载时的激活项。-->
                <el-menu :collapse="isCollapse" :router="true" :default-active="activeIndex">
                    <!-- <template> 标签的v-for渲染-->
                    <template v-for="route in navRoutes">
                        <!-- 若有子路由，渲染这个 --> <!-- 以:index作为path进行路由跳转 -->
                        <el-sub-menu v-if="route.children?.length" :index="route.name" :key="route.path">
                            <!-- sub-menu子菜单的名字插槽 -->
                            <template #title>
                                <svg-icon v-if="route.meta.nav?.icon" :name="route.meta.nav.icon" width="24px"
                                    height="18px" style="margin-right: 5px;"></svg-icon>
                                <span>{{ route.meta.nav.title }}</span>
                            </template>
                            <el-menu-item-group>
                                <!-- :route 属性是一个自定义的属性，用于存储与当前菜单项关联的路由信息 -->
                                <el-menu-item v-for="data in route.children" :key="data.name" :index="data.name"
                                    :route="{ name: data.name }">
                                    <svg-icon v-if="data.meta.nav?.icon" :name="data.meta.nav.icon" width="24px"
                                        height="18px" style="margin-right: 5px;"></svg-icon>
                                    <span>{{ data.meta.nav.title }}</span>
                                </el-menu-item>
                            </el-menu-item-group>
                        </el-sub-menu>

                        <!-- 若没有子路由，渲染这个 -->
                        <el-menu-item v-else :key="route.name" :index="route.name" :route="{ name: route.name }">
                            <svg-icon v-if="route.meta.nav?.icon" :name="route.meta.nav.icon" width="24px" height="18px"
                                style="margin-right: 5px;"></svg-icon>
                            <span>{{ route.meta.nav.title }}</span>
                        </el-menu-item>
                    </template>
                </el-menu>
            </el-scrollbar>
        </div>
    </div>
</template>

<style type="text/css" lang="less" scoped>
.sider-section {
    position: relative;
    width: 243px;
    background-color: #fff;
    transition: all 0.2s ease;
    display: flex;
    flex-direction: column;

    .menu-section {
        max-height: calc(100vh - 56px);
    }

    :deep(.el-menu) {
        border-right: none;
    }
}

.header-logo-container {
    display: flex;
    height: 54px;
    justify-content: center;
    align-items: center;

    .header-logo-element {
        margin: 0 20px;
        height: 100%;
    }

    .header-logo-collapse-btn {
        cursor: pointer;
        font-size: 20px;
        line-height: 54px;
        position: relative;
        text-align: center;
        top: 4px;
        transition: all 0.2s ease;
        width: 80px;
    }
}
</style>
