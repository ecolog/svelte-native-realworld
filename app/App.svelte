<page>
    <actionBar>
        <gridLayout columns="auto, *" rows="*" style="width: 100%;" horizontalAlignment="left">
            <label text="{icons.menu}" row="0" col="0" class="icon" horizontalAlignment="left"
                verticalAlignment="center" on:tap="{() => side_drawer.nativeView.toggleDrawerState() }" ></label>
            <label text="Home" row="0" col="1" class="title" horizontalAlignment="left"
                verticalAlignment="center"></label>
        </gridLayout>
    </actionBar>
    <radSideDrawer bind:this="{side_drawer}">
        <radSideDrawer.drawerContent>
           <SideMenu />
        </radSideDrawer.drawerContent>
        <radSideDrawer.mainContent>
            <stackLayout>    
                {#if $user_token}                
                    <tabView>
                        <tabViewItem title="Your Feed" >
                            <ArticleList filtertype="feed"  usertoken="{ $user_token }" />
                        </tabViewItem>
                        <tabViewItem title="Global Feed">
                            <ArticleList filtertype="global"  usertoken="{ $user_token }" />
                        </tabViewItem>
                    </tabView>
                {:else}
                    <ArticleList filtertype="global" />
                {/if}
            </stackLayout>
        </radSideDrawer.mainContent>
    </radSideDrawer>
</page>

<style>
    actionBar {
        background-color: black;
        color: white;
    }

</style>

<script>
    import { icons } from './utils/icons'
    import ArticleList from './components/ArticleList'
    import SideMenu from './components/SideMenu'
    import { user_token, user_profile } from './stores/user'
    import { onMount } from 'svelte'

    let side_drawer;

    onMount(()=>{
        console.log("starting with ", $user_token)
        if ($user_token) {
            user_profile.loadUserFromToken($user_token).catch(e=>console.log("error loading profile", e))
        }
    })
    
</script>