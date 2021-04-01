# Quick Start
## Setup Environment

1. **Download and install Docker**

  NT-Core platform depends on docker to start local instances. Please follow this <em>[docker official instruction](https://docs.docker.com/get-started/#download-and-install-docker)</em> to download and install the Docker engine.

2. **Checkout github repository**

  Clone the NT-Core repository via git clone command
  ```
  git clone https://github.com/dsp-columbus/ntcore.git
  ```

3. **Building frontend assets**

  Go to webapp folder under ntcore repository. Build frontend assets
  ```
  cd webapp/
  npm install .
  npm run build
  ```

4. **Starting NT Core server**

  ?> Please make sure your 8180 port is not in use before this step
  ```
    cd ../
    npm run dev
  ```

5. **Go to NT-Core Home page**
  Go to <em>http://localhost:8180/dsp/console/home</em> in your browser. If you can see following NT home page, that means the environment has been setup correctly and you can start play with the platform now.
  <img src="./media/nt-platform-home.png"  />


## Create Your First Model