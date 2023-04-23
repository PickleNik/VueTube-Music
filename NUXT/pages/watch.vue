<template>
  <div
    id="watch-body"
    class="background flex flex-column justify-center align-center"
  >
    <span
      class="background--text"
      :class="$vuetify.theme.dark ? 'text--lighten-4' : 'text--darken-4'"
      style="font-family: monospace"
    >
      Artist
    </span>
    <h1>Title</h1>

    <div
      class="background"
      style="
        width: 80%;
        margin: 0.5rem auto;
        aspect-ratio: 1/1 !important;
        border-radius: 3rem;
      "
      :class="$vuetify.theme.dark ? 'lighten-1' : 'darken-1'"
    ></div>
    <div class="flex justify-space-around" style="max-height: 6rem">
      <q-btn
        class="background"
        :class="$vuetify.theme.dark ? 'lighten-2' : 'darken-2'"
        style="height: 2rem !important; width: 2rem; border-radius: 100%"
      >
        <v-icon size="2.5rem">mdi-skip-previous</v-icon>
      </q-btn>
      <q-btn
        class="background"
        :class="$vuetify.theme.dark ? 'lighten-2' : 'darken-2'"
        style="
          height: 5rem !important;
          width: 5rem;
          aspect-ratio: 1/1 !important;
          border-radius: 5rem;
          margin: 2rem;
        "
      >
        <v-icon size="3.5rem">mdi-play</v-icon>
      </q-btn>
      <q-btn
        class="background"
        :class="$vuetify.theme.dark ? 'lighten-2' : 'darken-2'"
        style="height: 2rem !important; width: 2rem; border-radius: 100%"
        label="test"
      >
        <v-icon size="2.5rem">mdi-skip-next</v-icon>
      </q-btn>
    </div>
    <!-- <div id="player-container">
      <player
        v-if="sources.length > 0 && video.title && video.channelName"
        ref="player"
        :video="video"
        :sources="sources"
        :recommends="recommends"
      />
    </div> -->
  </div>
</template>

<script>
export default {
  layout: "empty",
};
// import { Share } from "@capacitor/share";
// import { getCpn } from "~/plugins/utils";
// import player from "~/components/Player/index.vue";
// import VidLoadRenderer from "~/components/vidLoadRenderer.vue";
// import ItemSectionRenderer from "~/components/SectionRenderers/itemSectionRenderer.vue";
// import mainCommentRenderer from "~/components/Comments/mainCommentRenderer.vue";
// import SlimVideoDescriptionRenderer from "~/components/UtilRenderers/slimVideoDescriptionRenderer.vue";

// import backType from "~/plugins/classes/backType";

// export default {
//   components: {
//     player,
//     VidLoadRenderer,
//     ItemSectionRenderer,
//     mainCommentRenderer,
//     SlimVideoDescriptionRenderer,
//   },
//   layout: "empty",
//   // transition(to) { // TODO: fix layout switching
//   //   return to.name == "watch"
//   //     ? { name: "slide-up", mode: "" }
//   //     : { name: "slide-down", mode: "" };
//   // },
//   data: function () {
//     return this.initializeState();
//   },
//   watch: {
//     // Watch for change in the route query string (in this case, ?v=xxxxxxxx to ?v=yyyyyyyy)
//     $route: {
//       deep: true,
//       handler(newRt, oldRt) {
//         if (newRt.query.v && newRt.query.v != oldRt.query.v) {
//           // Exit fullscreen if currently in fullscreen
//           // if (this.$refs.player) this.$refs.player.webkitExitFullscreen();
//           // Reset player and run getVideo function again
//           // this.startTime = Math.floor(Date.now() / 1000);
//           // this.getVideo();
//           clearInterval(this.interval);
//           Object.assign(this.$data, this.initializeState());
//           this.mountedInit();
//         }
//       },
//     },
//   },

//   mounted() {
//     this.mountedInit();
//     this.$vuetube.resetBackActions();
//   },

//   beforeDestroy() {
//     clearInterval(this.interval);
//   },

//   methods: {
//     getVideo() {
//       this.loaded = false;

//       this.$youtube.getVid(this.$route.query.v).then((result) => {
//         // TODO: sourt "tiny" (no qualityLabel) as audio and rest as video
//         this.sources = result.availableResolutionsAdaptive;
//         console.log("Video info data", result);
//         this.video = result;

//         //---   Content Stuff   ---//
//         // NOTE: extractor likes are broken, using RYD likes instead
//         // this.likes = result.metadata.likes.toLocaleString();
//         // this.interactions[0].value = result.metadata.likes.toLocaleString();
//         this.loaded = true;
//         this.recommends = result.renderedData.recommendations;
//         console.log("recommendations:", this.recommends);

//         //---   API WatchTime call   ---//
//         if (this.$store.state.watchTelemetry) {
//           this.playbackTracking = result.playbackTracking;
//           this.st = 0;
//           this.cpn = getCpn();
//           this.initWatchTime().then(() => {
//             this.sendWatchTime();
//             this.interval = setInterval(this.sendWatchTime, 60000);
//           });
//         }
//       });

//       this.$youtube.getReturnYoutubeDislike(this.$route.query.v, (data) => {
//         this.likes = data.likes.toLocaleString();
//         this.dislikes = data.dislikes.toLocaleString();
//         this.interactions[0].value = data.likes.toLocaleString();
//         this.interactions[1].value = data.dislikes.toLocaleString();
//       });
//     },
//     callMethodByName(name) {
//       // Helper function needed because of issues when directly calling method
//       // using item.action in the v-for loop
//       this[name]();
//     },
//     async share() {
//       // this.share = !this.share;
//       await Share.share({
//         title: this.video.title,
//         text: this.video.title,
//         url: "https://youtu.be/" + this.$route.query.v,
//         dialogTitle: "Share video",
//       });
//     },
//     sendWatchTime() {
//       const player = this.$refs.player.getPlayer();
//       const rt = Math.floor(Date.now() / 1000) - this.startTime;
//       const params = {
//         cpn: this.cpn,
//         rt: rt,
//         rti: rt,
//         rtn: rt,
//         cmt: player.currentTime,
//         et: player.currentTime,
//         st: this.st,
//         state: player.paused ? "paused" : "playing",
//         volume: 100,
//         muted: 0,
//         fmt: 396,
//       };
//       this.st = player.currentTime;
//       this.$youtube.saveApiStats(
//         params,
//         this.playbackTracking.videostatsWatchtimeUrl.baseUrl
//       );
//     },

//     async initWatchTime() {
//       await this.$youtube.saveApiStats(
//         {
//           cpn: this.cpn,
//           fmt: 243,
//           rtn: Math.floor(Date.now() / 1000) - this.startTime,
//           rt: Math.floor(Date.now() / 1000) - this.startTime,
//           muted: 0,
//         },
//         this.playbackTracking.videostatsPlaybackUrl.baseUrl
//       );
//     },

//     initializeState() {
//       return {
//         interactions: [
//           {
//             name: "Likes",
//             icon: "mdi-thumb-up-outline",
//             // action: this.like(),
//             actionName: "like",
//             value: this.likes,
//             disabled: true,
//           },
//           {
//             name: "Dislikes",
//             icon: "mdi-thumb-down-outline",
//             // action: this.dislike(),
//             actionName: "dislike",
//             value: this.dislikes,
//             disabled: true,
//           },
//           {
//             name: "Share",
//             icon: "mdi-share-outline",
//             // action: this.share(),
//             actionName: "share",
//             disabled: false,
//           },
//           {
//             name: "Save",
//             icon: "mdi-plus-box-multiple-outline",
//             actionName: "enqueue",
//             disabled: true,
//           },
//           // {
//           //   name: "Quality",
//           //   icon: "mdi-high-definition",
//           //   actionName: "quality",
//           //   disabled: false,
//           // },
//           // {
//           //   name: "Speed",
//           //   icon: "mdi-speedometer",
//           //   actionName: "speed",
//           //   disabled: false,
//           // },
//         ],
//         showMore: false,
//         showComments: false,
//         // share: false,
//         sources: [],
//         recommends: null,
//         loaded: false,
//         interval: null,
//         video: null,
//         backHierarchy: [],
//       };
//     },

//     mountedInit() {
//       this.startTime = Math.floor(Date.now() / 1000);
//       this.getVideo();

//       //  Reset vertical scrolling
//       const scrollableList = document.querySelectorAll(".overflow-y-auto");
//       scrollableList.forEach((scrollable) => {
//         scrollable.scrollTo(0, 0);
//       });
//     },

//     // Toggle this.showComments to true or false. If it is true, then add the dismiss function to backStack.
//     toggleComment() {
//       document.getElementById("content-container").scrollTo(0, 0);
//       this.showComments = !this.showComments;
//       if (this.showComments) {
//         const dismissComment = new backType(
//           () => {
//             this.showComments = false;
//           },
//           () => {
//             return this.showComments;
//           }
//         );
//         this.$vuetube.addBackAction(dismissComment);
//       }
//     },
//   },
// };
</script>

<style>
.comments {
  left: 0;
  right: 0;
  bottom: 0;
  opacity: 0;
  z-index: 2;
  height: 100%;
  max-height: 100%;
  position: absolute;
  pointer-events: none;
  transform: translateY(100%);
  transition: transform 0.3s ease-in-out, opacity 0.3s ease-in-out;
}
.comments-open {
  transform: translatey(0);
  pointer-events: auto;
  opacity: 1;
}
#watch-body {
  height: 100%;
  max-height: 100vh;
  overflow: hidden;
  display: flex;
  flex-direction: column;
}

#content-container {
  height: 100%;
  position: relative;
}

.vertical-button span.v-btn__content {
  flex-direction: column;
  justify-content: space-around;
}

.channel-section,
.comment-renderer {
  display: flex;
  align-items: center;
}

.channel-section #details,
.comment-renderer .comment-count {
  flex-grow: 1;
  display: flex;
  align-items: center;
  min-width: 0;
}

.channel-section .channel-byline {
  min-width: 0;
}

.channel-section .avatar-thumbnail {
  border-radius: 50%;
  width: 35px;
  height: 35px;
}

.channel-section .channel-name {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.keep-spaces {
  white-space: pre-wrap;
}

.v-card__title {
  word-break: break-word;
}
</style>
