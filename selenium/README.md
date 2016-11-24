selenium-node-phantomjs
=======================

## Hub (1.2.3.4)

```yaml
hub:
  image: selenium/hub
  ports:
    - "4444:4444"
  environment:
    - GRID_TIMEOUT=600
    - GRID_BROWSER_TIMEOUT=60
  restart: always
```

```bash
$ docker-compose up -d hub
```

## Node (5.6.7.8)

```yaml
phantomjs:
  image: easypi/selenium-node-phantomjs
  environment:
    - HOST=5.6.7.8
    - HUB_HOST=1.2.3.4
    - HUB_PORT=4444
  restart: always

phantomjs-1:
  extends:
    service: phantomjs
  ports:
    - "5001:5001"
  environment:
    - PORT=5001

phantomjs-2:
  extends:
    service: phantomjs
  ports:
    - "5002:5002"
  environment:
    - PORT=5002

phantomjs-3:
  extends:
    service: phantomjs
  ports:
    - "5003:5003"
  environment:
    - PORT=5003

phantomjs-4:
  extends:
    service: phantomjs
  ports:
    - "5004:5004"
  environment:
    - PORT=5004
```

```bash
$ docker-compose up -d phantomjs-{1..4}
Creating phantomjs_phantomjs-4_1
Creating phantomjs_phantomjs-3_1
Creating phantomjs_phantomjs-2_1
Creating phantomjs_phantomjs-1_1

$ docker-compose ps
Name                        Command               State                Ports
---------------------------------------------------------------------------------------------------
phantomjs_phantomjs-1_1   /bin/sh -c phantomjs --web ...   Up      0.0.0.0:5001->5001/tcp, 5555/tcp
phantomjs_phantomjs-2_1   /bin/sh -c phantomjs --web ...   Up      0.0.0.0:5002->5002/tcp, 5555/tcp
phantomjs_phantomjs-3_1   /bin/sh -c phantomjs --web ...   Up      0.0.0.0:5003->5003/tcp, 5555/tcp
phantomjs_phantomjs-4_1   /bin/sh -c phantomjs --web ...   Up      0.0.0.0:5004->5004/tcp, 5555/tcp
```

## Client (python)

```python
#!/usr/bin/env python

from selenium import webdriver

phantom = webdriver.Remote(
    command_executor='http://1.2.3.4:4444/wd/hub',
    desired_capabilities=webdriver.DesiredCapabilities.PHANTOMJS
)

phantom.get('http://ifconfig.co')
print phantom.find_element_by_css_selector('code.ip').text
phantom.quit()
```
