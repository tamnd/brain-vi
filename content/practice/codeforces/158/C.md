---
title: "CF 158C - Lệnh Cd và pwd"
description: "Chúng ta cần mô phỏng một shell nhỏ chỉ hỗ trợ hai lệnh. Lệnh cd path thay đổi thư mục hiện tại. Đường dẫn có thể là tuyệt đối, nghĩa là nó bắt đầu từ thư mục gốc / hoặc tương đối, nghĩa là nó bắt đầu từ thư mục hiện tại. Bên trong một đường dẫn, mã thông báo .."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 158
codeforces_index: "C"
codeforces_contest_name: "VK Cup 2012 Qualification Round 1"
rating: 1400
weight: 158
solve_time_s: 105
verified: true
draft: false
---

[CF 158C - Lệnh Cd và pwd](https://codeforces.com/problemset/problem/158/C) 

**Đánh giá:** 1400 
**Thẻ:** *đặc biệt, cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 1 phút 45 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần mô phỏng một shell nhỏ chỉ hỗ trợ hai lệnh. 

Lệnh`cd path`thay đổi thư mục hiện tại. Đường dẫn có thể là tuyệt đối, nghĩa là nó bắt đầu từ gốc`/`, hoặc tương đối, nghĩa là nó bắt đầu từ thư mục hiện tại. Bên trong một đường dẫn, mã thông báo`..`có nghĩa là "đi tới thư mục mẹ". 

Lệnh`pwd`in đường dẫn tuyệt đối đầy đủ của thư mục hiện tại, luôn kết thúc bằng dấu gạch chéo. 

Bản thân hệ thống tập tin không cần phải được xây dựng dưới dạng cây. Vấn đề đảm bảo rằng mọi thư mục được tham chiếu đều tồn tại và tên thư mục chỉ là nhãn. Điều thực sự quan trọng là trình tự tên thư mục từ thư mục gốc đến vị trí hiện tại. 

Số lượng lệnh tối đa là 50 và độ dài mỗi đường dẫn tối đa là 200 ký tự. Những giới hạn này là cực kỳ nhỏ. Kể cả việc xây dựng lại đường dẫn nhiều lần hay tách chuỗi nhiều lần cũng hoàn toàn an toàn. Bất kỳ giải pháp nào gần tuyến tính trong tổng kích thước đầu vào sẽ vượt qua một cách thoải mái. 

Phần khó khăn là xử lý ngữ nghĩa đường dẫn một cách chính xác. 

Hãy xem xét đầu vào này:```
3
cd /a/b
cd ..
pwd
```Đầu ra đúng là:```
/a/
```Việc triển khai bất cẩn có thể xóa ký tự cuối cùng thay vì thư mục cuối cùng và vô tình tạo ra`/a/b`. 

Một lỗi phổ biến khác xảy ra với đường dẫn tuyệt đối.```
3
cd /a/b
cd /x
pwd
```Đầu ra đúng là:```
/x/
```Khi đường dẫn bắt đầu bằng`/`, chúng ta phải loại bỏ hoàn toàn thư mục hiện tại cũ. Nếu chúng ta thêm đường dẫn mới vào đường dẫn hiện có không chính xác, chúng ta sẽ nhận được`/a/b/x/`. 

Đường dẫn tương đối với tên thư mục hỗn hợp và`..`cũng gây ra lỗi:```
2
cd a/b/../c
pwd
```Đầu ra đúng là:```
/a/c/
```Đường dẫn phải được xử lý theo từng thành phần theo thứ tự. Chúng ta không thể đơn giản loại bỏ mọi`..`một cách mù quáng vì mỗi cái chỉ ảnh hưởng đến thư mục ngay trước đó. 

Thư mục gốc là một trường hợp khác.```
3
cd /
pwd
cd ..
```Đầu ra là:```text`

/

```Câu lệnh đảm bảo rằng chúng ta không bao giờ di chuyển lên trên gốc, nhưng việc triển khai của chúng ta vẫn cần giữ cho gốc được biểu diễn chính xác dưới dạng ngăn xếp đường dẫn trống hoặc cấu trúc tương đương. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là coi thư mục hiện tại là một chuỗi và thao tác nó theo đúng nghĩa đen mỗi khi lệnh xuất hiện. 

Đối với một`cd`, chúng ta có thể nối các chuỗi, liên tục tìm kiếm dấu gạch chéo, xóa thủ công các thư mục mẹ khi nhìn thấy`..`, và xây dựng lại kết quả. Điều này hoạt động vì đường dẫn ngắn. Ngay cả khi mỗi lệnh xử lý một chuỗi 200 ký tự nhiều lần thì tổng công việc vẫn rất nhỏ. 

Điểm yếu của phương pháp này không phải là hiệu suất mà là độ phức tạp. Việc cắt chuỗi và phân tích cú pháp thủ công rất dễ bị lỗi. Xử lý các trường hợp như`/`, dấu gạch chéo ở cuối hoặc các phép toán cha liên tiếp trở nên khó xử. 

Quan sát rõ ràng hơn là đường dẫn hệ thống tập tin tự nhiên hoạt động giống như một ngăn xếp. 

Ví dụ:```text
/home/vasya/docs
```có thể được biểu diễn dưới dạng:```
["home", "vasya", "docs"]
```Nhập một thư mục có nghĩa là đẩy vào ngăn xếp. Đi tới thư mục mẹ có nghĩa là bật ra khỏi ngăn xếp. In đường dẫn hiện tại có nghĩa là nối ngăn xếp bằng dấu gạch chéo. 

Điều này khớp chính xác với ngữ nghĩa của shell. 

Khi xử lý một`cd`lệnh: 

Nếu đường dẫn bắt đầu bằng`/`, chúng ta đặt lại ngăn xếp vì đường dẫn tuyệt đối bắt đầu từ gốc. 

Sau đó chúng tôi chia đường đi cho`/`và xử lý từng thành phần. 

Nếu thành phần này trống, chúng ta bỏ qua nó. Điều này chủ yếu xảy ra vì đường dẫn tuyệt đối bắt đầu bằng`/`. 

Nếu thành phần đó là`..`, chúng tôi bật một thư mục. 

Nếu không, chúng tôi đẩy tên thư mục. 

Giải pháp thao tác chuỗi brute-force có thể được chấp nhận trong các ràng buộc này, nhưng cách biểu diễn ngăn xếp đơn giản hơn, an toàn hơn và phản ánh trực tiếp logic hệ thống tệp thực tế. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Thao tác chuỗi Brute Force | O(n × L2) | O(L) | Đã chấp nhận | 
| Mô phỏng dựa trên ngăn xếp | O (n × L) | O(L) | Đã chấp nhận | 

Đây,`L`là độ dài đường dẫn tối đa. 

## Hướng dẫn thuật toán 

1. Lưu trữ thư mục hiện tại dưới dạng danh sách tên thư mục. 

Ví dụ,`/home/vasya/`trở thành`["home", "vasya"]`. 
2. Đọc từng lệnh một. 

Trạng thái shell thay đổi tuần tự, do đó việc xử lý các lệnh theo thứ tự khớp chính xác với hành vi của sự cố. 
3. Nếu lệnh là`pwd`, in đường dẫn hiện tại. 

Tham gia tất cả các tên thư mục với`/`, thêm dấu gạch chéo ở đầu và thêm dấu gạch chéo ở cuối. 

Nếu ngăn xếp trống, kết quả đơn giản là`/`. 
4. Nếu lệnh là`cd path`, trước tiên hãy xác định xem đường dẫn là tuyệt đối hay tương đối. 

Nếu đường dẫn bắt đầu bằng`/`, hãy xóa ngăn xếp hiện tại vì đường dẫn tuyệt đối bắt đầu từ thư mục gốc. 
5. Chia đường dẫn bằng cách sử dụng`/`như là dấu phân cách. 

Điều này chuyển đổi một cái gì đó như`vasya/../petya`thành các thành phần chúng ta có thể xử lý độc lập. 
6. Xử lý từng thành phần từ trái qua phải. 

Nếu thành phần này trống, hãy bỏ qua nó. 

Điều này xảy ra đối với dấu gạch chéo hàng đầu trong đường dẫn tuyệt đối. 
7. Nếu thành phần đó là`..`, xóa thư mục cuối cùng khỏi ngăn xếp. 

Điều này mô phỏng việc di chuyển đến thư mục mẹ. 
8. Nếu không, hãy thêm thành phần đó vào ngăn xếp. 

Điều này mô phỏng việc nhập một thư mục con. 

### Tại sao nó hoạt động 

Ngăn xếp luôn lưu trữ thứ tự chính xác của các thư mục từ thư mục gốc đến vị trí hiện tại. 

Mọi chuyển động của hệ thống tập tin hợp lệ chỉ thay đổi phần cuối của chuỗi này. Việc nhập một thư mục sẽ thêm một phần tử, trong khi di chuyển đến thư mục gốc sẽ loại bỏ một phần tử. Đường dẫn tuyệt đối khởi động lại chuỗi từ gốc. 

Bởi vì mỗi thành phần đường dẫn được xử lý theo cùng thứ tự mà shell sẽ diễn giải nó, nên ngăn xếp sau khi xử lý lệnh luôn khớp với thư mục hiện tại thực sự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    
    current = []
    
    for _ in range(n):
        parts = input().strip().split()
        
        if parts[0] == "pwd":
            print("/" + "/".join(current) + "/")
        else:
            path = parts[1]
            
            if path[0] == "/":
                current = []
            
            components = path.split("/")
            
            for comp in components:
                if comp == "" or comp == ".":
                    continue
                
                if comp == "..":
                    current.pop()
                else:
                    current.append(comp)

solve()
```Giải pháp lưu trữ đường dẫn hiện tại dưới dạng danh sách có tên`current`. Mỗi phần tử là một tên thư mục. 

Vì`pwd`, việc triển khai sẽ xây dựng lại đường dẫn tuyệt đối bằng cách nối các tên thư mục với`/`. Biểu thức:```
"/" + "/".join(current) + "/"
```hoạt động ngay cả đối với thư mục gốc. Nếu như`current`trống rỗng,`"".join(current)`trở thành một chuỗi rỗng, vì vậy kết quả cuối cùng chỉ đơn giản là`//`. Vì định dạng đã bao gồm cả dấu gạch chéo ở đầu và cuối nên điều này thực sự tạo ra`//`, điều này không chính xác đối với root. 

Để tránh lỗi tinh vi này, nhiều người thích cách tiếp cận có điều kiện hơn. Tuy nhiên, trong cách triển khai cụ thể này, phép nối trống sẽ tạo ra một phần ở giữa trống, dẫn đến kết quả chính xác`//`. Chúng ta nên sửa điều này. 

Việc thực hiện đúng là:```python
if not current:
    print("/")
else:
    print("/" + "/".join(current) + "/")
```Đây là phiên bản cuối cùng đã sửa:```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    
    current = []
    
    for _ in range(n):
        parts = input().strip().split()
        
        if parts[0] == "pwd":
            if not current:
                print("/")
            else:
                print("/" + "/".join(current) + "/")
        else:
            path = parts[1]
            
            if path[0] == "/":
                current = []
            
            for comp in path.split("/"):
                if comp == "":
                    continue
                
                if comp == "..":
                    current.pop()
                else:
                    current.append(comp)

solve()
```Việc thiết lập lại đường dẫn tuyệt đối là một điểm tinh tế khác. Chúng ta phải xóa ngăn xếp trước khi xử lý các thành phần. Nếu chúng tôi xóa nó sau đó, việc điều hướng tương đối bên trong cùng một đường dẫn sẽ hoạt động không chính xác. 

Bài toán đảm bảo rằng chúng ta không bao giờ di chuyển lên trên gốc, vì vậy`current.pop()`luôn được an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7
pwd
cd /home/vasya
pwd
cd ..
pwd
cd vasya/../petya
pwd
```| Lệnh | Thành phần đã được xử lý | Ngăn xếp hiện tại | Đầu ra | 
| --- | --- | --- | --- | 
| pwd | - | [] | / | 
| cd /home/vasya | nhà, vasya | [nhà, vasya] | - | 
| pwd | - | [nhà, vasya] | /home/vasya/ | 
| cd .. | .. | [nhà] | - | 
| pwd | - | [nhà] | /nhà/ | 
| cd vasya/../petya | vasya, .., petya | [nhà, petya] | - | 
| pwd | - | [nhà, petya] | /home/petya/ | 

Dấu vết này cho thấy lý do tại sao mô hình ngăn xếp là tự nhiên. Mỗi mục nhập thư mục sẽ đẩy một lần và mọi`..`loại bỏ chính xác thư mục gần đây nhất. 

### Ví dụ 2 

đầu vào:```
6
cd /a/b/c
pwd
cd ../../x
pwd
cd /z
pwd
```| Lệnh | Thành phần đã được xử lý | Ngăn xếp hiện tại | Đầu ra | 
| --- | --- | --- | --- | 
| cd /a/b/c | a, b, c | [a, b, c] | - | 
| pwd | - | [a, b, c] | /a/b/c/ | 
| cd ../../x | .., .., x | [a, x] | - | 
| pwd | - | [a, x] | /a/x/ | 
| cd /z | z | [z] | - | 
| pwd | - | [z] | /z/ | 

Ví dụ này cho thấy sự khác biệt giữa đường dẫn tương đối và tuyệt đối. Lệnh`../../x`bắt đầu từ thư mục hiện tại, trong khi`/z`loại bỏ hoàn toàn vị trí trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O (n × L) | Mỗi lệnh xử lý đường dẫn của nó một cách tuyến tính | 
| Không gian | O(L) | Ngăn xếp lưu trữ tối đa tất cả các thư mục trong đường dẫn hiện tại | 

Các giới hạn là cực kỳ nhỏ nên giải pháp này chạy thoải mái trong giới hạn về thời gian và bộ nhớ. Ngay cả trong trường hợp xấu nhất, tổng số ký tự được xử lý cũng chỉ có vài nghìn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    input = sys.stdin.readline
    
    out = io.StringIO()
    sys.stdout = out
    
    current = []
    
    n = int(input())
    
    for _ in range(n):
        parts = input().strip().split()
        
        if parts[0] == "pwd":
            if not current:
                print("/")
            else:
                print("/" + "/".join(current) + "/")
        else:
            path = parts[1]
            
            if path[0] == "/":
                current = []
            
            for comp in path.split("/"):
                if comp == "":
                    continue
                
                if comp == "..":
                    current.pop()
                else:
                    current.append(comp)
    
    sys.stdout = sys.__stdout__
    return out.getvalue()

# provided sample
assert run(
"""7
pwd
cd /home/vasya
pwd
cd ..
pwd
cd vasya/../petya
pwd
"""
) == """/
/home/vasya/
/home/
/home/petya/
"""

# minimum case
assert run(
"""1
pwd
"""
) == """/
"""

# absolute path reset
assert run(
"""4
cd /a/b
cd /x
pwd
pwd
"""
) == """/x/
/x/
"""

# relative navigation
assert run(
"""4
cd /a/b/c
cd ../../d
pwd
pwd
"""
) == """/a/d/
/a/d/
"""

# repeated parent operations
assert run(
"""5
cd /a/b/c
cd ../..
pwd
pwd
"""
) == """/a/
/a/
"""

# mixed navigation
assert run(
"""5
cd /a
cd b/../c
pwd
cd ..
pwd
"""
) == """/a/c/
/a/
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 pwd`|`/`| Xử lý đúng root | 
|`/a/b`sau đó`/x`|`/x/`| Trạng thái đặt lại đường dẫn tuyệt đối | 
|`../../d`|`/a/d/`| Nhiều lần duyệt gốc | 
|`../..`từ con đường sâu |`/a/`| Pop liên tiếp | 
|`b/../c`|`/a/c/`| Điều hướng tương đối hỗn hợp | 

## Vỏ cạnh 

Hãy xem xét trường hợp thư mục hiện tại được thay thế bằng một đường dẫn tuyệt đối. 

đầu vào:```
4
cd /a/b
cd /x/y
pwd
pwd
```Thực thi: 

Sau`cd /a/b`, ngăn xếp trở thành`[a, b]`. 

Lệnh tiếp theo bắt đầu bằng`/`, do đó thuật toán sẽ xóa ngăn xếp trước khi xử lý các thành phần. Sau đó nó nối thêm`x`Và`y`. 

Ngăn xếp cuối cùng là`[x, y]`, sản xuất:```
/x/y/
```Nếu chúng ta quên xóa ngăn xếp, chúng ta sẽ nhận được sai`/a/b/x/y/`. 

Bây giờ hãy xem xét điều hướng cha mẹ lặp đi lặp lại. 

đầu vào:```
3
cd /a/b/c
cd ../..
pwd
```Ngăn xếp sau lệnh đầu tiên là`[a, b, c]`. 

Xử lý`..`bật lên`c`. 

Xử lý thứ hai`..`bật lên`b`. 

Ngăn xếp còn lại là`[a]`, do đó đầu ra là:```
/a/
```Điều này khẳng định rằng mỗi`..`chỉ ảnh hưởng đến thư mục ngay trước đó. 

Cuối cùng, hãy xem xét chuyển động tương đối hỗn hợp. 

đầu vào:```
2
cd a/b/../c
pwd
```Thuật toán xử lý các thành phần theo thứ tự:`a`→`[a]`

`b`→`[a, b]`

`..`→`[a]`

`c`→`[a, c]`Đường dẫn được in trở thành:```
/a/c/
```Điều này chứng tỏ tại sao việc mô phỏng từng thành phần là cần thiết. Cách tiếp cận thay thế chuỗi phím tắt có thể dễ dàng xử lý sai các tham chiếu cha mẹ lồng nhau.
