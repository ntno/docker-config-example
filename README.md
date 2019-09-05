# docker-config-example

### build development image 
`docker build hello-verity -t hello-verity --build-arg ENVIRON=dev`
or
`export ENVIRON=dev && docker build hello-verity -t hello-verity --build-arg ENVIRON=$ENVIRON`


### build prod image
`docker build hello-verity -t hello-verity --build-arg ENVIRON=prod`
or
`export ENVIRON=prod && docker build hello-verity -t hello-verity --build-arg ENVIRON=$ENVIRON`


*Note: image will default to the dev configuration if no build argument is specified*





## Future Enhancements

