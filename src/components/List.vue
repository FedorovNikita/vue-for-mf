<template>
  <Loader v-if="loading"/>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <ul class="cases-list">
      <li class="cases-item" v-for="caseItem in cases" :key="caseItem.id">
        <router-link class="cases-link" :to="`/recognition/${caseItem.id}`">
            <img class="cases-image" :src="caseItem.previewImagesUrls[0]" alt="preview">
            <div class="cases-wrap-info">
              <h3 class="cases-name">{{ caseItem.name }}</h3>
            </div>
            <span class="cases-text">Посмотреть как работает модель</span>
        </router-link>
      </li>
    </ul>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue'
import Loader from './Loader'

export default {
  name: 'MainList',
  props: {
    msg: String
  },
  components: { 
    Loader
  },
  setup () {
    const cases = ref(null)
    const loading = ref(true)

    onMounted(async () => {
      const casesResponse = await fetch(`https://dev.api.cvdp.itnap.ru/api/v1/cases`, {
        method: 'GET',
        headers: {
          accept: 'application/json'
        }
      })
      cases.value = await casesResponse.json()
      loading.value = false
    })

    return {
      cases,
      loading
    }
  }
}
</script>

<style lang="scss">
.cases-list {
	grid-gap: 80px 80px;
	display: grid;
	grid-template-columns: repeat(3, 1fr);
	margin-top: 90px;
	padding: 0;
	list-style: none;
  text-align: left;
}

.cases-item {
	min-width: 300px;
	min-height: 400px;
	background-color: #fff;
	overflow: hidden;
}

.cases-link {
	display: flex;
	flex-direction: column;
	position: relative;
	height: 100%;
	background-color: #fff;
	text-decoration: none;
	transition: all 0.3s ease-in;
	&::after {
		content: '';
		position: absolute;
		right: 20px;
		bottom: 20px;
		display: block;
		width: 31px;
		height: 22px;
		// background-image: url("../assets/arrow-next-black.png");
		transition: all ease-in 0.3s;
	}
	&:hover {
		background: #2a5ee9;
		&::after {
			// background-image: url("../assets/arrow-next-white.png");
		}
		.cases-wrap-info {
			transform: translateY(-98%);
			height: 100%;
			background-color: #2a5ee9;
			color: #fff;
		}
		.cases-text {
			opacity: 1;
		}
	}
}

.cases-image {
	height: 300px;
	min-height: 300px;
	object-fit: cover;
}

.cases-wrap-info {
	top: 0;
	margin: 0;
	padding: 23px 12px 26px 23px;
	padding-right: 55px;
	height: 100%;
	max-height: 300px;
	background-color: #fff;
	color: #000;
	transition: all 0.3s ease-in;
}

.cases-name {
	width: 100%;
	margin: 0;
	display: -webkit-box;
	-webkit-line-clamp: 5;
	-webkit-box-orient: vertical;
	font-size: 24px;
	line-height: 27px;
	overflow: hidden;
	text-overflow: ellipsis;
}

.cases-text {
	position: absolute;
	bottom: 23px;
	right: 110px;
	left: 23px;
	font-size: 16px;
	line-height: 19px;
	color: #fff;
	opacity: 0;
	transition: all ease-in 0.3s;
}
</style>
