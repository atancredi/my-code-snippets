# Logger


```python
import logging
import json
from datetime import datetime

# REGION logger
RESERVED = frozenset(
    (
        "args",
        "asctime",
        "created",
        "exc_info",
        "exc_text",
        "filename",
        "funcName",
        "id",
        "levelname",
        "levelno",
        "lineno",
        "module",
        "msecs",
        "message",
        "msg",
        "name",
        "pathname",
        "process",
        "processName",
        "relativeCreated",
        "stack_info",
        "thread",
        "threadName",
    )
)


class JSONFormatter(logging.Formatter):

    def __init__(self):
        super(JSONFormatter, self).__init__()

    @staticmethod
    def get_extra_keys(record):
        payload = {}
        for key, value in record.__dict__.items():
            if key not in RESERVED and not key.startswith("_"):
                try:
                    json.dumps(record.__dict__[key])  # serialization/type error check
                    payload[key] = record.__dict__[key]
                except TypeError:
                    payload[key] = str(record.__dict__[key])
        return payload

    def format(self, record):
        message = super(JSONFormatter, self).format(record)
        timestamp = datetime.fromtimestamp(record.created).isoformat()[:-3] + "Z"

        payload = {
            "message": message,
            "timestamp": timestamp,
            "thread": record.thread,
            "severity": record.levelname,
        }

        extra = self.get_extra_keys(record)
        if len(extra) > 0:
            payload["extra"] = extra

        return json.dumps(payload)


def get_logger(name=None, level=logging.DEBUG):
    logger = logging.getLogger(name)
    logger.setLevel(level)

    logger_handler = logging.StreamHandler()
    logger_handler.setLevel(level)
    logger_handler.setFormatter(JSONFormatter())

    logger.addHandler(logger_handler)

    return logger

```