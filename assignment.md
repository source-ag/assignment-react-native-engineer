# Assignment: Digital Twin

The goal of this assignment is to verify that you can apply software engineering techniques in
practice. To do so, we ask that you build a React Native app for a hypothetical part of our product.

## A note on using LLMs

You are welcome — and encouraged — to use LLMs (e.g. Cursor, Claude Code, Copilot, etc.) while working on this assignment. This reflects how we work day-to-day at Source AG.

That said, we expect you to have an in-depth understanding of the code you submit. During the
interview we will ask you to walk us through your implementation, explain the choices you made, and
discuss trade-offs. Be prepared to reason about any part of your codebase, including code that was
generated with AI assistance.

## Instructions

To track the state of the plants in the greenhouses, growers can create a digital twin of a plant. This twin represents the state of the plant at the moment of the measurement.

The digital twin starts with a stem, containing branches which contain fruits (bell peppers in this example).

[<img src="./design/digital-twin.png" width="100" align="right" style="margin-left: 20px" />](./design/digital-twin.png)

For this assignment you have to draw the digital twin, based on a data object. This data object contains the start of the stem (`root_node`), the children of this node, which can also contain children.

<br clear="right"/>
<br />

[<img src="./design/harvestable-fruit.png" width="100" align="right" style="margin-left: 20px" />](./design/harvestable-fruit.png)

When a fruit is selected, the sidebar opens, showing the status of the fruit. A fruit can have one of six fruit development stages. The different stages are described further in this assignment.

<br clear="right"/>
<br />

[<img src="./design/flower.png" width="100" align="right" style="margin-left: 20px" />](./design/flower.png)

The user can change the selected fruit development stage by selecting another stage.

Below the stages, there is a list of toggles. These toggles are not part of this assignment and can be ignored.

<br clear="right"/>

## Data

We will provide an API which contains all the data you need to complete this assignment.

### API

The base URL for this API is provided in the invitation email for the tech assignment.

You can use the following endpoints:

-   `GET /stems/<stem_id>`: Retrieve a specific stem by its ID. To get you started, there is an existing stem with id
    `9ac45c32-64f1-4a27-b554-e3544cbbe001` that you can use.
-   `PUT /stems/<stem_id>`: Update a specific stem. The request body must follow the data structure described below, but **you must omit the `id` field from the body**. Returns the updated stem.
-   `POST /stems`: Create a new stem. The request body must follow the data structure described below, but **you must omit the `id` field from the body**. Returns the newly created stem, including its generated `id`.

All endpoints return `application/json`. A `404` is returned if the stem is not found.

### Data format

Each stem has a top-level `id` and a `root_node`. The root node has one or more `children`, and each child can itself have children, forming a tree. Every node — including the root — also carries a list of `fruits`.

The `level` field on a child node controls how it is drawn:

-   `1` — the child grows **straight up** from its parent (continuing the main stem)
-   `2` — the child **branches off** to the side

Example data object:

```json
{
    "id": "7b02aa08-5f06-4ac5-ae2b-fb582a2e8372",
    "root_node": {
        "level": 1,
        "id": "1",
        "children": [
            {
                "level": 1,
                "id": "2",
                "children": [],
                "fruits": [
                    {
                        "development_state": "HARVESTABLE_FRUIT",
                        "id": "fb532607-a858-4ad2-98f9-7aff6f682e92"
                    }
                ]
            },
            {
                "level": 2,
                "id": "1a",
                "children": [],
                "fruits": [
                    {
                        "development_state": "HARVESTABLE_FRUIT",
                        "id": "e3a12091-bc44-11ee-a506-0242ac120002"
                    }
                ]
            }
        ],
        "fruits": [
            {
                "development_state": "HARVESTABLE_FRUIT",
                "id": "a1b2c3d4-e5f6-7890-abcd-ef1234567890"
            }
        ]
    }
}
```

This code example results in this small drawing (click to enlarge):<br />
[<img src="./design/example-result.png" width="200" />](./design/example-result.png)

As you can see, the root node (node 1) has two children. The first child (node 2) has level 1, so it grows straight up. The second child (node 1a) has level 2, so it branches off into a separate branch.

With more data you should end up with a drawing like this:<br />
[<img src="./design/result.png" width="200" />](./design/result.png)

As TypeScript types:

```typescript
type FruitDevelopmentState =
    | "BUD"
    | "FLOWER"
    | "SET_FRUIT"
    | "MATURE_GREEN_FRUIT"
    | "BREAKER_STAGE_FRUIT"
    | "HARVESTABLE_FRUIT";

interface Fruit {
    id: string;
    development_state: FruitDevelopmentState;
}

interface Node {
    id: string;
    level: 1 | 2;
    fruits: Fruit[];
    children: Node[];
}

interface Stem {
    id: string;
    root_node: Node;
}
```

### Fruit development stages

Each node contains a list of fruits. For this assignment every node will have exactly one fruit.

The fruit object has a field `development_state` with the current stage of the fruit in it.

This list shows the possible fruit development states:

| Image                                                         | Development state   | Data value          |
| ------------------------------------------------------------- | ------------------- | ------------------- |
| <img src = "./assets/bud.svg" width = "50" />                 | Bud                 | BUD                 |
| <img src = "./assets/flower.svg" width = "50" />              | Flower              | FLOWER              |
| <img src = "./assets/set-fruit.svg" width = "50" />           | Set fruit           | SET_FRUIT           |
| <img src = "./assets/mature-green-fruit.svg" width = "50" />  | Mature green fruit  | MATURE_GREEN_FRUIT  |
| <img src = "./assets/breaker-stage-fruit.svg" width = "50" /> | Breaker stage fruit | BREAKER_STAGE_FRUIT |
| <img src = "./assets/harvestable-fruit.svg" width = "50" />   | Harvestable fruit   | HARVESTABLE_FRUIT   |

## Assets

This repository contains an `/assets` directory where all required SVG assets can be found.

## Requirements

Your solution should include the following functionality:

1. Consume the provided API to fetch the stem data
2. Draw the plant based on the data from the API
3. Open a sidebar when a fruit is selected and show the fruit development stage
4. Let the user change the selected fruit development stage
5. Update the plant view with the newly selected stage
6. Call the API to persist the change to the backend

### Non-functional requirements

-   Use TypeScript
-   Use the latest stable version of React Native
-   The app should be optimized for running on **tablets**
-   This assignment is designed to be completed in 6–8 hours. The evaluation takes into account the
    choices you make and what you focus on given the time you have. It's up to you whether you spend
    less or more time on it.
-   Please make sure the final commit to your repository is done at least 24 hours before the start
    of your interview

## Deliverables

This assignment should be delivered in the following way:

-   All code is pushed to your private copy of this repository
-   Documentation is provided in the `README.md` explaining how the solution works, and how to run and test it
-   Any information, (dummy) data, files, and other assets needed to run the project are included in the repository

## Assessment Criteria

The solution will be assessed on the following criteria:

-   **Code structure** — Is the code well-organised and easy to follow?
-   **Documentation** — Is the README clear and complete?
-   **Correctness** — Are there any obvious bugs?
-   **Performance** — Does the solution perform well?
-   **Decision-making** — Can you clearly and concisely describe the process you followed and the choices you made, including any trade-offs? This applies equally to decisions made with or without AI assistance.
-   **Self-awareness** — Can you identify the biggest shortcomings of your solution and describe concrete steps to improve it?
