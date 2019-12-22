<template>
    <el-container>
        <el-aside style="width:200px; margin-top:20px">
            <switch></switch>
            <SideMenu></SideMenu>
        </el-aside>
        <el-main>
            <collocations class="books-area"></collocations>
        </el-main>
    </el-container>
</template>

<script>
import SideMenu from './SideMenu'
import Collocations from './Collocations'
export default {
    name: 'AppDress',
    components: {SideMenu,Collocations}
}
</script>

<style scoped>
  .books-area {
    width: 990px;
    margin-left: auto;
    margin-right: auto;
  }
</style>