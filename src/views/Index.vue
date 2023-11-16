<template>
    <!-- 居中,长度为8-->
    <el-row :gutter="20" type="flex" justify="center">
        <el-col :span="8">
            <el-autocomplete
                fit-input-width="true"
                v-model="state"
                :fetch-suggestions="querySearchAsync"
                placeholder="Please input"
                @select="handleSelect"
            />
        </el-col>
    </el-row>
</template>

<script lang="ts" setup>
    //导入vue相关组件
    import { ref,onMounted } from "vue";
    //导入axios
    import axios from "axios";


    const state = ref('')

    interface AssociationalWord {
        value: string
        id: number
    }

    const AssociationalWordList = ref<AssociationalWord[]>([])

    const loadAll = () => {
        //默认关联词
        AssociationalWordList.value = [
            {
                value: "vue",
                id: 1
            },
            {
                value: "react",
                id: 2
            },
            {
                value: "angular",
                id: 3
            },
            {
                value: "webpack",
                id: 4
            },
            {
                value: "vite",
                id: 5
            },
            {
                value: "vitepress",
                id: 6
            },
            {
                value: "vitepress-theme",
                id: 7
            },
            {
                value: "vitepress-theme-reco",
                id: 8
            },
            {
                value: "vitepress-theme-reco",
                id: 9
            },
            {
                value: "vitepress-theme-reco",
                id: 10
            }]
        return AssociationalWordList.value
    }

    let timeout: ReturnType<typeof setTimeout>
    const querySearchAsync = (queryString: string, cb: (arg: any) => void) => {
        //如果关键字为空，则返回默认关联词
        if (queryString === '') {
            cb(AssociationalWordList.value)
            return
        }
        //发起跨域请求
        // let results: AssociationalWord[] = []
        axios({
            method: 'get',
            url: 'https://www.swxnh.com/zhihu/associationalWord/getAssociationalWord',
            params: {
                keyword: queryString
            }
        })
        .then((res) => {
            if (res.data.code === 200) {
                cb(res.data.data)
            }else{
                cb(AssociationalWordList.value)
            }
        })


        // clearTimeout(timeout)
        // timeout = setTimeout(() => {
        //     cb(results)
        // }, 3000 * Math.random())
    }

    const createFilter = (queryString: string) => {
    return (associationalWord: AssociationalWord) => {
        return (
            //将关联词中包含的关键字高亮
            associationalWord.value.replaceAll(queryString, `<span style="color: #409eff">${queryString}</span>`)
        )
    }
    }

    const handleSelect = (item: Record<string, any>) => {
        console.log(item)
    }

    onMounted(() => {
        AssociationalWordList.value = loadAll()
    })
</script>
    
<style>

</style>