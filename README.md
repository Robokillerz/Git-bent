KRGBY + K2 + White String Art Generator
This app is a high-detail computational string art design studio that converts uploaded photographs into physical thread-art construction plans.
It generates layered thread paths using:
K = Black
R = Red
G = Green
B = Blue
Y = Yellow
K2 = Secondary black detail pass
WHITE = restrained highlight pass
The result is a multi-layer string-art image designed to resemble gallery-quality thread portraits and abstract textile artwork.
What the App Does
The app:
Lets you upload a photo
Lets you crop/zoom/pan the image directly on the canvas
Converts the image into thousands of pin-to-pin thread connections
Produces:
live thread rendering
TXT instructions
CSV instructions
SVG vector export
Supports:
circular canvases
square canvases
fully custom rectangular canvases
The generated instructions can be physically built using:
thread
embroidery floss
yarn
fishing line
wire
colored string
on real boards with physical pins/nails.
Main Canvas Modes
Circle Mode
Pins are placed around a circular perimeter.
Best for:
portraits
animals
classical string art
Square Mode
Pins are placed around the edge of a square.
Best for:
geometric art
modern wall pieces
poster-style layouts
Rectangle Mode
Pins are distributed around a custom rectangle perimeter.
You manually choose:
width
height
Best for:
landscape scenes
panoramic portraits
posters
widescreen artwork
How the Solver Works
The app uses a greedy optimization solver.
It repeatedly:
tests many possible pin-to-pin lines
measures which line improves the image most
applies that line
repeats thousands of times
Each thread color is applied in separate passes.
The Color System
K (Black)
Primary structural shading.
Creates:
major shadows
form
overall image depth
RGBY
Color passes that reconstruct:
skin tones
clothing
backgrounds
environmental color
R
Warm tones and reds.
G
Natural tones and greens.
B
Cool tones and blues.
Y
Highlights, warm lighting, golds, skin warmth.
K2
Secondary black refinement layer.
Used for:
edge sharpening
contrast enhancement
fine detail recovery
K2 usually improves:
eyes
facial definition
hair texture
contrast clarity
WHITE
Restrained highlight pass.
Used sparingly to:
lift highlights
restore sparkle
soften muddy darks
improve realism
White is intentionally limited so the image does not become washed out.
Image Cropping System
After uploading:
the image appears directly on the canvas
drag to reposition
pinch to zoom on mobile
mouse wheel zoom on desktop
The generator uses the exact crop shown on screen.
Physical Meaning of Pins
Each pin is numbered.
The instructions look like:
Plain text
145) R: 22 → 97
146) B: 97 → 201
147) K2: 201 → 11
Meaning:
connect Red thread from pin 22 to pin 97
then Blue from 97 to 201
then K2 black from 201 to 11
Controls Explained
Pins
Total perimeter pins.
Higher values:
more detail
slower generation
more difficult physical builds
Recommended:
220–360
Total Lines
Maximum generated thread segments.
Higher:
more realism
longer generation
denser artwork
Recommended:
3000–7000
Candidates
How many possible lines are tested each step.
Higher:
smarter solver
slower generation
Recommended:
160–240
Solver Width
Internal analysis resolution.
Higher:
better realism
slower performance
Recommended:
320–500
Subtract
Controls how strongly each line affects the internal image.
Lower:
softer
smoother
more layered
Higher:
stronger contrast
harsher thread buildup
Recommended:
0.05–0.10
Line Opacity
Visual render opacity.
Represents how translucent thread appears.
Lower:
softer blending
more realism
Higher:
stronger visible strings
Recommended:
0.12–0.22
Line Thickness
Thickness of rendered thread.
Lower:
embroidery/fine thread
Higher:
yarn/heavy cord appearance
Recommended:
0.35–0.8
White Thickness
Thickness only for White highlight thread.
Usually best slightly thinner than normal threads.
Recommended:
0.25–0.5
Min Skip
Minimum pin distance allowed.
Prevents tiny local lines.
Higher values:
cleaner global structure
fewer messy clusters
Recommended:
6–14
Candidate Modes
Wide / Chord-Biased
Prefers long crossing lines.
Best for:
realism
structure
portraits
Recommended default.
Random
More chaotic repo-style generation.
Best for:
artistic looks
experimentation
Scan All Pins
Tests every possible pin.
Best quality but much slower.
Share Controls
These control thread distribution.
K Share
Amount of primary black.
RGBY Share
Amount of color.
K2 Share
Amount of detail sharpening.
White Share
Amount of highlight restoration.
Export Features
TXT Export
Simple readable instructions.
CSV Export
Spreadsheet-compatible instructions.
Useful for:
sorting
filtering
automation
SVG Export
Vector preview of the generated artwork.
Can be:
printed
laser engraved
scaled infinitely
imported into design software
Typical Workflow
1. Choose canvas shape
Circle
Square
Rectangle
2. Set dimensions (Rectangle only)
Example:
1200 × 800
3. Upload image
4. Crop and zoom
Move image until composition looks correct.
5. Adjust settings
Typical realistic setup:
Pins: 300
Lines: 5000
Solver Width: 420
Opacity: 0.16
Thickness: 0.55
6. Generate
The app progressively builds the artwork.
7. Export
Download:
TXT
CSV
SVG
Best Images for Realism
Best results:
high contrast portraits
dramatic lighting
centered subjects
clean backgrounds
Harder subjects:
low contrast images
blurry photos
busy backgrounds
tiny distant faces
Physical Construction Tips
Recommended Materials
Fine Detail
embroidery floss
sewing thread
Balanced
crochet thread
quilting thread
Heavy Texture
yarn
cord
Recommended Boards
painted wood
MDF
plywood
Artistic Purpose
This app is designed for:
serious string artists
gallery artwork
computational textile art
modern craft installations
algorithmic art experiments
physical AI-assisted artwork
It combines:
optimization algorithms
digital image analysis
procedural geometry
physical thread simulation
into a single buildable art workflow.
