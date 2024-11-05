1. Build a image and then a container with this command:
  
  ```sh
  docker run -d --name nodejs node:latest /bin/bash -c "sleep infinity"
  ```

2. In my case, I'll add an alias to use the node in the container easy:

  ```sh
  # In my case, this is added on .zshrc file
  alias d_nodejs="docker exec -it nodejs"
  ```

3. Common commands (just for save it):

  ```sh
  docker start nodejs     # To start the container
  docker stop nodejs      # To stop the container
  ```
