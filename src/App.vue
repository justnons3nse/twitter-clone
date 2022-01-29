<template>
  <div id="app" class="flex container h-screen w-full">
    <!-- Side nav -->
    <div class="flex flex-col justify-between lg:w-1/4 border-r border-lighter px-2 lg:px-6 py-2">
      <div class="">
        <button class="h-12 w-12 hover:bg-lightblue text-3xl text-blue rounded-full">
          <i class="fab fa-twitter"></i>
        </button>
        <div>
          <button v-for="tab in tabs" :key="tab" @click="id = tab.id" :class="`flex items-center mr-auto py-2 px-4 rounded-full mb-3 hover:bg-lighter ${ id === tab.id ? 'font-bold' : '' }`">
            <i :class="`${ tab.icon } text-2xl mr-4 text-left`"></i>
            <p class="text-lg text-left hidden lg:block">{{ tab.title }}</p>
          </button>
        </div>
        <button button class="bg-blue text-white font-semibold text-lg lg:w-56 p-3 w-12 h-12 lg:h-auto rounded-full focus:outline-none hover:bg-darkblue">
          <p class="hidden lg:block">Tweet</p>
          <i class="fas fa-plus lg:hidden"></i>
        </button>
      </div>
      <!-- dropdown -->
      <div  class="lg:w-full relative">
        <button @click="dropdown = true" class="flex items-center p-3 w-full hover:bg-gray-200 rounded-full">
          <img src="profile.jpg" class="w-10 h-10 rounded-full" alt="profile picture">
          <div class="hidden lg:block ml-4">
            <p class="text-sm font-bold leading-tight">Dave Green</p>
            <p class="text-sm leading-tight">@DaveGre3n</p>
          </div>
          <i class="hidden lg:block fas fa-ellipsis-h ml-auto text-lg "></i>
        </button>
        <div v-if="dropdown === true"  class="absolute bottom-0 left-0 w-64 rounded-lg shadow-md border-lightest bg-white mb-20">
          <button @click="dropdown = false" class="flex w-full items-center p-3">
            <img src="profile.jpg" class="w-10 h-10 rounded-full" alt="profile picture">
            <div class="hidden lg:block ml-4">
              <p class="text-sm font-bold leading-tight">Dave Green</p>
              <p class="text-sm leading-tight">@DaveGre3n</p>
            </div>
            <i class="fas fa-check ml-auto text-blue "></i>
          </button>
          <button @click="dropdown = false"  class="w-full text-left  border-t border-lighter text-sm">
            <p class="p-3 hover:bg-gray-200">Add an existing account</p>
          </button>
          <button @click="dropdown = false" class="w-full text-left text-sm">
            <p class="p-3 hover:bg-gray-200">Log out @DaveGre3n</p>
          </button>
        </div>
      </div>
    </div>

    <!-- tweet section -->
    <div class="lg:w-1/2 h-full overflow-y-scroll">
      <div class="px-5 py-3 flex items-center justify-between">
        <h1 class="text-xl font-bold">Home</h1>
        <i class="far fa-star text-lg"></i>
      </div>

      <div class="px-5 py-3 flex border-b border-lighter">
        <div class="flex-none">
          <img src="profile.jpg" alt="Profile Picture"  class="w-12 h-12 rounded-full">
        </div>
        <form v-on:submit.prevent = "addNewTweet" action="" class="w-full px-4 relative">
          <textarea v-model="tweet.content"  placeholder="What's happening?" class="w-full focus:outline-none font-lg mt-3 pb-3"></textarea>
          <button class="text-blue hover:bg-lightblue rounded-full mb-2 font-semibold text-sm"><i class="fas fa-globe-americas"></i> Everyone can reply</button>
          <div class="flex items-center borer-t border-lighter"> 
            <i class="text-lg text-blue mr-4 far fa-image"></i>
            <i class="text-lg text-blue mr-4 fas fa-film"></i>
            <i class="text-lg text-blue mr-4 far fa-chart-bar"></i>
            <i class="text-lg text-blue mr-4 far fa-smile"></i>
            <i class="text-lg text-blue mr-4 fas fa-calendar "></i>
            <i class="text-lg text-blue mr-4 fas fa-map-marker-alt"></i>
          </div>
          <button type="submit" class="h-10 px-4 text-white font-semibold bg-blue hover:bg-dark focus:outline-none rounded-full absolute bottom-0 right-0">
            Tweet
          </button>
        </form>
      </div>

       <div class="flex flex-col-reverse cursor-pointer">
        <div v-for="tweet in tweets" :key="tweet" class="w-full p-4 border-b border-lighter hover:bg-lighter flex">
          <div class="flex-none mr-4">
            <img src="profile.jpg" class="h-12 w-12 rounded-full flex-none"/>
          </div>
          <div class="w-full">
            <div class="flex items-center w-full">
              <p class="font-semibold"> Dave Green </p>
              <p class="text-sm text-dark ml-2"> @DaveGre3n </p>
              <p class="text-sm text-dark ml-2"> 1 hr </p>
              <i class="fas fa-ellipsis-h text-dark ml-auto"></i>
            </div>
            <p class="py-2">
              {{ tweet.content }}
            </p>
            <div class="flex items-center justify-between w-full">
              <div class="flex items-center text-sm text-dark">
                <button class="hover:text-blue"><i class="far fa-comment mr-3"></i>0</button>
             
              </div>
              <div class="flex items-center text-sm text-dark">
                <button class="hover:text-green-400"><i class="fas fa-retweet mr-3"></i>0</button>
                
              </div>
              <div class="flex items-center text-sm text-dark">
                <button class="hover:text-red-400"><i class="fas fa-heart mr-3"></i>1</button>
              </div>
              <div class="flex items-center text-sm text-dark">
                <button class="hover:text-blue"><i class="fas fa-share-square mr-3"></i></button>
              </div>
            </div>
          </div>
        </div>
      </div>
      

      <!-- Actual tweets -->
      <div v-for="follow in following" :key="follow" class="w-full p-4 border-b border-lighter hover:bg-lighter flex cursor-pointer">
        <div class="flex-none mr-4">
          <img :src="`${follow.src}`" alt="Profile Picture"  class="w-12 h-12 rounded-full flex-none">
        </div>
        <div class="w-full">
          <div class="flex items-center w-full">
            <p class="font-semibold"> {{follow.name}}</p>
            <p class="text-sm text-dark ml-2"> {{follow.handle}}</p>
            <p class="text-sm text-dark ml-2"> {{follow.time}}</p>
            <i class="fas fa-ellipsis-h text-dark ml-auto"></i>
          </div>
          <p class="py-2">{{follow.tweet}}</p>
          <div class="flex items-center justify-between w-full">
            <div class="flex items-center text-sm text-dark">
              <button class="hover:text-blue"><i class="far fa-comment mr-3"></i>{{follow.comments}}</button>
            </div>
            <div class="flex items-center text-sm text-dark">
              <button class="hover:text-green-400"><i class="fas fa-retweet mr-3"></i>{{follow.retweets}}</button>
            </div>
            <div class="flex items-center text-sm text-dark">
              <button class="hover:text-red-400"><i class="far fa-heart mr-3"></i>{{follow.like}}</button>
            </div>
            <div class="flex items-center text-sm text-dark">
              <button class="hover:text-blue"><i class="fas fa-share-square mr-3"></i></button>
            </div>
          </div>
        </div>
      </div>

    </div>

    <!-- trending section -->
    <div class="lg:block hidden w-1/3 h-full border-l border-lighter py-2 px-6 overflow-y-scroll relative">
      <input class="pl-14 rounded-full w-80 p-3 bg-lighter text-sm focus:outline-blue" placeholder="Search Twitter" type="text">
      <i class="fas fa-search absolute left-0 top-0 mt-5 ml-12 text-sm text-light"></i>

      <div class="w-80 mt-4 rounded-xl bg-lightest">
        <div class="flex items-center justify-between p-3">
          <p class="font-bold text-lg">Worldwide trends</p>
          <i class="fas fa-cog text-lg"></i>
        </div>
        <button v-for="trend in trending" :key="trend" class="w-80 flex justify-between hover:bg-lighter p-3">
            <div>
              <p class="text-sm text-gray text-left leading-tight">{{ trend.top }}</p>
              <p class="font-bold text-left leading-tight">{{ trend.title }}</p>
              <p class="text-sm text-gray text-left leading-tight">{{ trend.bottom }}</p>
            </div>
             <i class="fas fa-ellipsis-h text-gray text-sm"></i>
        </button>
        <button class="w-80 hover:bg-lighter rounded-lg  p-3">
          <p class="text-blue text-left">Show More</p>
        </button>
      </div>

      <div class="w-80 mt-4 rounded-xl bg-lightest my-4">
        <div class="p-3">
          <p class="font-bold text-lg">Who to follow</p>
        </div>
        <button v-for="friend in friends" :key="friend" class="w-full flex hover:bg-lighter p-3">
            <img :src="`${friend.src}`" class="w-12 h-12 rounded-full" alt="profile picture">
            <div class="hidden lg:block ml-4">
              <p class="text-sm font-bold leading-tight">{{friend.name}}</p>
              <p class="text-sm leading-tight">{{friend.handle}}</p>
            </div>
        <button class=" ml-auto text-sm font-semibold text-white bg-black rounded-full py-2 px-5 rounded-ful border-1 border-lighter">Follow</button>
        </button>
        <button class="w-80 hover:bg-lighter rounded-lg  p-3">
          <p class="text-blue text-left">Show More</p>
        </button>
      </div>
      
    </div>
  </div>
</template>

<script>

export default {
  name: 'app',
  components: {
  },
  data() {
    return {
      tabs: [
        {icon: 'fas fa-home', title: 'Home', id:'home'},
        {icon: 'fas fa-hashtag', title: 'Explore', id: 'explore'},
        {icon: 'far fa-bell', title: 'Notifications', id: 'notifications'},
        {icon: 'far fa-envelope', title: 'Messages', id: 'messages'},
        {icon: 'far fa-bookmark', title: 'Bookmarks', id: 'bookmarks'},
        {icon: 'fas fa-clipboard-list', title: 'Lists', id: 'lists'},
        {icon: 'far fa-user', title: 'Profile', id: 'profile'},
        {icon: 'fas fa-ellipsis-h', title: 'More', id: 'more'}
      ],
      id:'home',
      dropdown: false,

      trending: [
        {top: '1. Trending in Slovakia', title: '#DaveNaHrad', bottom: '545 tweets'},
        {top: '2. Culture', title: '#SpiderManNoWayHome', bottom: '135K Tweets'},
        {top: '3. Gaming', title: '#ELDENRING', bottom: '40k tweets'},
        {top: '4. Trending in US', title: '#biden', bottom: '40K tweets'},
        {top: '5. Trending', title: '#문빈의_스물다섯번째_발자국', bottom: '142K tweets'},
      ],

      following: [
        {src: 'elon.jpg', name: 'Elon Musk', handle: '@teslaBoy', time: '20 min', tweet: 'Should I make another crypto???', comments: '1,000', retweets: '550', like: '1,000,003'},
        {src: 'eldenring.png', name: 'ELDEN RING', handle: '@ELDENRING', time: '55 min', tweet: '1 month to go!', comments: '2,030', retweets: '50', like: '20,003'},
        {src: 'ufc.jpg', name: 'UFC', handle: '@finishHim', time: '40 min', tweet: 'Get ready, UFC 540: Biden vs Bush', comments: '9,000', retweets: '55470', like: '8,000,003'},
        {src: 'disney.jpg', name: 'Dinsey+', handle: '@disneyplus', time: '1.4 hr', tweet: 'Maybe this time we will actually make great Star Wars trilogy. Who knows? lmao', comments: '100,000', retweets: '1,000,002', like: '5,000,003'},
        {src: 'jeff.jpg', name: 'Jeff Bezos', handle: '@moneeeey', time: '1.4 hr', tweet: 'HUGE ANNOUNCEMENT: I will be staring in our new LOTR series as Gollum ', comments: '100,500', retweets: '1,000,032', like: '5,000,103'}
      ],

        friends: [
        {src: 'milan.png', name: 'AC Milan', handle: '@acmilan'},
        {src: 'gamepass.png', name: 'Xbox Game Pass', handle: '@XboxGamePass'},
        {src: 'evan.jpg', name: 'Evan You', handle: '@vueGuy'}
      ],

      tweets: [
        {content: 'Spider-man : No Way home is such a great movie ♥'}
      ],
      tweet: {content: ''}
      
    }
  },
  methods: {
    addNewTweet(){
      let newTweet = {
        content: this.tweet.content
      };
      this.tweets.push(newTweet);
    }
  }
}
</script>
