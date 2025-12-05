<script setup>
import { ref } from "vue";


const sideList = ref([
  { id: 0, name: 'Home', icon: 'home' },
  { id: 1, name: 'Order', icon: 'order' },
  { id: 2, name: 'Message', icon: 'message' },
])
const mediaList = ref([
  { id: 0, name: 'ins', active: false, site: '', tempSite: '' },
  { id: 1, name: 'dy', active: false, site: '', tempSite: '' },
  { id: 2, name: 'yt', active: false, site: '', tempSite: '' },
  { id: 3, name: 'x', active: false, site: '', tempSite: '' },
  { id: 4, name: 'site', active: false, site: '', tempSite: '' },
])
const sideActiveId = ref(0);
const avatarUrl = ref("");
const bgUrl = ref("");

const site = ref("Closr.so");
const userName = ref("Please Enter");
const userDes = ref("Please Enter");
const text = ref("");

const handleAvatarSuccess = (response, uploadFile) => {
  avatarUrl.value = URL.createObjectURL(uploadFile.raw);
};

const handleBgSuccess = (response, uploadFile) => {
  bgUrl.value = URL.createObjectURL(uploadFile.raw);
};

const clickMedia = item => {
  item.active = !item.active;
  item.tempSite = item.site;
}
const delMedia = item => {
  item.site = '';
  item.tempSite = '';
}
const confirmMedia = item => {
  item.site = item.tempSite;
}
</script>

<template>
  <div class="common-layout">
    <el-container class="app-container">
      <el-header class="header">
        <span class="logo">Closr</span>

        <div class="avatar">
          <el-avatar :size="50" src="https://cube.elemecdn.com/0/88/03b0d39583f48206768a7534e55bcpng.png" />
          <div class="user-info">
            <span class="name">Chris Wu
              <img class="user-info-tag" src="/src/assets/tag.png"></span>
            <span class="id">ID:9732002</span>
          </div>
          <el-icon>
            <ArrowDown />
          </el-icon>
        </div>
      </el-header>

      <el-container>
        <el-aside class="side">
          <span v-for="item in sideList" :key="item.id" :class="`side-option ${ item.id === sideActiveId ? 'side-active' : ''}`"><img class="side-icon" :src="`/src/assets/${ item.icon }.png`">{{ item.name }}</span>
        </el-aside>

        <el-main class="main">
          <div class="creator-form">
            <div class="form-box">
              <el-breadcrumb class="breadcrumb" :separator-icon="ArrowRight">
                <el-breadcrumb-item>Homepage</el-breadcrumb-item>
                <el-breadcrumb-item>Manage my page</el-breadcrumb-item>
                <el-breadcrumb-item :to="{ path: '/' }">Become a creator</el-breadcrumb-item>
                <el-breadcrumb-item></el-breadcrumb-item>
              </el-breadcrumb>

              <div class="form-items">
                <span class="form-name">Become a creator</span>
                <div style="width: 600px">
                  <el-steps direction="vertical">
                    <el-step title="Choose your URL">
                      <template #description>
                        <div class="form-des">
                          <el-input v-model="site" placeholder="Please Enter" />
                        </div>
                      </template>
                    </el-step>

                    <el-step title="Username">
                      <template #description>
                        <div class="form-des">
                          <el-input v-model="userName" placeholder="Please Enter" />
                        </div>
                      </template>
                    </el-step>

                    <el-step title="Profile Background">
                      <template #description>
                        <div class="form-des" style="display: flex; flex-direction: row">
                          <el-upload class="avatar-uploader" :show-file-list="false" :on-progress="handleAvatarSuccess">
                            <img v-if="avatarUrl" :src="avatarUrl" class="avatar-uploader-icon avatar" />
                            <el-icon v-else class="avatar-uploader-icon">
                              <Plus />
                            </el-icon>
                          </el-upload>

                          <el-upload class="avatar-uploader" :show-file-list="false" :on-progress="handleBgSuccess">
                            <img v-if="bgUrl" :src="bgUrl" class="avatar-uploader-icon uploader-bg" />
                            <el-icon v-else class="avatar-uploader-icon uploader-bg">
                              <Plus />
                            </el-icon>
                          </el-upload>
                        </div>
                      </template>
                    </el-step>

                    <el-step title="About me">
                      <template #description>
                        <div class="form-des">
                          <el-input v-model="userDes" show-word-limit maxlength="500" resize="none" :autosize="{ minRows: 4, maxRows: 8 }" type="textarea" placeholder="Please Enter" />
                        </div>
                      </template>
                    </el-step>

                    <el-step title="Social Media">
                      <template #description>
                        <div class="media-list">
                          <img :class="`${item.site ? 'media-img-active' : 'media-img'}`" v-for="item in mediaList" :key="item.id" :src="`/src/assets/${ item.name }.png`" @click="clickMedia(item)">
                        </div>

                        <template v-for="item in mediaList" :key="item.id">
                          <div class="form-des" v-if="item.active">
                            <el-input v-model="item.tempSite" placeholder="Please Enter">

                              <template #prepend>
                                <img class="media-img" :src="`/src/assets/${ item.name }.png`">
                              </template>

                              <template v-if="item.site" #append><el-icon class="media-del" @click="delMedia(item)">
                                  <Delete />
                                </el-icon>
                              </template>
                              <template v-else #append><el-icon class="media-confirm" @click="confirmMedia(item)">
                                  Confirm
                                </el-icon>
                              </template>
                            </el-input>
                          </div>
                        </template>

                      </template>
                    </el-step>

                    <el-step title="Tags">
                      <template #description>
                        <el-tag class="tags" type="info"><el-icon>
                            <ChromeFilled />
                          </el-icon>Health
                          manintenance</el-tag>
                        <el-tag class="tags" type="info"><el-icon>
                            <ChromeFilled />
                          </el-icon>Food
                          travel</el-tag>
                        <el-tag class="tags" type="info"><el-icon>
                            <ChromeFilled />
                          </el-icon>Art
                          design</el-tag>
                        <el-tag class="tags" type="info"><el-icon>
                            <ChromeFilled />
                          </el-icon>Sport</el-tag>
                        <el-tag class="tags" type="info"><el-icon>
                            <ChromeFilled />
                          </el-icon>Travel</el-tag>
                      </template>
                    </el-step>
                  </el-steps>
                </div>
                <el-button class="continue" round>Continue</el-button>
              </div>
            </div>
            <div class="user-info-view">
              <div v-if="!bgUrl" class="user-bg"></div>
              <img v-else :src="bgUrl" class="user-bg-img" />
              <el-avatar class="user-avatar" shape="square" :size="140" :src="avatarUrl" />
              <img class="avatar-tag" src="/src/assets/tag.png">
              <div class="user-name">{{ userName }}</div>
              <div class="user-des">
                <div class="user-des-title">About me</div>
                <div class="des">{{ userDes }}</div>
              </div>
            </div>
          </div>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>


<style lang="scss" scoped>
body {
  margin: 0 !important;
}
.app-container {
  background: #fff;
  .header {
    padding: 0;
    display: flex;
    align-items: center;
    justify-content: space-between;

    .logo {
      width: 240px;
      font-size: 28px;
      color: #0b1526;
      font-weight: bold;
      text-align: center;
    }

    .avatar {
      width: 300px;
      display: flex;
      align-items: center;
      justify-content: center;

      .user-info {
        display: flex;
        margin: 0 10px;
        flex-direction: column;

        .name {
          font-weight: bold;
          font-size: 16px;
          display: flex;
          align-items: center;
        }

        .id {
          color: #b4b4b4;
          font-size: 12px;
        }

        .user-info-tag {
          width: 20px;
          height: 20px;
          filter: grayscale(100%);
        }
      }
    }
  }

  .side {
    display: flex;
    width: 240px;
    padding-top: 20px;
    align-items: center;
    flex-direction: column;

    .side-option {
      width: 180px;
      padding: 10px;
      margin-bottom: 10px;
      background: #fff;
      border-radius: 8px;
      display: flex;
      align-items: center;
      color: #1f1f1f;
      font-weight: 400;
      font-size: 16px;
      cursor: pointer;

      .side-icon {
        width: 20px;
        height: 20px;
        margin-right: 6px;
      }
    }

    .side-active,
    .side-option:hover {
      color: #fff;
      background: #1f1f1f;
      font-weight: bold;
    }
  }

  .main {
    border-radius: 10px;
    background: #fafafa;
    display: flex;
    justify-content: center;

    .creator-form {
      display: flex;

      ::v-deep .el-steps {
        .el-step__line {
          border-left: 1px dashed #f3f3f3;
          width: 1px;
          background-color: transparent;
        }
        .is-text {
          width: 28px;
          height: 28px;
          color: #979797;
        }
        .el-step__icon-inner {
          font-size: 16px;
          color: #1f1f1f;
        }
        .el-step__title {
          color: #1f1f1f;
          font-size: 20px;
          font-weight: bold;
          margin-left: 6px;
        }
        .el-input {
            display: flex;
        }
        .el-input__wrapper,
        .el-input-group__prepend,
        .el-input-group__append,
        .el-textarea__inner {
          background: #fafafa;
          border-radius: 12px;
          border-style: none;
          height: 42px;
          border: none;
          box-shadow: 0 0 0 0px;
        }
        .el-input__inner {
          width: 420px;
        }

        .el-input-group__prepend {
          height: 42px;
          width: 42px;
          border-radius: 12px;
          background: #fafafa;
          margin-right: 10px;
          padding: 0;
          img {
            width: 42px;
            height: 42px;
          }
        }
        .el-input-group__append {
          height: 42px;
          padding: 0;
          background: #fff;
          margin-left: 10px;
          color: #000;
          border: 1px solid #E7E7E7;
        }
        .media-del {
          width: 42px;
          color: #000;
          margin: 20px;
          font-weight: bold;
          cursor: pointer;
        }
        .media-confirm {
          width: 81px;
          font-weight: bold;
          font-style: inherit;
          cursor: pointer;
        }
        .el-input__count {
          background: #fafafa;
        }
        .el-tag {
          padding: 10px 14px;
          margin: 0 10px 10px 0;
        }
        .el-tag__content {
          display: flex;
          min-width: 50px;
          align-items: center;
        }
        .el-input__inner,
        .el-textarea__inner {
          color: #bfbfbf;
          font-weight: 300;
        }
      }

      .form-box {
        width: 665px;
        .breadcrumb {
          height: 40px;
          font-size: 14px;
          color: #808080;
        }

        .form-items {
          padding: 20px;
          border-radius: 10px;
          background: #fff;

          .form-name {
            font-size: 24px;
            font-weight: bold;
            margin-bottom: 20px;
            display: flex;
            color: #1f1f1f;
          }

          .media-list {
            display: flex;
            margin-bottom: 10px;

            .media-img,
            .media-img-active {
              filter: grayscale(100%);
              width: 38px;
              height: 38px;
              border-radius: 50px;
              background: #fafafa;
              margin-right: 10px;
              cursor: pointer;
            }


            .media-img-active {
              filter: grayscale(0);
            }
          }

          .form-des {
            display: flex;
            align-items: center;
            margin: 20px 0;

            .avatar-uploader-icon {
              width: 120px;
              height: 120px;
              border-radius: 12px;
              border: 1px dashed #f3f3f3;
              margin-right: 20px;
            }

            .uploader-bg {
              width: 364px;
              height: 120px;
              display: flex;
              border-radius: 12px;
              justify-content: center;
              border: 1px dashed #f3f3f3;
            }
          }
          .continue {
            width: 160px;
            height: 42px;
            color: #fff;
            font-size: 14px;
            background: #3478ff;
            margin-top: 20px;
          }
        }

        .tags {
            background: #FAFAFA;
            height: 40px;
            border-radius: 8px;
            border: 0;
            font-size: 14px;
            color: #1F1F1F;
            font-weight: bold;
            cursor: pointer;
        }
      }

      .user-info-view {
        position: relative;
        border-radius: 20px;
        width: 385px;
        height: 346px;
        background: #fff;
        overflow: hidden;
        margin: 40px 20px;

        .user-bg {
          height: 140px;
          background: #3278ff;
        }

        .user-bg-img {
          height: 140px;
          width: 385px;
        }

        .user-avatar {
          top: 60px;
          left: calc((385px - 140px) / 2);
          border-radius: 12px;
          border: 1px dashed #fafafa;
          position: absolute;
          background: #eaeaea;
        }

        .avatar-tag {
          width: 30px;
          height: 30px;
          position: absolute;
          top: 180px;
          left: 245px;
          color: #3278ff;
        }

        .user-name {
          top: 210px;
          width: 385px;
          text-align: center;
          font-size: 26px;
          color: #1f1f1f;
          font-weight: bold;
          text-align: center;
          position: absolute;
          white-space: nowrap;
          overflow: hidden;
          text-overflow: ellipsis;
        }

        .user-des {
          top: 260px;
          left: 15px;
          position: absolute;
          .user-des-title {
            color: #333;
            font-size: 24px;
            font-weight: bold;
            margin-bottom: 10px;
          }

          .des {
            height: 40px;
            width: 353px;
            font-size: 16px;
            color: #808080;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
          }
        }
      }
    }
  }
}
</style>
