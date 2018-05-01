# Build a Calculator with Vue.js

## Usage

```sh
npm install -g @vue/cli
vue serve App.vue
```

## Build

```sh
vue build App.vue
```

## Steps

* [x] Create App.vue
* [x] Create basic layout
* [x] Store buttons on data
  * text
  * type
* [ ] Style it!
  * [x] Button Spacing
  * [x] Button Colors
  * [x] Button Hover
  * [x] Responsive
    * [x] Size based on screen size
    * [x] Centered
* [ ] Make it work!
  * [x] Number button appends displayed number to screen
  * [x] AC button clears the display
  * [x] +/- button inverts the display value and updates the display
  * [x] Operation button (+ - ÷ ×) stores displayed number/sets operation
  * [x] Number button pressed after operation, update display with new number
  * [x] Equals button performs operation and updates display
  * [x] Period sets decimal, cannot add more than 1 period
  * [x] Should not allow zero at beginning of number
* [ ] *STRETCH* Display text grows/shrinks to length of display
