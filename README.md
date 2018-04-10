# Delhi High Court Website Scraper #

# README #

This project uses splash to first render a webpage in docker and then parse it using scrapy. This approach is well suited for JS enabled websites.

## Steps for Server Execution ##
* run the virtual environment by moving to the project directory `cd assignment/`
* a folder `env` should be visible now. Run `source env/bin/activate` to start the virtual environment
* Run spider with:
```
$ scrapy crawl test
```
* A file `json_file.txt` will be created having all the data.

### The Working ###

* The URLs that need to be parsed are first placed in the urls.txt file
* The case type, case number, case year, petitioner, respondent, advocate, court number, next date and last date and city is parsed from the given url page
* The code will send a GET request to the sever to get the 'created url' which contains the details which we want to scrape.
* Then we apply `regex` to clean the data.
* The data is then converted to dictionary and then parsed in json format to be written to the file.
* Error log is maintained in `error.log` file

### Steps To Run ###

* Docker needs to be running([Install Here](https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/#install-docker-ce))
* Pull Docker Image : 
```
sudo docker pull scrapinghub/splash:2.3.3
```
* Start Container :
```
$ sudo docker run -d --restart=always -p 8050:8050 -p 5023:5023 scrapinghub/splash:2.3.3 --max-timeout 99999`

```
### Dependencies ###
`scrapy`
`scrapy-splash`
`requests`
`lxml`
`bs4`

 The exact dependencies can be downloaded by running `pip install -r requirements.txt`

* install requests using `pip install requests`

Run these:
`sudo apt-get install libxml2-dev libxslt1-dev python3-dev python3-setuptools`


### Common Issues ###

* The most common issue seems to be splash. This script uses splash 2.3.3. Splashv3 was released but does not work well with the script. Always run the container with `--max-timeout` arguement set to a large value. This is the time after which splash drops the request execution, default is 60 sec.
* Scrapy is asynchronous. It generates all the requests and queues them. So there is a possibility that a request generated late in the code can be executed earlier or some other variation of this. Best possible way to overcome this is to set the `priority` arguement of scrapy in order in which you want the requests to be executed. Higher the number, higher the priority.
* Splash settings need to be set in settings.py. [See Here](https://github.com/scrapy-plugins/scrapy-splash)
* The wait times can be altered but do not decrease it way down. If you get error like image_urls not found or a variation of this then increase the wait times and run the script again. Lesser wait times might decrease program execution but if the wait period times out before the server respondes(or due to connection problems) image might not be scraped.
* On server `sudo apt-get install build-essential libssl-dev python3-dev` as these may cause problems in scrapy installation

### Make a virtual environment(optional) ###
* run `virtualenv -p python3 env` inside the project directory
