---
title: "CF 20A - Hệ thống tập tin BerOS"
description: "Chúng tôi được cung cấp một đường dẫn hệ thống tập tin dưới dạng một chuỗi. Trong hệ điều hành này, nhiều ký tự '/' liên tiếp được xử lý giống hệt như một ký tự '/'. Điều đó có nghĩa là các đường dẫn như ///home//user///docs và /home/user/docs tham chiếu đến cùng một vị trí."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 20
codeforces_index: "A"
codeforces_contest_name: "Codeforces Alpha Round 20 (Codeforces format)"
rating: 1700
weight: 20
solve_time_s: 80
verified: true
draft: false
---
[CF 20A - Hệ thống tệp BerOS](https://codeforces.com/problemset/problem/20/A) 

**Đánh giá:** 1700 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một đường dẫn hệ thống tập tin dưới dạng một chuỗi. Trong hệ điều hành này, nhiều liên tiếp`'/'`các ký tự được xử lý giống hệt như một ký tự đơn`'/'`. Điều đó có nghĩa là những con đường như`///home//user///docs`Và`/home/user/docs`đề cập đến cùng một vị trí. 

Nhiệm vụ là chuyển đổi đường dẫn thành dạng chuẩn hóa của nó. Một đường dẫn chuẩn hóa chỉ giữ một`'/'`giữa các tên thư mục. Ngoại lệ duy nhất là chính thư mục gốc, thư mục này phải được giữ nguyên chính xác.`"/"`. 

Độ dài đầu vào tối đa là 100 ký tự, cực kỳ nhỏ. Ngay cả các nghiệm bậc hai cũng có thể chạy thoải mái trong giới hạn. Quét tuyến tính vẫn là cách tiếp cận rõ ràng nhất vì vấn đề đương nhiên yêu cầu chúng ta xử lý từng ký tự một và quyết định xem nên giữ hay loại bỏ từng dấu gạch chéo. 

Phần khó khăn là xử lý dấu gạch chéo ở cuối một cách chính xác. Một con đường như:```
/usr/local///
```phải trở thành:```
/usr/local
```Việc triển khai bất cẩn chỉ nén các dấu gạch chéo liên tiếp sẽ tạo ra không chính xác:```
/usr/local/
```Một trường hợp khác là chính thư mục gốc. Ví dụ:```
////
```nên bình thường hóa thành:```
/
```Nếu chúng ta mù quáng loại bỏ tất cả các dấu gạch chéo lặp lại và sau đó loại bỏ các dấu gạch chéo ở cuối, chúng ta có thể vô tình tạo ra một chuỗi trống thay vì đường dẫn gốc được yêu cầu. 

Một trường hợp tinh tế hơn là khi có dấu gạch chéo ở giữa:```
/a////b///c
```Đầu ra đúng là:```
/a/b/c
```Mỗi nhóm dấu gạch chéo liên tiếp phải thu gọn thành đúng một dấu gạch chéo chứ không phải bằng 0. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ liên tục tìm kiếm trong chuỗi các lần xuất hiện của`"//"`và thay thế chúng bằng`"/"`cho đến khi không còn dấu gạch chéo kép. Sau đó, chúng ta có thể xóa dấu gạch chéo ở cuối nếu đường dẫn kết quả có độ dài lớn hơn một. 

Điều này có hiệu quả vì mỗi lần thay thế sẽ rút ngắn chuỗi và cuối cùng không còn dấu gạch chéo liên tiếp. Với độ dài chuỗi chỉ 100, ngay cả việc thay thế lặp đi lặp lại cũng đủ nhanh. 

Điểm yếu là về nguyên tắc việc tái cấu trúc chuỗi lặp đi lặp lại không hiệu quả. Giả sử độ dài chuỗi lớn hơn nhiều, chẳng hạn`10^5`. Mỗi lần thay thế có thể mất thời gian tuyến tính vì các chuỗi là bất biến và chúng tôi có thể thực hiện nhiều lần thay thế. Trong trường hợp xấu nhất, đại loại như:```
//////////////////////////////////////////////////
```sẽ liên tục thu nhỏ từng ký tự một, dẫn đến hành vi bậc hai. 

Quan sát tốt hơn là chúng ta thực sự không bao giờ cần phải xem lại các ký tự trước đó. Khi quét từ trái sang phải, điều quan trọng duy nhất là liệu ký tự trước đó chúng ta giữ có phải là`'/'`. 

Nếu ký tự hiện tại không`'/'`, chúng tôi luôn giữ nó. 

Nếu ký tự hiện tại là`'/'`, chúng tôi chỉ giữ nó khi ký tự được giữ trước đó không còn nữa`'/'`. 

Điều này biến vấn đề thành một đường truyền tuyến tính duy nhất trên chuỗi. 

Sau khi xây dựng đường dẫn nén, chúng ta vẫn cần xử lý dấu gạch chéo ở cuối. Mọi dấu gạch chéo ở cuối phải được loại bỏ trừ khi toàn bộ đường dẫn chỉ`"/"`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Đã chấp nhận | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đường dẫn đầu vào. 
2. Tạo một danh sách trống sẽ lưu trữ các ký tự được chuẩn hóa. 
3. Lặp lại từng ký tự trong đường dẫn. 
4. Nếu ký tự hiện tại không`'/'`, nối nó trực tiếp vào kết quả. 

Tên và chữ cái thư mục phải luôn không thay đổi. 
5. Nếu ký tự hiện tại là`'/'`, kiểm tra ký tự cuối cùng đã được lưu trong kết quả. 

Nếu kết quả trống hoặc ký tự được lưu trữ cuối cùng không`'/'`, nối thêm dấu gạch chéo. 

Nếu không thì bỏ qua, vì dấu gạch chéo liên tiếp tương đương với một dấu gạch chéo. 
6. Chuyển đổi các ký tự đã thu thập thành chuỗi. 
7. Xóa dấu gạch chéo ở cuối khi độ dài chuỗi lớn hơn một. 

điều kiện`length > 1`bảo vệ đường dẫn thư mục gốc`"/"`khỏi trở nên trống rỗng. 
8. In đường dẫn chuẩn hóa cuối cùng. 

### Tại sao nó hoạt động 

Thuật toán duy trì một bất biến đơn giản trong quá trình quét: kết quả được xây dựng một phần không bao giờ chứa hai dấu gạch chéo liên tiếp. 

Bất cứ khi nào chúng tôi gặp dấu gạch chéo, chúng tôi chỉ thêm nó nếu ký tự được giữ trước đó không phải là dấu gạch chéo. Điều đó đảm bảo mọi khối dấu gạch chéo tối đa trong chuỗi gốc sẽ trở thành chính xác một dấu gạch chéo trong kết quả. 

Tất cả các ký tự không gạch chéo được sao chép không thay đổi và theo thứ tự, do đó tên thư mục vẫn được giữ nguyên. 

Cuối cùng, việc loại bỏ các dấu gạch chéo ở cuối trong khi vẫn giữ lại ít nhất một ký tự sẽ đảm bảo rằng các đường dẫn như`"/usr///"`trở nên`"/usr"`trong khi các đường dẫn đại diện cho thư mục gốc vẫn giữ nguyên chính xác`"/"`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

path = input().strip()

result = []

for ch in path:
    if ch != '/':
        result.append(ch)
    else:
        if not result or result[-1] != '/':
            result.append('/')

ans = ''.join(result)

while len(ans) > 1 and ans[-1] == '/':
    ans = ans[:-1]

print(ans)
```Phần đầu tiên của mã thực hiện quét tuyến tính. các`result`list hoạt động như một trình tạo chuỗi có thể thay đổi, hiệu quả hơn so với việc nối các chuỗi liên tục. 

Điều kiện:```
if not result or result[-1] != '/':
```là bước then chốt. Nó quyết định xem dấu gạch chéo hiện tại bắt đầu một khối phân cách mới hay thuộc về một khối đã tồn tại. 

Việc sử dụng danh sách thay vì nối chuỗi sẽ tránh việc xây dựng lại các chuỗi trung gian không cần thiết. Với một hạn chế nhỏ như vậy, điều này không bắt buộc nhưng đây là cách thực hành lập trình cạnh tranh tốt. 

trận chung kết`while`vòng lặp loại bỏ dấu gạch chéo ở cuối một cách cẩn thận:```
while len(ans) > 1 and ans[-1] == '/':
```các`len(ans) > 1`tình trạng đang rất nguy kịch. Không có nó, đầu vào:```
////
```sẽ trở thành một chuỗi trống không chính xác thay vì`"/"`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
//usr///local//nginx/sbin
```| Nhân vật hiện tại | Kết quả trước | Hành động | Kết quả sau | 
| --- | --- | --- | --- | 
|`/`|`""`| giữ dấu gạch chéo |`/`| 
|`/`|`/`| bỏ qua dấu gạch chéo |`/`| 
|`u`|`/`| giữ char |`/u`| 
|`s`|`/u`| giữ char |`/us`| 
|`r`|`/us`| giữ char |`/usr`| 
|`/`|`/usr`| giữ dấu gạch chéo |`/usr/`| 
|`/`|`/usr/`| bỏ qua dấu gạch chéo |`/usr/`| 
|`/`|`/usr/`| bỏ qua dấu gạch chéo |`/usr/`| 
|`l`|`/usr/`| giữ char |`/usr/l`| 

Quá trình tương tự tiếp tục cho các ký tự còn lại. Mỗi khối gạch chéo sẽ sụp đổ thành một dấu gạch chéo. 

Đầu ra cuối cùng:```
/usr/local/nginx/sbin
```Dấu vết này thể hiện sự bất biến rằng kết quả được xây dựng không bao giờ chứa các dấu gạch chéo liên tiếp. 

### Ví dụ 2 

đầu vào:```
/////
```| Nhân vật hiện tại | Kết quả trước | Hành động | Kết quả sau | 
| --- | --- | --- | --- | 
|`/`|`""`| giữ dấu gạch chéo |`/`| 
|`/`|`/`| bỏ qua dấu gạch chéo |`/`| 
|`/`|`/`| bỏ qua dấu gạch chéo |`/`| 
|`/`|`/`| bỏ qua dấu gạch chéo |`/`| 
|`/`|`/`| bỏ qua dấu gạch chéo |`/`| 

Sau khi quét, kết quả trung gian là:```
/
```Việc dọn dẹp dấu gạch chéo ở cuối không làm gì cả vì độ dài chuỗi đã được`1`. 

Đầu ra cuối cùng:```
/
```Ví dụ này xác minh rằng thư mục gốc được bảo tồn chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần | 
| Không gian | O(n) | Danh sách kết quả lưu trữ đường dẫn chuẩn hóa | 

Độ dài đầu vào tối đa chỉ là 100 ký tự nên thuật toán dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. Ngay cả những cách tiếp cận chậm hơn cũng sẽ vượt qua, nhưng quét tuyến tính là giải pháp rõ ràng nhất và có khả năng mở rộng nhất. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    import sys
    input = sys.stdin.readline

    path = input().strip()

    result = []

    for ch in path:
        if ch != '/':
            result.append(ch)
        else:
            if not result or result[-1] != '/':
                result.append('/')

    ans = ''.join(result)

    while len(ans) > 1 and ans[-1] == '/':
        ans = ans[:-1]

    print(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    backup_stdout = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup_stdout

    return out.getvalue().strip()

# provided sample
assert run("//usr///local//nginx/sbin\n") == "/usr/local/nginx/sbin", "sample 1"

# minimum-size input
assert run("/\n") == "/", "single root slash"

# multiple slashes representing root
assert run("//////\n") == "/", "root normalization"

# trailing slash removal
assert run("/usr/local///\n") == "/usr/local", "remove trailing slashes"

# consecutive middle slashes
assert run("/a////b///c\n") == "/a/b/c", "compress middle slashes"

# already normalized path
assert run("/home/user/docs\n") == "/home/user/docs", "unchanged normalized path"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`/`|`/`| Đầu vào hợp lệ tối thiểu | 
|`//////`|`/`| Bảo quản thư mục gốc | 
|`/usr/local///`|`/usr/local`| Dọn dẹp dấu gạch chéo | 
|`/a////b///c`|`/a/b/c`| Nén các khối gạch chéo nội bộ | 
|`/home/user/docs`|`/home/user/docs`| Đường dẫn đã được chuẩn hóa vẫn không thay đổi | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
/usr/local///
```Trong quá trình quét, thuật toán sẽ nén dữ liệu cuối cùng`///`thành một dấu gạch chéo duy nhất, tạo ra:```
/usr/local/
```Sau đó, vòng lặp dọn dẹp sẽ loại bỏ dấu gạch chéo ở cuối vì độ dài chuỗi lớn hơn một. Câu trả lời cuối cùng trở thành:```
/ usr/local
```không có không gian:```
/usr/local
```Điều này xác nhận rằng các đường dẫn không phải gốc không bao giờ kết thúc bằng các dấu gạch chéo không cần thiết. 

Bây giờ hãy xem xét:```
////
```Quá trình quét chỉ giữ lại dấu gạch chéo đầu tiên và bỏ qua tất cả những dấu gạch chéo còn lại. Kết quả trung gian là:```
/
```Vòng lặp dọn dẹp kiểm tra:```
len(ans) > 1
```Vì chiều dài bằng`1`, không có gì bị loại bỏ. Thư mục gốc vẫn hợp lệ. 

Cuối cùng, hãy xem xét:```
/a////b///c
```Quá trình quét xử lý từng nhóm dấu gạch chéo một cách độc lập. Mỗi khối liên tiếp tối đa sẽ trở thành một dấu gạch chéo, tạo ra:```
/ a / b / c
```không có khoảng trống:```
/ a/b/c
```và cuối cùng:```
/a/b/c
```Điều này xác nhận rằng thuật toán bảo toàn cấu trúc thư mục trong khi loại bỏ các dấu phân cách dư thừa.
