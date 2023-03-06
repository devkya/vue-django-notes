# Eslint & Prettier 세팅하기

[참고링크1](https://helloinyong.tistory.com/325)  
[참고링크2](https://vueschool.io/articles/vuejs-tutorials/eslint-and-prettier-with-vite-and-vue-js-3/)  
[한번에 보기](https://velog.io/@cosmickj/Vite-Vue3-TypeScript-%EC%84%B8%ED%8C%85%ED%95%98%EA%B8%B0)

## Eslint vs Prettier

> eslint 는 코드 퀄리티를 보장하고, prettier 는 코드 스타일을 통일하게 도와줌

- Eslint는 여러가지 코드 작성법을 일관성 있는 방식으로 구현할 수 있도록 잡아주는 역할을 수행함
- prettier는 줄 바꿈, 공백, 들여쓰기 등 에디터에서 텍스트를 일관되게 작성하는 역할을 수행함

## ESLint 세팅

1. `npm install -D eslint`
2. `vscode eslint extension` 반드시 설치해야 함
3. `.eslintrc`
   - `root : true` => `eslintrc` 파일을 찾을 때 해당 프로젝트 디렉토리에서 찾음
   - parser => 각 코드 파일을 검사할 때 파서를 설정하는 부분
   - extends => eslint rule이 저장되어 있는 외부 파일을 extends 하는 부분 => 커스터마이징 하고 싶은 부분은 rules에서 할 수 있음
   - rules => 직접 lint rule을 적용하는 부분

## Prettier 세팅

1. (방법1) 별도의 prettier 관련 플러그인을 npm으로 설치하지 않고 vscode extension을 설치 => 내 환경의 vscode 에서만 prettier 방식이 적용
2. (방법2) prettier 플러그인을 직접 설치 후 eslintrc에 세팅 => 프로젝트 자체에 적용하는 방법으로 해당 프로젝트를 다른 환경에서 돌려도 동일하게 적용됨
   > yarn, npm으로 prettier를 설치했다면 `.prettierrc` 파일을 꼭 이용해야 함. \* `.prettierrc` 파일이 존재하면 vscode prettier 세팅은 무시되고 해당 파일 룰로 적용됨
   1. `npm install -D prettier`
   2. prettier 공식 Docs에서 eslint를 사용할 시, `eslint-config-prettier`를 설치해서 세팅하라고 알려주고 있음
      > `eslint-config-prettier` => eslint와 중복되는 규칙을 prettier 쪽에서 알아서 꺼주는 역할 수행함
   3. `eslintrc`에서 `"extends": ["plugin:prettier/recommended"]` 설정하면 `eslint-config-prettier` 활성화 & prettier 규칙에 맞지 않는 요소들을 error로 판단하도록 하는 설정 등 알아서 다 됨
   4. 자동 저장 기능 활성화 `VSCode Settings.json` - `  "editor.codeActionsOnSave": { "source.fixAll.eslint": true },` => save 할 때 eslint 관련 요소들을 전체적으로 수정함 - `{
"editor.formatOnSave": true,
"editor.defaultFormatter": "esbenp.prettier-vscode"
} ` => save할 때 VScode Extension 설정으로 저장

## Vite 프로젝트에서 ESLint & Prettier 세팅

1. vite project 생성
2. `npm install -D prettier` & `npm install -D eslint`
3. `npm install -D @vue/eslint-config-typescript`
4. `.prettierrc.json` Empty 파일 생성
5. `npm install -D eslint eslint-plugin-vue`
6. `.eslintrc.js`

   ```javascript
   module.exports = {
     env: {
       node: true,
     },
     extends: [
       "eslint:recommended",
       "plugin:vue/vue3-recommended",
       "prettier",

       // berry Template settings
       "eslint:recommended",
       "plugin:vue/vue3-essential",
       "prettier",
     ],
     rules: {
       // override/add rules settings here, such as:
       // 'vue/no-unused-vars': 'error'
     },
   };
   ```

7. `npm install eslint-config-prettier --save-dev` => prettier와 ESLint 충돌 방지
8. VSCode extensions Volor(vue3) 설치
9. default 값 설정 해제

```javascript
// .eslintrc.js
rules: {
  //...
  "vue/require-default-prop": "off",
},
```

9. 자동 저장 설정

```json
// Code/User/settings.json
{
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  }
}
```

10. `npm install -D vite-plugin-eslint` => 브라우저에서 ESLint Error 확인 가능
11. plugin은 `vite.config.js`에 등록해야 함
