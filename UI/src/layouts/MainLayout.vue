<template>
    <el-container class="main-layout">
        <el-aside :width="isCollapsed ? '64px' : '200px'" class="sidebar">
            <div class="sidebar-header">
                <GradientText v-if="!isCollapsed"
                    :colors="['#4079ff', '#40ffaa', '#549EBB', '#4079ff', '#40B5FF', '#549EBB']" :animationSpeed="8"
                    :showBorder="false" customClass="sidebar-title-gradient">
                    Hyper 简生
                </GradientText>
                <el-icon v-else :size="28" color="#337ecc">
                    <MagicStick />
                </el-icon>
            </div>
            <el-menu :default-active="$route.path" class="sidebar-menu" :collapse="isCollapsed" router>
                <el-menu-item index="/home">
                    <el-icon>
                        <HomeFilled />
                    </el-icon>
                    <template #title>首页</template>
                </el-menu-item>
                <el-menu-item index="/generator">
                    <el-icon>
                        <Document />
                    </el-icon>
                    <template #title>简历生成</template>
                </el-menu-item>
                <el-menu-item index="/store">
                    <el-icon>
                        <ShoppingCart />
                    </el-icon>
                    <template #title>模板商城</template>
                </el-menu-item>
            </el-menu>
            <div class="sidebar-bottom-content">
                <InfoButton :is-collapsed="isCollapsed"></InfoButton>
                <div v-if="!isCollapsed" class="beian-info">
                    <a href="https://beian.miit.gov.cn/" target="_blank">桂ICP备2024034221号-2</a>
                </div>
            </div>
            <div class="sidebar-toggle" @click="isCollapsed = !isCollapsed">
                <el-icon>
                    <Fold v-if="!isCollapsed" />
                    <Expand v-else />
                </el-icon>
            </div>
        </el-aside>

        <el-container>
            <el-header class="main-header">
                <div class="header-content">
                    <div class="breadcrumb">
                        <!-- Breadcrumb can be added here if needed -->
                    </div>
                    <div class="user-info">
                        <el-dropdown @command="handleCommand">
                            <span class="el-dropdown-link">
                                欢迎, {{ authStore.user?.username }}
                                <el-icon class="el-icon--right">
                                    <ArrowDown />
                                </el-icon>
                            </span>
                            <template #dropdown>
                                <el-dropdown-menu>
                                    <el-dropdown-item command="profile">个人中心</el-dropdown-item>
                                    <el-dropdown-item command="logout" divided>退出登录</el-dropdown-item>
                                </el-dropdown-menu>
                            </template>
                        </el-dropdown>
                    </div>
                </div>
            </el-header>
            <el-main class="content-area">
                <router-view></router-view>
            </el-main>
        </el-container>
    </el-container>
</template>

<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import { useAuthStore } from '@/stores/auth';
import GradientText from '@/components/GradientText.vue';
import InfoButton from '@/components/InfoButton.vue';
import {
    HomeFilled,
    Document,
    ShoppingCart,
    Fold,
    Expand,
    MagicStick,
    ArrowDown
} from '@element-plus/icons-vue';

const isCollapsed = ref(false);
const authStore = useAuthStore();
const router = useRouter();

const handleCommand = (command) => {
    if (command === 'logout') {
        authStore.logout();
        router.push('/login');
    } else if (command === 'profile') {
        router.push('/profile');
    }
};
</script>

<style scoped>
.main-layout {
    height: 100vh;
    background-color: #f0f2f5;
}

.sidebar {
    background-color: #ffffff;
    border-right: 1px solid #e9e9e9;
    transition: width 0.3s ease;
    display: flex;
    flex-direction: column;
    overflow: visible !important;
    /* 允许显示溢出的子元素 */
}

.sidebar-header {
    height: 60px;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
}

.sidebar-title {
    font-size: 20px;
    font-weight: 600;
    color: #337ecc;
    white-space: nowrap;
    overflow: hidden;
}

.sidebar-title-gradient {
    font-size: 20px;
    font-weight: 600;
}

.sidebar-menu {
    flex-grow: 1;
    border-right: none;
    background: #ffffff;
}

.sidebar-bottom-content {
    flex-shrink: 0;
    transition: all 0.3s ease;
}

.el-menu-item {
    font-weight: 500;
}

.sidebar-toggle {
    height: 50px;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    background-color: #f9fafb;
    color: #606266;
    flex-shrink: 0;
    border-top: 1px solid #e9e9e9;
}

.main-header {
    background: #ffffff;
    border-bottom: 1px solid #e9e9e9;
    display: flex;
    align-items: center;
    justify-content: flex-end;
    /* Align items to the right */
    padding: 0 20px;
}

.user-info .el-dropdown-link {
    cursor: pointer;
    display: flex;
    align-items: center;
    font-size: 16px;
}

.content-area {
    padding: 20px;
}

.beian-info {
    padding: 15px 0;
    text-align: center;
    font-size: 12px;
    color: #909399;
    flex-shrink: 0;
}

.beian-info a {
    color: inherit;
    text-decoration: none;
    transition: color 0.3s;
}

.beian-info a:hover {
    color: #409eff;
    text-decoration: underline;
}
</style>