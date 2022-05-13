<template>
  <header>
    <h1>Canciones en Vivo</h1>
  </header>
  <main>
    <section class="player">
      <h2 class="song-title">{{current.title}} - <span>{{current.author}}</span></h2>
      <div class="controls">
        <button class="prev" @click="prev">Prev</button>
        <button class="play" v-if="!isPlaying" @click="play">Play</button>
        <button class="pause" v-else @click="pause">Pause</button>
        <button class="next" @click="next">Next</button>
      </div>
    </section>
    <section class="playlist">
      <h3>Playlist</h3>
      <button v-for="song in songs" :key="song.src" 
      @click="play(song)"
      :class="(song.src == current.src )?'song playing' :'song'"
      >
        {{song.title}} - {{song.author}}
      </button>
    </section>
  </main>
</template>

<script>


export default {
  name: 'App',
  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs:[
        {
          title: 'El corazón sobre todo',
          author: 'Estelares',
          src: require('./assets/Estelares-ElCorazonSobreTodo.mp3')
        },
        {
          title: 'El Profeta',
          author: 'La Vela Puerca',
          src: require('./assets/LaVelaPuerca-ElProfeta.mp3')
        },
        {
          title: 'Para no verme más',
          author: 'La Vela Puerca',
          src: require('./assets/LaVelaPuerca-ParaNoVermeMas.mp3')
        },
        {
          title: 'Va a Escampar',
          author: 'La Vela Puerca',
          src: require('./assets/LaVelaPuerca-VaAEscampar.mp3')
        }
      ],
      player: new Audio(),
    }
  },
  methods: {
    play(song){
      if(typeof song.src != 'undefined'){
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play();
      this.player.addEventListener('ended',function(){
        this.index++;
        if(this.index > this.songs.length -1){
        this.index = 0;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
      }.bind(this))
      this.isPlaying = true;
    },
    pause(){
      this.player.pause();
      this.isPlaying = false;
    },
    prev(){
      this.index--;
      if(this.index < 0){
        this.index = this.songs.length - 1;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    },
    next(){
      this.index++;
      if(this.index > this.songs.length -1){
        this.index = 0;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    }
  },
  created(){
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  },
}
</script>

<style>
*{
  margin:0;
  padding:0;
  box-sizing: border-box;  
}
body{
  font-family: sans-serif;
}
header{
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  background-color: #ff6600;
  color: #FFF;
}
main{
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px
}

.song-title{
  color: #53565A;
  font-size: 32px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center; 
}

.song-title span {
  font-weight: 400;
  font-style: italic;
}

.controls{
  display: flex;
  justify-content: center;
  padding: 30px 15px;
  align-items: center;
}

button{
  appearance: none;
  background: none;
  outline: none;
  border: none;
  cursor: pointer;
}

button:hover{
  opacity: .7;
}

.play, .pause{
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0 15px;
  border-radius: 8px;
  color: #FFF;
  background-color: #ff761b;
}

.next, .prev{
  font-size: 16px;
  font-weight: 700;
  padding: 10px 20px;
  margin: 0 15px;
  border-radius: 6px;
  color: #FFF;
  background-color: #e66a18;
}

.playlist{
  padding: 0px 30px;
}

.playlist h3{
  color: #212121;
  font-size: 28px;
  font-weight: 400;
  margin-bottom: 30px;
  text-align: center; 
}

.playlist .song{
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
}

.playlist .song:hover{
  color: #e66a18;
}

.playlist .song.playing{
  color: #FFF;
  background-image: linear-gradient(to right, #ff6600, #fc9957)
}
</style>
