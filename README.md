# Yliu5125_9103_tut03_02
# Quiz 9: Design Research – Imaging + Coding Techniques



## 🎨 Part 1: Imaging Technique Inspiration

After researching the background of *Broadway Boogie Woogie* by Piet Mondrian, I discovered that its title was inspired by a style of jazz music known as Boogie Woogie. This connection between visual rhythm and sound led me to explore other artistic works that visualize music.

One of the most inspiring examples is *LINES*, an interactive sound installation created in 2016 by Swedish composer Anders Lind. In this exhibition, colored lines are fixed on walls, floors, and suspended from ceilings, embedded with sensors and electronics to form three types of musical instruments. The audience can trigger harmonies simply by moving under or interacting with the lines—no musical experience required.

Both Mondrian's painting and LINES express music visually, but LINES also transforms interaction into musical output. This inspired me to consider: what if I use digital media to do the same? Could I turn a static painting like *Broadway Boogie Woogie* into a reactive interface, where hovering the mouse or pressing keys over different colored blocks generates chords? By borrowing the interaction model from LINES, I aim to explore how abstract art can be transformed into an audiovisual experience.



![Mondrian's Broadway Boogie Woogie](https://github.com/Lyx000023/Yliu5125_9103_tut03_02/blob/main/Original%20work%20concept.png?raw=true)
![LINES interaction](https://github.com/Lyx000023/Yliu5125_9103_tut03_02/blob/main/inspiration.png?raw=true)

----

## 💻 Part 2: Coding Technique Exploration

To simulate the mirrored and fragmented visuals, I found a **WebGL fragment shader technique** using **Three.js**. It can create dynamic kaleidoscope and reflection effects by manipulating image textures in real time. This technique supports interactive transformation and could enhance user engagement through movement and distortion.

![Shader Code Preview](https://i.imgur.com/M7cMgEX.png)

🔗 Example implementation: [https://codepen.io/Yakudoo/pen/PojJvQW](https://codepen.io/Yakudoo/pen/PojJvQW)
