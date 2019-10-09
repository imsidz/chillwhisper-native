<template>
    <Page class="page">
        <ActionBar class="action-bar">
            <!-- 
            Use the NavigationButton as a side-drawer button in Android
            because ActionItems are shown on the right side of the ActionBar
            -->
            <NavigationButton ios:visibility="collapsed" icon="res://menu" @tap="onDrawerButtonTap"></NavigationButton>
            <!-- 
            Use the ActionItem for IOS with position set to left. Using the
            NavigationButton as a side-drawer button in iOS is not possible,
            because its function is to always navigate back in the application.
            -->
            <ActionItem icon="res://navigation/menu" 
                android:visibility="collapsed" 
                @tap="onDrawerButtonTap"
                ios.position="left">
            </ActionItem>
            <Label class="action-bar-title" text="Home"></Label>
        </ActionBar>
        <!-- <text>HEllo Shirt</text> -->
        
        <ListView for="video in videos" @itemTap="onItemTap">
            <v-template>
                <GridLayout class="list-group-item" rows="*" columns="auto, *">
                    <Image row="0" col="0" :src="video.thumbnail" class="thumb" />
                    <Label row="0" col="1" :text="video.title" />
                </GridLayout>
                <!-- <Image :src="video.thumbnail" stretch="fill" />
                <Label :text="video.title" /> -->
            </v-template>
        </ListView>
    </Page>
</template>

<script>
    import * as utils from "~/shared/utils";
    import SelectedPageService from "../shared/selected-page-service";
    import axios from 'axios/dist/axios';
    import Watch from './Watch'

    export default {
        data(){
            return {
                videos: [],
                message: 'IS this working yes'
            }
        },
        mounted() {
            this.getVideos();
            SelectedPageService.getInstance().updateSelectedPage("Home");
        },
        components: {
            Watch
        },
        // created() {
        //     fetch("https://api.chillwisper.tk/api/videos")
        //     .then((response) => response.json())
        //     .then((r) => {
        //         this.videos = r.data
        //     }).catch((e) => {
        //         console.log(e.response);
        //     });
        // },
        methods: {
            onDrawerButtonTap() {
                utils.showDrawer();
            },
            getVideos(){
                fetch("https://api.chillwisper.tk/api/videos")
                .then((response) => response.json())
                .then((r) => {
                    this.videos = r.data
                }).catch((e) => {
                    console.log(e.response);
                });
            },
            onItemTap(event){
                console.log(event.item)
                this.$navigateTo(Watch, {
                    transition: {
                        name:'fade',
                        duration: 200
                    },
                    props: {
                        video: event.item 
                        // this.$data.videos[event.index].videoId
                        // somethingElse: 'cat'
                    }
                })
            }
        }
    };
</script>

<style scoped lang="scss">
    // Start custom common variables
    @import '../app-variables';
    // End custom common variables

    // Custom styles
</style>