# Deployment for the Accent Training project

This project is used to deploy a telgeram bot and an accent prediction algorithm.
There are two optional compose files

1. keras_training
   1. Used to train and learn
2. prediciton_bot
   1. Used to serve the mode in a predictor and loads a chatbot that interfaces with it.

The project is seperated into two docker-compose files for convinice, but they share a volume for the saved models


## Development Installation
1. In the same folder clode the following repositories:
   - [accent-bot](https://github.com/guyeshet/accent-bot.git)
   - [keras-accent-trainer](https://github.com/guyeshet/keras-accent-trainer.git)
   - [accent-training-deployment](https://github.com/guyeshet/accent-training-deployment.git)
2. Update *BOT_ID* in **docker-compose.production.yml** with you Telegram Bot ID
3. Update *MODEL_TYPE* and *MODEL_NUM* in **docker-compose.production.yml** with the models you want to serve
4. Stop tracking *docker-compose.production.yml*:
    ```
    git update-index --assume-unchanged docker-compose.production.yml
    ```
5. To continue tracking the file *docker-compose.production.yml*:
    ```
    git update-index --no-assume-unchanged docker-compose.production.yml
    ```

## Choosing a Prediction model
The compose file for production chooses the ID served model
It follows the Comet-ML experiment IDs folder scheme
```
├── saved_models              - parent directory shared in a volume
    ├── <experiment name>     - this folder contains all experiments in a certain type
        ├── <experiment id>   - multiple folder for every experiement.
            |── model.h5      - save Keras model weights
```

## Deployment
After the repositories have been download and the ID configured you should be good to go
```
docker-compose up -d --build
```