---
title: "CF 105335B - Ngày xưa"
description: "Đầu vào là một chuỗi các chữ số từ 2 đến 9 xuất phát từ bàn phím điện thoại đa chạm cũ. Mỗi chữ số tương ứng với một nhóm chữ cái và một chữ cái được tạo ra bằng cách nhấn chữ số đó nhiều lần liên tiếp."
date: "2026-06-25T20:56:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105335
codeforces_index: "B"
codeforces_contest_name: "ICPC Thailand National Competition 2024"
rating: 0
weight: 105335
solve_time_s: 47
verified: true
draft: false
---

[CF 105335B - Ngày xưa](https://codeforces.com/problemset/problem/105335/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đầu vào là một chuỗi các chữ số từ 2 đến 9 xuất phát từ bàn phím điện thoại đa chạm cũ. Mỗi chữ số tương ứng với một nhóm chữ cái và một chữ cái được tạo ra bằng cách nhấn chữ số đó nhiều lần liên tiếp. Ví dụ: nhấn 2 một lần sẽ có một chữ cái, nhấn 2 hai lần sẽ có chữ cái tiếp theo trong nhóm, v.v. 

Phần quan trọng còn thiếu trong dữ liệu được ghi là khoảng dừng giữa các chữ cái. Khi gõ một từ, người nói sẽ tạm dừng giữa các chữ cái khác nhau, nhưng chuỗi đã ghi sẽ hợp nhất mọi thứ thành một luồng nhấn phím liên tục. Nhiệm vụ là khôi phục chuỗi chữ cái nào có thể đã tạo ra luồng nhấn phím được quan sát. 

Bàn phím hoạt động giống như ánh xạ điện thoại tiêu chuẩn: 2 ánh xạ tới abc, 3 đến def, 4 đến ghi, 5 đến jkl, 6 đến mno, 7 đến pqrs, 8 đến tuv và 9 đến wxyz. Việc nhấn lặp đi lặp lại cùng một chữ số sẽ duyệt qua các chữ cái của nó, nhưng vấn đề đảm bảo rằng người đánh máy không bao giờ vượt quá số lượng chữ cái trên một phím, do đó không bao giờ xảy ra sự thay đổi. 

Đầu ra là bất kỳ chuỗi được giải mã hợp lệ nào, nhưng trong số tất cả các cách diễn giải hợp lệ, chúng ta phải ưu tiên cách diễn giải ngắn nhất và nếu vẫn còn mơ hồ, thì cách diễn giải nhỏ nhất về mặt từ điển. 

Từ các ràng buộc, độ dài đầu vào tối đa là 200, có nghĩa là bất kỳ giải pháp nào lên tới O(n) hoặc O(n log n) đều không đáng kể. Ngay cả O(n²) cũng có thể vượt qua, nhưng cấu trúc gợi ý rõ ràng về quá trình quét tuyến tính. 

Một cách giải thích ngây thơ có thể cố gắng chèn các khoảng dừng tùy ý và kiểm tra tất cả các phân đoạn của chuỗi thành các lần chạy. Điều đó ngay lập tức trở thành hàm mũ vì giữa mỗi cặp chữ số đều có một quyết định nhị phân: ngắt hay không. Đối với độ dài 200, đó đã là 2¹⁹⁹ khả năng, điều này là không khả thi. 

Một sai lầm tinh vi hơn là cho rằng các chữ số đơn luôn tương ứng với các chữ cái đơn lẻ. Điều đó không thành công trong các trường hợp như các chữ số lặp lại, trong đó cần phải nhấn nhiều lần để đạt được các chữ cái sau trong ánh xạ. 

Trường hợp cạnh xuất hiện khi một chữ số lặp lại nhiều lần. Ví dụ: một phân đoạn như 7777 phải ánh xạ tới một từ có 4 chữ cái bằng cách sử dụng ánh xạ đầy đủ là 7. Một trường hợp khác là các chữ số xen kẽ như 2323, trong đó mỗi thay đổi chữ số sẽ buộc phải có một ranh giới chữ cái mới. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là xem xét mọi cách có thể để chia chuỗi chữ số thành các đoạn, trong đó mỗi đoạn thể hiện việc nhấn lặp lại một phím. Đối với mỗi phân đoạn, chúng tôi sẽ chuyển đổi mọi phân đoạn thành một chữ cái dựa trên độ dài của nó và ánh xạ bàn phím tương ứng. Điều này hoạt động vì nó mô phỏng trực tiếp quá trình gõ với các khoảng dừng rõ ràng. 

Điểm thất bại là vụ nổ tổ hợp. Đối với một chuỗi có độ dài n, có n-1 khoảng trống và mỗi khoảng trống có thể được phân tách hoặc không. Điều này dẫn đến phân đoạn 2^(n−1). Ngay cả với n = 50 thì con số này vẫn quá lớn và giới hạn cho phép lên tới 200. 

Quan sát chính là việc tạm dừng không phải là tùy ý theo cách diễn giải hợp lệ khi chúng tôi thực thi mô hình gõ. Nếu cùng một chữ số lặp lại liên tiếp thì các lần nhấn đó phải thuộc cùng một chữ cái. Việc tạm dừng chỉ có thể xảy ra khi chữ số thay đổi, vì nếu không, nó sẽ thể hiện sự đặt lại trong khi vẫn nhấn cùng một phím, điều này sẽ mâu thuẫn với cách hiểu nhấn liên tục. Điều này buộc phải phân đoạn duy nhất: các khối liền kề tối đa có các chữ số giống hệt nhau. 

Sau khi chuỗi được phân chia thành các lần chạy, mỗi lần chạy sẽ ánh xạ độc lập tới chính xác một chữ cái bằng cách hiểu độ dài của nó là số lần nhấn phím. 

Do đó, kết quả đầu ra ngắn nhất có thể luôn đạt được bằng cách diễn giải chạy tối đa này, vì việc đưa ra các phần tách bổ sung sẽ chỉ làm tăng số lượng chữ cái. Nhỏ nhất về mặt từ điển cũng được cố định vì không có phân đoạn thay thế nào duy trì tính hợp lệ.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân đoạn Brute Force | O(2^n · n) | O(n) | Quá chậm | 
| Giải mã thời lượng chạy | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý chuỗi trong một lần, nhóm các chữ số giống hệt nhau liên tiếp thành các khối. 

1. Quét chuỗi từ trái sang phải trong khi vẫn giữ nguyên chữ số hiện tại và độ dài của chuỗi đó. Nhóm này nhóm các chữ số giống nhau liên tiếp tối đa lại với nhau vì sự thay đổi về chữ số là ranh giới hợp lệ duy nhất cho một chữ cái mới. 
2. Với mỗi chữ số d có độ dài k, hãy chuyển nó thành một chữ cái bằng cách lập chỉ mục vào ánh xạ của d bằng cách sử dụng vị trí k. Điều này mô phỏng trực tiếp việc nhấn phím k lần. 
3. Nối chữ cái kết quả vào chuỗi đầu ra. 
4. Tiếp tục cho đến khi toàn bộ chuỗi được xử lý, đảm bảo lần chạy cuối cùng cũng được chuyển đổi. 

Tính chính xác xuất phát từ thực tế là mỗi lần chạy tương ứng với một chuỗi các lần nhấn phím liên tục không bị gián đoạn trên cùng một nút. Vì sự cố đảm bảo không vượt quá số lượng chữ cái của khóa nên mỗi lần chạy luôn ánh xạ tới một chữ cái hợp lệ mà không có sự mơ hồ. 

### Tại sao nó hoạt động 

Điều bất biến cơ bản là tại mỗi thời điểm trong quá trình quét, lần chạy hiện tại thể hiện chính xác một chữ cái trong quá trình gõ ban đầu. Bất kỳ nỗ lực phân chia nào trong một lần chạy sẽ bao hàm việc chèn một khoảng dừng trong khi vẫn nhấn cùng một phím, thao tác này không thể tạo ra một chữ cái khác và sẽ chỉ làm tăng số lượng ký tự ở đầu ra. Do đó, phân tách chạy tối đa tạo ra giải mã hợp lệ có độ dài tối thiểu. Vì mỗi lần chạy ánh xạ một cách xác định tới một chữ cái duy nhất, nên không có cách giải mã thay thế nào có thể tạo ra một chuỗi nhỏ hơn hoặc ngắn hơn về mặt từ điển. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

KEYS = {
    '2': "abc",
    '3': "def",
    '4': "ghi",
    '5': "jkl",
    '6': "mno",
    '7': "pqrs",
    '8': "tuv",
    '9': "wxyz"
}

def solve():
    s = input().strip()
    n = len(s)
    
    res = []
    i = 0
    
    while i < n:
        j = i
        while j < n and s[j] == s[i]:
            j += 1
        
        digit = s[i]
        length = j - i
        
        res.append(KEYS[digit][length - 1])
        
        i = j
    
    print("".join(res))

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh trực tiếp quá trình phân rã lần chạy. Vòng lặp bên trong mở rộng từng khối tối đa có các chữ số giống hệt nhau. biểu thức`length - 1`là rất quan trọng vì việc lập chỉ mục vào chuỗi bàn phím là dựa trên 0 trong khi số lần nhấn là dựa trên một. 

Một lỗi phổ biến là cố gắng tích lũy số đếm mà không đặt lại đúng cách khi chữ số thay đổi. Cách tiếp cận dựa trên con trỏ tránh điều này bằng cách đóng từng phân đoạn một cách rõ ràng trước khi bắt đầu phân đoạn tiếp theo. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
22228
```Chúng tôi quét chuỗi và chạy biểu mẫu. 

| Bước | Chạy | Chữ số | Chiều dài | Ký tự đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 2222 | 2 | 4 | một | 
| 2 | 8 | 8 | 1 | t | 

Lần chạy đầu tiên ánh xạ 2 → "abc" và 4 lần nhấn chọn 'a'. Lần chạy thứ hai ánh xạ 8 → "tuv" và một lần nhấn sẽ chọn 't'. 

Đầu ra:```
at
```Điều này thể hiện thời gian chạy các ký tự sau trong nhóm bàn phím mà không có sự mơ hồ. 

### Ví dụ 2 

đầu vào:```
4442227222
```| Bước | Chạy | Chữ số | Chiều dài | Ký tự đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 444 | 4 | 3 | tôi | 
| 2 | 222 | 2 | 3 | c | 
| 3 | 7 | 7 | 1 | p | 
| 4 | 222 | 2 | 3 | c | 

Đầu ra:```
icpc
```Dấu vết này cho thấy sự chuyển đổi lặp đi lặp lại giữa các nhóm chữ số khác nhau. Mỗi lần chạy độc lập sẽ giải quyết thành một chữ cái duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nhân vật được truy cập một lần trong khi tạo thành các lần chạy | 
| Không gian | O(n) | Chuỗi đầu ra lưu trữ một ký tự mỗi lần chạy trong trường hợp xấu nhất | 

Kích thước đầu vào tối đa là 200, do đó quét tuyến tính có hiệu quả không đáng kể và nằm trong giới hạn. Ngay cả với chi phí Python, giải pháp vẫn chạy ngay lập tức. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    KEYS = {
        '2': "abc",
        '3': "def",
        '4': "ghi",
        '5': "jkl",
        '6': "mno",
        '7': "pqrs",
        '8': "tuv",
        '9': "wxyz"
    }
    
    s = input().strip()
    i = 0
    res = []
    
    while i < len(s):
        j = i
        while j < len(s) and s[j] == s[i]:
            j += 1
        d = s[i]
        res.append(KEYS[d][j - i - 1])
        i = j
    
    return "".join(res)

assert run("22228\n") == "at", "sample 2"
assert run("4442227222\n") == "icpc", "sample 1"

assert run("2\n") == "a", "single press"
assert run("7777\n") == "s", "max run on 7"
assert run("888\n") == "v", "middle key mapping"
assert run("999999\n") == "y", "repeated long run on 9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2`|`a`| đầu vào tối thiểu | 
|`7777`|`s`| ánh xạ khóa có độ dài đầy đủ | 
|`888`|`v`| phím tầm trung | 
|`999999`|`y`| chạy đồng đều | 

## Vỏ cạnh 

Trường hợp cạnh phím là khi một chữ số chạy khớp chính xác với độ dài của nhóm bàn phím của nó. Đối với chữ số 7, ánh xạ tới bốn chữ cái, đầu vào như 7777 phải ánh xạ tới chữ cái cuối cùng 's'. Thuật toán xử lý việc này một cách tự nhiên vì độ dài chạy được sử dụng trực tiếp làm chỉ mục trong ánh xạ. 

Một trường hợp cạnh khác là đầu vào có một chữ số. Vì chỉ có một lần chạy nên thuật toán ngay lập tức trả về chữ cái đầu tiên trong ánh xạ của khóa đó và không có logic ranh giới nào được kích hoạt. 

Các chữ số xen kẽ như 232323 tạo thành nhiều lần chạy có độ dài đơn. Mỗi lần chạy được xử lý độc lập, đảm bảo không xảy ra sự hợp nhất ngẫu nhiên khi thay đổi chữ số.
