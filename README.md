# Starwars Vue Application

## Live
Site is live at [https://stupefied-beaver-32cd52.netlify.com](https://stupefied-beaver-32cd52.netlify.com)

## To do (in order of completed):

1. [x] See a list of all starships
2. [x] See individual starship
3. [x] See info on pilots attached to individual starships
4. [x] Search list
5. [x] Filter list by price
6. Sort list by price
 
## Issues

See Home.vue line 86, we are hitting an infinite loop, I suspect its because our array in the nested loop is growing infinetely and we dont have good exit conditions/the if statement is not specific enough

## To run

To run this project, either visit the link at the top of the readme or follow the below instructions
1. in terminal, type the following:
2. `git clone https://github.com/riderjensen/starwars-vue.git`
3. `cd starwars-vue`
4. `npm install`*
5. `npm run serve`
6. The application should be running on localhost:8080

*Note that you should have nodejs (and npm that installs along with it) installed before you can run this project.

## To Build
Run `npm run build` and a dist/ folder will be generated for you
