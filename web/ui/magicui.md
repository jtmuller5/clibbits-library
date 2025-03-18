<!-- └── content
    └── docs
        └── components
            ├── android.mdx
            ├── animated-beam.mdx
            ├── animated-circular-progress-bar.mdx
            ├── animated-gradient-text.mdx
            ├── animated-grid-pattern.mdx
            ├── animated-list.mdx
            ├── animated-shiny-text.mdx
            ├── animated-subscribe-button.mdx
            ├── aurora-text.mdx
            ├── avatar-circles.mdx
            ├── bento-grid.mdx
            ├── blur-fade.mdx
            ├── border-beam.mdx
            ├── box-reveal.mdx
            ├── code-comparison.mdx
            ├── confetti.mdx
            ├── cool-mode.mdx
            ├── dock.mdx
            ├── dot-pattern.mdx
            ├── file-tree.mdx
            ├── flickering-grid.mdx
            ├── flip-text.mdx
            ├── globe.mdx
            ├── grid-pattern.mdx
            ├── hero-video-dialog.mdx
            ├── hyper-text.mdx
            ├── icon-cloud.mdx
            ├── interactive-grid-pattern.mdx
            ├── interactive-hover-button.mdx
            ├── iphone-15-pro.mdx
            ├── lens.mdx
            ├── line-shadow-text.mdx
            ├── magic-card.mdx
            ├── marquee.mdx
            ├── meteors.mdx
            ├── morphing-text.mdx
            ├── neon-gradient-card.mdx
            ├── number-ticker.mdx
            ├── orbiting-circles.mdx
            ├── particles.mdx
            ├── pointer.mdx
            ├── pulsating-button.mdx
            ├── rainbow-button.mdx
            ├── retro-grid.mdx
            ├── ripple-button.mdx
            ├── ripple.mdx
            ├── safari.mdx
            ├── scratch-to-reveal.mdx
            ├── script-copy-btn.mdx
            ├── scroll-based-velocity.mdx
            ├── scroll-progress.mdx
            ├── shimmer-button.mdx
            ├── shine-border.mdx
            ├── shiny-button.mdx
            ├── sparkles-text.mdx
            ├── spinning-text.mdx
            ├── terminal.mdx
            ├── text-animate.mdx
            ├── text-reveal.mdx
            ├── tweet-card.mdx
            ├── typing-animation.mdx
            ├── warp-background.mdx
            └── word-rotate.mdx


/content/docs/components/android.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Android
 3 | date: 2024-12-20
 4 | description: A mockup of an Android device.
 5 | author: dillionverma
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="android-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/android"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="android" />
34 | 
35 | </Steps>
36 | 
37 | </TabsContent>
38 | 
39 | </Tabs>
40 | 
41 | ## Examples
42 | 
43 | ### With Image
44 | 
45 | <ComponentPreview name="android-demo-2" />
46 | 
47 | ### With Video
48 | 
49 | <ComponentPreview name="android-demo-3" />
50 | 
51 | ## Props
52 | 
53 | | Prop       | Type     | Default | Description                        |
54 | | ---------- | -------- | ------- | ---------------------------------- |
55 | | `width`    | `number` | `433`   | The width of the Android window    |
56 | | `height`   | `number` | `882`   | The height of the Android window   |
57 | | `src`      | `string` | `-`     | The source of the image to display |
58 | | `videoSrc` | `string` | `-`     | The source of the video to display |
59 | 
60 | The `Android` component also accepts all properties of the `SVGElement` type.
61 | 


--------------------------------------------------------------------------------
/content/docs/components/animated-beam.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Animated Beam
 3 | date: 2024-02-11
 4 | description: An animated beam of light which travels along a path. Useful for showcasing the "integration" features of a website.
 5 | author: dillionverma
 6 | published: true
 7 | video: https://cdn.magicui.design/animated-beam.mp4
 8 | ---
 9 | 
10 | <ComponentPreview name="animated-beam-demo" />
11 | 
12 | ## Installation
13 | 
14 | <Tabs defaultValue="cli">
15 | 
16 | <TabsList>
17 |   <TabsTrigger value="cli">CLI</TabsTrigger>
18 |   <TabsTrigger value="manual">Manual</TabsTrigger>
19 | </TabsList>
20 | <TabsContent value="cli">
21 | 
22 | ```bash
23 | npx shadcn@latest add "https://magicui.design/r/animated-beam"
24 | ```
25 | 
26 | </TabsContent>
27 | 
28 | <TabsContent value="manual">
29 | 
30 | <Steps>
31 | 
32 | <Step>Copy and paste the following code into your project.</Step>
33 | 
34 | <ComponentSource name="animated-beam" />
35 | 
36 | <Step>Update the import paths to match your project setup.</Step>
37 | 
38 | </Steps>
39 | 
40 | </TabsContent>
41 | 
42 | </Tabs>
43 | 
44 | ## Examples
45 | 
46 | ### Animated Beam Uni-Directional
47 | 
48 | <ComponentPreview name="animated-beam-unidirectional" />
49 | 
50 | ### Animated Beam Bi-Directional
51 | 
52 | <ComponentPreview name="animated-beam-bidirectional" />
53 | 
54 | ### Animated Beam Multiple Inputs
55 | 
56 | <ComponentPreview name="animated-beam-multiple-inputs" />
57 | 
58 | ### Animated Beam Multiple Outputs
59 | 
60 | <ComponentPreview name="animated-beam-multiple-outputs" />
61 | 
62 | ## Props
63 | 
64 | ### Animated Beam
65 | 
66 | | Prop                 | Type      | Default   | Description                                              |
67 | | -------------------- | --------- | --------- | -------------------------------------------------------- |
68 | | `className`          | `string`  | `-`       | The class name for the component.                        |
69 | | `containerRef`       | `ref`     | `-`       | The container ref.                                       |
70 | | `fromRef`            | `ref`     | `-`       | The ref of the element from which the beam should start. |
71 | | `toRef`              | `ref`     | `-`       | The ref of the element to which the beam should end.     |
72 | | `curvature`          | `number`  | `0`       | The curvature of the beam.                               |
73 | | `reverse`            | `boolean` | `false`   | Whether the beam should be reversed.                     |
74 | | `duration`           | `number`  | `5`       | The duration of the beam.                                |
75 | | `delay`              | `number`  | `0`       | The delay of the beam.                                   |
76 | | `pathColor`          | `string`  | `gray`    | The color of the beam.                                   |
77 | | `pathWidth`          | `number`  | `2`       | The width of the beam.                                   |
78 | | `pathOpacity`        | `number`  | `0.2`     | The opacity of the beam.                                 |
79 | | `gradientStartColor` | `string`  | `#ffaa40` | The start color of the gradient.                         |
80 | | `gradientStopColor`  | `string`  | `#9c40ff` | The stop color of the gradient.                          |
81 | | `startXOffset`       | `number`  | `0`       | The start x offset of the beam.                          |
82 | | `startYOffset`       | `number`  | `0`       | The start y offset of the beam.                          |
83 | | `endXOffset`         | `number`  | `0`       | The end x offset of the beam.                            |
84 | | `endYOffset`         | `number`  | `0`       | The end y offset of the beam.                            |
85 | 
86 | ## Credits
87 | 
88 | - Credit to [@itsarghyadas](https://twitter.com/itsarghyadas) for figuring out the foundation of this!
89 | 


--------------------------------------------------------------------------------
/content/docs/components/animated-circular-progress-bar.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Animated Circular Progress Bar
 3 | date: 2024-05-28
 4 | description: Animated Circular Progress Bar is a component that displays a circular gauge with a percentage value.
 5 | author: luis-codex
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="animated-circular-progress-bar-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/animated-circular-progress-bar"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="animated-circular-progress-bar" />
34 | 
35 | <Step>Update the import paths to match your project setup.</Step>
36 | 
37 | </Steps>
38 | 
39 | </TabsContent>
40 | 
41 | </Tabs>
42 | 
43 | ## Props
44 | 
45 | | Prop                  | Type     | Default | Description                                   |
46 | | --------------------- | -------- | ------- | --------------------------------------------- |
47 | | `className`           | `string` | `-`     | The class name to be applied to the component |
48 | | `max`                 | `number` | `100`   | The maximum value of the gauge                |
49 | | `min`                 | `number` | `0`     | The minimum value of the gauge                |
50 | | `value`               | `number` | `0`     | The current value of the gauge                |
51 | | `gaugePrimaryColor`   | `string` | `-`     | The primary color of the gauge                |
52 | | `gaugeSecondaryColor` | `string` | `-`     | The secondary color of the gauge              |
53 | 
54 | ## Credits
55 | 
56 | - Credit to [@luis-code](https://luis-code.vercel.app/)
57 | 


--------------------------------------------------------------------------------
/content/docs/components/animated-gradient-text.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Animated Gradient Text
 3 | date: 2024-04-09
 4 | description: An animated gradient background which transitions between colors for text.
 5 | author: dillionverma
 6 | published: true
 7 | video: https://cdn.magicui.design/animated-gradient-text.mp4
 8 | ---
 9 | 
10 | <ComponentPreview name="animated-gradient-text-demo" />
11 | 
12 | ## Installation
13 | 
14 | <Tabs defaultValue="cli">
15 | 
16 | <TabsList>
17 |   <TabsTrigger value="cli">CLI</TabsTrigger>
18 |   <TabsTrigger value="manual">Manual</TabsTrigger>
19 | </TabsList>
20 | <TabsContent value="cli">
21 | 
22 | ```bash
23 | npx shadcn@latest add "https://magicui.design/r/animated-gradient-text"
24 | ```
25 | 
26 | </TabsContent>
27 | 
28 | <TabsContent value="manual">
29 | 
30 | <Steps>
31 | 
32 | <Step>Copy and paste the following code into your project.</Step>
33 | 
34 | <ComponentSource name="animated-gradient-text" />
35 | 
36 | <Step>Update the import paths to match your project setup.</Step>
37 | 
38 | <Step>Update `tailwind.config.js`</Step>
39 | 
40 | Add the following animations to your `tailwind.config.js` file:
41 | 
42 | ```js title="tailwind.config.js" {5-14}
43 | /** @type {import('tailwindcss').Config} */
44 | module.exports = {
45 |   theme: {
46 |     extend: {
47 |       animation: {
48 |         gradient: "gradient 8s linear infinite",
49 |       },
50 |       keyframes: {
51 |         gradient: {
52 |           to: {
53 |             backgroundPosition: "var(--bg-size) 0",
54 |           },
55 |         },
56 |       },
57 |     },
58 |   },
59 | };
60 | ```
61 | 
62 | </Steps>
63 | 
64 | </TabsContent>
65 | 
66 | </Tabs>
67 | 
68 | ## Examples
69 | 
70 | ### Custom Speed
71 | 
72 | <ComponentPreview name="animated-gradient-text-demo-2" />
73 | 
74 | ## Props
75 | 
76 | | Prop        | Type     | Default     | Description                            |
77 | | ----------- | -------- | ----------- | -------------------------------------- |
78 | | `children`  | `node`   | `-`         | The children passed into the component |
79 | | `className` | `string` | `-`         | The class name to be applied           |
80 | | `speed`     | `number` | `1`         | The speed of the gradient animation    |
81 | | `colorFrom` | `string` | `"#ffaa40"` | The starting color of the gradient     |
82 | | `colorTo`   | `string` | `"#9c40ff"` | The ending color of the gradient       |
83 | 


--------------------------------------------------------------------------------
/content/docs/components/animated-grid-pattern.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Animated Grid Pattern
 3 | date: 2024-02-07
 4 | description: A animated background grid pattern made with SVGs, fully customizable using Tailwind CSS.
 5 | author: dillionverma
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="animated-grid-pattern-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/animated-grid-pattern"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="animated-grid-pattern" />
34 | 
35 | <Step>Update the import paths to match your project setup.</Step>
36 | 
37 | </Steps>
38 | 
39 | </TabsContent>
40 | 
41 | </Tabs>
42 | 
43 | ## Props
44 | 
45 | ### GridPattern
46 | 
47 | | Prop              | Type     | Default | Description                                   |
48 | | ----------------- | -------- | ------- | --------------------------------------------- |
49 | | `className`       | `string` | `-`     | Additional classes to be added to the pattern |
50 | | `width`           | `number` | `40`    | Width of the pattern                          |
51 | | `height`          | `number` | `40`    | Height of the pattern                         |
52 | | `x`               | `number` | `-1`    | X offset of the pattern                       |
53 | | `y`               | `number` | `-1`    | Y offset of the pattern                       |
54 | | `strokeDasharray` | `number` | `0`     | Stroke dash array of the pattern              |
55 | | `numSquares`      | `number` | `200`   | Number of squares in the pattern              |
56 | | `maxOpacity`      | `number` | `0.5`   | Maximum opacity of the pattern                |
57 | | `duration`        | `number` | `1`     | Duration of the animation                     |
58 | | `repeatDelay`     | `number` | `0.5`   | Repeat delay of the animation                 |
59 | 


--------------------------------------------------------------------------------
/content/docs/components/animated-list.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Animated List
 3 | date: 2023-12-12
 4 | description: A list that animates each item in sequence with a delay. Used to showcase notifications or events on your landing page.
 5 | author: dillionverma
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="animated-list-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/animated-list"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="animated-list" />
34 | 
35 | <Step>Update the import paths to match your project setup.</Step>
36 | 
37 | </Steps>
38 | 
39 | </TabsContent>
40 | 
41 | </Tabs>
42 | 
43 | ## Props
44 | 
45 | ### Animated List
46 | 
47 | | Prop        | Type     | Default | Description                       |
48 | | ----------- | -------- | ------- | --------------------------------- |
49 | | `className` | `string` | `-`     | The class name for the component  |
50 | | `delay`     | `number` | `1000`  | The delay between each item in ms |
51 | 


--------------------------------------------------------------------------------
/content/docs/components/animated-shiny-text.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Animated Shiny Text
 3 | date: 2024-01-16
 4 | description: A light glare effect which pans across text making it appear as if it is shimmering.
 5 | author: dillionverma
 6 | published: true
 7 | video: https://cdn.magicui.design/animated-shiny-text.mp4
 8 | ---
 9 | 
10 | <ComponentPreview name="animated-shiny-text-demo" />
11 | 
12 | ## Installation
13 | 
14 | <Tabs defaultValue="cli">
15 | 
16 | <TabsList>
17 |   <TabsTrigger value="cli">CLI</TabsTrigger>
18 |   <TabsTrigger value="manual">Manual</TabsTrigger>
19 | </TabsList>
20 | <TabsContent value="cli">
21 | 
22 | ```bash
23 | npx shadcn@latest add "https://magicui.design/r/animated-shiny-text"
24 | ```
25 | 
26 | </TabsContent>
27 | 
28 | <TabsContent value="manual">
29 | 
30 | <Steps>
31 | 
32 | <Step>Copy and paste the following code into your project.</Step>
33 | 
34 | <ComponentSource name="animated-shiny-text" />
35 | 
36 | <Step>Update the import paths to match your project setup.</Step>
37 | 
38 | <Step>Update `tailwind.config.js`</Step>
39 | 
40 | Add the following animations to your `tailwind.config.js` file:
41 | 
42 | ```js title="tailwind.config.js" {5-17}
43 | /** @type {import('tailwindcss').Config} */
44 | module.exports = {
45 |   theme: {
46 |     extend: {
47 |       animation: {
48 |         "shiny-text": "shiny-text 8s infinite",
49 |       },
50 |       keyframes: {
51 |         "shiny-text": {
52 |           "0%, 90%, 100%": {
53 |             "background-position": "calc(-100% - var(--shiny-width)) 0",
54 |           },
55 |           "30%, 60%": {
56 |             "background-position": "calc(100% + var(--shiny-width)) 0",
57 |           },
58 |         },
59 |       },
60 |     },
61 |   },
62 | };
63 | ```
64 | 
65 | </Steps>
66 | 
67 | </TabsContent>
68 | 
69 | </Tabs>
70 | 
71 | ## Props
72 | 
73 | | Prop           | Type     | Default | Description                                  |
74 | | -------------- | -------- | ------- | -------------------------------------------- |
75 | | `children`     | `node`   | `-`     | The text to be shimmered.                    |
76 | | `className`    | `string` | `-`     | The class name to be applied to the shimmer. |
77 | | `shimmerWidth` | `number` | `100`   | The width of the shimmer in pixels.          |
78 | 


--------------------------------------------------------------------------------
/content/docs/components/animated-subscribe-button.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Animated Subscribe Button
 3 | date: 2024-05-30
 4 | description: An animated subscribe button useful for showing a micro animation from intial to final result.
 5 | author: pilladipesh33
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="animated-subscribe-button-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/animated-subscribe-button"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="animated-subscribe-button" />
34 | 
35 | <Step>Update the import paths to match your project setup.</Step>
36 | 
37 | </Steps>
38 | 
39 | </TabsContent>
40 | 
41 | </Tabs>
42 | 
43 | ## Props
44 | 
45 | | Prop              | Type              | Default | Description                                                                                                                                       |
46 | | ----------------- | ----------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
47 | | `subscribeStatus` | `boolean`         | `false` | A boolean flag to check the condition for the button. This property can be used to toggle the button's state, such as subscribed or unsubscribed. |
48 | | `children`        | `React.ReactNode` | `-`     | The content to be displayed inside the button. Should contain two children - first for unsubscribed state and second for subscribed state.        |
49 | | `className`       | `string`          | `-`     | Optional class name to be applied to the button for custom styling.                                                                               |
50 | 
51 | ## Credits
52 | 
53 | - Credit to [@dipesh](https://github.com/pilladipesh33)
54 | 


--------------------------------------------------------------------------------
/content/docs/components/aurora-text.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Aurora Text
 3 | date: 2025-01-12
 4 | description: A beautiful aurora text effect
 5 | author: Dillon
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="aurora-text-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/aurora-text"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="aurora-text" />
34 | 
35 | <Step>Update the import paths to match your project setup.</Step>
36 | 
37 | </Steps>
38 | 
39 | </TabsContent>
40 | 
41 | </Tabs>
42 | 
43 | ## Props
44 | 
45 | | Prop        | Type        | Default                                                              | Description                                                   |
46 | | ----------- | ----------- | -------------------------------------------------------------------- | ------------------------------------------------------------- |
47 | | `className` | `string`    | `""`                                                                 | The class name to be applied to the component                 |
48 | | `children`  | `ReactNode` | `-`                                                                  | Children elements                                             |
49 | | `colors`    | `string[]`  | `["#FF0080", "#7928CA", "#0070F3", "#38bdf8", "#a855f7", "#2dd4bf"]` | Array of colors used for the aurora effect                    |
50 | | `speed`     | `number`    | `1`                                                                  | Animation speed multiplier (1 is default, 2 is twice as fast) |
51 | 


--------------------------------------------------------------------------------
/content/docs/components/avatar-circles.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Avatar Circles
 3 | date: 2024-05-22
 4 | description: Overlapping circles of avatars.
 5 | author: tomonarifeehan
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="avatar-circles-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/avatar-circles"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="avatar-circles" />
34 | 
35 | <Step>Update the import paths to match your project setup.</Step>
36 | 
37 | </Steps>
38 | 
39 | </TabsContent>
40 | 
41 | </Tabs>
42 | 
43 | ## Props
44 | 
45 | | Prop        | Type     | Default | Description                                   |
46 | | ----------- | -------- | ------- | --------------------------------------------- |
47 | | `className` | `string` | `-`     | The class name to be applied to the component |
48 | | `numPeople` | `number` | `99`    | The number appearing in the last circle       |
49 | 


--------------------------------------------------------------------------------
/content/docs/components/bento-grid.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Bento Grid
 3 | date: 2023-11-09
 4 | description: Bento grid is a layout used to showcase the features of a product in a simple and elegant way.
 5 | author: dillionverma
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="bento-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/bento-grid"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="bento-grid" />
34 | 
35 | <Step>Update the import paths to match your project setup.</Step>
36 | 
37 | </Steps>
38 | 
39 | </TabsContent>
40 | 
41 | </Tabs>
42 | 
43 | ## Examples
44 | 
45 | <ComponentPreview name="bento-demo-vertical" />
46 | 


--------------------------------------------------------------------------------
/content/docs/components/blur-fade.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Blur Fade
 3 | date: 2024-07-07
 4 | description: Blur fade in and out animation. Used to smoothly fade in and out content.
 5 | author: dillionverma
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="blur-fade-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/blur-fade"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="blur-fade" />
34 | 
35 | <Step>Update the import paths to match your project setup.</Step>
36 | 
37 | </Steps>
38 | 
39 | </TabsContent>
40 | 
41 | </Tabs>
42 | 
43 | ## Examples
44 | 
45 | <ComponentPreview name="blur-fade-text-demo" />
46 | 
47 | ## Props
48 | 
49 | | Prop           | Type              | Default   | Description                                                 |
50 | | -------------- | ----------------- | --------- | ----------------------------------------------------------- |
51 | | `children`     | `React.ReactNode` | `-`       | The content to be animated                                  |
52 | | `className`    | `string`          | `-`       | The class name to be applied to the component               |
53 | | `variant`      | `object`          | `-`       | Custom animation variants for motion component              |
54 | | `duration`     | `number`          | `0.4`     | Duration (seconds) for the animation                        |
55 | | `delay`        | `number`          | `0`       | Delay (seconds) before the animation starts                 |
56 | | `offset`       | `number`          | `6`       | Offset for the animation                                    |
57 | | `direction`    | `string`          | `"down"`  | Direction for the animation (`up`, `down`, `left`, `right`) |
58 | | `inView`       | `boolean`         | `false`   | Whether to trigger animation when component is in view      |
59 | | `inViewMargin` | `MarginType`      | `"-50px"` | Margin for triggering the in-view animation                 |
60 | | `blur`         | `string`          | `"6px"`   | Amount of blur to apply during the animation                |
61 | 


--------------------------------------------------------------------------------
/content/docs/components/border-beam.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Border Beam
 3 | date: 2024-02-06
 4 | description: An animated beam of light which travels along the border of its container.
 5 | author: dillionverma
 6 | published: true
 7 | video: https://cdn.magicui.design/border-beam.mp4
 8 | ---
 9 | 
10 | <ComponentPreview name="border-beam-demo" />
11 | 
12 | ## Installation
13 | 
14 | <Tabs defaultValue="cli">
15 | 
16 | <TabsList>
17 |   <TabsTrigger value="cli">CLI</TabsTrigger>
18 |   <TabsTrigger value="manual">Manual</TabsTrigger>
19 | </TabsList>
20 | <TabsContent value="cli">
21 | 
22 | ```bash
23 | npx shadcn@latest add "https://magicui.design/r/border-beam"
24 | ```
25 | 
26 | </TabsContent>
27 | 
28 | <TabsContent value="manual">
29 | 
30 | <Steps>
31 | 
32 | <Step>Copy and paste the following code into your project.</Step>
33 | 
34 | <ComponentSource name="border-beam" />
35 | 
36 | </Steps>
37 | 
38 | </TabsContent>
39 | 
40 | </Tabs>
41 | 
42 | ## Examples
43 | 
44 | ### 2 Border Beams
45 | 
46 | <ComponentPreview name="border-beam-demo-2" />
47 | 
48 | ### Reverse
49 | 
50 | <ComponentPreview name="border-beam-demo-3" />
51 | 
52 | ### Spring Animation
53 | 
54 | <ComponentPreview name="border-beam-demo-4" />
55 | 
56 | ## Usage
57 | 
58 | ```tsx
59 | import { BorderBeam } from "@/components/magicui/border-beam.tsx";
60 | ```
61 | 
62 | ```tsx
63 | <div className="relative overflow-hidden">
64 |   <BorderBeam />
65 | </div>
66 | ```
67 | 
68 | ## Props
69 | 
70 | ### Border Beam
71 | 
72 | | Prop            | Type                  | Default   | Description                            |
73 | | --------------- | --------------------- | --------- | -------------------------------------- |
74 | | `className`     | `string`              | `-`       | Custom class to apply to the component |
75 | | `size`          | `number`              | `50`      | Size of the beam                       |
76 | | `duration`      | `number`              | `6`       | Duration of the animation in seconds   |
77 | | `delay`         | `number`              | `0`       | Delay before the animation starts      |
78 | | `colorFrom`     | `string`              | `#ffaa40` | Start color of the beam gradient       |
79 | | `colorTo`       | `string`              | `#9c40ff` | End color of the beam gradient         |
80 | | `transition`    | `Transition`          | `-`       | Custom motion transition configuration |
81 | | `style`         | `React.CSSProperties` | `-`       | Custom CSS styles to apply             |
82 | | `reverse`       | `boolean`             | `false`   | Whether to reverse animation direction |
83 | | `initialOffset` | `number`              | `0`       | Initial offset position (0-100)        |
84 | 


--------------------------------------------------------------------------------
/content/docs/components/box-reveal.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Box Reveal Animation
 3 | date: 2024-05-23
 4 | description: Sliding box animation that reveals text behind it.
 5 | author: tomonarifeehan
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="box-reveal-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/box-reveal"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="box-reveal" />
34 | 
35 | <Step>Update the import paths to match your project setup.</Step>
36 | 
37 | </Steps>
38 | 
39 | </TabsContent>
40 | 
41 | </Tabs>
42 | 
43 | ## Props
44 | 
45 | | Prop        | Type     | Default   | Description                                   |
46 | | ----------- | -------- | --------- | --------------------------------------------- |
47 | | `className` | `string` | `-`       | The class name to be applied to the component |
48 | | `boxColor`  | `string` | `#5046e6` | Color of the box overlaying the text          |
49 | | `duration`  | `number` | `0.5`     | Durations (seconds) of the animation          |
50 | 


--------------------------------------------------------------------------------
/content/docs/components/code-comparison.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Code Comparison
 3 | date: 2024-10-02
 4 | description: A component which compares two code snippets.
 5 | author: dillionverma
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="code-comparison-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/code-comparison"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="code-comparison" />
34 | 
35 | </Steps>
36 | 
37 | </TabsContent>
38 | 
39 | </Tabs>
40 | 
41 | ## Props
42 | 
43 | | Prop         | Type     | Default        | Description                                            |
44 | | ------------ | -------- | -------------- | ------------------------------------------------------ |
45 | | `className`  | `string` | `-`            | The class name to be applied to the component          |
46 | | `beforeCode` | `string` | `-`            | The code snippet to display in the "before" section    |
47 | | `afterCode`  | `string` | `-`            | The code snippet to display in the "after" section     |
48 | | `language`   | `string` | `-`            | The language of the code snippets (e.g., "typescript") |
49 | | `filename`   | `string` | `-`            | The filename to display for the code snippets          |
50 | | `lightTheme` | `string` | `github-light` | The theme to use for light mode                        |
51 | | `darkTheme`  | `string` | `github-dark`  | The theme to use for dark mode                         |
52 | 


--------------------------------------------------------------------------------
/content/docs/components/confetti.mdx:
--------------------------------------------------------------------------------
  1 | ---
  2 | title: Confetti
  3 | date: 2024-05-26
  4 | description: Confetti animations are best used to delight your users when something special happens
  5 | author: Bankkroll
  6 | published: true
  7 | ---
  8 | 
  9 | <ComponentPreview name="confetti-demo" />
 10 | 
 11 | ## Installation
 12 | 
 13 | <Tabs defaultValue="cli">
 14 | 
 15 | <TabsList>
 16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
 17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
 18 | </TabsList>
 19 | <TabsContent value="cli">
 20 | 
 21 | ```bash
 22 | npx shadcn@latest add "https://magicui.design/r/confetti"
 23 | ```
 24 | 
 25 | </TabsContent>
 26 | 
 27 | <TabsContent value="manual">
 28 | 
 29 | <Steps>
 30 | 
 31 | <Step>Install the following dependencies:</Step>
 32 | 
 33 | ```bash
 34 | npm install canvas-confetti
 35 | ```
 36 | 
 37 | <Step>Copy and paste the following code into your project.</Step>
 38 | 
 39 | <ComponentSource name="confetti" />
 40 | 
 41 | <Step>Update the import paths to match your project setup.</Step>
 42 | 
 43 | </Steps>
 44 | 
 45 | </TabsContent>
 46 | 
 47 | </Tabs>
 48 | 
 49 | ## Examples
 50 | 
 51 | ### Basic
 52 | 
 53 | <ComponentPreview name="confetti-basic-cannon" />
 54 | 
 55 | ### Random Direction
 56 | 
 57 | <ComponentPreview name="confetti-random-direction" />
 58 | 
 59 | ### Fireworks
 60 | 
 61 | <ComponentPreview name="confetti-fireworks" />
 62 | 
 63 | ### Side Cannons
 64 | 
 65 | <ComponentPreview name="confetti-side-cannons" />
 66 | 
 67 | ### Stars
 68 | 
 69 | <ComponentPreview name="confetti-stars" />
 70 | 
 71 | ### Custom Shapes
 72 | 
 73 | <ComponentPreview name="confetti-custom-shapes" />
 74 | 
 75 | ### Emoji
 76 | 
 77 | <ComponentPreview name="confetti-emoji" />
 78 | 
 79 | ## Props
 80 | 
 81 | ### Confetti
 82 | 
 83 | | Prop                      | Type                        | Default                                                                         | Description                                      |
 84 | | ------------------------- | --------------------------- | ------------------------------------------------------------------------------- | ------------------------------------------------ |
 85 | | `particleCount`           | `Integer`                   | `50`                                                                            | The number of confetti particles to launch       |
 86 | | `angle`                   | `Number`                    | `90`                                                                            | The angle in degrees at which to launch confetti |
 87 | | `spread`                  | `Number`                    | `45`                                                                            | The spread in degrees of the confetti            |
 88 | | `startVelocity`           | `Number`                    | `45`                                                                            | The initial velocity of the confetti             |
 89 | | `decay`                   | `Number`                    | `0.9`                                                                           | The rate at which confetti slows down            |
 90 | | `gravity`                 | `Number`                    | `1`                                                                             | The gravity applied to confetti particles        |
 91 | | `drift`                   | `Number`                    | `0`                                                                             | The horizontal drift applied to particles        |
 92 | | `flat`                    | `Boolean`                   | `false`                                                                         | Whether confetti particles are flat              |
 93 | | `ticks`                   | `Number`                    | `200`                                                                           | The number of frames confetti lasts              |
 94 | | `origin`                  | `Object`                    | `{ x: 0.5, y: 0.5 }`                                                            | The origin point of the confetti                 |
 95 | | `colors`                  | `Array of Strings`          | `['#26ccff', '#a25afd', '#ff5e7e', '#88ff5a', '#fcff42', '#ffa62d', '#ff36ff']` | Array of color strings in HEX format             |
 96 | | `shapes`                  | `Array of Strings`          | `['square', 'circle']`                                                          | Array of shapes for the confetti                 |
 97 | | `zIndex`                  | `Integer`                   | `100`                                                                           | The z-index of the confetti                      |
 98 | | `disableForReducedMotion` | `Boolean`                   | `false`                                                                         | Disables confetti for users who prefer no motion |
 99 | | `useWorker`               | `Boolean`                   | `true`                                                                          | Use Web Worker for better performance            |
100 | | `resize`                  | `Boolean`                   | `true`                                                                          | Whether to resize the canvas                     |
101 | | `canvas`                  | `HTMLCanvasElement or null` | `null`                                                                          | Custom canvas element to draw confetti           |
102 | | `scalar`                  | `Number`                    | `1`                                                                             | Scaling factor for confetti size                 |
103 | 
104 | ### ConfettiButton
105 | 
106 | | Prop       | Type              | Default | Description                          |
107 | | ---------- | ----------------- | ------- | ------------------------------------ |
108 | | `options`  | `Object`          | `{}`    | Options for the confetti             |
109 | | `children` | `React.ReactNode` | `null`  | Children to render inside the button |
110 | 
111 | ## Credits
112 | 
113 | - Credit to [Bankk](https://www.x.com/bankkroll_eth)
114 | - Inspired by [canvas-confetti](https://www.npmjs.com/package/canvas-confetti)
115 | 


--------------------------------------------------------------------------------
/content/docs/components/cool-mode.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Cool Mode
 3 | date: 2024-06-01
 4 | description: Cool mode effect for buttons, links, and other DOMs
 5 | author: Bankkroll
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="cool-mode-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/cool-mode"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="cool-mode" />
34 | 
35 | <Step>Update the import paths to match your project setup.</Step>
36 | 
37 | </Steps>
38 | 
39 | </TabsContent>
40 | 
41 | </Tabs>
42 | 
43 | ## Examples
44 | 
45 | ### Custom Particle
46 | 
47 | <ComponentPreview name="cool-mode-custom" />
48 | 
49 | ## Props
50 | 
51 | | Prop            | Type     | Default    | Description                            |
52 | | --------------- | -------- | ---------- | -------------------------------------- |
53 | | `particle`      | `String` | `"circle"` | The particle URL for a custom particle |
54 | | `size`          | `Number` | `Varies`   | Size of the particle                   |
55 | | `particleCount` | `Number` | `Varies`   | The number of particles to generate    |
56 | | `speedHorz`     | `Number` | `Varies`   | Horizontal speed of the particles      |
57 | | `speedUp`       | `Number` | `Varies`   | Upward speed of the particles          |
58 | 
59 | ## Credits
60 | 
61 | - Credit to [Bankk](https://www.x.com/bankkroll_eth)
62 | - Inspired by [ClickFusion](https://github.com/BankkRoll/ClickFusion)
63 | 


--------------------------------------------------------------------------------
/content/docs/components/dock.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Dock
 3 | date: 2024-04-25
 4 | description: An implementation of the MacOS dock using react + tailwindcss + framer motion
 5 | author: dillionverma
 6 | published: true
 7 | video: https://cdn.magicui.design/dock.mp4
 8 | ---
 9 | 
10 | <ComponentPreview name="dock-demo" />
11 | 
12 | ## Installation
13 | 
14 | <Tabs defaultValue="cli">
15 | 
16 | <TabsList>
17 |   <TabsTrigger value="cli">CLI</TabsTrigger>
18 |   <TabsTrigger value="manual">Manual</TabsTrigger>
19 | </TabsList>
20 | <TabsContent value="cli">
21 | 
22 | ```bash
23 | npx shadcn@latest add "https://magicui.design/r/dock"
24 | ```
25 | 
26 | </TabsContent>
27 | 
28 | <TabsContent value="manual">
29 | 
30 | <Steps>
31 | 
32 | <Step>Copy and paste the following code into your project.</Step>
33 | 
34 | <ComponentSource name="dock" />
35 | 
36 | <Step>Update the import paths to match your project setup.</Step>
37 | 
38 | </Steps>
39 | 
40 | </TabsContent>
41 | 
42 | </Tabs>
43 | 
44 | ## Examples
45 | 
46 | ### Custom Direction
47 | 
48 | <ComponentPreview name="dock-demo-2" />
49 | 
50 | ### Custom magnification
51 | 
52 | <ComponentPreview name="dock-demo-3" />
53 | 
54 | ## Props
55 | 
56 | ### Dock
57 | 
58 | | Prop                | Type        | Default    | Description                          |
59 | | ------------------- | ----------- | ---------- | ------------------------------------ |
60 | | `className`         | `string`    | `-`        | Custom CSS class for styling         |
61 | | `children`          | `ReactNode` | `-`        | Children elements                    |
62 | | `iconSize`          | `number`    | `40`       | Size of the icon                     |
63 | | `iconMagnification` | `number`    | `60`       | Level of icon magnification          |
64 | | `iconDistance`      | `number`    | `140`      | Distance from cursor to magnify icon |
65 | | `direction`         | `string`    | `"middle"` | Direction of the dock                |
66 | 
67 | ### DockIcon
68 | 
69 | | Prop            | Type                | Default | Description                          |
70 | | --------------- | ------------------- | ------- | ------------------------------------ |
71 | | `size`          | `number`            | `40`    | Size of the icon                     |
72 | | `magnification` | `number`            | `60`    | Level of icon magnification          |
73 | | `distance`      | `number`            | `140`   | Distance from cursor to magnify icon |
74 | | `mouseX`        | `any`               | `-`     | Mouse X position for magnification   |
75 | | `className`     | `string`            | `-`     | Custom CSS class for styling         |
76 | | `children`      | `React.ReactNode`   | `-`     | Children elements                    |
77 | | `props`         | `PropsWithChildren` | `-`     | Additional props                     |
78 | 
79 | ## Credits
80 | 
81 | - Credits to [Build UI](https://buildui.com/recipes/magnified-dock) for this fantastic component
82 | - Credits to [Ritesh Bucha](https://twitter.com/bucha_ritesh) for finding and improving it
83 | 


--------------------------------------------------------------------------------
/content/docs/components/dot-pattern.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Dot Pattern
 3 | date: 2023-07-20
 4 | description: A background dot pattern made with SVGs, fully customizable using Tailwind CSS.
 5 | author: dillionverma
 6 | published: true
 7 | video: https://cdn.magicui.design/dot-pattern.mp4
 8 | ---
 9 | 
10 | <ComponentPreview name="dot-pattern-demo" />
11 | 
12 | ## Installation
13 | 
14 | <Tabs defaultValue="cli">
15 | 
16 | <TabsList>
17 |   <TabsTrigger value="cli">CLI</TabsTrigger>
18 |   <TabsTrigger value="manual">Manual</TabsTrigger>
19 | </TabsList>
20 | <TabsContent value="cli">
21 | 
22 | ```bash
23 | npx shadcn@latest add "https://magicui.design/r/dot-pattern"
24 | ```
25 | 
26 | </TabsContent>
27 | 
28 | <TabsContent value="manual">
29 | 
30 | <Steps>
31 | 
32 | <Step>Copy and paste the following code into your project.</Step>
33 | 
34 | <ComponentSource name="dot-pattern" />
35 | 
36 | <Step>Update the import paths to match your project setup.</Step>
37 | 
38 | </Steps>
39 | 
40 | </TabsContent>
41 | 
42 | </Tabs>
43 | 
44 | ## Examples
45 | 
46 | ### Linear Gradient
47 | 
48 | <ComponentPreview name="dot-pattern-linear-gradient" />
49 | 
50 | ### With Glow Effect
51 | 
52 | <ComponentPreview name="dot-pattern-with-glow-effect" />
53 | 
54 | ## Props
55 | 
56 | ### Dot Pattern
57 | 
58 | | Prop        | Type      | Default | Description                                  |
59 | | ----------- | --------- | ------- | -------------------------------------------- |
60 | | `width`     | `any`     | `16`    | Width of the dot pattern                     |
61 | | `height`    | `any`     | `16`    | Height of the dot pattern                    |
62 | | `x`         | `any`     | `0`     | X position of the dot                        |
63 | | `y`         | `any`     | `0`     | Y position of the dot                        |
64 | | `cx`        | `any`     | `1`     | X position of the circle                     |
65 | | `cy`        | `any`     | `1`     | Y position of the circle                     |
66 | | `cr`        | `any`     | `1`     | Radius of the circle                         |
67 | | `className` | `string`  | `-`     | Class name of the dot pattern                |
68 | | `glow`      | `boolean` | `false` | Activates the glow effect in the dot pattern |
69 | 


--------------------------------------------------------------------------------
/content/docs/components/file-tree.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: File Tree
 3 | date: 2023-07-20
 4 | description: A component used to showcase the folder and file structure of a directory.
 5 | author: dillionverma
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="file-tree-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/file-tree"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="file-tree" />
34 | 
35 | <Step>Update the import paths to match your project setup.</Step>
36 | 
37 | </Steps>
38 | 
39 | </TabsContent>
40 | 
41 | </Tabs>
42 | 
43 | ## Props
44 | 
45 | ### Tree
46 | 
47 | | Prop                   | Type                | Default | Description                                                  |
48 | | ---------------------- | ------------------- | ------- | ------------------------------------------------------------ |
49 | | `initialSelectedId`    | `string`            | `-`     | The ID of the initially selected item.                       |
50 | | `indicator`            | `boolean`           | `true`  | Whether to show the tree indicator line.                     |
51 | | `elements`             | `TreeViewElement[]` | `-`     | An array of tree view elements to render.                    |
52 | | `initialExpandedItems` | `string[]`          | `-`     | An array of IDs for items that should be initially expanded. |
53 | | `openIcon`             | `React.ReactNode`   | `-`     | Custom icon for open folders.                                |
54 | | `closeIcon`            | `React.ReactNode`   | `-`     | Custom icon for closed folders.                              |
55 | | `dir`                  | `"rtl" \| "ltr"`    | `"ltr"` | The text direction of the tree.                              |
56 | 
57 | ### Folder
58 | 
59 | | Prop           | Type      | Default | Description                               |
60 | | -------------- | --------- | ------- | ----------------------------------------- |
61 | | `element`      | `string`  | `-`     | The name of the folder.                   |
62 | | `value`        | `string`  | `-`     | The unique identifier for the folder.     |
63 | | `isSelectable` | `boolean` | `true`  | Whether the folder can be selected.       |
64 | | `isSelect`     | `boolean` | `-`     | Whether the folder is currently selected. |
65 | 
66 | ### File
67 | 
68 | | Prop           | Type              | Default | Description                             |
69 | | -------------- | ----------------- | ------- | --------------------------------------- |
70 | | `value`        | `string`          | `-`     | The unique identifier for the file.     |
71 | | `isSelectable` | `boolean`         | `true`  | Whether the file can be selected.       |
72 | | `isSelect`     | `boolean`         | `-`     | Whether the file is currently selected. |
73 | | `fileIcon`     | `React.ReactNode` | `-`     | Custom icon for the file.               |
74 | 
75 | ### CollapseButton
76 | 
77 | | Prop        | Type                | Default | Description                                |
78 | | ----------- | ------------------- | ------- | ------------------------------------------ |
79 | | `elements`  | `TreeViewElement[]` | `-`     | An array of tree view elements to control. |
80 | | `expandAll` | `boolean`           | `false` | Whether to expand all elements initially.  |
81 | 


--------------------------------------------------------------------------------
/content/docs/components/flickering-grid.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Flickering Grid
 3 | date: 2024-08-15
 4 | description: A flickering grid background made with SVGs, fully customizable using Tailwind CSS.
 5 | author: dillionverma
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="flickering-grid-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/flickering-grid"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="flickering-grid" />
34 | 
35 | <Step>Update the import paths to match your project setup.</Step>
36 | 
37 | </Steps>
38 | 
39 | </TabsContent>
40 | 
41 | </Tabs>
42 | 
43 | ## Examples
44 | 
45 | ### Rounded
46 | 
47 | <ComponentPreview name="flickering-grid-rounded-demo" />
48 | 
49 | ## Props
50 | 
51 | | Prop            | Type     | Default          | Description                           |
52 | | --------------- | -------- | ---------------- | ------------------------------------- |
53 | | `squareSize`    | `number` | `4`              | Size of each square in the grid       |
54 | | `gridGap`       | `number` | `6`              | Gap between squares in the grid       |
55 | | `flickerChance` | `number` | `0.3`            | Probability of a square flickering    |
56 | | `color`         | `string` | `"rgb(0, 0, 0)"` | Color of the squares                  |
57 | | `width`         | `number` | `-`              | Width of the canvas                   |
58 | | `height`        | `number` | `-`              | Height of the canvas                  |
59 | | `className`     | `string` | `-`              | Additional CSS classes for the canvas |
60 | | `maxOpacity`    | `number` | `0.2`            | Maximum opacity of the squares        |
61 | 


--------------------------------------------------------------------------------
/content/docs/components/flip-text.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Flip Text
 3 | date: 2024-05-23
 4 | description: Text flipping character animation
 5 | author: wellitsabhi
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="flip-text-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/flip-text"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="flip-text" />
34 | 
35 | <Step>Update the import paths to match your project setup.</Step>
36 | 
37 | </Steps>
38 | 
39 | </TabsContent>
40 | 
41 | </Tabs>
42 | 
43 | ## Props
44 | 
45 | | Prop            | Type              | Default    | Description                                   |
46 | | --------------- | ----------------- | ---------- | --------------------------------------------- |
47 | | `className`     | `string`          | `-`        | The class name to be applied to the component |
48 | | `duration`      | `number`          | `0.5`      | Duration of the animation                     |
49 | | `delayMultiple` | `number`          | `0.08`     | Transition delay multiplier.                  |
50 | | `variants`      | `Variants`        | `{}`       | An object containing motion variants          |
51 | | `as`            | `ElementType`     | `span`     | The element type of the component             |
52 | | `children`      | `React.ReactNode` | `required` | The children of the component                 |
53 | 


--------------------------------------------------------------------------------
/content/docs/components/globe.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Globe
 3 | date: 2023-08-02
 4 | description: An autorotating, interactive, and highly performant globe made using WebGL.
 5 | author: dillionverma
 6 | published: true
 7 | video: https://cdn.magicui.design/globe.mp4
 8 | ---
 9 | 
10 | <ComponentPreview name="globe-demo" />
11 | 
12 | ## Installation
13 | 
14 | <Tabs defaultValue="cli">
15 | 
16 | <TabsList>
17 |   <TabsTrigger value="cli">CLI</TabsTrigger>
18 |   <TabsTrigger value="manual">Manual</TabsTrigger>
19 | </TabsList>
20 | <TabsContent value="cli">
21 | 
22 | ```bash
23 | npx shadcn@latest add "https://magicui.design/r/globe"
24 | ```
25 | 
26 | </TabsContent>
27 | 
28 | <TabsContent value="manual">
29 | 
30 | <Steps>
31 | 
32 | <Step>Install the following dependencies:</Step>
33 | 
34 | ```bash
35 | npm install cobe
36 | ```
37 | 
38 | <Step>Copy and paste the following code into your project.</Step>
39 | 
40 | <ComponentSource name="globe" />
41 | 
42 | <Step>Update the import paths to match your project setup.</Step>
43 | 
44 | </Steps>
45 | 
46 | </TabsContent>
47 | 
48 | </Tabs>
49 | 
50 | ## Props
51 | 
52 | | Prop        | Type          | Default | Description                                                                                    |
53 | | ----------- | ------------- | ------- | ---------------------------------------------------------------------------------------------- |
54 | | `className` | `string`      | `-`     | The css classes for the component                                                              |
55 | | `config`    | `COBEOptions` | `{}`    | The configuration options for the globe. More details [here](https://cobe.vercel.app/docs/api) |
56 | 
57 | ## Credits
58 | 
59 | This component is built on top of [Cobe](https://cobe.vercel.app/docs/api).
60 | 


--------------------------------------------------------------------------------
/content/docs/components/grid-pattern.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Grid Pattern
 3 | date: 2023-07-18
 4 | description: A background grid pattern made with SVGs, fully customizable using Tailwind CSS.
 5 | author: dillionverma
 6 | published: true
 7 | video: https://cdn.magicui.design/grid-pattern.mp4
 8 | ---
 9 | 
10 | <ComponentPreview name="grid-pattern-demo" />
11 | 
12 | ## Installation
13 | 
14 | <Tabs defaultValue="cli">
15 | 
16 | <TabsList>
17 |   <TabsTrigger value="cli">CLI</TabsTrigger>
18 |   <TabsTrigger value="manual">Manual</TabsTrigger>
19 | </TabsList>
20 | <TabsContent value="cli">
21 | 
22 | ```bash
23 | npx shadcn@latest add "https://magicui.design/r/grid-pattern"
24 | ```
25 | 
26 | </TabsContent>
27 | 
28 | <TabsContent value="manual">
29 | 
30 | <Steps>
31 | 
32 | <Step>Copy and paste the following code into your project.</Step>
33 | 
34 | <ComponentSource name="grid-pattern" />
35 | 
36 | <Step>Update the import paths to match your project setup.</Step>
37 | 
38 | </Steps>
39 | 
40 | </TabsContent>
41 | 
42 | </Tabs>
43 | 
44 | ## Examples
45 | 
46 | ### Linear Gradient
47 | 
48 | <ComponentPreview name="grid-pattern-linear-gradient" />
49 | 
50 | ### Dashed Stroke
51 | 
52 | <ComponentPreview name="grid-pattern-dashed" />
53 | 
54 | ## Props
55 | 
56 | ### GridPattern
57 | 
58 | | Prop              | Type     | Default | Description                                   |
59 | | ----------------- | -------- | ------- | --------------------------------------------- |
60 | | `width`           | `number` | `40`    | Width of the pattern                          |
61 | | `height`          | `number` | `40`    | Height of the pattern                         |
62 | | `x`               | `number` | `-1`    | X offset of the pattern                       |
63 | | `y`               | `number` | `-1`    | Y offset of the pattern                       |
64 | | `squares`         | `number` | `[]`    | X Y coordinates of filled squares as 2D array |
65 | | `strokeDasharray` | `string` | `0`     | Stroke dash array for the pattern             |
66 | 


--------------------------------------------------------------------------------
/content/docs/components/hero-video-dialog.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Hero Video Dialog
 3 | date: 2024-08-26
 4 | description: A hero video dialog component.
 5 | author: dillionverma
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="hero-video-dialog-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/hero-video-dialog"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="hero-video-dialog" />
34 | 
35 | <Step>Update the import paths to match your project setup.</Step>
36 | 
37 | </Steps>
38 | 
39 | </TabsContent>
40 | 
41 | </Tabs>
42 | 
43 | ## Examples
44 | 
45 | ### Top-in-bottom-out
46 | 
47 | <ComponentPreview name="hero-video-dialog-demo-top-in-bottom-out" />
48 | 
49 | ## Props
50 | 
51 | | Prop             | Type     | Default             | Description                      |
52 | | ---------------- | -------- | ------------------- | -------------------------------- |
53 | | `animationStyle` | `string` | `"from-center"`     | Animation style for the dialog   |
54 | | `videoSrc`       | `string` | `-`                 | URL of the video to be played    |
55 | | `thumbnailSrc`   | `string` | `-`                 | URL of the thumbnail image       |
56 | | `thumbnailAlt`   | `string` | `"Video thumbnail"` | Alt text for the thumbnail image |
57 | 
58 | ## Animation Styles
59 | 
60 | The `animationStyle` prop accepts the following values:
61 | 
62 | - `"from-bottom"`: Dialog enters from the bottom and exits to the bottom
63 | - `"from-center"`: Dialog scales up from the center and scales down to the center
64 | - `"from-top"`: Dialog enters from the top and exits to the top
65 | - `"from-left"`: Dialog enters from the left and exits to the left
66 | - `"from-right"`: Dialog enters from the right and exits to the right
67 | - `"fade"`: Dialog fades in and out
68 | - `"top-in-bottom-out"`: Dialog enters from the top and exits to the bottom
69 | - `"left-in-right-out"`: Dialog enters from the left and exits to the right
70 | 
71 | ## Note
72 | 
73 | - If using a YouTube video, make sure to use the `embed` version of the video URL.
74 | 


--------------------------------------------------------------------------------
/content/docs/components/hyper-text.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Hyper Text
 3 | date: 2024-08-03
 4 | description: A text animation that scrambles letters before revealing the final text.
 5 | author: SwayambhuPrasad
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="hyper-text-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/hyper-text"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Install the following dependencies:</Step>
32 | 
33 | ```bash
34 | npm install motion
35 | ```
36 | 
37 | <Step>Copy and paste the following code into your project.</Step>
38 | 
39 | <ComponentSource name="hyper-text" />
40 | 
41 | <Step>Update the import paths to match your project setup.</Step>
42 | 
43 | </Steps>
44 | 
45 | </TabsContent>
46 | 
47 | </Tabs>
48 | 
49 | ## Props
50 | 
51 | | Prop             | Type                | Default | Description                                   |
52 | | ---------------- | ------------------- | ------- | --------------------------------------------- |
53 | | `children`       | `string`            | `-`     | Text content to animate                       |
54 | | `className`      | `string`            | `-`     | The class name to be applied to the component |
55 | | `duration`       | `number`            | `800`   | Duration of the animation in milliseconds     |
56 | | `delay`          | `number`            | `0`     | Delay before animation starts (in ms)         |
57 | | `as`             | `React.ElementType` | `"div"` | Component to render as                        |
58 | | `startOnView`    | `boolean`           | `false` | Start animation when component is in view     |
59 | | `animateOnHover` | `boolean`           | `true`  | Enable hover animation                        |
60 | | `characterSet`   | `string[]`          | `A-Z`   | Custom character set for scramble effect      |
61 | 


--------------------------------------------------------------------------------
/content/docs/components/icon-cloud.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Icon Cloud
 3 | date: 2024-05-24
 4 | description: An interactive 3D tag cloud component
 5 | author: dillionverma
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="icon-cloud-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/icon-cloud"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="icon-cloud" />
34 | 
35 | </Steps>
36 | 
37 | </TabsContent>
38 | 
39 | </Tabs>
40 | 
41 | ## With Images
42 | 
43 | <ComponentPreview name="icon-cloud-demo-2" />
44 | 
45 | ## With SVG Icons
46 | 
47 | <ComponentPreview name="icon-cloud-demo-3" />
48 | 
49 | ## Props
50 | 
51 | | Prop     | Type                | Default | Description                                |
52 | | -------- | ------------------- | ------- | ------------------------------------------ |
53 | | `icons`  | `React.ReactNode[]` | `[]`    | Array of icons to render in the cloud      |
54 | | `images` | `string[]`          | `[]`    | Array of image URLs to render in the cloud |
55 | 


--------------------------------------------------------------------------------
/content/docs/components/interactive-grid-pattern.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Interactive Grid Pattern
 3 | date: 2024-12-31
 4 | description: A interactive background grid pattern made with SVGs, fully customizable using Tailwind CSS.
 5 | author: h3rmel
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="interactive-grid-pattern-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/interactive-grid-pattern"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="interactive-grid-pattern" />
34 | 
35 | <Step>Update the import paths to match your project setup.</Step>
36 | 
37 | </Steps>
38 | 
39 | </TabsContent>
40 | 
41 | </Tabs>
42 | 
43 | ## Examples
44 | 
45 | ### Colorful
46 | 
47 | <ComponentPreview name="interactive-grid-pattern-demo-2" />
48 | 
49 | ## Props
50 | 
51 | | Prop               | Type               | Description                                                                                   | Default   |
52 | | ------------------ | ------------------ | --------------------------------------------------------------------------------------------- | --------- |
53 | | `width`            | `number`           | Width of each square in the grid                                                              | `40`      |
54 | | `height`           | `number`           | Height of each square in the grid                                                             | `40`      |
55 | | `squares`          | `[number, number]` | Number of squares in the grid. First number is horizontal squares, second is vertical squares | `[24,24]` |
56 | | `className`        | `string`           | Class name applied to the grid container                                                      | -         |
57 | | `squaresClassName` | `string`           | Class name applied to individual squares in the grid                                          | -         |
58 | 


--------------------------------------------------------------------------------
/content/docs/components/interactive-hover-button.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Interactive Hover Button
 3 | date: 2024-12-17
 4 | description: A visually engaging button component that responds to hover with dynamic transitions, adapting smoothly between light and dark modes for enhanced user interactivity.
 5 | author: aayushbharti
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="interactive-hover-button-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/interactive-hover-button"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="interactive-hover-button" />
34 | 
35 | <Step>Update the import paths to match your project setup.</Step>
36 | 
37 | </Steps>
38 | 
39 | </TabsContent>
40 | 
41 | </Tabs>
42 | 
43 | ## Props
44 | 
45 | | Prop        | Type     | Default  | Description                                   |
46 | | ----------- | -------- | -------- | --------------------------------------------- |
47 | | `text`      | `string` | `Button` | The text to be displayed inside the button    |
48 | | `className` | `string` | `-`      | Additional class names to style the component |
49 | 
50 | ## Credits
51 | 
52 | - Credit to [@AayushBharti](https://github.com/AayushBharti)
53 | 


--------------------------------------------------------------------------------
/content/docs/components/iphone-15-pro.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: iPhone 15 Pro
 3 | date: 2024-09-01
 4 | description: A mockup of the iPhone 15 Pro
 5 | author: dillionverma
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="iphone-15-pro-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/iphone-15-pro"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="iphone-15-pro" />
34 | 
35 | <Step>Update the import paths to match your project setup.</Step>
36 | 
37 | </Steps>
38 | 
39 | </TabsContent>
40 | 
41 | </Tabs>
42 | 
43 | ## Examples
44 | 
45 | ### With Image
46 | 
47 | <ComponentPreview name="iphone-15-pro-demo-2" />
48 | 
49 | ### With Video
50 | 
51 | <ComponentPreview name="iphone-15-pro-demo-3" />
52 | 
53 | ## Props
54 | 
55 | | Prop       | Type     | Default | Description                            |
56 | | ---------- | -------- | ------- | -------------------------------------- |
57 | | `width`    | `number` | `433`   | The width of the iPhone 15 Pro window  |
58 | | `height`   | `number` | `882`   | The height of the iPhone 15 Pro window |
59 | | `src`      | `string` | `-`     | The source of the image to display     |
60 | | `videoSrc` | `string` | `-`     | The source of the video to display     |
61 | 
62 | The `Iphone15Pro` component also accepts all properties of the `SVGElement` type.
63 | 


--------------------------------------------------------------------------------
/content/docs/components/lens.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Lens
 3 | date: 2025-01-13
 4 | description: A interactive component that enables zooming into images, videos and other elements.
 5 | author: h3rmel
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="lens-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/lens"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="lens" />
34 | 
35 | <Step>Update the import paths to match your project setup.</Step>
36 | 
37 | </Steps>
38 | 
39 | </TabsContent>
40 | 
41 | </Tabs>
42 | 
43 | ## Examples
44 | 
45 | ### Static Lens
46 | 
47 | <ComponentPreview name="lens-demo-2" />
48 | 
49 | ### Lens with a Default Position
50 | 
51 | <ComponentPreview name="lens-demo-3" />
52 | 
53 | ## Props
54 | 
55 | | Property          | Type              | Default | Description                                                |
56 | | ----------------- | ----------------- | ------- | ---------------------------------------------------------- |
57 | | `children`        | `React.ReactNode` | -       | The content that will be magnified by the lens             |
58 | | `zoomFactor`      | `number`          | 1.3     | The magnification factor of the lens                       |
59 | | `lensSize`        | `number`          | 170     | The size of the lens in pixels (works as a diameter)       |
60 | | `position`        | `Position`        | -       | The current position of the lens                           |
61 | | `defaultPosition` | `Position`        | -       | The initial position of the lens                           |
62 | | `isStatic`        | `boolean`         | false   | Determines if the lens will remain in a fixed position     |
63 | | `duration`        | `number`          | 0.1     | Duration of the animation when the lens moves (in seconds) |
64 | | `lensColor`       | `string`          | -       | The color of the lens (CSS color value)                    |
65 | | `ariaLabel`       | `string`          | -       | Accessibility label for the lens component                 |
66 | 


--------------------------------------------------------------------------------
/content/docs/components/line-shadow-text.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Line Shadow Text
 3 | date: 2025-01-11
 4 | description: A text component with a moving line shadow.
 5 | author: magicui
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="line-shadow-text-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/line-shadow-text"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="line-shadow-text" />
34 | 
35 | <Step>Update the import paths to match your project setup.</Step>
36 | 
37 | <Step>Update `tailwind.config.js`</Step>
38 | 
39 | Add the following animations to your `tailwind.config.js` file:
40 | 
41 | ```js title="tailwind.config.js" {5-13}
42 | /** @type {import('tailwindcss').Config} */
43 | module.exports = {
44 |   theme: {
45 |     extend: {
46 |       animation: {
47 |         "line-shadow": "line-shadow 15s linear infinite",
48 |       },
49 |       keyframes: {
50 |         "line-shadow": {
51 |           "0%": { "background-position": "0 0" },
52 |           "100%": { "background-position": "100% -100%" },
53 |         },
54 |       },
55 |     },
56 |   },
57 | };
58 | ```
59 | 
60 | </Steps>
61 | 
62 | </TabsContent>
63 | 
64 | </Tabs>
65 | 
66 | ## Props
67 | 
68 | | Prop        | Type   | Default   | Description                            |
69 | | ----------- | ------ | --------- | -------------------------------------- |
70 | | shadowColor | string | `"black"` | The color of the shadow effect         |
71 | | children    | string | -         | The text to display with shadow effect |
72 | | as          | string | `"span"`  | The HTML element to render the text as |
73 | 


--------------------------------------------------------------------------------
/content/docs/components/magic-card.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Magic Card
 3 | date: 2024-07-07
 4 | description: A spotlight effect that follows your mouse cursor and highlights borders on hover.
 5 | author: dillionverma
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="magic-card-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/magic-card"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="magic-card" />
34 | 
35 | <Step>Update the import paths to match your project setup.</Step>
36 | 
37 | </Steps>
38 | 
39 | </TabsContent>
40 | 
41 | </Tabs>
42 | 
43 | ## Usage
44 | 
45 | ```tsx
46 | import { MagicCard } from "@/registry/magicui/magic-card";
47 | ```
48 | 
49 | ```tsx
50 | <MagicCard>Hello World</MagicCard>
51 | ```
52 | 
53 | ## Props
54 | 
55 | ### MagicCard
56 | 
57 | | Prop name         | Type              | Default   | Description                                 |
58 | | ----------------- | ----------------- | --------- | ------------------------------------------- |
59 | | `children`        | `React.ReactNode` | `-`       | The content to be rendered inside the card  |
60 | | `className`       | `string`          | `-`       | Additional CSS classes to apply to the card |
61 | | `gradientSize`    | `number`          | `200`     | Size of the gradient effect                 |
62 | | `gradientColor`   | `string`          | `#262626` | Color of the gradient effect                |
63 | | `gradientOpacity` | `number`          | `0.8`     | Opacity of the gradient effect              |
64 | | `gradientFrom`    | `string`          | `#9E7AFF` | Start color of the gradient border          |
65 | | `gradientTo`      | `string`          | `#FE8BBB` | End color of the gradient border            |
66 | 


--------------------------------------------------------------------------------
/content/docs/components/marquee.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Marquee
 3 | date: 2023-07-26
 4 | description: An infinite scrolling component that can be used to display text, images, or videos.
 5 | author: dillionverma
 6 | published: true
 7 | video: https://cdn.magicui.design/marquee.mp4
 8 | ---
 9 | 
10 | <ComponentPreview name="marquee-demo" />
11 | 
12 | ## Installation
13 | 
14 | <Tabs defaultValue="cli">
15 | 
16 | <TabsList>
17 |   <TabsTrigger value="cli">CLI</TabsTrigger>
18 |   <TabsTrigger value="manual">Manual</TabsTrigger>
19 | </TabsList>
20 | <TabsContent value="cli">
21 | 
22 | ```bash
23 | npx shadcn@latest add "https://magicui.design/r/marquee"
24 | ```
25 | 
26 | </TabsContent>
27 | 
28 | <TabsContent value="manual">
29 | 
30 | <Steps>
31 | 
32 | <Step>Copy and paste the following code into your project.</Step>
33 | 
34 | <ComponentSource name="marquee" />
35 | 
36 | <Step>Update the import paths to match your project setup.</Step>
37 | 
38 | <Step>Update `tailwind.config.js`</Step>
39 | 
40 | Add the following animations to your `tailwind.config.js` file:
41 | 
42 | ```js title="tailwind.config.js" {5-18}
43 | /** @type {import('tailwindcss').Config} */
44 | module.exports = {
45 |   theme: {
46 |     extend: {
47 |       animation: {
48 |         marquee: "marquee var(--duration) linear infinite",
49 |         "marquee-vertical": "marquee-vertical var(--duration) linear infinite",
50 |       },
51 |       keyframes: {
52 |         marquee: {
53 |           from: { transform: "translateX(0)" },
54 |           to: { transform: "translateX(calc(-100% - var(--gap)))" },
55 |         },
56 |         "marquee-vertical": {
57 |           from: { transform: "translateY(0)" },
58 |           to: { transform: "translateY(calc(-100% - var(--gap)))" },
59 |         },
60 |       },
61 |     },
62 |   },
63 | };
64 | ```
65 | 
66 | </Steps>
67 | 
68 | </TabsContent>
69 | 
70 | </Tabs>
71 | 
72 | ## Examples
73 | 
74 | ### Vertical
75 | 
76 | <ComponentPreview name="marquee-demo-vertical" />
77 | 
78 | ### 3D
79 | 
80 | <ComponentPreview name="marquee-3d" />
81 | 
82 | ## Props
83 | 
84 | | Prop           | Type      | Default | Description                                                                  |
85 | | -------------- | --------- | ------- | ---------------------------------------------------------------------------- |
86 | | `className`    | `string`  | `-`     | The class name to apply to the component.                                    |
87 | | `reverse`      | `boolean` | `false` | Whether or not to reverse the direction of the marquee.                      |
88 | | `pauseOnHover` | `boolean` | `false` | Whether or not to pause the marquee when the user hovers over the component. |
89 | | `vertical`     | `boolean` | `false` | Whether or not to display the marquee vertically.                            |
90 | | `children`     | `node`    | `-`     | The content to display in the marquee.                                       |
91 | | `repeat`       | `number`  | `1`     | The number of times to repeat the content.                                   |
92 | 


--------------------------------------------------------------------------------
/content/docs/components/meteors.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Meteors
 3 | date: 2023-07-13
 4 | description: A meteor shower effect.
 5 | author: dillionverma
 6 | published: true
 7 | video: https://cdn.magicui.design/meteors.mp4
 8 | ---
 9 | 
10 | <ComponentPreview name="meteors-demo" />
11 | 
12 | ## Installation
13 | 
14 | <Tabs defaultValue="cli">
15 | 
16 | <TabsList>
17 |   <TabsTrigger value="cli">CLI</TabsTrigger>
18 |   <TabsTrigger value="manual">Manual</TabsTrigger>
19 | </TabsList>
20 | <TabsContent value="cli">
21 | 
22 | ```bash
23 | npx shadcn@latest add "https://magicui.design/r/meteors"
24 | ```
25 | 
26 | </TabsContent>
27 | 
28 | <TabsContent value="manual">
29 | 
30 | <Steps>
31 | 
32 | <Step>Copy and paste the following code into your project.</Step>
33 | 
34 | <ComponentSource name="meteors" />
35 | 
36 | <Step>Update the import paths to match your project setup.</Step>
37 | 
38 | <Step>Update `tailwind.config.js`</Step>
39 | 
40 | Add the following animations to your `tailwind.config.js` file:
41 | 
42 | ```js title="tailwind.config.js" {5-17}
43 | /** @type {import('tailwindcss').Config} */
44 | module.exports = {
45 |   theme: {
46 |     extend: {
47 |       animation: {
48 |         meteor: "meteor 5s linear infinite",
49 |       },
50 |       keyframes: {
51 |         meteor: {
52 |           "0%": {
53 |             transform: "rotate(var(--angle)) translateX(0)",
54 |             opacity: "1",
55 |           },
56 |           "70%": { opacity: "1" },
57 |           "100%": {
58 |             transform: "rotate(var(--angle)) translateX(-500px)",
59 |             opacity: "0",
60 |           },
61 |         },
62 |       },
63 |     },
64 |   },
65 | };
66 | ```
67 | 
68 | </Steps>
69 | 
70 | </TabsContent>
71 | 
72 | </Tabs>
73 | 
74 | ## Props
75 | 
76 | ### Meteors
77 | 
78 | | Prop          | Type     | Default | Description                                             |
79 | | ------------- | -------- | ------- | ------------------------------------------------------- |
80 | | `number`      | `number` | `20`    | Number of meteors                                       |
81 | | `minDelay`    | `number` | `0.2`   | Minimum delay in seconds before meteor animation starts |
82 | | `maxDelay`    | `number` | `1.2`   | Maximum delay in seconds before meteor animation starts |
83 | | `minDuration` | `number` | `2`     | Minimum duration in seconds for meteor animation        |
84 | | `maxDuration` | `number` | `10`    | Maximum duration in seconds for meteor animation        |
85 | | `angle`       | `number` | `215`   | Angle in degrees for meteor trajectory                  |
86 | | `className`   | `string` | -       | Optional additional CSS classes                         |
87 | 


--------------------------------------------------------------------------------
/content/docs/components/morphing-text.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Morphing Text
 3 | date: 2024-09-02
 4 | description: A dynamic text morphing component for Magic UI.
 5 | author: magicui
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="morphing-text-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/morphing-text"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="morphing-text" />
34 | 
35 | </Steps>
36 | 
37 | </TabsContent>
38 | 
39 | </Tabs>
40 | 
41 | ## Props
42 | 
43 | | Prop        | Type       | Default | Description                          |
44 | | ----------- | ---------- | ------- | ------------------------------------ |
45 | | `texts`     | `string[]` | `[]`    | Array of texts to morph between      |
46 | | `className` | `string?`  | `""`    | Additional classes for the container |
47 | 
48 | This `MorphingText` component dynamically transitions between an array of text strings, creating a smooth, engaging visual effect.
49 | 
50 | ## Credits
51 | 
52 | - Credit to [@luis-code](https://luis-code.vercel.app/)
53 | 


--------------------------------------------------------------------------------
/content/docs/components/neon-gradient-card.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Neon Gradient Card
 3 | date: 2024-05-26
 4 | description: A beautiful neon card effect
 5 | author: Draxx0
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="neon-gradient-card-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/neon-gradient-card"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="neon-gradient-card" />
34 | 
35 | <Step>Update the import paths to match your project setup.</Step>
36 | 
37 | <Step>Update `tailwind.config.js`</Step>
38 | 
39 | Add the following animations to your `tailwind.config.js` file:
40 | 
41 | ```js title="tailwind.config.js" {5-14}
42 | /** @type {import('tailwindcss').Config} */
43 | module.exports = {
44 |   theme: {
45 |     extend: {
46 |       animation: {
47 |         "background-position-spin":
48 |           "background-position-spin 3000ms infinite alternate",
49 |       },
50 |       keyframes: {
51 |         "background-position-spin": {
52 |           "0%": { backgroundPosition: "top center" },
53 |           "100%": { backgroundPosition: "bottom center" },
54 |         },
55 |       },
56 |     },
57 |   },
58 | };
59 | ```
60 | 
61 | </Steps>
62 | 
63 | </TabsContent>
64 | 
65 | </Tabs>
66 | 
67 | ## Props
68 | 
69 | | Prop           | Type        | Default                                             | Description                                   |
70 | | -------------- | ----------- | --------------------------------------------------- | --------------------------------------------- |
71 | | `className`    | `string`    | `-`                                                 | The class name to be applied to the component |
72 | | `children`     | `ReactNode` | `-`                                                 | Children elements                             |
73 | | `borderSize`   | `number`    | `5`                                                 | The size of the border                        |
74 | | `borderRadius` | `number`    | `20`                                                | The size of the radius                        |
75 | | `neonColors`   | `object`    | `{ firstColor: "#ff00aa", secondColor: "#00FFF1" }` | The colors of the neon gradient               |
76 | 
77 | ## Credits
78 | 
79 | - Credit to [@Unleashed-Design](https://unleashed-design.de/)
80 | 


--------------------------------------------------------------------------------
/content/docs/components/number-ticker.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Number Ticker
 3 | date: 2023-11-18
 4 | description: Animate numbers to count up or down to a target number
 5 | author: dillionverma
 6 | published: true
 7 | video: https://cdn.magicui.design/number-ticker.mp4
 8 | ---
 9 | 
10 | <ComponentPreview name="number-ticker-demo" />
11 | 
12 | ## Installation
13 | 
14 | <Tabs defaultValue="cli">
15 | 
16 | <TabsList>
17 |   <TabsTrigger value="cli">CLI</TabsTrigger>
18 |   <TabsTrigger value="manual">Manual</TabsTrigger>
19 | </TabsList>
20 | <TabsContent value="cli">
21 | 
22 | ```bash
23 | npx shadcn@latest add "https://magicui.design/r/number-ticker"
24 | ```
25 | 
26 | </TabsContent>
27 | 
28 | <TabsContent value="manual">
29 | 
30 | <Steps>
31 | 
32 | <Step>Copy and paste the following code into your project.</Step>
33 | 
34 | <ComponentSource name="number-ticker" />
35 | 
36 | <Step>Update the import paths to match your project setup.</Step>
37 | 
38 | </Steps>
39 | 
40 | </TabsContent>
41 | 
42 | </Tabs>
43 | 
44 | ## Example
45 | 
46 | ### Decimal
47 | 
48 | <ComponentPreview name="number-ticker-decimal-demo" />
49 | 
50 | ### Start Value
51 | 
52 | <ComponentPreview name="number-ticker-demo-2" />
53 | 
54 | ## Props
55 | 
56 | | Prop            | Type         | Default | Description                          |
57 | | --------------- | ------------ | ------- | ------------------------------------ |
58 | | `value`         | `int`        | `0`     | The value to count to                |
59 | | `direction`     | `up \| down` | `"up"`  | The direction to count in            |
60 | | `delay`         | `number`     | `0`     | The delay before counting            |
61 | | `decimalPlaces` | `number`     | `0`     | The number of decimal places to show |
62 | | `startValue`    | `number`     | `0`     | The value to start counting from     |
63 | 


--------------------------------------------------------------------------------
/content/docs/components/orbiting-circles.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Orbiting Circles
 3 | date: 2024-04-24
 4 | description: A collection of circles which move in orbit along a circular path
 5 | author: dillionverma
 6 | published: true
 7 | video: https://cdn.magicui.design/orbiting-circles.mp4
 8 | ---
 9 | 
10 | <ComponentPreview name="orbiting-circles-demo" />
11 | 
12 | ## Installation
13 | 
14 | <Tabs defaultValue="cli">
15 | 
16 | <TabsList>
17 |   <TabsTrigger value="cli">CLI</TabsTrigger>
18 |   <TabsTrigger value="manual">Manual</TabsTrigger>
19 | </TabsList>
20 | <TabsContent value="cli">
21 | 
22 | ```bash
23 | npx shadcn@latest add "https://magicui.design/r/orbiting-circles"
24 | ```
25 | 
26 | </TabsContent>
27 | 
28 | <TabsContent value="manual">
29 | 
30 | <Steps>
31 | 
32 | <Step>Copy and paste the following code into your project.</Step>
33 | 
34 | <ComponentSource name="orbiting-circles" />
35 | 
36 | <Step>Update the import paths to match your project setup.</Step>
37 | 
38 | <Step>Update `tailwind.config.js`</Step>
39 | 
40 | Add the following animations to your `tailwind.config.js` file:
41 | 
42 | ```js title="tailwind.config.js" {5-19}
43 | /** @type {import('tailwindcss').Config} */
44 | module.exports = {
45 |   theme: {
46 |     extend: {
47 |       animation: {
48 |         orbit: "orbit calc(var(--duration)*1s) linear infinite",
49 |       },
50 |       keyframes: {
51 |         orbit: {
52 |           "0%": {
53 |             transform:
54 |               "rotate(calc(var(--angle) * 1deg)) translateY(calc(var(--radius) * 1px)) rotate(calc(var(--angle) * -1deg))",
55 |           },
56 |           "100%": {
57 |             transform:
58 |               "rotate(calc(var(--angle) * 1deg + 360deg)) translateY(calc(var(--radius) * 1px)) rotate(calc((var(--angle) * -1deg) - 360deg))",
59 |           },
60 |         },
61 |       },
62 |     },
63 |   },
64 | };
65 | ```
66 | 
67 | </Steps>
68 | 
69 | </TabsContent>
70 | 
71 | </Tabs>
72 | 
73 | ## Props
74 | 
75 | | Prop        | Type              | Default | Description                                      |
76 | | ----------- | ----------------- | ------- | ------------------------------------------------ |
77 | | `className` | `string`          | `-`     | The class name for the component                 |
78 | | `children`  | `React.ReactNode` | `-`     | The children nodes of the component              |
79 | | `reverse`   | `boolean`         | `false` | If true, the animation plays in reverse          |
80 | | `duration`  | `number`          | `20`    | The duration of the animation in seconds         |
81 | | `delay`     | `number`          | `10`    | The delay before the animation starts in seconds |
82 | | `radius`    | `number`          | `160`   | The radius of the orbit in pixels                |
83 | | `path`      | `boolean`         | `true`  | If true, a path is displayed for the orbit       |
84 | | `iconSize`  | `number`          | `30`    | The size of the icon in pixels                   |
85 | | `speed`     | `number`          | `1`     | The speed of the animation                       |
86 | 


--------------------------------------------------------------------------------
/content/docs/components/particles.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Particles
 3 | date: 2024-06-04
 4 | description: Particles are a fun way to add some visual flair to your website. They can be used to create a sense of depth, movement, and interactivity.
 5 | author: dillionverma
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="particles-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/particles"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="particles" />
34 | 
35 | <Step>Update the import paths to match your project setup.</Step>
36 | 
37 | </Steps>
38 | 
39 | </TabsContent>
40 | 
41 | </Tabs>
42 | 
43 | ## Props
44 | 
45 | | Prop        | Type      | Default   | Description                      |
46 | | ----------- | --------- | --------- | -------------------------------- |
47 | | `className` | `string`  | `-`       | The class name for the component |
48 | | `quantity`  | `number`  | `100`     | The number of particles          |
49 | | `staticity` | `number`  | `50`      | The staticity of the particles   |
50 | | `ease`      | `number`  | `50`      | The ease of the particles        |
51 | | `size`      | `number`  | `0.4`     | The size of the particles        |
52 | | `refresh`   | `boolean` | `false`   | Whether to refresh the particles |
53 | | `color`     | `string`  | `#ffffff` | The color of the particles       |
54 | | `vx`        | `number`  | `0`       | The x velocity of the particles  |
55 | | `vy`        | `number`  | `0`       | The y velocity of the particles  |
56 | 


--------------------------------------------------------------------------------
/content/docs/components/pointer.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Pointer
 3 | date: 2025-02-17
 4 | description: A component that displays a pointer when hovering over an element
 5 | author: h3rmel
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="pointer-demo-1" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/pointer"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="pointer" />
34 | 
35 | <Step>Update the import paths to match your project setup.</Step>
36 | 
37 | </Steps>
38 | 
39 | </TabsContent>
40 | 
41 | </Tabs>
42 | 
43 | ## Usage
44 | 
45 | ```tsx
46 | import { Pointer } from "@/components/magicui/pointer";
47 | ```
48 | 
49 | ```tsx
50 | <Pointer />
51 | ```
52 | 


--------------------------------------------------------------------------------
/content/docs/components/pulsating-button.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Pulsating Button
 3 | date: 2024-07-23
 4 | description: An animated pulsating button useful for capturing attention of users.
 5 | author: shikhap04
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="pulsating-button-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/pulsating-button"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="pulsating-button" />
34 | 
35 | <Step>Update the import paths to match your project setup.</Step>
36 | 
37 | <Step>Update `tailwind.config.js`</Step>
38 | 
39 | Add the following animations to your `tailwind.config.js` file:
40 | 
41 | ```js title="tailwind.config.js" {5-13}
42 | /** @type {import('tailwindcss').Config} */
43 | module.exports = {
44 |   theme: {
45 |     extend: {
46 |       animation: {
47 |         pulse: "pulse var(--duration) ease-out infinite",
48 |       },
49 |       keyframes: {
50 |         pulse: {
51 |           "0%, 100%": { boxShadow: "0 0 0 0 var(--pulse-color)" },
52 |           "50%": { boxShadow: "0 0 0 8px var(--pulse-color)" },
53 |         },
54 |       },
55 |     },
56 |   },
57 | };
58 | ```
59 | 
60 | </Steps>
61 | 
62 | </TabsContent>
63 | 
64 | </Tabs>
65 | 
66 | ## Props
67 | 
68 | | Prop         | Type              | Default | Description                                              |
69 | | ------------ | ----------------- | ------- | -------------------------------------------------------- |
70 | | `children`   | `React.ReactNode` | `-`     | The content of the button.                               |
71 | | `className`  | `string`          | `-`     | Additional class names for the button.                   |
72 | | `pulseColor` | `string`          | `-`     | The rbg numbers only for the color of the pulsing waves. |
73 | | `duration`   | `string`          | `-`     | The time span of one pulse.                              |
74 | 
75 | ## Credits
76 | 
77 | - Credit to [@shikhap04](https://github.com/shikhap04)
78 | 


--------------------------------------------------------------------------------
/content/docs/components/rainbow-button.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Rainbow Button
 3 | date: 2024-09-19
 4 | description: An animated button with a rainbow effect.
 5 | author: dillionverma
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="rainbow-button-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/rainbow-button"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="rainbow-button" />
34 | 
35 | <Step>Update the import paths to match your project setup.</Step>
36 | 
37 | <Step>Update `globals.css`</Step>
38 | 
39 | Add the following to your `globals.css` file:
40 | 
41 | ```css
42 | :root {
43 |   --color-1: 0 100% 63%;
44 |   --color-2: 270 100% 63%;
45 |   --color-3: 210 100% 63%;
46 |   --color-4: 195 100% 63%;
47 |   --color-5: 90 100% 63%;
48 | }
49 | ```
50 | 
51 | <Step>Update `tailwind.config.js`</Step>
52 | 
53 | Add the following animations to your `tailwind.config.js` file:
54 | 
55 | ```js title="tailwind.config.js" {5-20}
56 | /** @type {import('tailwindcss').Config} */
57 | module.exports = {
58 |   theme: {
59 |     extend: {
60 |       colors: {
61 |         "color-1": "hsl(var(--color-1))",
62 |         "color-2": "hsl(var(--color-2))",
63 |         "color-3": "hsl(var(--color-3))",
64 |         "color-4": "hsl(var(--color-4))",
65 |         "color-5": "hsl(var(--color-5))",
66 |       },
67 |       animation: {
68 |         rainbow: "rainbow var(--speed, 2s) infinite linear",
69 |       },
70 |       keyframes: {
71 |         rainbow: {
72 |           "0%": { "background-position": "0%" },
73 |           "100%": { "background-position": "200%" },
74 |         },
75 |       },
76 |     },
77 |   },
78 | };
79 | ```
80 | 
81 | </Steps>
82 | 
83 | </TabsContent>
84 | 
85 | </Tabs>
86 | 
87 | ## Props
88 | 
89 | | Prop        | Type              | Default | Description                                         |
90 | | ----------- | ----------------- | ------- | --------------------------------------------------- |
91 | | `children`  | `React.ReactNode` | `-`     | The content to be displayed inside the button.      |
92 | | `className` | `string`          | `-`     | Additional CSS classes to be applied to the button. |
93 | 


--------------------------------------------------------------------------------
/content/docs/components/retro-grid.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Retro Grid
 3 | date: 2023-11-23
 4 | description: An animated scrolling retro grid effect
 5 | author: dillionverma
 6 | published: true
 7 | video: https://cdn.magicui.design/retro-grid.mp4
 8 | ---
 9 | 
10 | <ComponentPreview name="retro-grid-demo" />
11 | 
12 | ## Installation
13 | 
14 | <Tabs defaultValue="cli">
15 | 
16 | <TabsList>
17 |   <TabsTrigger value="cli">CLI</TabsTrigger>
18 |   <TabsTrigger value="manual">Manual</TabsTrigger>
19 | </TabsList>
20 | <TabsContent value="cli">
21 | 
22 | ```bash
23 | npx shadcn@latest add "https://magicui.design/r/retro-grid"
24 | ```
25 | 
26 | </TabsContent>
27 | 
28 | <TabsContent value="manual">
29 | 
30 | <Steps>
31 | 
32 | <Step>Copy and paste the following code into your project.</Step>
33 | 
34 | <ComponentSource name="retro-grid" />
35 | 
36 | <Step>Update the import paths to match your project setup.</Step>
37 | 
38 | <Step>Update `tailwind.config.js`</Step>
39 | 
40 | Add the following animations to your `tailwind.config.js` file:
41 | 
42 | ```js title="tailwind.config.js" {5-13}
43 | /** @type {import('tailwindcss').Config} */
44 | module.exports = {
45 |   theme: {
46 |     extend: {
47 |       animation: {
48 |         grid: "grid 15s linear infinite",
49 |       },
50 |       keyframes: {
51 |         grid: {
52 |           "0%": { transform: "translateY(-50%)" },
53 |           "100%": { transform: "translateY(0)" },
54 |         },
55 |       },
56 |     },
57 |   },
58 | };
59 | ```
60 | 
61 | </Steps>
62 | 
63 | </TabsContent>
64 | 
65 | </Tabs>
66 | 
67 | ## Props
68 | 
69 | | Prop             | Type     | Default  | Description                                   |
70 | | ---------------- | -------- | -------- | --------------------------------------------- |
71 | | `className`      | `string` | `-`      | Additional CSS classes for the grid container |
72 | | `angle`          | `number` | `65`     | Rotation angle of the grid in degrees         |
73 | | `cellSize`       | `number` | `60`     | Grid cell size in pixels                      |
74 | | `opacity`        | `number` | `0.5`    | Grid opacity value between 0 and 1            |
75 | | `lightLineColor` | `string` | `"gray"` | Grid line color in light mode                 |
76 | | `darkLineColor`  | `string` | `"gray"` | Grid line color in dark mode                  |
77 | 


--------------------------------------------------------------------------------
/content/docs/components/ripple-button.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Ripple Button
 3 | date: 2024-11-17
 4 | description: An animated button with ripple useful for user engagement.
 5 | author: Sidd5arth
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="ripple-button-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/ripple-button"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="ripple-button" />
34 | 
35 | <Step>Update the import paths to match your project setup.</Step>
36 | 
37 | <Step>Update `tailwind.config.js`</Step>
38 | 
39 | Add the following animations to your `tailwind.config.js` file:
40 | 
41 | ```js title="tailwind.config.js" {5-20}
42 | /** @type {import('tailwindcss').Config} */
43 | // tailwind.config.js
44 | module.exports = {
45 |   theme: {
46 |     extend: {
47 |       animation: {
48 |         rippling: "rippling var(--duration) ease-out",
49 |       },
50 |       keyframes: {
51 |         rippling: {
52 |           "0%": {
53 |             opacity: "1",
54 |           },
55 |           "100%": {
56 |             transform: "scale(2)",
57 |             opacity: "0",
58 |           },
59 |         },
60 |       },
61 |     },
62 |   },
63 | };
64 | ```
65 | 
66 | </Steps>
67 | 
68 | </TabsContent>
69 | 
70 | </Tabs>
71 | 
72 | ## Props
73 | 
74 | | Prop          | Type              | Default | Description                                               |
75 | | ------------- | ----------------- | ------- | --------------------------------------------------------- |
76 | | `children`    | `React.ReactNode` | `-`     | The content of the button.                                |
77 | | `className`   | `string`          | `-`     | Additional class names for the button.                    |
78 | | `rippleColor` | `string`          | `-`     | The rbg numbers only for the color of the rippling waves. |
79 | | `duration`    | `string`          | `-`     | The time span of one ripple.                              |
80 | 
81 | ## Credits
82 | 
83 | - Credit to [@Sidd5arth](https://github.com/Sidd5arth)
84 | 


--------------------------------------------------------------------------------
/content/docs/components/ripple.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Ripple
 3 | date: 2023-11-18
 4 | description: An animated ripple effect typically used behind elements to emphasize them.
 5 | author: dillionverma
 6 | published: true
 7 | video: https://cdn.magicui.design/ripple.mp4
 8 | ---
 9 | 
10 | <ComponentPreview name="ripple-demo" />
11 | 
12 | ## Installation
13 | 
14 | <Tabs defaultValue="cli">
15 | 
16 | <TabsList>
17 |   <TabsTrigger value="cli">CLI</TabsTrigger>
18 |   <TabsTrigger value="manual">Manual</TabsTrigger>
19 | </TabsList>
20 | <TabsContent value="cli">
21 | 
22 | ```bash
23 | npx shadcn@latest add "https://magicui.design/r/ripple"
24 | ```
25 | 
26 | </TabsContent>
27 | 
28 | <TabsContent value="manual">
29 | 
30 | <Steps>
31 | 
32 | <Step>Copy and paste the following code into your project.</Step>
33 | 
34 | <ComponentSource name="ripple" />
35 | 
36 | <Step>Update the import paths to match your project setup.</Step>
37 | 
38 | <Step>Update `tailwind.config.js`</Step>
39 | 
40 | Add the following animations to your `tailwind.config.js` file:
41 | 
42 | ```js title="tailwind.config.js" {5-17}
43 | /** @type {import('tailwindcss').Config} */
44 | module.exports = {
45 |   theme: {
46 |     extend: {
47 |       animation: {
48 |         ripple: "ripple var(--duration,2s) ease calc(var(--i, 0)*.2s) infinite",
49 |       },
50 |       keyframes: {
51 |         ripple: {
52 |           "0%, 100%": {
53 |             transform: "translate(-50%, -50%) scale(1)",
54 |           },
55 |           "50%": {
56 |             transform: "translate(-50%, -50%) scale(0.9)",
57 |           },
58 |         },
59 |       },
60 |     },
61 |   },
62 | };
63 | ```
64 | 
65 | </Steps>
66 | 
67 | </TabsContent>
68 | 
69 | </Tabs>
70 | 
71 | ## Props
72 | 
73 | | Prop                | Type     | Default | Description                            |
74 | | ------------------- | -------- | ------- | -------------------------------------- |
75 | | `mainCircleSize`    | `number` | `210`   | The size of the main circle in pixels  |
76 | | `mainCircleOpacity` | `number` | `0.24`  | The opacity of the main circle         |
77 | | `numCircles`        | `number` | `8`     | The number of ripple circles to render |
78 | 


--------------------------------------------------------------------------------
/content/docs/components/safari.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Safari
 3 | date: 2024-09-01
 4 | description: A safari browser mockup to showcase your website.
 5 | author: dillionverma
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="safari-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/safari"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="safari" />
34 | 
35 | <Step>Update the import paths to match your project setup.</Step>
36 | 
37 | </Steps>
38 | 
39 | </TabsContent>
40 | 
41 | </Tabs>
42 | 
43 | ## Examples
44 | 
45 | ### With Image
46 | 
47 | <ComponentPreview name="safari-demo-2" />
48 | 
49 | ### With Video
50 | 
51 | <ComponentPreview name="safari-demo-3" />
52 | 
53 | ### Simple Mode
54 | 
55 | <ComponentPreview name="safari-demo-4" />
56 | 
57 | ## Props
58 | 
59 | | Prop       | Type         | Default     | Description                                                 |
60 | | ---------- | ------------ | ----------- | ----------------------------------------------------------- |
61 | | `url`      | `string`     | `-`         | The URL to display in the Safari address bar                |
62 | | `imageSrc` | `string`     | `-`         | The source URL of the image to display in the Safari window |
63 | | `videoSrc` | `string`     | `-`         | The source URL of the video to display in the Safari window |
64 | | `width`    | `number`     | `1203`      | The width of the Safari window                              |
65 | | `height`   | `number`     | `753`       | The height of the Safari window                             |
66 | | `mode`     | `SafariMode` | `"default"` | The display mode of the Safari window.                      |
67 | 
68 | type `SafariMode` = `"default" | "simple"`.
69 | 
70 | The `Safari` component also accepts all properties of the `SVGElement` type.
71 | 


--------------------------------------------------------------------------------
/content/docs/components/scratch-to-reveal.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Scratch To Reveal
 3 | date: 2024-06-28
 4 | description: The ScratchToReveal component creates an interactive scratch-off effect with customizable dimensions and animations, revealing hidden content beneath.
 5 | author: dipesh_the_dev
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="scratch-to-reveal-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/scratch-to-reveal"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="scratch-to-reveal" />
34 | 
35 | </Steps>
36 | 
37 | </TabsContent>
38 | 
39 | </Tabs>
40 | 
41 | ## Props
42 | 
43 | | Prop                   | Type       | Default | Description                                                                                   |
44 | | ---------------------- | ---------- | ------- | --------------------------------------------------------------------------------------------- |
45 | | `className`            | `string`   | `-`     | The class name to apply to the component.                                                     |
46 | | `width`                | `number`   | `-`     | Width of the scratch container.                                                               |
47 | | `height`               | `number`   | `-`     | Height of the scratch container.                                                              |
48 | | `minScratchPercentage` | `number`   | `50`    | Minimum percentage of scratched area to be considered as completed (Value between 0 and 100). |
49 | | `children`             | `node`     | `-`     | The content to display in the marquee.                                                        |
50 | | `onComplete`           | `function` | `-`     | Callback function called when scratch is completed                                            |
51 | | `gradientColors`       | `string[]` | `-`     | Gradient colors for the scratch effect.                                                       |
52 | 
53 | ## Credits
54 | 
55 | - Credit to [dipesh_the_dev](https://www.x.com/dipesh_the_dev)
56 | 


--------------------------------------------------------------------------------
/content/docs/components/script-copy-btn.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Script Copy Button
 3 | date: 2023-11-18
 4 | description: Copy code to clipboard
 5 | author: dillionverma
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="script-copy-btn-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/script-copy-btn"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="script-copy-btn" />
34 | 
35 | </Steps>
36 | 
37 | </TabsContent>
38 | 
39 | </Tabs>
40 | 
41 | ## Props
42 | 
43 | | Prop                         | Type                     | Default | Description                                                 |
44 | | ---------------------------- | ------------------------ | ------- | ----------------------------------------------------------- |
45 | | `className`                  | `string`                 | `-`     | The class name to be applied to the component               |
46 | | `showMultiplePackageOptions` | `boolean`                | `true`  | Whether to show options for multiple package managers       |
47 | | `codeLanguage`               | `string`                 | `-`     | The language of the code snippet (e.g., "shell")            |
48 | | `lightTheme`                 | `string`                 | `-`     | The theme to use for light mode                             |
49 | | `darkTheme`                  | `string`                 | `-`     | The theme to use for dark mode                              |
50 | | `commandMap`                 | `Record<string, string>` | `-`     | A map of package manager names to their respective commands |
51 | 


--------------------------------------------------------------------------------
/content/docs/components/scroll-based-velocity.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Scroll Based Velocity
 3 | date: 2024-05-22
 4 | description: Scrolling text whose speed changes based on scroll speed
 5 | author: whyismynamerudy
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="scroll-based-velocity-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/scroll-based-velocity"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Install the following dependencies:</Step>
32 | 
33 | ```bash
34 | npm install motion
35 | ```
36 | 
37 | <Step>Copy and paste the following code into your project.</Step>
38 | 
39 | <ComponentSource name="scroll-based-velocity" />
40 | 
41 | <Step>Update the import paths to match your project setup.</Step>
42 | 
43 | </Steps>
44 | 
45 | </TabsContent>
46 | 
47 | </Tabs>
48 | 
49 | ## Props
50 | 
51 | | Prop              | Type        | Default | Description                                   |
52 | | ----------------- | ----------- | ------- | --------------------------------------------- |
53 | | `className`       | `string`    | `-`     | The class name to be applied to the component |
54 | | `children`        | `ReactNode` | `-`     | Content to be animated                        |
55 | | `defaultVelocity` | `number`    | `5`     | Base scroll velocity of text                  |
56 | | `numRows`         | `number`    | `2`     | Number of rows to be animated                 |
57 | 


--------------------------------------------------------------------------------
/content/docs/components/scroll-progress.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Scroll Progress
 3 | date: 2024-12-19
 4 | description: Animated Scroll Progress for your pages
 5 | author: dipesh_the_dev
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="scroll-progress-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | <TabsList>
15 |     <TabsTrigger value="cli">CLI</TabsTrigger>
16 |     <TabsTrigger value="manual">Manual</TabsTrigger>
17 | </TabsList>
18 | <TabsContent value="cli">
19 | 
20 | ```bash
21 | npx shadcn@latest add "https://magicui.design/r/scroll-progress"
22 | ```
23 | 
24 | </TabsContent>
25 | 
26 | <TabsContent value="manual">
27 | 
28 | <Steps>
29 | 
30 | <Step>Copy and paste the following code into your project.</Step>
31 | 
32 | <ComponentSource name="scroll-progress" />
33 | 
34 | </Steps>
35 | 
36 | </TabsContent>
37 | 
38 | </Tabs>
39 | 
40 | ## Props
41 | 
42 | | Prop        | Type     | Default | Description                                   |
43 | | ----------- | -------- | ------- | --------------------------------------------- |
44 | | `className` | `string` | `-`     | The class name to be applied to the component |
45 | 
46 | The `ScrollProgress` component also accepts all properties of the `HTMLDivElement` type.
47 | 
48 | ## Credits
49 | 
50 | - Credit to [dipesh_the_dev](https://twitter.com/dipesh_the_dev)
51 | 


--------------------------------------------------------------------------------
/content/docs/components/shimmer-button.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Shimmer Button
 3 | date: 2023-08-10
 4 | description: A button with a shimmering light which travels around the perimeter.
 5 | author: dillionverma
 6 | published: true
 7 | video: https://cdn.magicui.design/shimmer-button.mp4
 8 | ---
 9 | 
10 | <ComponentPreview name="shimmer-button-demo" />
11 | 
12 | ## Installation
13 | 
14 | <Tabs defaultValue="cli">
15 | 
16 | <TabsList>
17 |   <TabsTrigger value="cli">CLI</TabsTrigger>
18 |   <TabsTrigger value="manual">Manual</TabsTrigger>
19 | </TabsList>
20 | <TabsContent value="cli">
21 | 
22 | ```bash
23 | npx shadcn@latest add "https://magicui.design/r/shimmer-button"
24 | ```
25 | 
26 | </TabsContent>
27 | 
28 | <TabsContent value="manual">
29 | 
30 | <Steps>
31 | 
32 | <Step>Copy and paste the following code into your project.</Step>
33 | 
34 | <ComponentSource name="shimmer-button" />
35 | 
36 | <Step>Update the import paths to match your project setup.</Step>
37 | 
38 | <Step>Update `tailwind.config.js`</Step>
39 | 
40 | Add the following animations to your `tailwind.config.js` file:
41 | 
42 | ```js title="tailwind.config.js" {5-30}
43 | /** @type {import('tailwindcss').Config} */
44 | module.exports = {
45 |   theme: {
46 |     extend: {
47 |       animation: {
48 |         "shimmer-slide":
49 |           "shimmer-slide var(--speed) ease-in-out infinite alternate",
50 |         "spin-around": "spin-around calc(var(--speed) * 2) infinite linear",
51 |       },
52 |       keyframes: {
53 |         "spin-around": {
54 |           "0%": {
55 |             transform: "translateZ(0) rotate(0)",
56 |           },
57 |           "15%, 35%": {
58 |             transform: "translateZ(0) rotate(90deg)",
59 |           },
60 |           "65%, 85%": {
61 |             transform: "translateZ(0) rotate(270deg)",
62 |           },
63 |           "100%": {
64 |             transform: "translateZ(0) rotate(360deg)",
65 |           },
66 |         },
67 |         "shimmer-slide": {
68 |           to: {
69 |             transform: "translate(calc(100cqw - 100%), 0)",
70 |           },
71 |         },
72 |       },
73 |     },
74 |   },
75 | };
76 | ```
77 | 
78 | </Steps>
79 | 
80 | </TabsContent>
81 | 
82 | </Tabs>
83 | 
84 | ## Props
85 | 
86 | | Prop              | Type              | Default            | Description                         |
87 | | ----------------- | ----------------- | ------------------ | ----------------------------------- |
88 | | `shimmerColor`    | `string`          | `#ffffff`          | The color of the shimmer            |
89 | | `shimmerSize`     | `string`          | `0.05em`           | The size of the shimmer             |
90 | | `borderRadius`    | `string`          | `100px`            | The border radius of the button     |
91 | | `shimmerDuration` | `string`          | `3s`               | The duration of the spark animation |
92 | | `background`      | `string`          | `rgba(0, 0, 0, 1)` | The background of the button        |
93 | | `className`       | `string`          | `undefined`        | The class name of the button        |
94 | | `children`        | `React.ReactNode` | `undefined`        | The children of the button          |
95 | 
96 | ## Credits
97 | 
98 | Credit to [@jh3yy](https://twitter.com/jh3yy/status/1656423856276488192) for the inspiration behind this component.
99 | 


--------------------------------------------------------------------------------
/content/docs/components/shine-border.mdx:
--------------------------------------------------------------------------------
  1 | ---
  2 | title: Shine Border
  3 | date: 2024-05-25
  4 | description: Shine border is an animated background border effect.
  5 | author: unnamed-lab
  6 | published: true
  7 | ---
  8 | 
  9 | <ComponentPreview name="shine-border-demo" />
 10 | 
 11 | ## Installation
 12 | 
 13 | <Tabs defaultValue="cli">
 14 | 
 15 | <TabsList>
 16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
 17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
 18 | </TabsList>
 19 | <TabsContent value="cli">
 20 | 
 21 | ```bash
 22 | npx shadcn@latest add "https://magicui.design/r/shine-border"
 23 | ```
 24 | 
 25 | </TabsContent>
 26 | 
 27 | <TabsContent value="manual">
 28 | 
 29 | <Steps>
 30 | 
 31 | <Step>Copy and paste the following code into your project.</Step>
 32 | 
 33 | <ComponentSource name="shine-border" />
 34 | 
 35 | <Step>Update the import paths to match your project setup.</Step>
 36 | 
 37 | <Step>Update `tailwind.config.js`</Step>
 38 | 
 39 | Add the following animations to your `tailwind.config.js` file:
 40 | 
 41 | ```js title="tailwind.config.js" {5-14}
 42 | /** @type {import('tailwindcss').Config} */
 43 | module.exports = {
 44 |   theme: {
 45 |     extend: {
 46 |       animation: {
 47 |         shine: "shine var(--duration) infinite linear",
 48 |       },
 49 |       keyframes: {
 50 |         shine: {
 51 |           "0%": {
 52 |             "background-position": "0% 0%",
 53 |           },
 54 |           "50%": {
 55 |             "background-position": "100% 100%",
 56 |           },
 57 |           to: {
 58 |             "background-position": "0% 0%",
 59 |           },
 60 |         },
 61 |       },
 62 |     },
 63 |   },
 64 | };
 65 | ```
 66 | 
 67 | </Steps>
 68 | 
 69 | </TabsContent>
 70 | 
 71 | </Tabs>
 72 | 
 73 | ## Examples
 74 | 
 75 | ### Monotone
 76 | 
 77 | <ComponentPreview name="shine-border-demo-2" />
 78 | 
 79 | ## Usage
 80 | 
 81 | ```tsx
 82 | import { ShineBorder } from "@/registry/magicui/shine-border";
 83 | ```
 84 | 
 85 | ```tsx
 86 | <div className="relative overflow-hidden">
 87 |   <ShineBorder />
 88 | </div>
 89 | ```
 90 | 
 91 | ## Props
 92 | 
 93 | | Prop          | Type                  | Default     | Description                                                         |
 94 | | ------------- | --------------------- | ----------- | ------------------------------------------------------------------- |
 95 | | `className`   | `string`              | `-`         | The class name to be applied to the component.                      |
 96 | | `duration`    | `number`              | `14`        | Defines the animation duration to be applied on the shining border. |
 97 | | `shineColor`  | `string \| string[]`  | `"#000000"` | Color of the border, can be a single color or an array of colors.   |
 98 | | `borderWidth` | `number`              | `1`         | Width of the border in pixels.                                      |
 99 | | `style`       | `React.CSSProperties` | `-`         | Additional styles to be applied to the component.                   |
100 | 


--------------------------------------------------------------------------------
/content/docs/components/shiny-button.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Shiny Button
 3 | date: 2024-05-24
 4 | description: A shiny button component with dynamic styles in the dark mode or light mode.
 5 | author: luis-codex
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="shiny-button-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/shiny-button"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="shiny-button" />
34 | 
35 | <Step>Update the import paths to match your project setup.</Step>
36 | 
37 | </Steps>
38 | 
39 | </TabsContent>
40 | 
41 | </Tabs>
42 | 
43 | ## Props
44 | 
45 | | Prop        | Type              | Default | Description                                   |
46 | | ----------- | ----------------- | ------- | --------------------------------------------- |
47 | | `className` | `string`          | `-`     | The class name to be applied to the component |
48 | | `children`  | `React.ReactNode` | `-`     | The content to be displayed inside the button |
49 | 
50 | ## Credits
51 | 
52 | - Credit to [@luis-code](https://luis-code.vercel.app/)
53 | 


--------------------------------------------------------------------------------
/content/docs/components/sparkles-text.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Sparkles Text
 3 | date: 2024-05-30
 4 | description: A dynamic text that generates continuous sparkles with smooth transitions, perfect for highlighting text with animated stars.
 5 | author: Draxx0
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="sparkles-text-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/sparkles-text"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="sparkles-text" />
34 | 
35 | <Step>Update the import paths to match your project setup.</Step>
36 | 
37 | </Steps>
38 | 
39 | </TabsContent>
40 | 
41 | </Tabs>
42 | 
43 | ## Props
44 | 
45 | | Prop            | Type     | Default                                 | Description                                        |
46 | | --------------- | -------- | --------------------------------------- | -------------------------------------------------- |
47 | | `text`          | `string` | `-`                                     | The text to display                                |
48 | | `className`     | `string` | `-`                                     | The class name to be applied to the sparkles text. |
49 | | `sparklesCount` | `number` | `10`                                    | sparkles count that appears on the text            |
50 | | `colors`        | `object` | `{first: '#A07CFE', second: '#FE8FB5'}` | the sparkles colors                                |
51 | 
52 | ## Credits
53 | 
54 | - Credit to [@simonlejeune](https://www.simlej.dev/)
55 | 


--------------------------------------------------------------------------------
/content/docs/components/spinning-text.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Spinning Text
 3 | date: 2025-02-09
 4 | description: The Spinning Text component animates text in a circular motion with customizable speed, direction, color, and transitions for dynamic and engaging effects.
 5 | author: aayushbharti
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="spinning-text-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | 
20 | <TabsContent value="cli">
21 | 
22 | ```bash
23 | npx shadcn@latest add "https://magicui.design/r/spinning-text"
24 | ```
25 | 
26 | </TabsContent>
27 | 
28 | <TabsContent value="manual">
29 | 
30 | <Steps>
31 | 
32 | <Step>Copy and paste the following code into your project.</Step>
33 | 
34 | <ComponentSource name="spinning-text" />
35 | 
36 | <Step>Update the import paths to match your project setup.</Step>
37 | 
38 | </Steps>
39 | 
40 | </TabsContent>
41 | 
42 | </Tabs>
43 | 
44 | ## Examples
45 | 
46 | ### Reverse
47 | 
48 | <ComponentPreview name="spinning-text-demo-2" />
49 | 
50 | ## Props
51 | 
52 | | Prop         | Type                                         | Default | Description                                             |
53 | | ------------ | -------------------------------------------- | ------- | ------------------------------------------------------- |
54 | | `children`   | `ReactElement`                               |         | The text content to be animated in a circular motion.   |
55 | | `style`      | `CSSProperties`                              | `{}`    | Custom styles for the text container.                   |
56 | | `duration`   | `number`                                     | `10`    | The duration of the full circular rotation animation.   |
57 | | `className`  | `string`                                     |         | A custom class name for the text container.             |
58 | | `reverse`    | `boolean`                                    | `false` | Determines if the animation should rotate in reverse.   |
59 | | `fontSize`   | `number`                                     | `1`     | The font size of the text being animated in rem.        |
60 | | `radius`     | `number`                                     | `5`     | The radius of the circular path for the text animation. |
61 | | `transition` | `Transition`                                 |         | Custom transition effects for the animation.            |
62 | | `variants`   | `{ container?: Variants; item?: Variants; }` |         | Variants for container and item animations.             |
63 | 
64 | ## Credits
65 | 
66 | - Credit to [@AayushBharti](https://github.com/AayushBharti)
67 | 


--------------------------------------------------------------------------------
/content/docs/components/terminal.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Terminal
 3 | date: 2025-01-16
 4 | description: An implementation of the MacOS terminal. Useful for showcasing a command line interface.
 5 | author: dillionverma
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="terminal-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/terminal"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="terminal" />
34 | 
35 | </Steps>
36 | 
37 | </TabsContent>
38 | 
39 | </Tabs>
40 | 
41 | ## Props
42 | 
43 | ### Terminal
44 | 
45 | | Prop        | Type        | Default | Description                              |
46 | | ----------- | ----------- | ------- | ---------------------------------------- |
47 | | `children`  | `ReactNode` | `-`     | Content to be typed out in the terminal. |
48 | | `className` | `string`    | `-`     | Custom CSS class for styling.            |
49 | 
50 | ### AnimatedSpan
51 | 
52 | | Prop        | Type        | Default | Description                                        |
53 | | ----------- | ----------- | ------- | -------------------------------------------------- |
54 | | `children`  | `ReactNode` | `-`     | Content to be animated.                            |
55 | | `delay`     | `number`    | `0`     | Delay in milliseconds before the animation starts. |
56 | | `className` | `string`    | `-`     | Custom CSS class for styling.                      |
57 | 
58 | ### TypingAnimation
59 | 
60 | | Prop        | Type                | Default  | Description                                        |
61 | | ----------- | ------------------- | -------- | -------------------------------------------------- |
62 | | `children`  | `ReactNode`         | `-`      | Content to be animated.                            |
63 | | `delay`     | `number`            | `0`      | Delay in milliseconds before the animation starts. |
64 | | `className` | `string`            | `-`      | Custom CSS class for styling.                      |
65 | | `duration`  | `number`            | `100`    | Duration in milliseconds for each character typed. |
66 | | `as`        | `React.ElementType` | `"span"` | The component type to render.                      |
67 | 


--------------------------------------------------------------------------------
/content/docs/components/text-animate.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Text Animate
 3 | date: 2025-01-01
 4 | description: A text animation component that animates text using a variety of different animations.
 5 | author: dillionverma
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="text-animate-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/text-animate"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="text-animate" />
34 | 
35 | <Step>Update the import paths to match your project setup.</Step>
36 | 
37 | </Steps>
38 | 
39 | </TabsContent>
40 | 
41 | </Tabs>
42 | 
43 | ## Examples
44 | 
45 | ### Blur In by Text
46 | 
47 | <ComponentPreview name="text-animate-demo-2" />
48 | 
49 | ### Slide Up by Word
50 | 
51 | <ComponentPreview name="text-animate-demo-3" />
52 | 
53 | ### Scale Up by Text
54 | 
55 | <ComponentPreview name="text-animate-demo-4" />
56 | 
57 | ### Fade In by Line
58 | 
59 | <ComponentPreview name="text-animate-demo-5" />
60 | 
61 | ### Slide Left by Character
62 | 
63 | <ComponentPreview name="text-animate-demo-6" />
64 | 
65 | ### With Delay
66 | 
67 | <ComponentPreview name="text-animate-demo-7" />
68 | 
69 | ### With Duration
70 | 
71 | <ComponentPreview name="text-animate-demo-8" />
72 | 
73 | ### With Custom Motion Variants
74 | 
75 | <ComponentPreview name="text-animate-demo-9" />
76 | 
77 | ## Props
78 | 
79 | | Prop          | Type                                        | Default    | Description                                               |
80 | | ------------- | ------------------------------------------- | ---------- | --------------------------------------------------------- |
81 | | `children`    | `string`                                    | `-`        | The text content to animate                               |
82 | | `className`   | `string`                                    | `-`        | The class name to be applied to the component             |
83 | | `delay`       | `number`                                    | `0`        | Delay before animation starts                             |
84 | | `duration`    | `number`                                    | `0.3`      | Duration of the animation                                 |
85 | | `variants`    | `Variants`                                  | `-`        | Custom motion variants for the animation                  |
86 | | `as`          | `ElementType`                               | `"p"`      | The element type to render                                |
87 | | `by`          | `"text" \| "word" \| "character" \| "line"` | `"word"`   | How to split the text ("text", "word", "character")       |
88 | | `startOnView` | `boolean`                                   | `true`     | Whether to start animation when component enters viewport |
89 | | `once`        | `boolean`                                   | `false`    | Whether to animate only once                              |
90 | | `animation`   | `AnimationVariant`                          | `"fadeIn"` | The animation preset to use                               |
91 | 


--------------------------------------------------------------------------------
/content/docs/components/text-reveal.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Text Reveal
 3 | date: 2024-04-08
 4 | description: Fade in text as you scroll down the page.
 5 | author: dillionverma
 6 | published: true
 7 | video: https://cdn.magicui.design/text-reveal.mp4
 8 | ---
 9 | 
10 | <ComponentPreview name="text-reveal-demo" />
11 | 
12 | ## Installation
13 | 
14 | <Tabs defaultValue="cli">
15 | 
16 | <TabsList>
17 |   <TabsTrigger value="cli">CLI</TabsTrigger>
18 |   <TabsTrigger value="manual">Manual</TabsTrigger>
19 | </TabsList>
20 | <TabsContent value="cli">
21 | 
22 | ```bash
23 | npx shadcn@latest add "https://magicui.design/r/text-reveal"
24 | ```
25 | 
26 | </TabsContent>
27 | 
28 | <TabsContent value="manual">
29 | 
30 | <Steps>
31 | 
32 | <Step>Copy and paste the following code into your project.</Step>
33 | 
34 | <ComponentSource name="text-reveal" />
35 | 
36 | <Step>Update the import paths to match your project setup.</Step>
37 | 
38 | </Steps>
39 | 
40 | </TabsContent>
41 | 
42 | </Tabs>
43 | 
44 | ## Usage
45 | 
46 | ```tsx
47 | import { TextReveal } from "@/components/magicui/text-reveal";
48 | ```
49 | 
50 | ```tsx
51 | <TextReveal>Magic UI will change the way you design.</TextReveal>
52 | ```
53 | 


--------------------------------------------------------------------------------
/content/docs/components/tweet-card.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Tweet Card
 3 | date: 2023-10-15
 4 | description: A card that displays a tweet with the author's name, handle, and profile picture.
 5 | author: dillionverma
 6 | published: true
 7 | video: https://cdn.magicui.design/tweet-card.mp4
 8 | ---
 9 | 
10 | <ComponentPreview name="tweet-card-demo" />
11 | 
12 | <Steps>
13 | 
14 | ### Installation
15 | 
16 | ```bash
17 | npm install react-tweet
18 | ```
19 | 
20 | ### Installation React Server Component (Next.js 13+):
21 | 
22 | Copy and paste the following code into your project.
23 | 
24 | <ComponentSource name="tweet-card" />
25 | 
26 | ### Installation Client Side
27 | 
28 | Copy and paste the following code into your project.
29 | 
30 | <ComponentSource name="client-tweet-card" />
31 | 
32 | </Steps>
33 | 
34 | ## Usage
35 | 
36 | To render on server side using RSC (Next.js 13):
37 | 
38 | ```tsx
39 | import { TweetCard } from "@/components/magicui/tweet-card.tsx";
40 | 
41 | export default async function App() {
42 |   return <TweetCard id="1441032681968212480" />;
43 | }
44 | ```
45 | 
46 | To render on client side:
47 | 
48 | ```tsx
49 | "use client";
50 | 
51 | import { ClientTweetCard } from "@/components/magicui/client-tweet-card.tsx";
52 | 
53 | export default function App() {
54 |   return <ClientTweetCard id="1441032681968212480" />;
55 | }
56 | ```
57 | 
58 | ## Examples
59 | 
60 | ### Tweet Card With Image Carousel
61 | 
62 | <ComponentPreview name="tweet-card-images" />
63 | 
64 | ### Tweet Card With Meta URL Preview
65 | 
66 | <ComponentPreview name="tweet-card-meta-preview" />
67 | 
68 | ## Props
69 | 
70 | ### ClientTweetCard
71 | 
72 | | Prop | Type     | Default | Description                     |
73 | | ---- | -------- | ------- | ------------------------------- |
74 | | `id` | `string` | `-`     | The id of the tweet to display. |
75 | 
76 | ### TweetCard
77 | 
78 | | Prop | Type     | Default | Description                     |
79 | | ---- | -------- | ------- | ------------------------------- |
80 | | `id` | `string` | `-`     | The id of the tweet to display. |
81 | 
82 | ## Credits
83 | 
84 | This component is built on top of [React Tweet](https://react-tweet.vercel.app/).
85 | 


--------------------------------------------------------------------------------
/content/docs/components/typing-animation.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Typing Animation
 3 | date: 2024-05-22
 4 | description: Characters appearing in typed animation
 5 | author: imanubhardwaj
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="typing-animation-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/typing-animation"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="typing-animation" />
34 | 
35 | <Step>Update the import paths to match your project setup.</Step>
36 | 
37 | </Steps>
38 | 
39 | </TabsContent>
40 | 
41 | </Tabs>
42 | 
43 | ## Props
44 | 
45 | | Prop          | Type                | Default | Description                                   |
46 | | ------------- | ------------------- | ------- | --------------------------------------------- |
47 | | `children`    | `string`            | `-`     | Text content to animate                       |
48 | | `className`   | `string`            | `-`     | The class name to be applied to the component |
49 | | `duration`    | `number`            | `100`   | Duration to wait in between each char type    |
50 | | `delay`       | `number`            | `0`     | Delay before animation starts (in ms)         |
51 | | `as`          | `React.ElementType` | `"div"` | Component to render as                        |
52 | | `startOnView` | `boolean`           | `false` | Start animation when component is in view     |
53 | 


--------------------------------------------------------------------------------
/content/docs/components/warp-background.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Warp Background
 3 | date: 2024-12-24
 4 | description: A card with a time warping background effect.
 5 | author: magicui
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="warp-background-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/warp-background"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Copy and paste the following code into your project.</Step>
32 | 
33 | <ComponentSource name="warp-background" />
34 | 
35 | <Step>Update the import paths to match your project setup.</Step>
36 | 
37 | </Steps>
38 | 
39 | </TabsContent>
40 | 
41 | </Tabs>
42 | 
43 | ## Props
44 | 
45 | | Prop           | Type              | Default                | Description                                     |
46 | | -------------- | ----------------- | ---------------------- | ----------------------------------------------- |
47 | | `children`     | `React.ReactNode` | `-`                    | The content to be put inside the warp animation |
48 | | `perspective`  | `number`          | `100`                  | The perspective of the warp animation           |
49 | | `beamsPerSide` | `number`          | `3`                    | The number of beams per side                    |
50 | | `beamSize`     | `number`          | `5`                    | The size of the beams                           |
51 | | `beamDelayMax` | `number`          | `3`                    | The maximum delay of the beams                  |
52 | | `beamDelayMin` | `number`          | `0`                    | The minimum delay of the beams                  |
53 | | `beamDuration` | `number`          | `3`                    | The duration of the beams                       |
54 | | `gridColor`    | `string`          | `"hsl(var(--border))"` | The color of the grid lines                     |
55 | 


--------------------------------------------------------------------------------
/content/docs/components/word-rotate.mdx:
--------------------------------------------------------------------------------
 1 | ---
 2 | title: Word Rotate
 3 | date: 2024-05-21
 4 | description: A vertical rotation of words
 5 | author: dillionverma
 6 | published: true
 7 | ---
 8 | 
 9 | <ComponentPreview name="word-rotate-demo" />
10 | 
11 | ## Installation
12 | 
13 | <Tabs defaultValue="cli">
14 | 
15 | <TabsList>
16 |   <TabsTrigger value="cli">CLI</TabsTrigger>
17 |   <TabsTrigger value="manual">Manual</TabsTrigger>
18 | </TabsList>
19 | <TabsContent value="cli">
20 | 
21 | ```bash
22 | npx shadcn@latest add "https://magicui.design/r/word-rotate"
23 | ```
24 | 
25 | </TabsContent>
26 | 
27 | <TabsContent value="manual">
28 | 
29 | <Steps>
30 | 
31 | <Step>Install the following dependencies:</Step>
32 | 
33 | ```bash
34 | npm install motion
35 | ```
36 | 
37 | <Step>Copy and paste the following code into your project.</Step>
38 | 
39 | <ComponentSource name="word-rotate" />
40 | 
41 | <Step>Update the import paths to match your project setup.</Step>
42 | 
43 | </Steps>
44 | 
45 | </TabsContent>
46 | 
47 | </Tabs>
48 | 
49 | ## Props
50 | 
51 | | Prop          | Type              | Default | Description                                   |
52 | | ------------- | ----------------- | ------- | --------------------------------------------- |
53 | | `className`   | `string`          | `-`     | The class name to be applied to the component |
54 | | `duration`    | `number`          | `2500`  | Duration of the animation                     |
55 | | `words`       | `string[]`        | `""`    | An array of words to rotate through           |
56 | | `motionProps` | `HTMLMotionProps` | `{}`    | An object containing motion animation props   |
57 | 


-------------------------------------------------------------------------------- -->