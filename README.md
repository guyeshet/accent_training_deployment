# Deployment for the Accent Training project


This project is used to deploy a telgeram bot and an accent prediction algorithm.
It is additionally used to learn and train the model.

There are two optional compose files
1. keras_training
   1. Train and learn the CNN on sound files
2. prediciton_bot
   1. Serves the model as a microservice and starts a Telegram chatbot that communicates with it.

The project is seperated into two docker-compose files for convinice which share a volume for the saved models

## Development Installation
- Clone the following repositories in the same directory:
   - [accent-bot](https://github.com/guyeshet/accent-bot.git)
   - [keras-accent-trainer](https://github.com/guyeshet/keras-accent-trainer.git)
   - [accent-training-deployment](https://github.com/guyeshet/accent-training-deployment.git)

## Deploying keras_training
To load the artifacts and create the csv file:
```
docker-compose -f "keras_training/docker-compose.yml" up --build
```

To run the prediction
```
docker-compose -f "keras_training/docker-compose.predict.yml" up
```

## Choosing a Prediction model
The compose file for production chooses the ID served model.
When running through compose the saved models are located in */share/saved_models*
It follows the Comet-ML experiment IDs folder scheme
```
├── saved_models              - parent directory shared in a volume
    ├── <experiment name>     - this folder contains all experiments in a certain type
        ├── <experiment id>   - multiple folder for every experiement.
            |── model.h5      - save Keras model weights
```

## Deploying prediction_bot
1. Update *BOT_ID* in **docker-compose.production.yml** with your Telegram Bot ID
2. Update *MODEL_TYPE* and *MODEL_NUM* in **docker-compose.production.yml** with the models you want to serve
   The model depend on the model you trained in the previous stage
3. Stop tracking *docker-compose.production.yml*:
    ```
    git update-index --assume-unchanged docker-compose.production.yml
    ```
4. To continue tracking the file *docker-compose.production.yml*:
    ```
    git update-index --no-assume-unchanged docker-compose.production.yml
    ```
5. run:
    ```
    docker-compose -f "prediction_bot/docker-compose.yml" -f "prediction_bot/docker-compose.production.yml" up -d --build
    ```