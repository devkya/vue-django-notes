# Django & Vue 연동을 위한 세팅
# Django
1. Create Project
    ```bash
    git init

    pipenv --python 3.x
    pipinv shell
    pipenv install django, djangorestframework ...

    django-admin startproject 'project name'
    ```

2. Create App
    ```bash
    cd 'project name'
    django-admin startapp 'app name'
    ```

3. Secret key, API Key, DB 정보 -> secrets.json 분리
    ```json
    // secrets.json
    {
        "SECRET_KEY":  "django secrey key",
        "DATABASES" : {
            "default": {
                "ENGINE": "mssql",
                "NAME": "RTDB",
                "USER": "sa",
                "PASSWORD": "devkya9465!",
                "HOST": "localhost",
                "PORT": "1433",
            }
        }
    }
    ```
    ```python
    # settings.py
    import json
    import os

    secret_file = os.path.join(BASE_DIR, 'secrets.json')

    with open(secret_file) as f:
        secrets = json.loads(f.read())

    def get_secret(setting, secrets=secrets):
        try:
            return secrets[setting]
        except KeyError:
            error_msg = 'Set the {} enviroment var'.format(setting) 
    ```
4. settings.py -> base.py & develop.py & product.py 분리
5. static & media files
    ```python
    # settings.py
    # static files
    STATICFILES_DIRS = (os.path.join(BASE_DIR, 'static'),) # 정적 파일 위치 경로
    STATIC_URL = '/static/' # 실제 웹에서 접근하는 URL
    STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')
    # prj에서 사용하는 모든 정적파일을 한 곳에 모아놓는 경로, collectstatic 명령을 하였을 때 모음
    # 개발 과정에서는 사용되지 않음(DEBUG = False)
    ```
    ```python
    # settings.py
    # media files
    MEDIA_URL = '/media/'
    MEDIA_ROOT  = os.path.join(BASE_DIR, 'media') # 실제 저장할 경로
    ```
    ```python
    # urls.py
    # 개발환경에서 media 파일을 서빙
    from django.conf import settings
    from django.conf.urls.static import static

    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
    ```
6. CORS(Cross origin Resource Sharing) 이슈 해결
    * django-cors-headers 설치
        ```bash
        pipenv install django-cors-headers
        ```
    * installed apps 목록에 추가
        ```python
        # settings.py
        INSTALLED_APPS = [
            ...
            'corsheaders'
            ...
        ]
        MIDDLEWARE = [
            'corsheaders.middleware.CorsMiddleware', # 가장 높게 위치시켜야 한다.
            ...
        ]
        ALLOWED_HOST = ['*']

        CORS_ORIGIN_ALLOW_ALL = True #  모든 호스트 허용
        # or 
        CORS_ORIGIN_WHITELIST = (
            "https://example.com",
            "https://sub.example.com",
            "http://localhost:8080",
            "http://127.0.0.1:9000"
        )
        ```

***
# Vue.js
## 기본 세팅
1. NVM(Node Version Manager)  
    [git을 활용한 설치 방법](https://github.com/nvm-sh/nvm)
    ```bash
    nvm install version
    nvm use version
    nvm list
    ```

2. Create Project  
    ```bash
    npm install -g @vue/cli
    vue create 'project name'
    vue add vuetify
    ```

3. eslint & prettier setting
    * VScode Extension -> eslint & prettier 설치
    * prettier -> disable(workspace)
    * `.eslintrc.js`에 rules 부분에 추가
        ```javascript
        'prettier/prettier': [
			'error',
			{
				singleQuote: true,
				semi: true,
				useTabs: true,
				tabWidth: 2,
				trailingComma: 'all',
				printWidth: 80,
				bracketSpacing: true,
				arrowParens: 'avoid',
			},
		],
		'vue/multi-word-component-names': 0, // vuetify 충돌 해결함
        ```
    * eslint:validate -> settings.json 수정
    ```javascript
     "eslint.validate": [
      "vue",
      "javascript",
      "javascriptreact",
      "typescript",
      "typescriptreact"
    ],
    "eslint.workingDirectories": [
      {"mode": "auto"}
    ],
    "editor.codeActionsOnSave": {
      "source.fixAll.eslint": true
    },
    // don't format on save
    "editor.formatOnSave": false
    ```
4. `npm run lint`

## vue.config.js
1. build 시 outputDir, assetsDir 수정
    ```javascript
    outputDir: 'dist', // default : 'dist'
	publicPath: '/',   // default : '/'
	assetsDir: 'static',
    ```
2. devServer proxy 설정
    ```javascript
    devServer: {
        proxy: 'http://localhost:8000',
    }
    ```
    
3. MPA 일 경우, pages option 추가
    ```javascript
    pages: {
        index: {
            entry: 'src/pages/main.js' // entry for the page
            template: 'public/index.html' // output as dist
            // when using title option,
            // template title tag needs to be <title><%= htmlWebpackPlugin.options.title %></title>
            title: 'Index Page',
            // chunks to include on this page, by default includes
            // extracted common chunks and vendor chunks.
            chunks: ['chunk-vendors', 'chunk-common', 'index']
        }
    }
    ```

## 웹팩(Webpack) 설정
> 웹팩이란 모듈 번들러이다.  
> 서로 연관되어 있는 모듈 간의 관계를 해석하여(예, Components) 정적 자원으로 변환해주는 도구  
> [VUE CLI 참고 링크](https://cli.vuejs.org/config/)

1. `npm install filemanager-webpack-plugin --save-dev` -> vue.config.js 추가
    ```javascript
    const FileManagerPlugin = require('filemanager-webpack-plugin');

	configureWebpack: {
		plugins: [
			new FileManagerPlugin({
				events: {
					onStart: {
						delete: [
							{ source: '../backend/templates/', options: { force: true } },
							{ source: '../backend/static/', options: { force: true } },
						],
						mkdir: ['../backend/templates/', '../backend/static/'],
					},
					onEnd: {
						copy: [
							{ source: './dist/static', destination: '../backend/static/' },
							{ source: './dist/*.html', destination: '../backend/templates/' },
						],
					},
				},
			}),
		],
	},
    ```
    




