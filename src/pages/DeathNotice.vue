<template>
  <q-page padding class="flex flex-center">
    <div class="row q-col-gutter-md">
      <div class="col-12 col-md-6">
        <q-card
          style="min-width: 500px"
          v-for="(item, i) in dNotices"
          :key="'item' + i"
          class="q-mb-md"
        >
          <q-card-section>
            <q-list>
              <q-item-section>
                <div class="text-h6 text-center q-pb-md">第{{ i + 1 }}项</div>
              </q-item-section>
              <q-item-section class="q-mb-md">
                <div class="row">
                  <q-input
                    class="col-8"
                    dense
                    v-model="item.attacker"
                    filled
                    label="击杀者"
                  />
                  <q-btn
                    class="col-3 q-ml-md"
                    color="primary"
                    :label="item.attackerCamp"
                    :class="item.attackerCamp"
                    @click="toggleAttackerCamp(item)"
                  />
                </div>
              </q-item-section>
              <q-item-section class="q-mb-md">
                <div class="row">
                  <q-input
                    class="col-8"
                    dense
                    v-model="item.victim"
                    filled
                    label="被杀者"
                  />
                  <q-btn
                    class="col-3 q-ml-md"
                    color="primary"
                    :label="item.victimCamp"
                    :class="item.victimCamp"
                    @click="toggleVictimCamp(item)"
                  />
                </div>
              </q-item-section>
              <q-item-section class="q-mb-md">
                <div class="row">
                  <q-select
                    dense
                    label="前置图标"
                    filled
                    v-model="item.prefixIcon"
                    use-input
                    use-chips
                    multiple
                    hide-dropdown-icon
                    new-value-mode="add"
                    class="col-8"
                    :options="prefixIcons"
                  />
                  <q-btn
                  class="col-3 q-ml-md"
                    color="primary"
                    label="边框"
                    @click="toggleBorder(item)"
                  />
                </div>
              </q-item-section>
              <q-item-section class="q-mb-md">
                <div class="row">
                  <q-select
                    dense
                    label="后置图标"
                    filled
                    v-model="item.suffixIcon"
                    use-input
                    use-chips
                    multiple
                    hide-dropdown-icon
                    new-value-mode="add"
                    class="col-8"
                    :options="suffixIcons"
                  />
                </div>
              </q-item-section>
              <q-item-section class="q-mb-md">
                <div class="row">
                  <q-select
                    class="col-8"
                    filled
                    dense
                    v-model="item.weapon"
                    :options="weapons"
                    label="武器"
                  />
                  <!-- <template v-slot:before>
                      <q-avatar style="background: rgba(0,0,0,0.1);border-radius: 4px">
                        <img :src="require(`../assets/svg/${item}.svg`)" />
                      </q-avatar>
                    </template> -->
                  
                </div>
              </q-item-section>
            </q-list>
          </q-card-section>
        </q-card>
      </div>
      <div class="col-12 col-md-6">
        <q-card class="q-mb-md q-px-xl">
          <q-card-section>
            <q-list>
              <q-item-section>
                <div class="text-h6 text-center q-pb-md">偏好设置</div>
              </q-item-section>
              <q-item-section class="q-mb-md" v-if="!fold">
                <div class="row justify-between">
                  <q-input class="col-5" dense v-model="w" filled label="宽" />
                  <q-input class="col-5" dense v-model="h" filled label="高" />
                </div>
              </q-item-section>
              <q-item-section v-if="!fold" class="q-mb-md">
                <q-input
                  class="col-4"
                  dense
                  v-model="hidpi"
                  filled
                  label="HiDPI"
                />
              </q-item-section>
              <q-item-section v-if="!fold" class="q-mb-md">
                <q-input
                  class="col-4"
                  dense
                  v-model="prefix"
                  filled
                  label="文件前缀"
                />
              </q-item-section>
              <q-item-section class="q-px-lg">
                <div class="row justify-center">
                  <q-btn
                    class="col-3 q-mr-md"
                    color="primary"
                    label="-"
                    @click="minus"
                  />
                  <q-btn class="col-3" color="primary" label="+" @click="add" />
                </div>
              </q-item-section>
              <q-card-section class="q-px-lg">
                <div class="row justify-center">
                  <q-btn
                    class="col-3 q-mr-md"
                    color="primary"
                    label="生成"
                    @click="generate"
                  />
                  <q-btn
                    class="col-3 q-mr-md"
                    color="primary"
                    label="全屏"
                    @click="toggleFullScreen"
                  />
                  <q-btn
                    class="col-3"
                    color="primary"
                    label="折叠"
                    @click="toggleFold"
                  />
                </div>
              </q-card-section>
            </q-list>
          </q-card-section>
        </q-card>
        <q-card style="min-width: 400px;" class="q-mb-md">
          <q-card-section>
            <q-list>
              <q-item-section>
                <div class="text-h6 text-center q-pb-md">预览</div>
              </q-item-section>
              <div class="text-center">
                <div
                  class="deathNotice"
                  v-for="(item, i) in dNotices"
                  :key="i"
                  :class="{ DispRedBorder: item.redBorder }"
                >
                  <!-- 击杀者         -->
                  <span class="attacker" :class="item.attackerCamp">{{
                    item.attacker
                  }}</span>
                  <!-- 前缀图标         -->
                  <span v-for="(preIcon, j) in item.prefixIcon" :key="j">
                    <img
                      class="prefix"
                      :src="require(`../assets/svg/${preIcon}.svg`)"
                    />
                  </span>
                  <!-- 武器         -->
                  <img
                    class="weapon"
                    :src="require(`../assets/svg/${item.weapon}.svg`)"
                  />
                  <!-- 后缀图标         -->
                  <span v-for="(sufIcon, k) in item.suffixIcon" :key="k">
                    <img
                      class="suffix"
                      :src="require(`../assets/svg/${sufIcon}.svg`)"
                    />
                  </span>
                  <!-- 受害者         -->
                  <span class="victim" :class="item.victimCamp">{{
                    item.victim
                  }}</span>
                </div>
              </div>
            </q-list>
          </q-card-section>
        </q-card>
      </div>
          <div
      id="OutputDiv"
      v-show="generating"
      :style="{ height: h + 'px', width: w + 'px' }"
    >
      <div id="DNArea">
        <div
          class="deathNotice"
          v-for="(item, i) in dNotices"
          :key="i"
          :class="{
            DispRedBorder: item.redBorder,
            'dn-transparent': item.hide
          }"
        >
          <!-- 击杀者         -->
          <span class="attacker" :class="item.attackerCamp">{{
            item.attacker
          }}</span>
          <!-- 前缀图标         -->
          <span v-for="(preIcon, j) in item.prefixIcon" :key="j">
            <img
              class="prefix"
              :src="require(`../assets/svg/${preIcon}.svg`)"
            />
          </span>
          <!-- 武器         -->
          <img
            class="weapon"
            :src="require(`../assets/svg/${item.weapon}.svg`)"
          />
          <!-- 后缀图标         -->
          <span v-for="(sufIcon, k) in item.suffixIcon" :key="k">
            <img
              class="suffix"
              :src="require(`../assets/svg/${sufIcon}.svg`)"
            />
          </span>
          <!-- 受害者         -->
          <span class="victim" :class="item.victimCamp">{{ item.victim }}</span>
        </div>
      </div>
    </div>
    </div>
  </q-page>
</template>

<script>
import html2canvas from "html2canvas";
export default {
  name: "DeathNotice",
  data() {
    return {
      weapons: [
        "ak47",
        "ammobox",
        "ammobox_threepack",
        "armor",
        "armor_helmet",
        "assaultsuit",
        "aug",
        "awp",
        "axe",
        "bayonet",
        "bizon",
        "breachcharge",
        "breachcharge_projectile",
        "bumpmine",
        "c4",
        "controldrone",
        "cz75a",
        "deagle",
        "decoy",
        "defuser",
        "diversion",
        "dronegun",
        "elite",
        "famas",
        "firebomb",
        "fists",
        "fiveseven",
        "flashbang",
        "flashbang_assist",
        "frag_grenade",
        "g3sg1",
        "galilar",
        "glock",
        "grenadepack",
        "grenadepack2",
        "hammer",
        "healthshot",
        "heavy_armor",
        "hegrenade",
        "helmet",
        "hkp2000",
        "incgrenade",
        "inferno",
        "kevlar",
        "knife",
        "knife_bowie",
        "knife_butterfly",
        "knife_canis",
        "knife_cord",
        "knife_css",
        "knife_falchion",
        "knife_flip",
        "knife_gut",
        "knife_gypsy_jackknife",
        "knife_karambit",
        "knife_m9_bayonet",
        "knife_push",
        "knife_skeleton",
        "knife_stiletto",
        "knife_survival_bowie",
        "knife_t",
        "knife_tactical",
        "knife_ursus",
        "knife_widowmaker",
        "knifegg",
        "m4a1",
        "m4a1_silencer",
        "m4a1_silencer_off",
        "m249",
        "mac10",
        "mag7",
        "molotov",
        "mp5sd",
        "mp7",
        "mp9",
        "negev",
        "nova",
        "p90",
        "p250",
        "p2000",
        "planted_c4_survival",
        "prop_exploding_barrel",
        "radarjammer",
        "revolver",
        "sawedoff",
        "scar20",
        "sg556",
        "shield",
        "smokegrenade",
        "snowball",
        "spanner",
        "ssg08",
        "stomp_damage",
        "tablet",
        "tagrenade",
        "taser",
        "tec9",
        "ump45",
        "usp_silencer",
        "usp_silencer_off",
        "xm1014",
        "zone_repulsor"
      ],
      prefixIcons: ["revenge", "domination", "blindkill"],
      suffixIcons: [
        "noscope",
        "jumpkill",
        "throughsmoke",
        "penetrate",
        "headshot",
        "suicide",
        "kill360"
      ],
      dNotices: [
        {
          attacker: "Attacker",
          attackerCamp: "T",
          victim: "Victim",
          victimCamp: "CT",
          weapon: "ak47",
          prefixIcon: ["blindkill"],
          suffixIcon: ["headshot"],
          redBorder: false,
          hide: false
        },
        {
          attacker: "中文字体样式.gg",
          attackerCamp: "CT",
          victim: "Purp1e",
          victimCamp: "T",
          weapon: "awp",
          prefixIcon: ["revenge", "blindkill"],
          suffixIcon: [],
          redBorder: true,
          hide: false
        }
      ],
      generating: false,
      w: 1920,
      h: 1080,
      hidpi: 2,
      prefix: "击杀-",
      current: 0,
      fold: false,
      toggle: false,
      modelAdd: null
    };
  },
  methods: {
    toggleAttackerCamp(item) {
      if (item.attackerCamp === "CT") {
        item.attackerCamp = "T";
      } else {
        item.attackerCamp = "CT";
      }
    },
    toggleVictimCamp(item) {
      if (item.victimCamp === "CT") {
        item.victimCamp = "T";
      } else {
        item.victimCamp = "CT";
      }
    },
    minus() {
      this.dNotices.pop();
    },
    add() {
      this.dNotices.push({
        attacker: "Attacker",
        attackerCamp: "T",
        victim: "Victim",
        victimCamp: "CT",
        weapon: "ak47",
        prefixIcon: [],
        suffixIcon: [],
        redBorder: false,
        hide: false
      });
    },
    toggleFold() {
      this.fold = !this.fold;
    },
    toggleFullScreen() {
      if (
        !document.fullscreenElement &&
        !document.mozFullScreenElement &&
        !document.webkitFullscreenElement
      ) {
        // current working methods
        if (document.documentElement.requestFullscreen) {
          document.documentElement.requestFullscreen();
        } else if (document.documentElement.mozRequestFullScreen) {
          document.documentElement.mozRequestFullScreen();
        } else if (document.documentElement.webkitRequestFullscreen) {
          document.documentElement.webkitRequestFullscreen(
            Element.ALLOW_KEYBOARD_INPUT
          );
        }
      } else {
        if (document.cancelFullScreen) {
          document.cancelFullScreen();
        } else if (document.mozCancelFullScreen) {
          document.mozCancelFullScreen();
        } else if (document.webkitCancelFullScreen) {
          document.webkitCancelFullScreen();
        }
      }
    },
    generate() {
      // 先显示outputDiv，再延迟50ms生成图片
      this.generating = true;
      // 设置参数确保第一张图片只有第一个击杀
      this.current = 0;
      for (let j = 0; j < this.dNotices.length; j++) {
        this.dNotices[j].hide = j !== 0;
      }

      setTimeout(this.html2canvas, 50);
    },
    html2canvas() {
      // html2canvas获取元素、生成图片、并跳转下载
      const e = document.getElementById("OutputDiv");
      // 滚动条置顶解决生成图片不全的问题
      window.pageYOffset = 0;
      document.documentElement.scrollTop = 0;
      document.documentElement.scrollLeft = 0;
      document.body.scrollTop = 0;

      const hidpi = this.hidpi; // 缩放倍率，不随浏览器缩放改变
      html2canvas(e, {
        allowTaint: false,
        useCORS: false,
        backgroundColor: "rgba(0,0,0,0)",
        scale: hidpi
      }).then(canvas => {
        const dataURL = canvas.toDataURL("image/png");
        this.imgUrl = dataURL;
        if (this.imgUrl !== "") {
          const link = document.createElement("a");
          const context = canvas.getContext("2d");
          // [重要]关闭抗锯齿
          context.mozImageSmoothingEnabled = false;
          context.webkitImageSmoothingEnabled = false;
          context.msImageSmoothingEnabled = false;
          context.imageSmoothingEnabled = false;
          link.href = canvas.toDataURL();
          // !文件名设置
          link.setAttribute(
            "download",
            this.prefix + (this.current + 1) + ".png"
          );
          link.style.display = "none";
          document.body.appendChild(link);
          link.click();

          // !确保一个一个生成
          if (++this.current < this.dNotices.length) {
            for (let j = 0; j < this.dNotices.length; j++) {
              this.dNotices[j].hide = j !== this.current;
            }
            setTimeout(this.html2canvas, 50);
          } else {
            this.generating = false;
          }
        }
      });
    },
    toggleBorder(item) {
      item.redBorder = item.redBorder !== true;
    }
  }
};
</script>

<style scoped>
@font-face {
  font-family: "Stratum2";
  src: url("~assets/fonts/Stratum2.ttf");
  font-style: normal;
  font-weight: bold;
}

@media screen and (max-width: 960px) {
  /* 当屏幕宽度小于960px时，适配的CSS代码块*/
  .main-container {
    width: max-content;
  }
  .dn-item-container {
    width: 60vw;
  }
  .sidebar {
    width: 32vw;
  }
  .preview-container {
    /*margin:  475px 30px 30px auto;*/
    margin: 415px 30px 30px auto;
  }
}
@media screen and (min-width: 961px) {
  /* 当屏幕宽度大于等于961px时，适配的CSS代码块*/
  .main-container {
    width: 900px;
  }
  .dn-item-container {
    width: 600px;
  }
  .sidebar {
    width: 300px;
  }
  .preview-container {
    /*margin:  475px 30px 30px auto;*/
    margin: 415px 30px 30px auto;
  }
}

.main-container {
  margin: 0 auto;
  display: block;
}

.sidebar {
  float: right;
}

/*击杀信息设置区域的容器*/
.dn-item-container {
  float: left;
  /*display: block;*/
  background: white;

  transition: 0.3s;
}

/*设置单个击杀信息的面板*/
.dn-item {
  background: #fefefe;
  height: max-content;
  margin: 30px 30px 30px auto;
  border-radius: 6px;
  padding: 20px;
  mso-border-shadow: yes;
  filter: drop-shadow(4px 4px 10px rgba(0, 0, 0, 0.1));
}

.preference-container {
  position: fixed;
  background: #fefefe;
  height: max-content;
  margin: 30px 30px 30px auto;
  border-radius: 6px;
  padding: 20px;
  mso-border-shadow: yes;
  filter: drop-shadow(4px 4px 10px rgba(0, 0, 0, 0.1));
}

.preview-container {
  position: fixed;
  background: #fefefe;
  height: max-content;
  border-radius: 6px;
  padding: 20px;
  mso-border-shadow: yes;
  filter: drop-shadow(4px 4px 10px rgba(0, 0, 0, 0.1));
}

.preview-fold {
  margin: 240px 30px 30px auto;
}

#OutputDiv {
  clear: both;
  /*width: 1920px;*/
  /*height: 1080px;*/
  /*position: fixed;*/
  /*float: end;*/
  margin: 30px auto;
  /*background: pink;  !*debug用的颜色*!*/
  background: rgba(0, 0, 0, 0);
  /**/
  /*font-weight: bold;*/
  /*font-family: 'Stratum2';*/
  /* src: url("../assets/font/Stratum2.ttf") format('truetype'); */
}

#DNArea {
  width: max-content;
  float: right;
  /*display: flex;*/
  /*justify-content: end;*/
  /*float: right;*/
  /*horiz-align: right;*/
  margin-top: 72px; /*距离顶边的距离*/
  margin-right: 10px; /*距离右侧边的距离*/
  /*text-align: center;*/
  /*line-height: 24px;*/
}

.deathNotice {
  font-family: "Stratum2", "Arial Unicode MS";
  font-size: 18px;
  font-weight: bold;
  width: max-content;
  /*float: right;*/
  /* right: 0; */
  margin: 2px 0 0 auto; /*击杀条之间的距离*/
  padding: 5px 10px 5px 10px;
  transition-property: opacity;
  transition-timing-function: ease-out;
  background-color: rgba(0, 0, 0, 0.65);
  border-radius: 4px;
  text-align: center;
  /*box-shadow: inset #e10000e6 0px 0px 1px;*/
  /*border: 2px solid #e10000;*/
  /*border: 2px outset rgba(0,0,0,0.44);*/
}

.dn-transparent {
  background-color: rgba(0, 0, 0, 0);
  opacity: 0;
}

.btnRedBorder {
  border-color: #e10000;
  /*border: 1px solid #e10000;*/
}

.DispRedBorder {
  /*height: 30px;*/
  border: 2px solid #e10000;
  padding: 3px 8px 3px 8px;
}

.attacker {
  padding-right: 4px;
  font-size: 16px;
}

.victim {
  padding-left: 4px;
  font-size: 16px;
}

.CT {
  color: rgb(111, 156, 230);
}

.T {
  color: rgb(234, 190, 84);
}

.weapon {
  background: rgba(0, 0, 0, 0);
  height: 23px;
  vertical-align: middle;
  visibility: visible;
}

.prefix {
  padding-right: 2px;
  height: 24px;
}

.suffix {
  padding-left: 2px;
  height: 24px;
}
</style>
