https://www.digitalocean.com/community/tutorials/how-to-package-and-distribute-python-applications

&nbsp;
# Setup.py
```python
from distutils.core import setup
from setuptools import find_packages
import os

current_directory = os.path.dirname(os.path.abspath(__file__))

try:
	with open(os.path.join(current_directory, 'README.md'), encoding='utf-8') as f:
	long_description = f.read()
except Exception:
	long_description = ''

setup(
	name='test',
	packages=find_packages(','),
	version='0.0.1',
	license='',
	description='',
	long_description=long_description,
	long_description_content_type='text/markdown',
	author='',
	author_email='',
	url='',
	download_url='',
	keywords=[],
	install_requires=[],
	classifiers=[]
)
```