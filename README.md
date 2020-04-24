# slider-select

### Project setup
```
npm install
```

#### Compiles and hot-reloads for development
```
npm run serve
```

#### Compiles and minifies for production
```
npm run build
```

#### Lints and fixes files
```
npm run lint
```

#### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).



# Issues


Given the short deadline, the implementation has following issues: 
- onClick and onMouse events are bubbling, so current implementation of onClick event interferes with the logic of dragging and releasing of drag. Thus, onClick is disabled.

- I have tried capturing the event, adding flags to allow click to trigger, but I didn't give me a desired effect

- Animations are lacking. I did try adding them, but CSS animations are not my strongest point, so I decided to focus my time on setting up the core logic and idea that can be easily built upon

- Touch support is missing

- There is no test coverage (although, component can be easily tested with Vue Dev tools, which allow easy modification of Props and Data values, but it's not automated)


# How to use

- Use mouse to drag the elements left and right, untill desired element is bellow "Selected value"

- For keyboard support, click tab until element is in focus. If component has initial view on, click enter and then tab again to focus on the wheel. Use left and right arrow keys to navigate to desired value. *NB! In these kind of situations, when tabbing is detected, it would be nice to prompt a user about posibility of using arrow keys*

# Props and events

There are only a few props for the sake of demo:

- name: name that will be attached to native select. Useful for form submitting
- topText: Text that will be displayed above the elements
- bottomText: Text displayed bellow the elements
- elements: array of strings that will be added into the wheel
- default: default element

- @submit: event emitted from the component when selection of an element is done


# Future upgrades and ideas

- Solve above mentioned issues

- Expose more events for extra flexibility. Perhaps app wants to do something while user is dragging the wheel? Or maybe something when user starts to drag the wheel?

- Make calculation of elemnet sizes and element snapping position more dynamic, and not fixed to one size so that the different element sizes can be accounted for

- Allow for array of objects to be passed, with extra configuration for display text and key

- Allow change of direction of dragging. Perhaps in some cases, app wants to have vertical dragging. 

- different focus styles, to fit better with the enviroment of the app and give a better visual feedback.


# Comments

When I first saw the task, initial question was "why complicate a simple select input?" but it then occured to me that the slider can be easily used in different scenarios where such user input (swiping, dragging) feels more intuitive (like in the Swiper demos, for galeries).

I am by far not an expert in UX and gathering user experiance data, but these were just first thoughts that popped in my mind after seeing the component and then doing a bit of research for usability of these type of elements.

First example would be a mobile divice user - the intuitive thing to do is to swipe top-to-bottom, and many applications out there support this interaction to such a degree that it's intuitive. Breaking user flow with a simple select input that now needs side-to-side swiping doesn't feel like the right thing to do, mainly since swiping side-to-side is usually reserved for opening hidden menus or deleting notifications in status bar.

I this case, a simple top-to-bottom select scroll would do just fine.

On desktop divices is seems to be even worse. Using a mouse to swipe is so far from intuitive behavior, that it doesn't feel fun but rather tideous. In example element, this seems to be somehow dealt with by an onClick on element that scrolls direcltly . I couldn't test if it works with a mouse scroll, since I am using a latop with a touch pad.