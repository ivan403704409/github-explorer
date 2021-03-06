<template lang="html">
    <div id="nav-menu">
        <div id="search-bar" v-el:searchbar>
            <search-input
                placeholder="Search by username..."
                :searchtext.sync="searchText"
                @search="getUsers"
                @focus="fullNavMenu"
            ></search-input>
            <div id="cancel-button"
                @click="openNavMenu"
            >Cancel</div>
        </div>
        <div id="user-list"
            @scroll="highlightSearchbar"
            v-el:userlist
        >
            <div id="loading" v-if="searching">
                <div class="loading"></div>
            </div>
            <a class="user-item animated"
                @click="userClick(`/user/${user.login}`)"
                transition="fade"
                stagger="100"
                v-for="user in users"
            >
                <avatar
                    class="user-avatar"
                    :src="`https://avatars.githubusercontent.com/u/${user.id.split('-')[1]}`"
                ></avatar>
                <div class="user-info">
                    <div class="fullname">{{user.fullname || user.login}}</div>
                    <div class="username">{{user.login || user.fullname}}</div>
                </div>
            </a>
        </div>
    </div>

</template>

<script>
import SearchInput from './SearchInput';
import Avatar from './Avatar';

import {
    openNavMenu,
    fullNavMenu,
    closeNavMenu
    } from '../vuex/actions';

export default {
    data() {
        return {
            searchText: '',
            users: [],
            searching: true,

            wait: false
        }
    },
    vuex: {
        actions: {
            openNavMenu,
            fullNavMenu,
            closeNavMenu
        }
    },
    attached() {
        this.getUsers();
    },
    components: {
        SearchInput,
        Avatar
    },
    methods: {
        getUsers() {
            this.searching = true;
            this.$http
            .get('https://api.github.com/legacy/user/search/' +
                `${this.searchText || Math.random().toString(36).split('')[2]}%20sort:followers`)
            .then(response => response.json().users.slice(0, 15))
            .then(users => {
                this.users = users;
                this.searching = false;
            });
        },
        userClick(url) {
            this.closeNavMenu();
            setTimeout(() => {
                this.$router.go(url);
            }, 300)
        },
        highlightSearchbar() {
            let lastScrollTop = this.$els.userlist.scrollTop;
            if (!this.wait) {
                window.requestAnimationFrame(() => {
                    if (lastScrollTop > 0) {
                        this.$els.searchbar.classList.add('dark-bg');
                    } else {
                        this.$els.searchbar.classList.remove('dark-bg');
                    }
                    this.wait = false;
                });
                this.wait = true;
            }
        }
    },
    transitions: {
        'fade': {
            enterClass: 'fadeInUp',
            leaveClass: 'fadeOutRight'
        }
    }
}
</script>

<style lang="less">
#nav-menu {
    background-color: #3d536d;
    min-height: 100%;
    width: 100%;
    height: 100vh;
    opacity: 1;
    transform: translate3d(-150px, 0, 0);
    transition: transform 0.3s cubic-bezier(0.7, 0, 0.25, 1),
                opacity 0.4s;

    #search-bar {
      position: relative;
      padding: 15px;
      transition: background-color 0.3s;
      z-index: 1;

      &.dark-bg {
          background-color: #2c4052;
      }

      // In case there is a search input
      #search-input {
          width: 84%;
      }

      #cancel-button {
          position: absolute;
          top: 0;
          right: 0;
          height: 70px;
          width: 16%;
          text-align: left;
          line-height: 70px;
          color: #fff;
          opacity: 0;
          transition: opacity 0.3s 0.2s;
          font-size: 14px;
      }
    }

    #user-list {
        position: absolute;
        left: 0;
        top: 70px;
        right: 0;
        bottom: 0;
        overflow-y: scroll; /* has to be scroll, not auto */
        overflow-x: hidden;
        -webkit-overflow-scrolling: touch;

        .user-item {
            display: flex;
            padding: 12px 15px;
            will-change: transform;

            .user-avatar {
                width: 40px;
                height: 40px;
                border-radius: 5px;
                background-position: center center;
                background-size: cover;
                margin-right: 20px;
                background-color: rgba(255, 255, 255, 0.5);
            }

            .fullname {
                font-family: GothamPro-Medium;
                font-size: 16px;
                color: #ffffff;
                margin-bottom: 9px;
            }

            .username {
                font-family: GothamPro;
                font-size: 12px;
                color: #8f9aa8;
            }
        }

        #loading {
            text-align: center;
        }
    }
}

@keyframes spin {
    0% {
        transform: rotate(0deg);
    }
    100% {
        transform: rotate(360deg);
    }
}

.loading {
    border-radius: 50%;
    width: 24px;
    height: 24px;
    border: .25rem solid rgba(255,255,255,0.2);
    border-top-color: white;
    animation: spin 1s infinite linear;
    margin: 0 auto;
}
</style>
