# js-bench

one empty run, then n function runs 

logs: 
```
-- dt: 500.0ms count: 1227657op cost: 0.000407ms
#0 dt: 500.0ms count: 1208001op cost: 0.000414ms x1.016 slower
#n ...
```

```javascript
function bench(...functions) {

	console.log(`\ntest bench of ${functions.length} functions`)
	
	let d = 500 // ms
	
	let t, dt, count, cost

	t = performance.now()
	count = 0

	while(performance.now() - t < d)
		count++

	dt = performance.now() - t
	cost = dt / count
	console.log(`-- dt: ${dt.toFixed(1)}ms count: ${count}op cost: ${cost.toFixed(6)}ms`)

	let refCount = count

	for (let [i, f] of functions.entries()) {

		t = performance.now()
		count = 0

		while(performance.now() - t < d) {

			f()
			count++

		}

		dt = performance.now() - t
		cost = dt / count

		let relCost = refCount / count

		console.log(`#${i} dt: ${dt.toFixed(1)}ms count: ${count}op cost: ${cost.toFixed(6)}ms x${relCost.toFixed(3)} slower`)

	}
	
}
```

# example:

```javascript
function bench(...functions) {

	console.log(`\ntest bench of ${functions.length} functions`)
	
	let d = 500 // ms
	
	let t, dt, count, cost

	t = performance.now()
	count = 0

	while(performance.now() - t < d)
		count++

	dt = performance.now() - t
	cost = dt / count
	console.log(`-- dt: ${dt.toFixed(1)}ms count: ${count}op cost: ${cost.toFixed(6)}ms`)

	let refCount = count

	for (let [i, f] of functions.entries()) {

		t = performance.now()
		count = 0

		while(performance.now() - t < d) {

			f()
			count++

		}

		dt = performance.now() - t
		cost = dt / count

		let relCost = refCount / count

		console.log(`#${i} dt: ${dt.toFixed(1)}ms count: ${count}op cost: ${cost.toFixed(6)}ms x${relCost.toFixed(3)} slower`)

	}
	
}

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




