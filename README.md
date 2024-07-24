import logging

# Create a custom logger
logger = logging.getLogger('my_package_logger')

# Set the default logging level (could be DEBUG, INFO, WARNING, ERROR, CRITICAL)
logger.setLevel(logging.DEBUG)

# Create handlers
file_handler = logging.FileHandler('my_package.log')
file_handler.setLevel(logging.DEBUG)

# Create formatters and add them to handlers
formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
file_handler.setFormatter(formatter)

# Add handlers to the logger
logger.addHandler(file_handler)






**********************
from logging_config import logger

class SomeClass:
    def __init__(self):
        self.logger = logger
        self.logger.info('SomeClass instance created')

    def some_method(self):
        try:
            # Some code that might throw an exception
            pass
        except Exception as e:
            self.logger.error('Error occurred in some_method: %s', e)


******************************
tail -f my_package.log



******************************
