# reguest
http://52.59.124.14:10014

app.py
```
from flask import Flask, Response, request
import requests
import io

app = Flask(__name__)


@app.route('/')
def index():
	s = requests.Session()
	cookies = {'role': 'guest'}

	output = io.StringIO()
	output.write("Usage: Look at the code ;-)\n\n")
	try:
		output.write("Overwriting cookies with default value! This must be secure!\n")
		cookies = {**dict(request.cookies), **cookies}
		headers = {**dict(request.headers)}

		if cookies['role'] != 'guest':
			raise Exception("Illegal access!")

		r = requests.Request("GET", "http://backend:8080/whoami", cookies=cookies, headers=headers)
		prep = r.prepare()

		output.write("Prepared request cookies are: ")
		output.write(str(prep._cookies.items()))
		output.write("\n")
		output.write("Sending request...")
		output.write("\n")
		
		resp = s.send(prep, timeout=2.0)
		
		output.write("Request cookies are: ")
		output.write(str(resp.request._cookies.items()))
		output.write("\n\n")
		if 'Admin' in resp.content.decode():
			output.write("Someone's drunk oO\n\n")
		output.write("Response is: ")
		output.write(resp.content.decode())
		output.write("\n\n")
	except Exception as e:
		print(e)
		output.write("Error :-/" + str(e))
		output.write("\n\n")

	return Response(output.getvalue(), mimetype='text/plain')


if __name__ == "__main__":
	app.run(host='0.0.0.0', port='8080', debug=False)
```

backend.py
```
import os
from flask import Flask, request, Response

app = Flask(__name__)


@app.route('/whoami')
def whoami():
	role = request.cookies.get('role','guest')
	really = request.cookies.get('really', 'no')
	if role == 'admin':
		if really == 'yes':
			resp = 'Admin: ' + os.environ['FLAG']
		else:
			resp = 'Guest: Nope'
	else:
		resp = 'Guest: Nope'
	return Response(resp, mimetype='text/plain')

if __name__ == "__main__":
	app.run(host='0.0.0.0', port='8080', debug=False)
```

Mới vào trang có dạng 

![image](https://user-images.githubusercontent.com/97771705/224088900-a7ccd312-9399-4640-9582-8591cf5fca04.png)

Chú ý tại app.py
```
cookies = {**dict(request.cookies), **cookies}
```

Câu lệnh này lấy cặp giá trị key-value từ Cookie Header rồi tạo ra 1 dictionary mới -> Gợi ý ta sử dụng Header Cookie để input payload

Tại backend
```
  role = request.cookies.get('role','guest')
	really = request.cookies.get('really', 'no')
```  
'role' sẽ có giá trị của key=role từ Cookie header được gửi từ client, nếu không sẽ gán giá trị guest. Tương tự với 'really'

-> Ta phải gửi 2 cặp key-value: role và really

Khi role=admin và really=yes -> có flag

Payload rại burp

```Cookie: really=yes;role=admin```

![image](https://user-images.githubusercontent.com/97771705/224090802-06a87925-8318-403e-974b-873f2a4d0419.png)

>Flag:  ENO{R3Qu3sts_4r3_s0m3T1m3s_we1rd_dont_get_confused}


