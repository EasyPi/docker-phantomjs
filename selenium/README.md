selenium-node-phantomjs
=======================

## Hub (1.2.3.4)

```yaml
hub:
  image: selenium/hub
  ports:
    - "4444:4444"
  environment:
    - GRID_TIMEOUT=60
    - GRID_BROWSER_TIMEOUT=30
  restart: always
```

```bash
$ docker-compose up -d hub
```

## Node (5.6.7.8)

```yaml
phantomjs:
  image: easypi/selenium-node-phantomjs
  ports:
    - "5555:5555"
  environment:
    - HOST=5.6.7.8
    - PORT=5555
    - HUB_HOST=1.2.3.4
    - HUB_PORT=4444
  restart: always
```

```bash
$ docker-compose up -d phantomjs
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
