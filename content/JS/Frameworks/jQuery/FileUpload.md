# Upload a file with jQuery AJAX
```js
$.ajax({
	url: '/convert',
	type: 'POST',
	data: fd,
	cache: false,
	contentType: false,
	processData: false,
	success: (res) => {
		console.log(res)
	},
	error: (err) =>{
		console.error("failed")
	},
	complete: () => {
		setTimeout(()=>{$('progress').attr({value: 0});}, 500);
	},
	xhr: function () {
		var myXhr = $.ajaxSettings.xhr();
		if (myXhr.upload) {
			// For handling the progress of the upload
			myXhr.upload.addEventListener('progress', function (e) {
				if (e.lengthComputable) {
					$('progress').attr({
						value: e.loaded,
						max: e.total,
					});
				}
			}, false);
		}
	return myXhr;
	}
});
```