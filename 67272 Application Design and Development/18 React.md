## Programming Paradigms

- OOP
- Functional Programming 
- Reactive Programming
	- about data streams
	- arose from the problem of how to handle streams of data from a variety of sources
	- key concept: **observer pattern**
		- allow other objects to observe events and get notifications when there's state change
		- Useful when state is regularly changing / many other objects need to know when state has changed

notify observers method 在employees里面

update method
when observed changes, i am going to react to it and change it.

## Difference between OP and RP

1. Paradigm: 
	1. **Object-Oriented Programming**: OOP is a programming paradigm that focuses on organizing code into objects, which encapsulate data and behavior. 
	2. **Reactive Programming**: Reactive programming is a programming paradigm that focuses on data streams and the propagation of changes. It is centered around the idea of reacting to data changes and events.
2. Data Flow: 
	1. **Object-Oriented Programming**: In OOP, data flows between objects through method calls and interactions. Objects encapsulate state and behavior, and they communicate with each other by invoking methods and exchanging messages. 
	2. **Reactive Programming**: In reactive programming, data flows through streams or event streams. Changes in one part of the system can be automatically propagated to other parts that are interested in those changes
3. Handling Change: 
	1. **Object-Oriented Programming**: In OOP, objects encapsulate state, and changes to the state are typically managed by invoking methods on objects. Objects can have mutable state that can be modified over time. 
	2. **Reactive Programming**: Reactive programming focuses on reacting to changes. It provides mechanisms to define reactions and transformations on data streams, allowing developers to handle changes in a declarative and composable manner
4. Concurrency and Asynchrony: 
	1. **Object-Oriented Programming**: Concurrency and asynchrony can be achieved in OOP through the use of threads, callbacks, and the like. However, managing concurrency can be challenging, especially in complex systems. 
	2. **Reactive Programming**: Reactive programming frameworks often provide built-in support for concurrency and asynchrony. They usually offer abstractions for handling asynchronous operations, such as reactive streams or reactive extensions (Rx), which simplify managing concurrency and handling asynchronous events
5. Error Handling: 
	1. **Object-Oriented Programming**: In OOP, error handling is typically done using exceptions or error codes. Developers need to explicitly handle exceptions or propagate them up the call stack. 
	2. **Reactive Programming**: Reactive programming provides error handling mechanisms specific to reactive streams. It often includes operators or constructs to handle errors within the stream, propagate errors downstream, or recover from errors in a controlled manner
6. Use Cases: 
	1. **Object-Oriented Programming**: OOP is a widely used paradigm and is suitable for a wide range of applications, including desktop applications, web development, and enterprise software. 
	2. **Reactive Programming**: Reactive programming is particularly useful for event-driven systems, real-time applications, and systems that handle a large volume of asynchronous data, such as web applications with live updates, IoT systems, or reactive user interfaces

- Object Oriented Programming and Reactive Programming are NOT mutually exclusive.
- The HTML DOM model is constructed as a tree of Objects

## How does React work?

1. Component-Based Architecture: React is based on a component-based architecture, where your user interface is composed of individual, selfcontained components. Each component represents a part of your UI, such as a button, a form, or a header. Components can be reused throughout your application, which promotes code reusability and maintainability. 
2. Virtual DOM: One of the key features of React is the Virtual DOM (Document Object Model). Instead of directly manipulating the actual browser DOM, React creates an in-memory representation of the DOM called the Virtual DOM. When data or state changes in your application, React first updates the Virtual DOM rather than making direct changes to the real DOM. 
3. Reconciliation: React employs a process called reconciliation to update the actual DOM efficiently. It compares the Virtual DOM to the previous version to determine the differences (known as "diffing") between the two. React then calculates the most efficient way to update the real DOM based on these differences. This approach minimizes the number of updates to the actual DOM, which can be computationally expensive. 
4. JSX (JavaScript XML): React uses JSX, an extension of JavaScript, to define the structure and appearance of your components. JSX allows you to write HTML-like code within your JavaScript files. Babel, a JavaScript compiler, is often used to transform JSX into regular JavaScript that browsers can understand.
5. State Management: React components can have their own internal state, which allows them to store and manage data that can change over time. When the state of a component changes, React automatically triggers a rerender of the component to update the UI. 
6. Props (Properties): Components can receive data from their parent components through props. Props are read-only and provide a way to pass data and configuration to child components. This enables the creation of reusable and customizable components. 
7. Lifecycle Methods: React components have a lifecycle with various methods that can be overridden. These methods allow you to perform actions at specific points in a component's lifecycle, such as when it's created, updated, or destroyed. Common lifecycle methods include `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount`. 
8. One-Way Data Flow: React follows a one-way data flow. Data flows from parent components down to child components via props. Child components cannot directly modify the state of their parent components; instead, they communicate changes through callbacks passed as props.
## Intermixing RP and OOP in Applications

export function -- other components can use it

default function -- main function of this file

advantage of oop approach: easy to update
advantage of react approach: faster, more intuitive, less code


takes time to gignyo bavk =and= proytin 
stay at the same page

减少user cognitive function
fatster response

how to create this use
using a Gem

-- react rails

gem rect_component -- 

sidebar: 我不做饭 饿哦套困了吧 啊 
不看热热啊了lnmnrnyts]]]]]]

only on e way 

export defalut -- use it ion efiffrrt ojrt finvtion;'

one main function per pgae

inve

i fony ro eivi

javadvtipyo drt== 

我好困

improt get fotr e

jgrkgnkldfn

log in ythe nbaknk

create this token,send the hidden token, send the request

brave

