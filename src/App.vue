<template>
  <div class="music-app" v-if="!isList">
    <div class="header">
      <div class="header--content">
        <div class="section section-1">NOW PLAYING</div>
        <div class="section section-2">
          <div class="favorite"><span>&#10084;</span></div>
          <div class="song-circle">
            <div
              class="song-image"
              :style="{
                backgroundImage: currentSong.coverImage,
                backgroundSize: 'cover',
              }"
            >
              <div class="song-disk"></div>
            </div>
          </div>
          <div class="is-saved" @click="toggleList">&#8801;</div>
        </div>
        <div class="section section-3">{{ currentSong.title }}</div>
        <div class="section section-4">{{ currentSong.artist }}</div>
      </div>
    </div>

    <div class="body">
      <div class="player">
        <div class="adjust" @click="prev">Prev</div>
        <div class="adjust play" v-if="!isPlaying" @click="play">Play</div>
        <div class="adjust play" v-else @click="pause">Pause</div>
        <div class="adjust" @click="next">Next</div>
      </div>
      <div
        class="list"
        :style="{
          backgroundImage: currentSong.backgroundImage,
          backgroundSize: 'cover',
        }"
      ></div>
    </div>
  </div>
  <div class="is-list" v-else>
    <div class="list-image">
      <div class="is-saved bars" @click="toggleList">&#8801;</div>
      <div class="song-circle">
        <div
          class="song-image"
          :style="{
            backgroundImage: currentSong.coverImage,
            backgroundSize: 'cover',
          }"
        >
          <div class="song-disk"></div>
        </div>
      </div>
    </div>
    <div class="list-song">
      <div
        class="list-songs"
        v-for="song in songs"
        :key="song.src"
        @click="play(song)"
        :class="song.src === currentSong.src ? 'list-songs-play' : 'list-songs'"
      >
        <div class="circle-play"><span>&#9658;</span></div>
        <div class="song-info">
          <div class="song-title">{{ song.title }}</div>
          <div class="song-artist">{{ song.artist }}</div>
        </div>
        <div class="circle-heart"><span>&#10084;</span></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      currentSong: {},
      index: 0,
      isPlaying: false,
      isList: false,
      songs: [
        {
          title: "ETA",
          artist: "NewJeans",
          src: require("./assets/eta.mp3"),
          coverImage: `url(${require("@/assets/supershy.jpg")})`,
          backgroundImage: `url(${require("@/assets/eta.jpg")})`,
        },

        {
          title: "OMG!!",
          artist: "NewJeans",
          src: require("./assets/omg.mp3"),
          coverImage: `url(${require("@/assets/supershy.jpg")})`,
          backgroundImage: `url(${require("@/assets/omg.jpg")})`,
        },

        {
          title: "Super Shy",
          artist: "NewJeans",
          src: require("./assets/supershy.mp3"),
          coverImage: `url(${require("@/assets/supershy.jpg")})`,
          backgroundImage: `url(${require("@/assets/1.jpg")})`,
        },
        {
          title: "ASAP",
          artist: "NewJeans",
          src: require("./assets/asap.mp3"),
          coverImage: `url(${require("@/assets/supershy.jpg")})`,
          backgroundImage: `url(${require("@/assets/asap.jpeg")})`,
        },
        {
          title: "New Jeans",
          artist: "NewJeans",
          src: require("./assets/newjeans.mp3"),
          coverImage: `url(${require("@/assets/supershy.jpg")})`,
          backgroundImage: `url(${require("@/assets/newjeans.jpg")})`,
        },
        {
          title: "Cool With You",
          artist: "NewJeans",
          src: require("./assets/cool.mp3"),
          coverImage: `url(${require("@/assets/supershy.jpg")})`,
          backgroundImage: `url(${require("@/assets/cool.jpg")})`,
        },
      ],
      player: new Audio(),
    };
  },

  methods: {
    play(song) {
      if (typeof song.src != "undefined") {
        this.currentSong = song;
        this.player.src = this.currentSong.src;
      }

      this.player.play();
      this.isPlaying = true;
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
    next() {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.currentSong = this.songs[this.index];
      this.play(this.currentSong);
    },
    prev() {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }
      this.currentSong = this.songs[this.index];
      this.play(this.currentSong);
    },
    toggleList() {
      this.isList = !this.isList;
    },
  },
  created() {
    this.currentSong = this.songs[this.index];
    this.player.src = this.currentSong.src;
  },
};
</script>

<style>
.is-list {
  max-width: 420px;
  height: 825px;
  max-height: 850px;
  background-color: #deeaf8;
  margin: 0 auto;
  position: relative;
  z-index: 2;
  transition: 0.4s ease-in-out;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
  letter-spacing: 3px;
  font-family: "Bricolage Grotesque", sans-serif;
  font-family: "Cabin", sans-serif;
  font-family: "Montserrat", sans-serif;
  font-family: "Odibee Sans", cursive;
}
.list-image {
  width: 85%;
  height: 30%;

  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
}
.bars {
  position: relative;
  top: 20px;
  left: 140px;
}
.list-song {
  width: 90%;
  height: 60%;

  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
}
.list-songs {
  width: 95%;
  height: 14%;
  background-color: rgba(242, 248, 253, 255);
  display: flex;
  flex-direction: row;
  align-items: center;
  border-radius: 15px;
  justify-content: space-around;
  box-shadow: 0px 1px 8px 0.2px#d4d5d7;
  cursor: pointer;
}
.list-songs-play {
  width: 95%;
  height: 14%;
  background-color: rgba(180, 174, 254, 255);
  display: flex;
  flex-direction: row;
  align-items: center;
  border-radius: 15px;
  justify-content: space-around;
  box-shadow: 0px 1px 8px 0.2px#d4d5d7;
}
.circle-play,
.circle-heart {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background-color: rgba(229, 236, 251, 255);
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
}
.circle-play span,
.circle-heart span {
  position: relative;
  left: 1.9px;
  color: #b4abfb;
}
.song-info {
  width: 60%;
  height: 79%;

  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: space-around;
}
.song-title {
  font-size: 30px;
}
.music-app {
  max-width: 420px;
  height: 825px;
  max-height: 850px;

  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  letter-spacing: 3px;
  font-family: "Bricolage Grotesque", sans-serif;
  font-family: "Cabin", sans-serif;
  font-family: "Montserrat", sans-serif;
  font-family: "Odibee Sans", cursive;
  transition: 0.4s ease-in-out;
}
.header {
  width: 95%;
  height: 60%;
  background-color: #7f5eff;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding-bottom: 20px;
  margin-bottom: -20px;
  position: relative;
  z-index: 1;
  border-bottom-right-radius: 30px;
  border-bottom-left-radius: 30px;
}

.header--content {
  width: 90%;
  height: 75%;
  background-color: transparent;

  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  color: whitesmoke;
}

.section {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 90%;
  background-color: transparent;
}
.section-1 {
  height: 12%;
  font-size: 30px;
  font-weight: bold;
  color: whitesmoke;
}
.section-2 {
  height: 60%;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
}
.favorite,
.is-saved {
  width: 40px;
  height: 40px;
  background-color: aliceblue;
  border-radius: 50%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-size: 19px;
  color: rgb(102, 100, 100);
  box-shadow: 0px 1px 8px 1px#9c9da1;
}
.is-saved {
  font-size: 30px;
  font-weight: bold;
}
.favorite span,
.is-saved span {
  position: relative;
  left: 1.2px;
}

.song-circle {
  width: 60%;
  height: 70%;
  border-radius: 50%;
  background-color: white;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  box-shadow: 0px 1px 8px 1px#b0b1b4;
}
.song-image {
  width: 90%;
  height: 90%;
  border-radius: 50%;
  background-color: chartreuse;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.song-disk {
  width: 25%;
  height: 25%;
  border-radius: 50%;
  background-color: rgb(243, 243, 244);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.section-3 {
  height: 15%;
  font-size: 60px;
  font-weight: bolder;
}
.section-4 {
  height: 9%;
  font-size: 25px;
  font-weight: bolder;
}

.body {
  width: 95%;
  height: 40%;
  background-color: #deeaf8;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
}
.player {
  width: 75%;
  height: 30%;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  position: relative;
  top: -30px;
  z-index: 1;
}
.adjust {
  width: 50px;
  height: 50px;
  font-size: 14px;
  background-color: azure;
  border-radius: 50%;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  box-shadow: 0px 1px 8px 1px#9c9da1;
}
.play {
  width: 80px;
  height: 80px;
}
.list {
  width: 90%;
  height: 70%;
  position: relative;
  top: -20px;
  border-radius: 15px;
  box-shadow: 0px 1px 8px 3px#9c9da1;
}
</style>
