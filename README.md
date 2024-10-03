# Pixel Papercraft Generator

Install Node and Git.

Node is best installed using [Node Version Manager](https://github.com/nvm-sh/nvm).

The project currently requires the following Node and NPM versions:

```
"engines": {
  "node": ">=18",
  "npm": ">=9"
}
```

Install Node.

```sh
nvm install v18
```

To ensure you have the latest NPM:

```
npm i -g npm@latest
```

Install Git [from the official site](https://git-scm.com/downloads).

Clone this repository and install the dependencies:

```sh
cd <parent_folder_for_the_generator_builder>
git clone https://github.com/pixelpapercraft/pixel-papercraft-generator generator-builder
cd generator-builder
npm install
```

### Set up TypeScript and Visual Studio Code

Generators are developed using the [TypeScript](https://typescriptlang.org/) programming language.

Install it with: 

```sh
npm install typescript
```

### Development

Ensure you are running the correct version of Node:

```
nvm use
```

In one terminal, compile the TypeScript to JavaScript:

```
ts-node [path/to/your/typescript/generator].ts
```

In another terminal, start the web server:

```
npm run dev
```

Then open your browser:

```
http://localhost:3000
```

## Example Code:

Here is an example on what your generator could look like:

```typescript
"use client";

import type {
  GeneratorDef,
  InstructionsDef,
  ImageDef,
  HistoryDef,
  TextureDef,
  ScriptDef,
} from "@genroot/builder/modules/generatorDef";
import { type Generator } from "@genroot/builder/modules/generator";

import skinImage from "./textures/Skin.png";
import backgroundImage from "./images/Background.png";
import foldsImage from "./images/Folds.png";

const id = "example";

const name = "Example";

const instructions: InstructionsDef = `
An example generator to demonstrate how to write a generator script.
`;

const history: HistoryDef = [];

const images: ImageDef[] = [
  { id: "Background", url: backgroundImage.src },
  { id: "Folds", url: foldsImage.src },
];

const textures: TextureDef[] = [
  {
    id: "Skin",
    url: skinImage.src,
    standardWidth: 64,
    standardHeight: 64,
  },
];

const script: ScriptDef = (generator: Generator) => {
  // Helper Function to draw heads
  const drawHead = (name: string, x: number, y: number) => {
    // Head Base
    generator.drawTexture(name, [0, 8, 8, 8], [x - 64, y + 0, 64, 64]); // Right
    generator.drawTexture(name, [8, 8, 8, 8], [x, y, 64, 64]); // Face
    generator.drawTexture(name, [16, 8, 8, 8], [x + 64, y + 0, 64, 64]); // Left
    generator.drawTexture(name, [24, 8, 8, 8], [x + 128, y + 0, 64, 64]); // Back
    generator.drawTexture(name, [8, 0, 8, 8], [x + 0, y - 64, 64, 64]); // Top
    generator.drawTexture(name, [16, 0, 8, 8], [x + 0, y + 64, 64, 64], {
      flip: "Vertical",
    }); // Bottom

    // Head Overlay
    generator.drawTexture(name, [32, 8, 8, 8], [x - 64, y + 0, 64, 64]); // Right
    generator.drawTexture(name, [40, 8, 8, 8], [x, y, 64, 64]); // Face
    generator.drawTexture(name, [48, 8, 8, 8], [x + 64, y + 0, 64, 64]); // Left
    generator.drawTexture(name, [56, 8, 8, 8], [x + 128, y + 0, 64, 64]); // Back
    generator.drawTexture(name, [40, 0, 8, 8], [x + 0, y - 64, 64, 64]); // Top
    generator.drawTexture(name, [48, 0, 8, 8], [x + 0, y + 64, 64, 64], {
      flip: "Vertical",
    }); // Bottom
  };

  generator.defineTextureInput("Skin", {
    standardWidth: 64,
    standardHeight: 64,
    choices: [],
    enableMinecraftSkinInput: true,
  });

  generator.defineBooleanInput("Show Folds", true);

  const showFolds = generator.getBooleanInputValue("Show Folds");

  generator.drawImage("Background", [0, 0]);

  drawHead("Skin", 185, 117);

  if (showFolds) {
    generator.drawImage("Folds", [0, 0]);
  }
};

export const generator: GeneratorDef = {
  id,
  name,
  thumbnail: null,
  video: null,
  instructions,
  history,
  images,
  textures,
  script,
};

```

This will be explained one by one later.

## Development

The source code for the generators can be found at `src/generators`. The structure for a generator is:

```ascii
/example
├── /images
│   ├── image1.png
│   ├── image2.png
│   └── image3.png
├── /textures
│   ├── texture1.png
│   ├── texture2.png
│   └── texture3.png
└── Example.res
```

This structure isn't required, but it is reccomended.

---

Every generator must have:

```typescript
export const generator: GeneratorDef = {
  id,
  name,
  thumbnail: null,
  video: null,
  instructions,
  history,
  images,
  textures,
  script,
};
```

This is an object which has a few properties. They include:


| Property     | Description                                      |
| ------------ | ------------------------------------------------ |
| id           | Unique Id for the generator, used in the URL     |
| name         | Name for the generator                           |
| images       | Array of image URLs                              |
| textures     | Array of texture image URLs                      |
| script       | Generator script                                 |
| history      | Array of strings, used as the history of updates |
| thumbnail    | Thumbnail image                                  |
| video        | Video (optional)                                 |
| instructions | Instructions for the generator                   |

### Explaining the [Example Code](#example-code)

Here is the explaination of the Example Code and what each thing does.

---

`"use client";` indicates that the code is to be run on the client side (or the browser).

```typescript
import type {
  GeneratorDef,
  InstructionsDef,
  ImageDef,
  HistoryDef,
  TextureDef,
  ScriptDef,
} from "@genroot/builder/modules/generatorDef";
import { type Generator } from "@genroot/builder/modules/generator";
```

Imports the neccesary types for the generator.

```typescript
import skinImage from "./textures/Skin.png";
import backgroundImage from "./images/Background.png";
import foldsImage from "./images/Folds.png";
```

Imports assets that will be used.

```typescript
```