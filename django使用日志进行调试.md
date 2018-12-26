https://stackoverflow.com/questions/5653566/django-print-information-on-production-server



```python
import logging


logging.basicConfig(
    level = logging.INFO,
    format = '%(asctime)s %(levelname)s %(message)s',
    filename = '/tmp/hk-debug.log',)
```



```python
import logging
logging.info(subscribeset_objs)
```

