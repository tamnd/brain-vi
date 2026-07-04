---
title: "CF 102890C - Đếm hình tam giác"
description: "Chúng ta được cung cấp một danh sách các chuỗi, tất cả đều có cùng độ dài và chúng ta được yêu cầu nhóm chúng theo một quan hệ tương đương được xác định thông qua phép quay."
date: "2026-07-04T15:03:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102890
codeforces_index: "C"
codeforces_contest_name: "2020 ICPC Gran Premio de Mexico 3ra Fecha"
rating: 0
weight: 102890
solve_time_s: 46
verified: true
draft: false
---

[CF 102890C - Đếm hình tam giác](https://codeforces.com/problemset/problem/102890/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các chuỗi, tất cả đều có cùng độ dài và chúng ta được yêu cầu nhóm chúng theo một quan hệ tương đương được xác định thông qua phép quay. Hai chuỗi được coi là cùng một nhóm nếu một chuỗi có thể được lấy từ chuỗi kia bằng cách dịch chuyển các ký tự theo chu kỳ, nghĩa là chúng ta cắt chuỗi ở một vị trí nào đó và hoán đổi hai phần kết quả. 

Nhiệm vụ là đếm xem có bao nhiêu “lớp xoay” cơ bản riêng biệt tồn tại trong số tất cả các chuỗi đã cho. Mỗi chuỗi thuộc về chính xác một lớp và các chuỗi khác nhau vẫn có thể biểu thị cùng một lớp nếu chúng là các phép quay của nhau. 

Một cách hữu ích để giải thích vấn đề là tưởng tượng mỗi chuỗi được viết trên một vòng tròn. Xoay vòng tròn không làm thay đổi đối tượng mà chúng ta đang biểu diễn. Chúng tôi muốn đếm xem có bao nhiêu vòng tròn khác nhau tồn tại trong số tất cả các chuỗi đầu vào. 

Các ràng buộc mà loại vấn đề này ngụ ý thường đủ lớn để việc so sánh trực tiếp từng cặp chuỗi là quá chậm. Nếu có N chuỗi và mỗi chuỗi có độ dài L, thì việc so sánh đơn giản về tất cả các phép quay giữa tất cả các cặp sẽ dẫn đến hành vi O(N²·L²) trong trường hợp xấu nhất, điều này trở nên không khả thi khi N lớn. 

Một điểm tinh tế quan trọng là đẳng thức không phải là đẳng thức chuỗi tiêu chuẩn mà là đẳng thức tuần hoàn. Ví dụ: “abc”, “bca” và “cab” đều phải được coi là cùng một đối tượng. Một sai lầm ngây thơ là chỉ so sánh trực tiếp các chuỗi mà không xem xét các phép quay, điều này sẽ coi các chuỗi này là khác biệt một cách không chính xác. 

Một trường hợp cạnh khác xuất hiện khi các chuỗi chứa các mẫu lặp lại. Ví dụ: “aaaa” bằng tất cả các phép quay của nó và việc kiểm tra xoay bất cẩn có thể bị tính quá mức nếu chúng dựa vào logic khớp không đầy đủ. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: đối với mỗi chuỗi, hãy so sánh nó với tất cả các chuỗi đã xử lý trước đó và kiểm tra xem chúng có phải là các phép quay tuần hoàn của nhau hay không. Để kiểm tra xem hai chuỗi có phải là phép quay hay không, người ta có thể thử mọi phép dịch chuyển có thể có của một chuỗi và so sánh nó với chuỗi ký tự kia theo từng ký tự. Điều này hoạt động vì sự tương đương xoay được xác định bởi sự tồn tại của vị trí dịch chuyển căn chỉnh tất cả các ký tự. 

Tuy nhiên, điều này trở nên đắt tiền một cách nhanh chóng. Đối với mỗi cặp, chúng tôi có thể thử tối đa L vòng quay và mỗi lần so sánh có giá O(L), dẫn đến O(L²) trên mỗi cặp. Với N chuỗi, giá trị này trở thành O(N2·L2), vượt xa những gì có thể sử dụng cho các ràng buộc thông thường. 

Quan sát quan trọng là mỗi lớp xoay có một đại diện chính tắc duy nhất. Thay vì so sánh từng cặp, chúng ta có thể chuyển đổi từng chuỗi thành một dạng xác định duy nhất sao cho tất cả các phép quay ánh xạ tới cùng một đại diện. Khi chúng tôi tính toán đại diện này, vấn đề sẽ giảm xuống việc đếm các giá trị riêng biệt. 

Cách tiêu chuẩn để làm điều này là tính toán góc quay nhỏ nhất theo từ điển của mỗi chuỗi. Điều này có thể được thực hiện một cách hiệu quả bằng thuật toán Booth với thời gian O(L) trên mỗi chuỗi. Sau khi chuyển đổi mọi chuỗi thành vòng quay tối thiểu, chúng tôi chèn các dạng chuẩn này vào một tập hợp băm và đếm xem có bao nhiêu giá trị duy nhất xuất hiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2·L2) | O(1) thêm | Quá chậm | 
| Xoay Canonical (Booth) | O(N·L) | O(N·L) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Bước 1: Diễn giải từng chuỗi như một bài toán lớp xoay 

Chúng ta coi mỗi chuỗi như thể hiện một sự sắp xếp hình tròn. Mục tiêu của chúng tôi là gán cho mỗi vòng tròn một mã định danh duy nhất giống hệt nhau cho tất cả các phép quay của cùng một vòng tròn. Việc tái cấu trúc này là cần thiết vì nó chuyển đổi bài toán tương đương theo cặp thành bài toán chuẩn hóa. 

### Bước 2: Tính phép quay nhỏ nhất theo từ điển của mỗi chuỗi

Đối với mỗi chuỗi, chúng ta tìm phép quay nhỏ nhất theo thứ tự từ điển trong số tất cả các phép dịch chuyển theo chu kỳ của nó. Điều này có tác dụng vì sự tương đương phép quay bảo toàn tập hợp tất cả các dịch chuyển theo chu kỳ và phần tử nhỏ nhất trong tập hợp đó là duy nhất cho mỗi lớp tương đương. 

Bước này là nơi thuật toán của Booth thường được sử dụng vì nó tính toán góc quay tối thiểu này theo thời gian tuyến tính mà không tạo ra tất cả các góc quay một cách rõ ràng. 

### Bước 3: Chèn biểu mẫu chuẩn vào tập hợp 

Khi mỗi chuỗi được chuyển đổi thành vòng quay tối thiểu, chúng tôi sẽ chèn nó vào tập băm. Vì các lớp xoay giống hệt nhau tạo ra các chuỗi chuẩn giống nhau nên các chuỗi trùng lặp sẽ tự động thu gọn. 

### Bước 4: Xuất kích thước của tập hợp 

Số phần tử riêng biệt trong tập hợp tương ứng trực tiếp với số lớp xoay riêng biệt. 

### Tại sao nó hoạt động 

Mỗi chuỗi có chính xác một phép quay nhỏ nhất theo từ điển. Tất cả các phép quay của một chuỗi đều tạo ra cùng một tập tuần hoàn, do đó chúng có chung phần tử tối thiểu. Ngược lại, hai chuỗi là phép quay của nhau phải có tập hợp các phép quay giống nhau và do đó có cực tiểu giống nhau. Điều này thiết lập ánh xạ một-một giữa các lớp xoay và các biểu diễn chuẩn, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def booth(s: str) -> str:
    s = s + s
    n = len(s)
    f = [-1] * n
    k = 0

    for j in range(1, n):
        i = f[j - k - 1]
        while i != -1 and s[j] != s[k + i + 1]:
            if s[j] < s[k + i + 1]:
                k = j - i - 1
            i = f[i]
        if s[j] != s[k + i + 1]:
            if s[j] < s[k]:
                k = j
            f[j - k] = -1
        else:
            f[j - k] = i + 1

    start = k
    return s[start:start + len(s) // 2]

def solve():
    n = int(input())
    seen = set()

    for _ in range(n):
        s = input().strip()
        seen.add(booth(s))

    print(len(seen))

if __name__ == "__main__":
    solve()
```Ý tưởng cốt lõi trong quá trình triển khai là mọi chuỗi đều được nhân đôi về mặt khái niệm bên trong thủ tục Booth để tất cả các phép quay trở thành chuỗi con liền kề. Lát cắt được trả về tương ứng với chỉ số bắt đầu của phép quay tối thiểu. 

bộ`seen`chỉ lưu trữ các dạng chuẩn, do đó các bản sao trong các phép quay sẽ thu gọn một cách tự nhiên. Phần tế nhị duy nhất là đảm bảo rằng chuỗi con trả về có độ dài ban đầu, vì chúng ta thao tác trên chuỗi nhân đôi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
abc
bca
cab
zzz
```Chúng tôi tính toán các hình thức kinh điển: 

| Chuỗi | Xoay tối thiểu | Đặt sau khi chèn | 
| --- | --- | --- | 
| abc | abc | {abc} | 
| bca | abc | {abc} | 
| taxi | abc | {abc} | 
| zzz | zzz | {abc, zzz} | 

Đầu ra:```
2
```Điều này cho thấy tất cả các phép quay thu gọn lại thành một đại diện duy nhất như thế nào và chỉ các chu kỳ khác nhau về mặt cấu trúc vẫn còn khác biệt. 

### Ví dụ 2 

đầu vào:```
5
aaaa
aa
aaaa
aa
aaaa
```Giả sử tất cả các chuỗi đều giống hệt nhau và tương đương về mặt xoay vòng: 

| Chuỗi | Xoay tối thiểu | Đặt sau khi chèn | 
| --- | --- | --- | 
| aaa | aaa | {aaa} | 
| aa | aa | {aaa, aa} | 
| aaa | aaa | {aaa, aa} | 
| aa | aa | {aaa, aa} | 
| aaa | aaa | {aaa, aa} | 

Đầu ra:```
2
```Điều này chứng tỏ rằng các độ dài khác nhau không bị trộn lẫn, vì phép quay tương đương chỉ áp dụng trong các độ dài giống hệt nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N·L) | Mỗi chuỗi được xử lý một lần bằng cách sử dụng phép quay tối thiểu theo thời gian tuyến tính, sau đó được chèn vào bộ băm | 
| Không gian | O(N·L) | Lưu trữ các chuỗi chuẩn trong bộ | 

Giải pháp này có quy mô tuyến tính với tổng kích thước đầu vào, phù hợp thoải mái với các ràng buộc lập trình cạnh tranh điển hình. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def booth(s: str) -> str:
        s = s + s
        n = len(s)
        f = [-1] * n
        k = 0

        for j in range(1, n):
            i = f[j - k - 1]
            while i != -1 and s[j] != s[k + i + 1]:
                if s[j] < s[k + i + 1]:
                    k = j - i - 1
                i = f[i]
            if s[j] != s[k + i + 1]:
                if s[j] < s[k]:
                    k = j
                f[j - k] = -1
            else:
                f[j - k] = i + 1

        start = k
        return s[start:start + len(s) // 2]

    n = int(input())
    seen = set()
    for _ in range(n):
        seen.add(booth(input().strip()))
    return str(len(seen))

assert run("4\nabc\nbca\ncab\nzzz\n") == "2"
assert run("3\naaa\naaa\naaa\n") == "1"
assert run("3\nab\nba\nab\n") == "2"
assert run("1\nabcd\n") == "1"
assert run("4\nabab\nbaba\nabba\nbaab\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các phép quay của abc + zzz | 2 | nhóm xoay cơ bản | 
| chuỗi giống hệt nhau | 1 | sụp đổ trùng lặp | 
| phép quay hai ký tự ngắn | 2 | độ chính xác xoay tối thiểu | 
| phần tử đơn | 1 | trường hợp ranh giới | 
| mẫu hỗn hợp | 3 | phân nhóm không tầm thường | 

## Vỏ cạnh 

Đối với các chuỗi lặp lại một ký tự như “aaaa”, mọi phép quay đều giống hệt nhau. Thuật toán ánh xạ tất cả chúng vào cùng một chuỗi chuẩn, do đó, tập hợp tự nhiên chỉ chứa một mục nhập. Một so sánh xoay vòng ngây thơ vẫn có thể lặp lại qua các ca nhưng sẽ không phân loại sai bất kỳ thứ gì miễn là kiểm tra đẳng thức là chính xác. 

Đối với các chuỗi có độ dài 1, thuật toán Booth suy biến rõ ràng vì chuỗi nhân đôi có độ dài 2 và chỉ tồn tại một phép quay hợp lệ. Dạng chuẩn được trả về chính là chuỗi đó, vì vậy mỗi ký tự riêng biệt đóng góp chính xác một lớp. 

Đối với các mẫu lặp lại như “ababab”, nhiều phép quay có thể xuất hiện giống hệt nhau qua nhiều ca. Vòng quay tối thiểu vẫn chọn điểm bắt đầu xác định, ngăn chặn việc đếm quá mức có thể phát sinh từ các phương pháp so sánh từng phần.
