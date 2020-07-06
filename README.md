# send-ajax
Make ajax requests without jQuery

### Installation
```$xslt
$ npm i send-ajax
```

Example with Promise:

```
import sendAjax from 'send-ajax';

const postRequest;

const postData = (ajaxUrl, data = {}) => {
  return new Promise((resolve, reject) => {
    postRequest = sendAjax({
      url: ajaxUrl,
      method: 'POST',
      data,
      responseType: 'json',
      success: data => {
        resolve(data);
      },
      error: err => {
        reject({ error: true, message: 'Error message', data: err });
      },
    });
  });
};

const sendDataToServer = (url, data = {}) => {
  postData(url, data)
    .then(response => {
      console.log(response);
    })
    .catch(error => {
      console.error(error);
    })
    .finally(() => {
      // always executed
    });
};
```

Abort request:
```$xslt
if (postRequest) {
  postRequest.abort();
}
```

Example without Promise:

```$xslt
import sendAjax from 'send-ajax';

const postData = (ajaxUrl, data, success, error, after) => {
  postRequest = sendAjax({
    url: ajaxUrl,
    method: 'POST',
    data,
    responseType: 'json',
    success: data => {
      success(data);
    },
    error: err => {
      error(err);
    },
    after: () => {
      // always executed
    }
  });
};

const sendDataToServer = (url, data = {}) => {
  const postRequest = postData(
    url,
    data,
    response => {
      console.log(response);
    },
    error => {
      console.error(error);
    },
    () => {
      // always executed
    },
  );
  if (postRequest) {
    postRequest.abort();
  }
};
```

### Request Config:
These are the available config options for making requests:

**url** - request url. Default ``'/'``.

**method** - request method. Default ```'GET'```.

**before** - function for execute before request send. Default ```null```.

**after** - function for execute after request send regardless of success response. Default ```null```.

**contentType** - sending content type. Default ```application/x-www-form-urlencoded```.

**responseType** - type of response data. Default ```text```.

**data** - sending data (for POST, PATCH or CREATE requests). Default ```{}```.

**async** - make request async. Default ```true```.

**timeout** - maximum server response timeout in milliseconds. Default ```0``` which means there is no timeout.

**credentials** - allow or not allow cookies. Default ```false```.

**success** - function for execute when request is success. Default ```null```.

**error** - function for execute when request came with an error. Default ```null```.

**headers** - request headers array. Default ```[]```.

## License

[MIT](https://github.com/axios/axios/blob/HEAD/LICENSE)
