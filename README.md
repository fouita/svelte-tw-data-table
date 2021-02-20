# Responsive DataTable with svelte and tailwindcss

Responsive data-table built with svelte and tailwindcss

## Installation
```bash
    $npm i @fouita/data-table -D
```

## Simple usage

To fill out the table you can add a simple json data and a head attribute to specify what to display.

![DataTable foutia](https://cdn.fouita.com/assets/pics/template/data-table/data-table-simple.jpg)

```html
<script>
	import DataTable from '@fouita/data-table'
		
	const data = [
		  {
		    "first_name": "Wynn",
		    "last_name": "Donovan",
		    "company": "EMTRAK",
		    "email": "wynn.donovan@emtrak.io",
		    "phone": "+1 (809) 577-3968",
		    "address": "555 Freeman Street, Winesburg, New Jersey, 8986"
		  },
		  {
		    "first_name": "Arlene",
		    "last_name": "Cooley",
		    "company": "XUMONK",
		    "email": "arlene.cooley@xumonk.us",
		    "phone": "+1 (926) 459-2880",
		    "address": "659 Dupont Street, Lynn, West Virginia, 7173"
		  },
		  {
		    "first_name": "Jewell",
		    "last_name": "Sellers",
		    "company": "TECHADE",
		    "email": "jewell.sellers@techade.info",
		    "phone": "+1 (957) 456-2432",
		    "address": "955 Beekman Place, Waikele, Northern Mariana Islands, 341"
		  }
	]

	const head= {
		first_name: 'First Name',
		last_name: 'Last Name',
		email: 'Email',
		phone: 'Phone',
		company: 'Company',
	}


</script>


<DataTable {head} rows={data} hover />
```

The above example does not show the attribute address because it's not included in the head. You can add a list of selection and update the head list dynamically if you want.

By adding `hover` we tell the table to change the background of the row when hovering.

## Stripped

by adding a `stripped` prop the table becames like the following

![DataTable foutia](https://cdn.fouita.com/assets/pics/template/data-table/data-table-stripped.jpg)

```html
<DataTable {head} rows={data} stripped />
```

## Customizing Header

We can customize the alignement of text, the background/text color of all/some columns.

By adding `_class` attribute inside the head we can change the style easily

![DataTable foutia](https://cdn.fouita.com/assets/pics/template/data-table/data-table-red.jpg)

```javascript
    const head= {
		first_name: 'First Name',
		last_name: 'Last Name',
		email: 'Email',
		phone: 'Phone',
		company: 'Company',
		_class: 'bg-red-200 text-red-700'
	}
```

To update the entire column we can attribute `_class` to the specific head property like the following

![DataTable foutia](https://cdn.fouita.com/assets/pics/template/data-table/data-table-red-blue.jpg)

```javascript
    const head= {
		first_name: 'First Name',
		last_name: 'Last Name',
		email: 'Email',
		phone: {value: 'Phone', _class: 'text-right bg-blue-200 text-blue-900'},
		company: {value: 'Company', _class: 'text-left'},
		_class: 'bg-red-200 text-red-700'
	}
```

This will override the global head class style.


## Customizing rows/cells

We can customize rows and cells by also adding `_class` attribute inside the data

![DataTable foutia](https://cdn.fouita.com/assets/pics/template/data-table/data-table-row.jpg)

```javascript
    const data = [
		  // ... 
		  {
		    "first_name": "Arlene",
		    "last_name": "Cooley",
		    "company": "XUMONK",
		    "email": "arlene.cooley@xumonk.us",
		    "phone": "+1 (926) 459-2880",
		    "address": "659 Dupont Street, Lynn, West Virginia, 7173",
			_class: 'bg-green-200 font-bold text-red-700 text-center'
		  },
		  // ...
	]
```

It goes the same for specific cells like the follwing

![DataTable foutia](https://cdn.fouita.com/assets/pics/template/data-table/data-table-cells.jpg)

```javascript
    const data = [
		  // ... 
		   {
		    "first_name": "Arlene",
		    "last_name": "Cooley",
		    "company": "XUMONK",
		    "email": "arlene.cooley@xumonk.us",
		    "phone": {
				value: "+1 (926) 459-2880", 
				_class: 'text-white bg-green-600 p-5 border-2 border-red-600 text-center'
			},
		    "address": "659 Dupont Street, Lynn, West Virginia, 7173",
				_class: 'bg-green-200 font-bold text-red-700 text-center'
		  },
		  // ...
	]
```

## Sort, Search and Pagination

To keep things simple, the sorting and pagination functions are custom depending on how you are planning to use the DataTable (with preloaded data or you prefer doing server request).

this is an example on how to use them.

### Data Sorting

![DataTable foutia](https://cdn.fouita.com/assets/pics/template/data-table/data-table-sort.jpg)

```html
<script>
	import DataTable from '@fouita/data-table'
		
	let rows = [
		  {
		    "first_name": "Wynn",
		    "last_name": "Donovan",
		    "company": "EMTRAK"
		  },
		  {
		    "first_name": "Arlene",
		    "last_name": "Cooley",
            "company": "XUMONK",
            _class: 'bg-green-200 font-bold text-red-700 text-center'
		  },
		  {
		    "first_name": "Jewell",
		    "last_name": "Sellers",
		    "company": "TECHADE"
		  }
	]
    
    // define your sort function (call API if you like), it should return an object with 'asc' and 'desc' methods
	const sortFn = (label) => {
		return {
			asc: () => {
				rows = rows.sort((a,b) => a[label]>b[label] ? 1:-1)
			},
			desc: () => {
				rows = rows.sort((a,b) => a[label]<b[label] ? 1:-1)
			}
		}
	}

    // add sort attribute in the column head and call the sort function
	let head= {
		first_name: {
			value: 'First Name', 
			sort: sortFn('first_name')
		},
		last_name: {
			value: 'Last Name', 
			sort: sortFn('last_name')
		},
		company: 'Company',
	}



</script>


<DataTable {head} {rows} hover />
```

### Data search

For the search we can add an input that filter the data rows and return a filtred list, no big deal!

![DataTable foutia](https://cdn.fouita.com/assets/pics/template/data-table/data-table-search.jpg)

We can create a `Search.svelte` component like the following

```html
<script>
	export let search = ''
	import {SearchIcon} from 'svelte-feather-icons'
</script>


<div class="flex flex-row" >
	<div class="text-gray-600 border border-gray-400 flex flex-row rounded max-w-xs my-3 bg-white" >
		<input type="search" bind:value={search} placeholder="Search..." class="h-10 w-64 rounded text-sm focus:outline-none px-3">
		<button class="m-3 focus:outline-none" >
			<SearchIcon class="w-4 h-4" />
		</button>
	</div>
</div>
```

And then use it with the search function, we can use [Fuse](https://fusejs.io) for simple search

```html
<script>
    import DataTable from '@fouita/data-table'
    import Fuse from 'fuse.js/dist/fuse.min.js';
    import Search from './Search.svelte'

    const users = [
		  {
		    "first_name": "Wynn",
		    "last_name": "Donovan",
		    "company": "EMTRAK"
		  },
		  {
		    "first_name": "Arlene",
		    "last_name": "Cooley",
            "company": "XUMONK",
            _class: 'bg-green-200 font-bold text-red-700 text-center'
		  },
		  {
		    "first_name": "Jewell",
		    "last_name": "Sellers",
		    "company": "TECHADE"
		  }
    ]

    const head= {
		first_name: 'First Name',
		last_name: 'Last Name',
		company: 'Company'
	}
    
    let rows = users
    let keywords = ''
    const options = {
	  keys: ['first_name', 'last_name', 'company']
    }
    const fuse = new Fuse(rows, options)

    let keywords=''

	$: if(keywords){
		// Call server here if you need server request
		let result = fuse.search(keywords)
		rows = result.map(r => r.item)
	}

	$: if(keywords==''){
		rows = users
	}
</script>

<Search />
<DataTable {head} {rows} />
```

### Pagination 

for the pagination you can checkout [Fouita Pagination](https://github.com/fouita/svelte-tw-pagination) and update the data each time you navigate (it's very simple!)



## About

[Fouita : UI framework for svelte + tailwind components](https://fouita.com)