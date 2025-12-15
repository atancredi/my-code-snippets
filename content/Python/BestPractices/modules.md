# Module importing
### A suitable compromise

Based on the above observations, I have developed the following style in my own code:

**`import module`** is the preferred style if the module name is short as for example most of the packages in the standard library. It is also the preferred style if I need to use names from the module in only two or three places in my own module; clarity trumps brevity then ([_"Readability counts"_](https://www.python.org/dev/peps/pep-0020/)).

**`import longername as ln`** is the preferred style in almost every other case. For instance, I might `import django.contrib.auth.backends as djcab`. By definition of criterion 1 above, the abbreviation will be used frequently and is therefore sufficiently easy to memorize.

Only these two styles are fully pythonic as per the [_"Explicit is better than implicit."_](https://www.python.org/dev/peps/pep-0020/) rule.

**`from module import xx`** still occurs sometimes in my code. I use it in cases where even the `as` format appears exaggerated, the most famous example being `from datetime import datetime` (but if I need more elements, I will `import datetime as dt`).