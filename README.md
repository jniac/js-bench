# js-bench

one empty run, then n function runs 

### usage:

```javascript
bench(myFunction1, myFunction2, ...)
// each function is tested for 500ms, to change that setting:
bench.duration = 2000
```

logs format: 
```
-- dt: 500.0ms count: 1227657op cost: 0.000407ms
#0 dt: 500.0ms count: 1208001op cost: 0.000414ms x1.016 slower
#n ...
```

### source:

```javascript
function bench(...functions) {

	const now = typeof performance === 'object' 
		? performance.now.bind(performance)
		: Date.now.bind(Date)

	console.log(`\ntest bench of ${functions.length} functions`)
	
	let { duration, costPrecision } = bench
	
	let t, dt, count, cost

	t = now()
	count = 0

	while(now() - t < duration)
		count++

	dt = now() - t
	cost = dt / count
	console.log(`-- dt: ${dt.toFixed(1)}ms count: ${count}op cost: ${cost.toFixed(costPrecision)}ms`)

	let refCount = count

	for (let [i, f] of functions.entries()) {

		t = now()
		count = 0

		while(now() - t < duration) {

			f()
			count++

		}

		dt = now() - t
		cost = dt / count

		let relCost = refCount / count

		console.log(`#${i} dt: ${dt.toFixed(1)}ms count: ${count}op cost: ${cost.toFixed(costPrecision)}ms x${relCost.toFixed(3)} slower`)

	}
	
}

Object.assign(bench, {

	duration: 500, 		// ms
	costPrecision: 6, 	// digit in output
	
})
```

[minified version](https://raw.githubusercontent.com/jniac/js-bench/master/bench.min.js), [via jscompress](https://jscompress.com/) (for copy/paste, still es7)
```javascript
function bench(...a){const b='object'==typeof performance?performance.now.bind(performance):Date.now.bind(Date);console.log(`\ntest bench of ${a.length} functions`);let{duration:c,costPrecision:d}=bench,e,g,h,j;for(e=b(),h=0;b()-e<c;)h++;g=b()-e,j=g/h,console.log(`-- dt: ${g.toFixed(1)}ms count: ${h}op cost: ${j.toFixed(d)}ms`);let k=h;for(let[l,m]of a.entries()){for(e=b(),h=0;b()-e<c;)m(),h++;g=b()-e,j=g/h;let n=k/h;console.log(`#${l} dt: ${g.toFixed(1)}ms count: ${h}op cost: ${j.toFixed(d)}ms x${n.toFixed(3)} slower`)}}Object.assign(bench,{duration:500,costPrecision:6});
```


# example:

```javascript
function bench(...functions) {

	const now = typeof performance === 'object' 
		? performance.now.bind(performance)
		: Date.now.bind(Date)

	console.log(`\ntest bench of ${functions.length} functions`)
	
	let { duration, costPrecision } = bench
	
	let t, dt, count, cost

	t = now()
	count = 0

	while(now() - t < duration)
		count++

	dt = now() - t
	cost = dt / count
	console.log(`-- dt: ${dt.toFixed(1)}ms count: ${count}op cost: ${cost.toFixed(costPrecision)}ms`)

	let refCount = count

	for (let [i, f] of functions.entries()) {

		t = now()
		count = 0

		while(now() - t < duration) {

			f()
			count++

		}

		dt = now() - t
		cost = dt / count

		let relCost = refCount / count

		console.log(`#${i} dt: ${dt.toFixed(1)}ms count: ${count}op cost: ${cost.toFixed(costPrecision)}ms x${relCost.toFixed(3)} slower`)

	}
	
}

Object.assign(bench, {

	duration: 500, 		// ms
	costPrecision: 6, 	// digit in output
	
})

a = { 
	x: .12345, 
	y: Math.PI, 
	width: 200, 
	height: 300, 
}

function f1() {

	return (
		a.x * 
		a.x + 
		a.y * 
		a.y + 
		a.x * 
		a.width + 
		a.y * 
		a.height
	)
	
}

function f2() {

	let { x, y, width:w, height:h } = a

	return (
		x * 
		x + 
		y * 
		y + 
		x * 
		w + 
		y * 
		h
	)
	
}

function f3() {
	
	let x = a.x
	let y = a.y
	let w = a.width
	let h = a.height

	return (
		x * 
		x + 
		y * 
		y + 
		x * 
		w + 
		y * 
		h
	)

}

bench(f1, f2, f3)
```

```
# outputs :

test bench of 3 functions
-- dt: 500.0ms count: 1227657op cost: 0.000407ms
#0 dt: 500.0ms count: 1208001op cost: 0.000414ms x1.016 slower
#1 dt: 500.0ms count: 1104582op cost: 0.000453ms x1.111 slower
#2 dt: 500.0ms count: 1169242op cost: 0.000428ms x1.050 slower
```

