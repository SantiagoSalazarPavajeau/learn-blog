---
layout: post
title:      "React-Typescript and vanilla CSS portfolio project"
date:       2021-01-29 09:06:19 +0000
permalink:  react-typescript_and_vanilla_css_portfolio_project
---


I am working on my new portfolio website with react-typescript. This is a pretty simple application but a productive way to keep working with frontend technologies JS, Typescript, React, and CSS.

To get started with a react-typescript project run the following on your terminal ([node](https://nodejs.org/en/), [npm](https://www.npmjs.com/get-npm) and [typescript](https://www.typescriptlang.org/download) have to be installed previously).

```
npx create-react-app my-app --template typescript
```

## Writing a presentational component with React-typescript

In this case, I made a `ProjectCard` component. It will render the information of the project into a card. 

``` react
import React from 'react';

export interface Project{
    title: string;
    description: string;
    demo: string;
    github: string;
    stack: string;  
}


export const ProjectCard = ({title, description, demo, github,stack}: Project) => {
    return (
        <>
            <div className="project-card">
                <h2>{title}</h2>
                <p>{description}</p>
                <button> <a href={demo}>Demo </a></button>

                <button><a href={github}>Github</a> </button>
                <p>{stack}</p>
            </div>          
        </>
    )
} 
```

In the arrow function component declaration, we set the type of the argument(props) to be `Project`. That way when we pass the project data all the properties will be required with the corresponding type. I did not use `React.FC` to type the function because of this [github issue](https://github.com/facebook/create-react-app/pull/8177) and also found some context for functional components with TypeScript [here](https://fettblog.eu/typescript-react-why-i-dont-use-react-fc/).

Setting the type of the arguments to `Project` makes all the arguments mandatory so the method to render the cards is:

```react
import {ProjectCard, Project} from './components/ProjectCard'

const App = () => {
  const renderProjects = () => {
    return projects.map((project: Project) => <ProjectCard title={project.title} description={project.description} demo={project.demo} github={project.github} stack={project.stack} />)
  }
 return(
  // html and components
  {renderProjects()}
  // html and components
 )
}
```

When any of the `ProjectCard` props are missing or if they have the wrong type, TypeScript will throw an error. We import the `Project` interface in order to give a type to the `project` we are iterating through with the `.map` callback as well.

## Vanilla CSS

In this project, I used both grid and flex side by side. Some of the highlights include using the grid to space the content horizontally into two "headers". The top "header" with my intro/profile and the bottom "header" with the projects. The settings for the horizontal grid were: 

```CSS
.App {
  display: grid;
  grid-template-columns: [left] 1fr [right];
}

.header{
 height: 500px;
}

.header2{
 height: 500px;
}
```

The header divs have the height as their property to set how big the horizontal spaces should be.


Another interesting CSS feature I implemented was a horizontal scroll for the project cards. To do this we can add the following flex CSS properties to the header2 and the project card.

``` css
.header2{
 display: flex;
 flex-wrap: nowrap;
 overflow-x: auto;
}

.project-card{
 flex: none;
}
```

These header2 properties with flex allow for the cards to keep rendering horizontally instead of creating a new row when the size of the container runs out. Then setting the flex to none on the project card allows the card to stay the same size regardless of the container size, this way the cards will stay readable and can be scrolled horizontally for navigation.

Some other CSS I added was basic responsiveness for the intro/profile header and styling to buttons. This is a work in progress so I am looking to add more features.

[Source code](https://github.com/SantiagoSalazarPavajeau/portfolio_react_typescript)

Feel more than welcome to reach out with any ideas/thoughts at [LinkedIn](https://www.linkedin.com/in/santiago-salazar-pavajeau/) or [Twitter](https://twitter.com/santispavajeau).
