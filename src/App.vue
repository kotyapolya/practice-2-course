<template>
  <div id="app">
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+HK:wght@300&display=swap" rel="stylesheet">
    <Title
        v-bind:music="this.playlist[index]"
        v-bind:seek="this.seek"
        v-on:change_current_time="change_current_time"
    />
    <Controls
        v-on:change-volume="changevolume"
        v-on:play-pause="playcontrol"
        v-on:previous_track="previous_track"
        v-on:next_track="next_track"
        v-bind:isplay="isplay"
    />
    <MusicList
        v-bind:playlist="filtered"
        v-on:play-music="playmusic"
        v-bind:isplay="isplay"
        v-bind:pre_id="previndex"
        v-bind:id="index"
    />
    <Search
        v-on:apply_filter="apply_filter"
    />

  </div>


</template>

<script>

import Title from './components/Title.vue'
import Controls from './components/Controls.vue'
import MusicList from './components/MusicList.vue'
import Search from './components/Search.vue'

export default {
  name: 'App',
  data() {
    return {
      playlist: [
        {
          id: 0,
          title: 'Bite Me',
          author: 'ENHTPEN',
          duration: '2:38',
          audio: null,
          path: require('@/audio/Bite Me.mp3'),
          display: true,

        },
        {
          id: 1,
          title: 'Heart Of Steel',
          author: 'TVORCHI',
          audio: null,
          path: require('@/audio/Heart Of Steel.mp3'),
          display: true
        },
        {
          id: 2,
          title: 'LO$ER=LO♡ER',
          author: 'TOMORROW X TOGETHER',
          audio: null,
          path: require('@/audio/LO$ER=LO♡ER.mp3'),
          display: true
        },

        {
          id: 3,
          title: 'Lonely Day',
          author: 'System Of A Down ',
          audio: null,
          path: require('@/audio/Lonely Day.mp3'),
          display: true
        },

        {
          id: 4,
          title: 'Notion',
          author: 'The Rare Occasions',
          audio: null,
          path: require('@/audio/Notion.mp3'),
          display: true
        },

        {
          id: 5,
          title: 'On The Ground',
          author: 'Rose',
          audio: null,
          path: require('@/audio/On The Ground.mp3'),
          display: true
        },
        {
          id: 6,
          title: 'Why Am I Like This',
          author: 'The World Alivw',
          audio: null,
          path: require('@/audio/Why Am I Like This.mp3'),
          display: true
        },

      ],
      index: 0,
      previndex: 0,
      isplay: false,
      volume_level: 0.5,
      seek: 0,
      filter: '',
    }
  },

  watch: {
    isplay() {
      this.seek = this.current.audio.currentTime;
      let updateSeek
      if (this.isplay) {
        updateSeek = setInterval(() => {
          this.seek = this.current.audio.currentTime;
        }, 500)
      } else {
        clearInterval(updateSeek)
      }
    },

    seek() {
      if (this.seek == this.current.audio.duration) {
        this.next_track();
      }
    }


  },
  computed: {
    filtered() {
      if (this.filter === '') {
        return this.playlist
      } else {
        return this.playlist.filter(t => t.title.match(this.filter));
      }

    }
  },

  methods: {
    apply_filter(filter) {
      this.filter = filter;
    },

    playcontrol() {
      this.playmusic(this.index);
    },

    previous_track() {
      if (this.playlist[this.index - 1] !== undefined) {
        this.playmusic(this.index - 1);

      }
    },

    next_track() {
      if (this.playlist[this.index + 1] !== undefined) {
        this.playmusic(this.index + 1);
      }
    },

    playmusic(i) {
      this.previndex = this.index;
      this.index = i;
      this.current = this.playlist[this.index];
      if (this.isplay === false) {
        this.current.audio.play();
        this.current.audio.volume = this.volume_level;
        this.isplay = true;
        this.icon_src = require('@/assets/pause.png')
      } else {
        this.current = this.playlist[this.previndex]
        this.current.audio.pause();
        this.isplay = false;
        this.icon_src = require('@/assets/play.png')
        if (this.previndex !== this.index) {
          this.current = this.playlist[this.index]
          this.current.audio.play();
          this.current.audio.volume = this.volume_level;
          this.isplay = true;
          this.icon_src = require('@/assets/pause.png')

        }
      }
    },


    changevolume(level) {
      if (this.current != undefined) {
        this.current.audio.volume = level / 100;
        this.volume_level = level / 100;
      }
    },

    change_current_time(current_time) {
      if (this.current != undefined) {
        this.current.audio.currentTime = current_time;
      }
    }
  },


  components: {
    Title,
    Controls,
    MusicList,
    Search


  },
  created() {
    this.playlist.forEach((track) => {
      track.audio = new Audio(track.path);
    })

  }
}
</script>

<style>

body {
  background-color: #303030;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  flex-direction: column;
  font-family: 'Noto Sans HK', sans-serif;
  color: white;
}

.container {
  width: 80%;
  margin-left: 10%;

}

hr {
  color: lightgrey;
  padding: 0;
  margin: 0;
}

img {
  opacity: 0.85;
}

img:active {
  padding-bottom: 3px;
}

img:hover {
  opacity: 1;
}
</style>
