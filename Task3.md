# WRITE-UP chết.vn
## Lab 1
Vào Burp là có flag
![image](https://user-images.githubusercontent.com/97771705/222034420-4dff8394-09a9-4498-aad6-71b5617661f4.png)

> FLAG{ddf1ea89-cd89-4da3-98bc-08028ac4dc7e}

## Lab 2
![image](https://user-images.githubusercontent.com/97771705/222038379-d1178f76-f78c-496f-9d96-6648bec6cf6b.png)

Bài này encode theo hướng str_rot13 -> bin2hex -> strrev -> hex2bin -> base64_encode nên ta decode theo hướng ngược lại base64_decode -> hex2bin -> strrev -> bin2hex ->str_rot13
![image](https://user-images.githubusercontent.com/97771705/222037542-7480fd09-3e44-4d86-a13b-ff7bceeb7ccd.png)
```
<?php
print(str_rot13(hex2bin(strrev(bin2hex(base64_decode('1wc39mNzY4NDE1NTk9JD5vbm0mNDQ0PSQ5MHU9L2g+YHQ2NTB7dF5JU1'))))));
```
> FLAG{c564ca8b-5c94-4446-aba4-955148676bfc}

**Từng bị stuck do nhầm lẫn hex2bin là từ dạng hex (0xaa) sang dạng binary (0b0101). Hóa ra hàm hex2bin là chuyển từ hex sang ASCII string :)**

## Lab 3
![image](https://user-images.githubusercontent.com/97771705/222038407-a9a38e55-b058-4f72-8b55-ae711a2a0003.png)

Nếu $number === '1337' thì không in ra flag, nhưng thỏa mãn hàm intval thì sẽ có flag
Hàm intval sẽ trả về giá trị nguyên 'làm tròn xuống' đối với 1 số thập phân. Ta chỉ cần điền ?number=1337.0 là có flag
![image](https://user-images.githubusercontent.com/97771705/222039062-63063d62-eeb6-48f1-853d-43ab5bec74f2.png)

> FLAG{554382f3-960e-4859-9c79-c64ecd4445e7}

## Lab 4
![image](https://user-images.githubusercontent.com/97771705/222039136-e899442d-60cf-4364-89e9-696b08467e8b.png)
```
$_GET[0] != $_GET[1]
Nhưng md5($_GET[0]) === md5($_GET[1]
```
Mình đã tìm kiếm MD5 hash conlision được 2 giá trị khác nhau nhưng chung hash MD5 là 240610708 và QNKCDZO
Nhưng nhập ?0=QNKCDZO&1=240610708 thì không ra kết quả
Sau khi đi hỏi thì mình biết là phải nhập mảng :)
```
?0[]=QNKCDZO&1[]=240610708
```
![image](https://user-images.githubusercontent.com/97771705/222040192-da6feaee-698c-4255-825a-9d965d15e1d7.png)

## Lab 5
![image](https://user-images.githubusercontent.com/97771705/222040698-88cd087d-ac5d-45c5-88af-e9fda0df3e86.png)

Hàm file_get_content sẽ đọc 1 file thành 1 chuỗi string, sau đó simplexml_load_string đọc tất cả các thành phần trong thẻ XML chuẩn. Load theo thứ tự các thẻ:
```
thẻ đầu tiên->f>l>a>g
```
Nếu thẻ g có chứa 'flagggggggggggggg' thì in ra flag

Tạo file trên paste bin https://pastebin.com/raw/nnyrNF4K
```
?inp=https://pastebin.com/raw/nnyrNF4K
```
![image](https://user-images.githubusercontent.com/97771705/222042835-4473d102-cd6d-486c-a9ee-c2556dc1a44d.png)

> FLAG{19ee9d17-7f23-4c03-b702-4276246ccdb2}

## Lab 6
![image](https://user-images.githubusercontent.com/97771705/222042946-e86b0cba-e460-46b1-b1b9-b7b098dbd791.png)

Bài này yêu cầu phần giá trị nhập vào không chứa chữ số, nhưng thỏa mãn inval ???
Search bypass PHP without number https://securityonline.info/bypass-waf-php-webshell-without-numbers-letters/
```
'!'=='@' là 0
Nhập ?number='!'=='@'
```
Nhưng như thế vẫn chưa đúng. Cách làm chính xác là nhập vào 1 mảng và thêm giá trị cho phần tử mảng đó.
![image](https://user-images.githubusercontent.com/97771705/222043729-2911ff5d-fb55-482f-9262-f2f26ae6b1d1.png)

> FLAG{2352ca3b-c94e-450b-b69a-1938cab26571}
## Lab 7
## Lab 8
![image](https://user-images.githubusercontent.com/97771705/222044306-12ae8fea-6695-43da-9023-6c5edade6a30.png)

Bài này ta sử dụng double encode url là ra. encode kí tự 'a' thành '%2561'
```
?u=%2561dmin
```
![image](https://user-images.githubusercontent.com/97771705/222044472-071d61f2-2432-4c69-91e8-4fc4b56c3be2.png)

> FLAG{0406eb88-39ce-4dcc-bace-b058a7e57dd0}

## Lab 9
## Lab 10
