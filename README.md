# client

## About

This repo contains the front end code for [Beginning Brawn](https://beginning-brawn.herokuapp.com/), a weight lifting app that I made to practice making a full stack Vue app. It guides users through a variant of the well-known strength training program, Starting Strength.

Not only can users log their workouts and track their progress, but the app provides personalized suggestions for weight and set/rep schemas that closely follow the Starting Strength programming, with a few of my own alterations, namely alternating squats and deadlifts rather than squatting every workout.

Below you can see an example of a workout log:

![Workout-screencap](https://user-images.githubusercontent.com/78166995/128382249-20998e8a-4c3c-415f-8970-54081aeca202.PNG)

And here is the landing page:

![homepage-screencap](https://user-images.githubusercontent.com/78166995/128382234-1be58aa5-a459-48d2-9076-180e8182cbf6.PNG)

Feel free to make an account and peruse the app.

## Technical Notes

The app is a Single Page App made with Vue js. It uses a Mongo database that is accessed via the API of a separate server I wrote with Express.

If you want to run the project yourself, there are something things you will have to do. First and foremost is setting up this repo:


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

### API Server

After you have this repo up and running, you must configure the API server. Head over to the Beginning-Brawn-Server for more details on the setup.

After you have done this, then the last thing to do is to alter the URI that the Vue app uses to ping the API. This can be found in src/services/Api.js. Within this file, you can change the baseURL for API calls to whatever you set up for your locally hosted server.
