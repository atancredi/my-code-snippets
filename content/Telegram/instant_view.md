
https://gist.github.com/HoussenMoshine/16599d3c977aac616bfb27a9ecc6a3b5
https://gist.github.com/imsus/9323848eac3eac4afe412f5d43ee0ea5

https://instantview.telegram.org/my/
https://instantview.telegram.org/samples/
https://instantview.telegram.org/docs
https://instantview.telegram.org/#templates-tutorial

# After creating template


### cool trick to remove empty blocks
```
# blocks like: <p>&nbsp;</p>
@replace("\u00a0", "null-tag"): //p
@remove: //p[.="null-tag"]
```
