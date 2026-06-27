---
title: "CF 105381M - Câu chuyện về giáo sư Alya và chỉ số H"
description: "Chúng ta được cung cấp một danh sách số lượng trích dẫn cho các bài báo của một nhà nghiên cứu, đã được sắp xếp theo thứ tự không tăng. Mỗi con số đại diện cho số lần một bài báo cụ thể đã được trích dẫn."
date: "2026-06-23T16:10:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105381
codeforces_index: "M"
codeforces_contest_name: "National Yang Ming Chiao Tung University 2024 Team Selection Programming Contest"
rating: 0
weight: 105381
solve_time_s: 43
verified: true
draft: false
---

[CF 105381M - Câu chuyện về giáo sư Alya và H-Index](https://codeforces.com/problemset/problem/105381/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách số lượng trích dẫn cho các bài báo của một nhà nghiên cứu, đã được sắp xếp theo thứ tự không tăng. Mỗi con số đại diện cho số lần một bài báo cụ thể đã được trích dẫn. Nhiệm vụ là tính chỉ số H, là số nguyên H lớn nhất sao cho ít nhất H bài báo có số lượng trích dẫn ít nhất là H. 

Một cách hữu ích để diễn giải định nghĩa là hãy tưởng tượng việc quét danh sách được sắp xếp từ bài báo được trích dẫn nhiều nhất trở đi. Tại vị trí i (1 chỉ mục), chúng tôi hỏi liệu bài viết này có còn đủ điều kiện là một phần của nhóm có kích thước i hay không, nghĩa là số lượng trích dẫn của nó ít nhất là i. Câu trả lời là vị trí lớn nhất mà điều kiện này vẫn đúng. 

Ràng buộc n có thể lớn tới 10^6, do đó giải pháp phải chạy theo thời gian tuyến tính. Bất cứ điều gì bậc hai, chẳng hạn như kiểm tra mọi H có thể và quét mảng mỗi lần, sẽ liên quan đến tối đa 10^12 phép tính trong trường hợp xấu nhất và sẽ không vượt qua. 

Trường hợp phức tạp xảy ra khi tất cả số lượng trích dẫn bằng 0. Ví dụ: nếu n = 5 và mảng là [0, 0, 0, 0, 0] thì không có vị trí nào thỏa mãn i ci, do đó câu trả lời phải là 0. Một cách triển khai đơn giản giả định rằng ít nhất một bài báo hợp lệ có thể trả về 1 không chính xác. 

Một trường hợp khác xuất hiện khi tất cả các trích dẫn đều lớn. Ví dụ: [10, 10, 10] phải trả về 3 vì cả ba bài viết đều thỏa mãn việc có ít nhất 3 trích dẫn, mặc dù chúng cũng đáp ứng các giá trị thô cao hơn riêng lẻ. Ràng buộc là về vị trí, không chỉ về mức độ trích dẫn. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là thử mọi giá trị ứng viên có thể có H từ 0 đến n và kiểm tra xem ít nhất H bài viết có ít nhất H trích dẫn hay không. Vì mảng được sắp xếp theo thứ tự giảm dần nên việc kiểm tra H cố định có thể được thực hiện bằng cách quét cho đến khi chúng ta tìm thấy chỉ mục đầu tiên trong đó ci < H và đếm xem có bao nhiêu phần tử hợp lệ. Việc kiểm tra này tốn O(n) và việc thực hiện nó cho tất cả các giá trị H dẫn đến tổng công việc là O(n^2), trở nên quá chậm đối với n lên đến một triệu. 

Quan sát chính xuất phát từ cấu trúc được sắp xếp. Vì các trích dẫn theo thứ tự giảm dần nên khi chúng ta đạt đến chỉ mục có ci < i, mọi phần tử sau này cũng sẽ thất bại vì mảng chỉ giảm đi. Điều này có nghĩa là câu trả lời được xác định bởi vị trí đầu tiên nơi điều kiện bị phá vỡ. Chúng ta có thể quét một lần từ trái sang phải và dừng ngay khi ci < i, trả về i - 1 làm chỉ số H. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Quét đơn | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Chiến lược tối ưu 

Chúng tôi khai thác thực tế là mảng đã được sắp xếp theo thứ tự không tăng. 

1. Bắt đầu quét mảng từ bài báo đầu tiên có chỉ số i = 1. Chúng tôi so sánh số lượng trích dẫn ci với i vì i đại diện cho số lượng bài báo mà chúng tôi đang cố gắng xác thực như một phần của nhóm chỉ mục H. Điều này trực tiếp phù hợp với định nghĩa của H-index. 
2. Với mỗi vị trí i, kiểm tra xem ci ≥ i. Nếu điều này đúng, điều đó có nghĩa là chúng ta vẫn có khả năng hình thành chỉ số H ít nhất là i vì chúng ta có ít nhất i bài báo có ít nhất i trích dẫn. 
3. Tiếp tục di chuyển về phía trước miễn là điều kiện vẫn đúng. Mỗi vị trí hợp lệ sẽ mở rộng chỉ số H có thể có. 
4. Lần đầu tiên gặp vị trí ci < i, chúng ta dừng lại. Khi đó không thể có chỉ số H nào lớn hơn i - 1 vì ngay cả bài báo thứ i cũng không thỏa mãn yêu cầu. 
5. Trả về i - 1 là đáp án cuối cùng. Nếu chúng ta không bao giờ phá vỡ điều kiện thì tất cả các giấy tờ đều thỏa mãn ci ≥ i, nên đáp án là n. 

### Tại sao nó hoạt động

Sự đúng đắn đến từ sự đơn điệu. Vì mảng được sắp xếp theo thứ tự giảm dần nên giá trị trích dẫn không bao giờ tăng khi chúng ta di chuyển sang phải. Trong khi đó, ngưỡng yêu cầu i tăng đúng 1 ở mỗi bước. Khi ci < i, chúng ta biết rằng với mọi j > i, cả cj ≤ ci và j > i, vì vậy cj < j cũng phải đúng. Điều này đảm bảo rằng điểm thất bại đầu tiên chính xác là điểm kết thúc của chỉ số H khả thi tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    if n == 0:
        print(0)
        return
    
    c = list(map(int, input().split()))
    
    for i, val in enumerate(c, start=1):
        if val < i:
            print(i - 1)
            return
    
    print(n)

if __name__ == "__main__":
    solve()
```Giải pháp đọc n và danh sách trích dẫn được sắp xếp. Sau đó, nó thực hiện một lượt duy nhất, theo dõi chỉ mục dựa trên 1 bằng cách sử dụng liệt kê. Lần đầu tiên giá trị trích dẫn giảm xuống dưới chỉ mục của nó, chúng tôi trả về chỉ mục trước đó dưới dạng chỉ mục H. Nếu không xảy ra sự gián đoạn như vậy thì chỉ số H bằng n. 

Một cạm bẫy phổ biến là quên rằng việc lập chỉ mục dựa trên 1 trong điều kiện, do đó việc liệt kê phải bắt đầu từ 1. Một cách tinh tế khác là xử lý n = 0, trong đó không có danh sách đầu vào nào tồn tại và câu trả lời trực tiếp là 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng đầu vào: [6, 5, 3, 1, 0] 

| tôi | ci | ci ≥ tôi | Hành động | 
| --- | --- | --- | --- | 
| 1 | 6 | đúng | tiếp tục | 
| 2 | 5 | đúng | tiếp tục | 
| 3 | 3 | đúng | tiếp tục | 
| 4 | 1 | sai | dừng lại | 

Chúng ta dừng lại ở i = 4 nên đáp án là 3. Điều này cho thấy thuật toán xác định chính xác lần vi phạm đầu tiên của điều kiện chỉ số H. 

### Ví dụ 2 

Mảng đầu vào: [10, 10, 10, 10] 

| tôi | ci | ci ≥ tôi | Hành động | 
| --- | --- | --- | --- | 
| 1 | 10 | đúng | tiếp tục | 
| 2 | 10 | đúng | tiếp tục | 
| 3 | 10 | đúng | tiếp tục | 
| 4 | 10 | đúng | tiếp tục | 

Không xảy ra ngắt nên đáp án là 4. Điều này khẳng định thuật toán xử lý được trường hợp tất cả các bài đều vượt quá ngưỡng yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Quét tuyến tính đơn qua danh sách trích dẫn | 
| Không gian | O(1) | Chỉ sử dụng các biến phụ không đổi | 

Quét tuyến tính là đủ cho n tối đa 10^6 vì nó thực hiện tối đa một so sánh cho mỗi phần tử, trong giới hạn thời gian thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    if n == 0:
        return "0"
    c = list(map(int, input().split()))
    
    for i, val in enumerate(c, start=1):
        if val < i:
            return str(i - 1)
    return str(n)

# provided samples
assert run("5\n6 5 3 1 0\n") == "3"
assert run("5\n10 10 10 10 9\n") == "5"

# custom cases
assert run("0\n") == "0"
assert run("1\n0\n") == "0"
assert run("3\n3 3 3\n") == "3"
assert run("4\n4 3 2 1\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 | 0 | xử lý mảng trống | 
| 1 tờ có 0 | 0 | ranh giới phần tử đơn | 
| 3 giá trị bằng nhau | 3 | trường hợp bão hòa đầy đủ | 
| 4 3 2 1 | 2 | nghỉ sớm đúng cách | 

## Vỏ cạnh 

Đối với một danh sách xuất bản trống, đầu vào là n = 0. Thuật toán ngay lập tức trả về 0 mà không cố truy cập vào mảng trích dẫn, điều này tránh các lần đọc không hợp lệ và khớp với định nghĩa rằng không có bài báo nào ngụ ý chỉ số H bằng 0. 

Đối với một mảng như [0, 0, 0], quá trình quét không thành công ở i = 1 vì 0 < 1. Thuật toán trả về 0, phản ánh chính xác rằng không có bài báo nào đáp ứng dù chỉ một ngưỡng trích dẫn. 

Đối với tập dữ liệu hoàn toàn mạnh như [100, 100, 100, 100], không xảy ra ngắt trong quá trình quét. Thuật toán đến cuối và trả về n = 4, vì cả bốn bài đều thỏa mãn điều kiện ci ≥ i với mọi i.
