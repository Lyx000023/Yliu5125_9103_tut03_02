# Yliu5125_9103_tut03_02
# Quiz 9: Design Research – Imaging + Coding Techniques



## Part 1: Imaging Technique Inspiration

After researching the background of *Broadway Boogie Woogie* by Piet Mondrian, I discovered that its title was inspired by a style of jazz music known as Boogie Woogie. This connection between visual rhythm and sound led me to explore other artistic works that visualize music.

One of the most inspiring examples is *LINES*, an interactive sound installation created in 2016 by Swedish composer Anders Lind. In this exhibition, colored lines are fixed on walls, floors, and suspended from ceilings, embedded with sensors and electronics to form three types of musical instruments. The audience can trigger harmonies simply by moving under or interacting with the lines—no musical experience required.

Both Mondrian's painting and LINES express music visually, but LINES also transforms interaction into musical output. This inspired me to consider: what if I use digital media to do the same? Could I turn a static painting like *Broadway Boogie Woogie* into a reactive interface, where hovering the mouse or pressing keys over different colored blocks generates chords? By borrowing the interaction model from LINES, I aim to explore how abstract art can be transformed into an audiovisual experience.



![Mondrian's Broadway Boogie Woogie](https://github.com/Lyx000023/Yliu5125_9103_tut03_02/blob/main/Original%20work%20concept.png?raw=true)
![LINES interaction](https://github.com/Lyx000023/Yliu5125_9103_tut03_02/blob/main/inspiration.png?raw=true)

In addition, I plan to explore the use of **Perlin noise** and **random values** to bring movement and variation into the interaction. While Mondrian’s painting appears static and structured, the Boogie Woogie rhythm behind it suggests fluidity and improvisation. By using Perlin noise to simulate smooth, organic motion, and randomness to introduce unexpected visual or audio elements, I aim to recreate the improvisational nature of jazz visually. This approach helps transform a static artwork into a dynamic, evolving system that reacts to user input in unpredictable yet musically coherent ways.


![Randomness](https://github.com/Lyx000023/Yliu5125_9103_tut03_02/blob/main/未命名的设计-3.png?raw=true)
----

## Part 2: Coding Technique Exploration

To implement the concept of turning Mondrian’s *Broadway Boogie Woogie* into an interactive audiovisual system, I plan to use **Processing** as the development environment, as it allows intuitive mapping between visual elements and sound. The interactivity will be built upon the combination of **Perlin noise**, **randomness**, and **user input events** to simulate the fluid, improvisational nature of Boogie Woogie music.

The following functions and techniques will be essential:

### User Interaction
- `mouseMoved()`, `mousePressed()` – to detect hovering or clicking on specific colored blocks.
- `keyPressed()` – to assign keyboard inputs to additional tonal responses.
- `dist(x1, y1, x2, y2)` – to calculate whether the mouse is close to a block’s center for interaction detection.
- `get(x, y)` – to sample color data from the painting and assign corresponding sounds.

###  Sound Feedback
- `import processing.sound.*` – to use sound capabilities in Processing.
- `SoundFile` or `SinOsc` – for playing pre-recorded chord samples or generating tones dynamically.
- `play()`, `stop()` – to control playback in response to user action.

###  Visual Animation
- `noise(x)` – Perlin noise to simulate smooth, organic movement of blocks or background elements.
- `random(min, max)` – for spontaneous variations in color, scale, and speed.
- `randomSeed()` / `noiseSeed()` – to fix or vary the randomness for experimentation and repeatability.
- `lerpColor()` – for interpolating between two colors when hovering or activating a block.

###  Control Logic & Layout
- `rect(x, y, w, h)` – to draw color blocks similar to Mondrian’s painting.
- `fill(r, g, b)` – to control dynamic color output.
- `map()` – to convert Perlin noise output into usable screen coordinates or color values.
- `frameCount` – for controlling time-based changes or rhythm-like visual pulsing.

This implementation draws on the interaction logic of Anders Lind’s *LINES* installation, while incorporating smooth visual flow using noise and unpredictability using randomness—both essential to creating a jazz-like digital experience.

Related references:
- [Sound library in Processing](https://processing.org/reference/libraries/sound/index.html)
- [tiles motion 240503](https://openprocessing.org/sketch/2259231)

To further support my concept, I studied an OpenProcessing sketch (https://openprocessing.org/sketch/2259231) that combines **Perlin noise**, **random seeds**, and **mouse interaction** to generate evolving visual grids. The system dynamically changes shape, color, and rotation styles based on noise values and random selection, resulting in an unpredictable yet smooth aesthetic rhythm. The user can click to regenerate the pattern, similar to jazz improvisation.


He used the following sketch code:


let cellStages = ['in', 'wait', 'out']
let styleOpts = []
let palette
let bg
let cells = []
let m
let lastTime = null
let noiseIndex = 0
let noiseIndexGrid = 0
let addTimeLast = 0
let addProgress = 0
let addIndex = 0
let seed

function setup() {
	createCanvas(windowWidth, windowHeight);
	createPattern()
}

function windowResized() {
	resizeCanvas(windowWidth, windowHeight)
	m = min(width, height) * 0.9
}

function mouseClicked() {
	createPattern()
}

function draw() {
	let t = millis()
	if (!lastTime) lastTime = t
	let delta = t - lastTime
	lastTime = t
	
	addProgress = min(1, addProgress + delta / DUR.enter)
	let progressEase = ease[EASE_ENTER](addProgress)
	addIndex = round(progressEase * (cells.length - 1))
	
	let cellSize = m / SIDES
	cellSize = ceil(cellSize)
	m = cellSize * SIDES
	
	push()
	translate((width - m) / 2, (height - m) / 2)
	background(bg)
	
	for (let i = 0; i <= min(addIndex, cells.length - 1); i++) {
		let cell = cells[i]
		let x = cell.nx * cellSize
		let y = cell.ny * cellSize
		let s = cell.s * cellSize
		if (cell.stage === 'pre') {
			updateCell(cell, t)
		}

		if (cell.p >= 1) {
			cell.stage = cellStages[(cellStages.indexOf(cell.stage) + 1) % 3]
			cell.p = 0
			if (cell.stage === 'in') {
				updateCell(cell, t)
			}
		}
		let dur = DUR[cell.stage]
		cell.p = min(1, cell.p + delta / dur)
		
		push()
		beginClip()
		rect(x, y, s, s)
		endClip()
		if (cell.style.includes('line')) {
			stroke(cell.color)
			noFill()
		} else {
			noStroke()
			fill(cell.color)			
		}

		let p = 0 
		if (cell.stage === 'wait') {
			p = 1
		} else if (cell.stage === 'in') {
			p = ease[EASE_IN](cell.p)
		} else if (cell.stage === 'out') {
			p = ease[EASE_OUT](1 - cell.p)
		}
		cellTransform(x, y, s, cell.rot, p)
		celldr[cell.style](s, p, cell.color)
		pop()
	}
	pop()
}

function createPattern() {
	m = min(width, height) * 0.9
	lastTime = null
	noiseIndex = 0 
	noiseIndexGrid = 0
	palette = shuffle([...random(palettes)])
	bg = palette.pop()
	styleOpts = shuffle(Object.keys(celldr))
	seed = floor(random(1000))
	noiseSeed(++seed)
	
	addTimeLast = 0
	addProgress = 0
	addIndex = 0

	let cellMap = Array.from({ length: SIDES * SIDES }, () => null)
	cells = []
	let i = 0 
	
	while (i < cellMap.length) {
		if (cellMap[i] !== null) {
			i++
			continue
		}
		
		let {x, y} = indexToPoint(i, SIDES) 
		let cell = { nx: x, ny: y, s: 1, rot: 0, stage: 'pre' }
		cellMap[i] = cell
		cells.push(cell)
		i++
	}
	
	cells = cells.sort((a, b) => {
		let diff = a.ny + a.nx - (b.ny + b.nx) 
		return diff
	})
}

function updateCell(cell, t) {
	noiseIndex++
	if (noiseIndex >= SIDES * SIDES) {
		noiseIndex = 0 
		noiseIndexGrid++
	}
	let n = noise(cell.nx * FREQ_SHAPE, cell.ny * FREQ_SHAPE, noiseIndexGrid)
	let ns = floor(n * styleOpts.length * 2)
	cell.style = styleOpts[ns % styleOpts.length]
	
	let rotOptions = [0, PI * 0.5, PI, PI * 1.5]
	let nr = floor((n + random(0.3)) * 4)
	cell.rot = rotOptions[nr % rotOptions.length]
	
	let nColor = noise(cell.nx * FREQ_COLOR, cell.ny * FREQ_COLOR, noiseIndexGrid)
	nColor = floor(nColor * palette.length * 2)
	cell.color = palette[nColor % palette.length]
	cell.p = 0 
	cell.stage = 'in'
}

function cellTransform(x, y, s, rot, p) {
	translate(x + s / 2,y + s / 2)
	rotate(rot)
	translate(-s / 2, -s / 2)
}

![Shader Code Preview](https://github.com/Lyx000023/Yliu5125_9103_tut03_02/blob/main/tiles%20motion%20240503.png?raw=true)

