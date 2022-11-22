# 형제 컴포넌트에서 클릭 이벤트
## 동작 원리 
1. `Child(1) Component` 에서 클릭 이벤트 발생
2. `$emit`을 이용하여 `Parent Component`로 이벤트 전달
3. `Child(2) Component` 에서 클릭 이벤트 발생 시 호출할 함수 생성
4. `Parent Component`에서 `this.$refs.ChildComponentRef.method(params)` 실행
***

>> ref를 사용하지 않는 것이 좋다고 하지만 부모 컴포넌트에서 자식 컴포넌트로 event 전달할 때는 사용해도 괜찮은것으로 판단함.

>> 사실 하고 싶었던 것은 Vue 원칙에 어긋나지 않게 클릭 이벤트 발생시 부모 컴포넌트에서 props를 사용해 데이터를 전달하고 자식 컴포넌트는 `Computed or Watch`를 사용하여 데이터 변화를 관찰해 함수를 호출하고 싶었으나, 같은 메뉴를 클릭시 이벤트 호출이 되지 않고, ref를 사용하는 것보다 더 복작함.

## 내가 사용한 곳 
>> `App Bar`에서 메뉴 클릭시 `router-view`로 되어있는 형제 컴포넌트에 이벤트를 전달하고 스크롤되기 위해 사용함

## 구성
### App Bar Component(Child Component)
```html
		<v-app-bar color="#494949" clipped-right app>
			<v-icon class="px-5" dark large>mdi-basketball</v-icon>
			<v-toolbar-title class="font-weight-bold text-h5" style="color: #fbbd28">
				VUE - DJANGO WEB
			</v-toolbar-title>
			<v-spacer></v-spacer>
			<!-- display breakpoint -->
			<!-- display lg down -->

			<v-app-bar-nav-icon
				class="hidden-lg-and-up"
				@click.stop="drawer = !drawer"
				dark
			>
			</v-app-bar-nav-icon>

			<!-- display lg up -->
			<v-slide-group class="hidden-md-and-down">
				<v-slide-item
					v-for="(item, index) in menu"
					:key="index"
					v-slot="{ active, toggle }"
				>
					<v-btn
						id="menu-btn"
						class="elevation-0 px-5"
						:input-value="active"
						v-model="menu"
						:style="active ? 'color: #fbbd28' : undefined"
						@click="
							toggle();
							$emit('menuChg', item.to);
						"
						plain
						dark
					>
						{{ item.title }}
					</v-btn>
				</v-slide-item>
			</v-slide-group>
		</v-app-bar>
```
### Parent Component
```html
<template>
	<v-app>
		<MenuBar @menuChg="scrollTo"></MenuBar>
		<v-main>
			<router-view ref="ContentComponent" :menu="menu" />
		</v-main>
	</v-app>
</template>
```
```javascript
<script>
import MenuBar from '@/components/common/MenuBar.vue';
export default {
	name: 'Home',
	components: { MenuBar },
	data: () => ({
		menu: '',
		//
	}),
	methods: {
		scrollTo(menu) {
			this.$refs.ContentComponent.scrollTo(menu);
		},
	},
};
</script>
```
### Content Component(Child Component) - router-view
```html
<template>
	<div>
		<Home-Img ref="home"></Home-Img>
		<Home-Post-List ref="posts"></Home-Post-List>
		<Home-Iam ref="iam"></Home-Iam>
	</div>
</template>
```
```javascript
<script>
import HomeImg from '@/components/home/HomeImg.vue';
import HomePostList from '@/components/home/HomePostList.vue';
import HomeIam from '@/components/home/HomeIam.vue';

export default {
	components: { HomeImg, HomePostList, HomeIam },
	data() {
		return {};
	},
	props: {},
	setup() {},
	created() {},
	mounted() {},
	unmounted() {},
	watched: {},
	computed: {},
	methods: {
		scrollTo(menu) {
			this.$vuetify.goTo(this.$refs[menu]);
		},
	},
};
</script>
``` 