<template>
  <div class="detail-wrapper">
    <div class="detail-post-user-area">
      <div class="detail-tag">投稿者</div>
      <div class="post-user-area">
        <div class="left-user-area">
          <div class="user-image"></div>
        </div>
        <div class="right-user-area">
          <div class="user-profile-area">
            <div class="user-name-are">
              <div class="user-name-tag">名前</div>
              <router-link :to="`/account/profile/${ job.userId }`"> 
                <div class="user-name">
                  {{ job.user.userName }}
                </div>
              </router-link>
            </div>
            <div class="user-introduce-area">
              <div class="introduce-tag">学習開始</div>
              <div class="introduce">
                {{ job.user.learningStartDate | moment("YYYY年 M月 D日") }}
              </div>
            </div>
          </div>
          <div class="user-url-area">
            <div class="user-github" @click="gitTab">
              <img class="img" src="@/assets/github.png" width="50" />
              </div>
            <div class="user-twtter" @click="twitterTab">
              Twiiter
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="detail-post-skill-area">
      <div class="detail-tag">開発技術</div>
      <div class="skill-detail-area">
        <div class="lang-area">
          <label for="name" class="name-tag">開発言語</label>
          <div class="lang-box">
            <div class="skill-tag"  v-for="langage in job.programingLanguage" :key="langage.id">
              {{ langage.programingLanguageName }}
            </div>
          </div>
        </div>
        <div class="lang-area">
          <label for="name" class="name-tag">フレームワーク</label>
          <div class="lang-box">
            <div class="flame-tag" v-for="framework in job.programingFramework" :key="framework.programingFrameworkName">
              {{ framework.programingFrameworkName }}
            </div>
          </div>
        </div>
        <div class="lang-area">
          <label for="name" class="name-tag">その他関連スキル</label>
          <div class="lang-box">
            <div class="other-tag" v-for="skill in job.skill" :key="skill.skillName">
              {{ skill.skillName }}
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="detail-post-detail-area">
      <div class="detail-area">
        <div class="detail-tag">開発詳細</div>
        <div class="dev-detail-area">
          <div class="detail-leff-area">
            <div class="detail-information">
              <div class="tag">タイトル</div>
              <div class="sub-area">{{ job.jobTitle }}</div>
            </div>
            <div class="detail-information">
              <div class="tag">募集人数</div>
              <div class="sub-area">{{ job.recruitmentNumbers }}人</div>
            </div>
            <div class="detail-information">
              <div class="tag">応募ケース</div>
              <div class="sub-area">新規開発</div>
            </div>
            <div class="detail-information">
              <div class="tag">開発期間</div>
              <div class="sub-area">{{ job.devStartDate | moment("YYYY年 M月 D日") }} ~ {{ job.devEndDate  | moment("YYYY年 M月 D日")}}</div>
            </div>
            <div class="detail-information">
              <div class="tag">応募ケース</div>
              <div class="sub-area">{{ job.jobDescription }}</div>
            </div>
          </div>
          <!-- <div class="detail-right-area">
            <div class="tag">募集内容詳細</div>
            <div class="description">
              {{ job.jobDescription }}
            </div>
          </div> -->
        </div>
      </div>
    </div>
    <div class="button-area">
      <div class="button-action-area" @click="registerRedirect">
          <button class="btn-box-apply">応募する</button>
        <div class="favorite-btn-area">
          <font-awesome-icon icon="heart" class="icon"/>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import moment from "moment";
// import Applybtn from '@/components/button/Applybtn'
// import FavoriteDetailBtn from '@/components/button/FavoriteDetailBtn'
// import Loading from '@/components/common/Loading'
// import ApplyModal from '@/components/modal/ApplyModal'
import GithubImage from '@/assets/github.png'
export default {
  props: {
    id: Number,
  },
  data() {
    return {
      job: {},
      jobs: [],
      assetsImage: GithubImage,
      assetsImage_NG: '@/assets/github.png',
      staticImage: '@/assets/github.png',
    }
  },
  filters: {
    moment(value, format) {
      return moment(value).format(format);
    }
  },
  created() {
    // * 詳細画面情報を取得
    axios.get(`${this.$httpPosts}/${this.id}/`)
      .then(response => {
          // this.loading = true;
          console.log(response.data)
          this.job = response.data
          console.log("よまれてるよ")
      })
  },
  methods: {
    registerRedirect() {
      alert("登録が必要です");
      this.$router.push('/register');
    },
    // * Twitter をタブで開く
    twitterTab() {
      axios.get(`${this.$httpPosts}/${this.id}/`)
      .then(response => {
        this.job = response.data
        return window.open(this.job.user.twitterAccount)
      })
    },
    // * Github をタブで開く
    gitTab() {
      axios.get(`${this.$httpPosts}/${this.id}/`)
      .then(response => {
        this.job = response.data
        console.log(this.job)
        return window.open(this.job.user.githubAccount)
      })
    },
  },
  components: {
    // Applybtn,
    // FavoriteDetailBtn,
    // Loading,
    // ApplyModal,
  }
}
</script>

<style lang="scss" scoped>
.router {
  text-decoration: none;
  color: $basic-white;
}

.detail-wrapper {
  width: 85%;
  padding: 3.5rem 0rem;
  margin: 0 auto;

  .detail-post-user-area {
    width: 80%;
    display: flex;
    flex-direction: column;
    text-align: left;
    margin: 0 auto;
  }
}

.detail-tag {
  color: $primary-color;
  text-align: left;
  font-size: 17px;
  font-weight: bold;
  margin-bottom: 0.7rem;
}

/* 投稿者 カード中身 */
.detail-wrapper .detail-post-user-area .post-user-area {
  @include card-border-color;
  border-radius: 4px;
  padding: 2rem 4rem;
  margin-bottom: 2rem;
  position: relative;
}

/* ユーザー画像 start*/
.post-user-area {
  .left-user-area {
    width: 20%;
    height: 100%;

    .user-image {
      @include user-image;
      width: 130px;
      height: 130px;
    }
  }

  .right-user-area {
    width: 70%;
    position: absolute;
    right: 0;
    top: 0;
    padding: 2rem 4rem 2rem 2rem;
    text-align: left;

    .user-profile-area {
      width: 65%;
      height: 100%;
      display: inline-block;
    }
  }
}

/* ユーザー 詳細情報 start */
.user-profile-area {
  .user-name-are {
    width: 45%;
    display: inline-block;

    .user-name-tag {
      font-weight: bold;
    }

    .user-name {
      margin-top: 0.2rem;
      font-size: 14px;
    }
  }

  .user-study-area {
    width: 45%;
    display: inline-block;

    .study-tag {
      font-weight: bold;
      width: 45%;
    }

    .stydy-time {
      margin-top: 0.2rem;
    }
  }

  .user-introduce-area {
    margin-top: 3.2rem;

    .introduce-tag {
      font-weight: bold;
    }

    .introduce {
      margin-top: 0.2rem;
      font-size: 14px;
    }
  }
}

.post-user-area .right-user-area .user-url-area {
  display: inline-block;
  width: 30%;
  position: absolute;
  top: 0;
  padding: 2.2rem 0 0 0;

  .user-github {
    /* width: 35%;
    padding: 0.8rem 1rem;
    background-color: #24292e;
    border-radius: 5px / 5px;
    margin-right: 10px;
    color: #FFFFFF;
    text-align: center;
    box-shadow: 10px 5px 5px grey;
    box-shadow: 0 0 3px 0 rgba(122, 122, 122, 0.705), 0 2px 3px 0 rgba(156, 156, 156, 0.993);
    font-weight: bold; */
    cursor: pointer;

    :hover {
      opacity: 0.8;
    }
  }

  .user-twtter {
    margin-top: 2rem;
    width: 68%;
    padding: 0.8rem;
    background-color: #1DA1F2;
    border-radius: 5px / 5px;
    color: #FFFFFF;
    text-align: center;
    box-shadow: 10px 5px 5px grey;
    box-shadow: 0 0 3px 0 rgba(0, 0, 0, 0.12), 0 2px 3px 0 rgba(0, 0, 0, 0.22);
    font-weight: bold;
    cursor: pointer;

    :hover {
      opacity: 0.8;
    }
  }
}

/* スキル カード */
.detail-wrapper .detail-post-skill-area {
  width: 80%;
  display: flex;
  flex-direction: column;
  text-align: left;
  margin: 0 auto 2rem auto;

  .skill-detail-area {
    @include card-border-color;
    border-radius: 4px;
    padding: 1.5rem 4rem 1rem 4rem;
    margin-bottom: 2rem;
    position: relative;
  }
}

.skill-detail-area .lang-area {
  width: 100%;
  position: relative;

  .name-tag {
    font-weight: bold;
    text-align: left;
    position: absolute;
    left: 0;
  }

  .lang-box {
    width: 95%;
    text-align: left;
    padding: 10px 20px 30px 0;

    .skill-tag {
      @include detail-language;
    }

    .flame-tag {
      @include detail-framework;
    }

    .other-tag {
      @include detail-skill;
    }
  }
}

/* 開発詳細 カード */
.detail-wrapper .detail-post-detail-area {
  width: 80%;
  display: flex;
  flex-direction: column;
  text-align: left;
  margin: 0 auto;

  .dev-detail-area {
    @include card-border-color;
    border-radius: 4px;
    padding: 1rem 4rem 1rem 4rem;
    margin-bottom: 2rem;
    position: relative;
    line-height: 1.8;
  }
}

.dev-detail-area .detail-leff-area {
  display: inline-block;
  width: 100%;
  height: 100%;

  .detail-information {
    margin-top: 1px;
    padding: 1rem 0;
    position: relative;
    flex-direction: column;
  }
}

.tag {
  font-weight: bold;
}

.detail-information .sub-area {
  right: 0;
  font-size: 14px;
}

.dev-detail-area .detail-right-area {
  line-height: 1.8;
  width: calc(50% - 5rem);
  display: inline-block;
  top: 0;
  padding: 2.5rem 5rem 0 0;
}

/* ボタン エリア */
.button-area {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  position: -webkit-sticky;
  position: sticky;
  left: 0;
  bottom: 0;

  .button-action-area {
    margin: 0em auto 4rem auto;
    width: 50%;
    position: relative;
  }
}

/* 応募するボタン */
.btn-box-apply {
  @include red-btn;
  @include box-shadow-btn;
  color: $basic-white;
  position: absolute;
  left: 0;
  top: 0;
  width: 60%;
  padding: 1.2rem 4rem;
  transition: .3s;
  border-radius: 50px;
  font-weight: 600;
  line-height: 1;
  text-align: center;
  margin: auto;
  font-size: 1.3rem;
  display: inline-block;
  cursor: pointer;
  border: none;

  &:hover {
    @include red-btn-hover;
  }
}

.favorite-btn-area {
  position: absolute;
  right: 0;
  top: 0;
  width: 50%;
}

.icon {
  font-size: 30px;
  padding: 10px;
  width: 38px;
  height: 38px;
  color: $basic-white;
  cursor: pointer;
  background-color: #d8d6d6;
  border-radius: 5px / 5px;
}

</style>