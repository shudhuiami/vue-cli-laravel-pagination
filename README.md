[![npm](https://img.shields.io/npm/v/laravel-vue-pagination.svg)](https://www.npmjs.com/package/vue-cli-laravel-pagination) 

[![Downloads](https://img.shields.io/npm/dt/laravel-vue-pagination.svg)](https://www.npmjs.com/package/vue-cli-laravel-pagination)


# Vue Cli Laravel Pagination 
vue-cli-laravel-pagination is a plugin for implementing laravel pagination in on a vue cli based project. it's light weight, easy to use, well documented and has best support.

## Requirements

* [Vue.js](https://vuejs.org/) 2.x
* [Laravel](http://laravel.com/docs/) 5.x or upper

## Install

```bash
npm install vue-cli-laravel-pagination
```


## Usage

In your main.js:

```javascript
import VueLaravelPagination from '../plugin/index'


Vue.use(VueLaravelPagination)
```

Use the component:

```html
<vue-cli-laravel-pagination :data="laravelData" align="center" :onChange="changed_value" buttonLimit="10"></vue-cli-laravel-pagination>
```

```javascript
export default {

	data() {
		return {
			laravelData: {},
		}
	},
	mounted() {
		this.fetch();
	},
	methods: {
        // Detect page change event
        changed_value(options){
            console.log(options.page)
            this.fetch(options.page)
        },

		// Fetch data from a Laravel endpoint
	fetch(page = 1) {
		axios.get('example.com/fetch-url-here?page=' + page)
			.then(response => {
			    this.laravelData = response.data;
			});
	    }
	}

}
```

## API

| Name | Type | Description |
| --- | --- | --- |
| `data` | Object | An object containing the structure of a [Laravel paginator](https://laravel.com/docs/8.x/pagination) response or a [Laravel API Resource](https://laravel.com/docs/8.x/pagination) response. |
| `align` | String | (optional) One of `left` (default), `center` or `right` |
| `onChange` | Function | Callback event to detect button click and get selected page count|
| `buttonLimit` | Number | (optional) Numbers of pages to show default `5` |
| `prevBtn` | String/HTML | (optional) Customize previous button |
| `nextBtn` | String/HTML | (optional) Customize Next button |



## Show your Support

To show your support for my work on this project:

* [Star this repository](https://github.com/shudhuiami/vue-cli-laravel-pagination)

## Credits

Vue Cli Laravel Pagination was created by [Ahmed Zobayer](http://shudhuiami.github.io). Released under the ISC license.
