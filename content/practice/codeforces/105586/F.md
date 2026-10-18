---
title: "CF 105586F - \u5b57\u7b26\u4e32\u7f29\u5199\u592a\u591a\u4e86\uff01"
description: "Chúng ta được cung cấp một tập hợp các chuỗi chữ thường riêng biệt. Từ các chuỗi này, chúng ta có thể chọn bất kỳ tập hợp con không trống nào và sắp xếp các chuỗi đã chọn theo bất kỳ thứ tự nào."
date: "2026-06-22T06:00:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105586
codeforces_index: "F"
codeforces_contest_name: "\u201c\u534e\u4e3a\u676f\u201d 2024 \u5e74\u5e7f\u4e1c\u5de5\u4e1a\u5927\u5b66 ACM \u65b0\u751f\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\uff08\u51b3\u8d5b\uff09"
rating: 0
weight: 105586
solve_time_s: 46
verified: true
draft: false
---

[CF 105586F - \u5b57\u7b26\u4e32\u7f29\u5199\u592a\u591a\u4e86\uff01](https://codeforces.com/problemset/problem/105586/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các chuỗi chữ thường riêng biệt. Từ các chuỗi này, chúng ta có thể chọn bất kỳ tập hợp con không trống nào và sắp xếp các chuỗi đã chọn theo bất kỳ thứ tự nào. Đối với mỗi lựa chọn có thứ tự, chúng ta tạo một chuỗi mới bằng cách lấy ký tự đầu tiên của mỗi chuỗi theo thứ tự, nối thành một “chữ viết tắt” ngắn. 

Hai lựa chọn có thứ tự khác nhau có thể tạo ra cùng một chuỗi viết tắt, vì vậy mỗi chữ viết tắt có một "số lược đồ", nghĩa là có bao nhiêu lựa chọn có thứ tự tạo ra chuỗi đó. Tác vụ yêu cầu tính tổng số lược đồ này trên tất cả các chuỗi viết tắt có thể có. 

Một cách khác để xem quy trình là mọi cách xây dựng hợp lệ đều được xác định bằng cách trước tiên chọn một tập hợp con các chuỗi rồi hoán vị nó. Mỗi hoán vị kết quả đóng góp chính xác một vào tổng số, nhưng các hoán vị khác nhau có thể thu gọn thành cùng một từ viết tắt. Vấn đề yêu cầu tổng đóng góp của tất cả các chữ viết tắt chứ không phải số lượng chữ viết tắt riêng biệt. 

Các ràng buộc cho phép tối đa 100000 chuỗi và tổng chiều dài lên tới 1e6, điều này ngay lập tức gợi ý rằng mọi giải pháp tùy thuộc vào các ký tự thực tế hoặc xây dựng các tập hợp con rõ ràng là không thể. Việc liệt kê các tập hợp con đã mang lại 2^n khả năng và thậm chí việc hạn chế hoán vị sẽ bùng nổ hơn nữa. Các giải pháp khả thi duy nhất là những giải pháp quy bài toán về biểu thức tổ hợp dạng đóng hoặc quét tuyến tính trên n. 

Một điểm tinh tế là nội dung thực tế của các chuỗi không ảnh hưởng đến việc đếm theo bất kỳ cách cấu trúc nào ngoài việc đảm bảo rằng mỗi chuỗi là khác biệt. Một người đọc ngây thơ có thể nghĩ rằng các chữ cái đầu tiên quan trọng đối với việc nhóm, nhưng kết quả đầu ra lại yêu cầu tổng cộng trên tất cả các cấu trúc, chứ không được nhóm theo danh tính viết tắt. 

Một lỗi phổ biến là cố gắng mô phỏng số lượng tập hợp con tạo ra mỗi chuỗi viết tắt và sau đó tính tổng các chữ viết tắt duy nhất. Cách tiếp cận đó thất bại vì số lượng chữ viết tắt riêng biệt tự nó tăng theo cấp số nhân trong trường hợp xấu nhất. Một sai lầm khác là cho rằng sự va chạm giữa các chữ cái đầu tiên cần phải được theo dõi cẩn thận. Ví dụ: nếu nhiều chuỗi bắt đầu bằng cùng một ký tự, người ta có thể cố gắng nhóm theo tần số ký tự, nhưng điều đó không liên quan vì chúng tôi không được yêu cầu đếm số chữ viết tắt riêng biệt, chỉ tính tổng lược đồ trên tất cả các lựa chọn. 

Như một ví dụ nhỏ về mức độ tỉnh táo, nếu có ba chuỗi, thì câu trả lời đúng là 15. Phương pháp nhóm theo cách viết tắt đơn giản có thể làm phức tạp vấn đề này bằng cách theo dõi xem có bao nhiêu tập hợp con tạo thành mỗi mẫu chữ cái, nhưng cách giải thích đúng chỉ đơn giản là đếm tất cả các lựa chọn có thứ tự có thể có ở bất kỳ kích thước nào. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản. Chúng tôi xem xét mọi tập hợp con không trống của n chuỗi, sau đó xem xét mọi hoán vị của tập hợp con đó. Mỗi hoán vị đóng góp một công trình hợp lệ. Đối với một tập con cố định có kích thước k thì có k! hoán vị, do đó tổng đóng góp từ tất cả các tập hợp con có kích thước k là C(n, k) · k!. Tổng hợp tất cả k sẽ đưa ra câu trả lời cuối cùng. Mặc dù điều này đúng về mặt toán học, nhưng việc tạo ra các đối tượng này một cách rõ ràng là không thể vì số lượng thuật ngữ tăng theo cấp số nhân. 

Quan sát quan trọng là cấu trúc hoàn toàn không phụ thuộc vào các chữ cái thực tế trong chuỗi. Hoạt động viết tắt loại bỏ tất cả ngoại trừ ký tự đầu tiên, nhưng vì chúng tôi tính tổng tất cả các hoán vị bất kể chữ viết tắt kết quả là gì, nên mọi lựa chọn có thứ tự đều có giá trị như nhau và đóng góp chính xác một. Vì vậy, vấn đề giảm xuống còn việc đếm tất cả các lựa chọn có thứ tự của k mục riêng biệt từ n cho tất cả k có thể.

Với k cố định, số lượng các lựa chọn có thứ tự như vậy chính xác là giai thừa giảm dần n · (n − 1) · ... · (n − k + 1), thường được viết là nPk. Do đó, câu trả lời chỉ đơn giản là tổng của tất cả các giai thừa rơi từ k = 1 đến n. Điều này có thể được tính toán tăng dần theo thời gian tuyến tính bằng cách duy trì sản phẩm hiện tại và giảm hệ số nhân ở mỗi bước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con và hoán vị Brute Force | O(n! Tổng hợp) | O(n) | Quá chậm | 
| Tích lũy giai thừa giảm | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi định dạng lại nhiệm vụ thành tổng số hoán vị cho tất cả các kích thước tập hợp con. 

1. Bắt đầu bằng một giá trị đang chạy biểu thị số lượng lựa chọn có thứ tự có kích thước 1. Đây đơn giản là n, vì bất kỳ chuỗi nào trong số n chuỗi đều có thể được chọn một mình. 
2. Duy trì hệ số nhân giảm dần phản ánh số lượng lựa chọn còn lại khi kích thước tập hợp con tăng lên. Sau khi chọn k − 1 phần tử, lựa chọn tiếp theo có (n − (k − 1)) khả năng. 
3. Đối với mỗi tập hợp con có kích thước k từ 1 đến n, hãy tính số lượng các lựa chọn có thứ tự có kích thước k bằng cách nhân giá trị trước đó với (n − k + 1). Điều này xây dựng chuỗi giai thừa giảm dần mà không cần tính toán lại từ đầu. 
4. Thêm từng giá trị được tính toán vào bộ tích lũy lưu trữ câu trả lời cuối cùng theo modulo 1e9 + 7. 
5. Tiếp tục cho đến khi tất cả các kích cỡ đã được xử lý. 

Việc triển khai không bao giờ cần phải tự kiểm tra các chuỗi vì tất cả các cấu trúc hợp lệ chỉ phụ thuộc vào việc chọn và sắp xếp các chỉ số. 

### Tại sao nó hoạt động 

Mỗi cấu trúc hợp lệ tương ứng duy nhất với một chuỗi các chuỗi riêng biệt có thứ tự. Với mỗi k, số chuỗi như vậy chính xác là số hoán vị có độ dài k được rút ra từ n phần tử riêng biệt. Tổng trên k đếm mọi cách xây dựng có thể chính xác một lần, vì mỗi công trình có kích thước k duy nhất và chỉ được bao gồm trong số hạng tương ứng của tổng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n = int(input())
    
    # We only need n, strings are irrelevant
    for _ in range(n):
        input()
    
    ans = 0
    cur = 1  # will build falling factorial incrementally
    
    for k in range(1, n + 1):
        cur = cur * (n - k + 1) % MOD
        ans = (ans + cur) % MOD
    
    print(ans)

if __name__ == "__main__":
    solve()
```Ý tưởng triển khai chính là`cur`đại diện cho nPk cho k hiện tại. Mỗi lần lặp nhân với hệ số giảm tiếp theo. Điều này tránh việc tính toán lại và giữ cho lời giải tuyến tính. 

Các chuỗi đầu vào được đọc và loại bỏ ngay lập tức vì chúng không ảnh hưởng đến số lượng tổ hợp. Trạng thái duy nhất quan trọng là n. 

Số học modulo được áp dụng ở mỗi bước nhân và cộng để ngăn chặn tràn và giữ các giá trị trong giới hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
aaa
aab
bba
```Chúng tôi theo dõi tính toán: 

| k | nPk hiện tại | tổng số tiền chạy | 
| --- | --- | --- | 
| 1 | 3 | 3 | 
| 2 | 6 | 9 | 
| 3 | 6 | 15 | 

Kết quả cuối cùng là 15, khớp với việc liệt kê tất cả các lựa chọn có thứ tự. 

Điều này xác nhận rằng công thức tổng hợp chính xác các đóng góp trên tất cả các kích thước tập hợp con. 

### Ví dụ 2 

đầu vào:```
4
a
b
c
d
```| k | nPk hiện tại | tổng số tiền chạy | 
| --- | --- | --- | 
| 1 | 4 | 4 | 
| 2 | 12 | 16 | 
| 3 | 24 | 40 | 
| 4 | 24 | 64 | 

Kết quả 64 tương ứng với tất cả các lựa chọn có thứ tự không trống có thể có của một tập hợp 4 phần tử, phù hợp với cách giải thích của bài toán khi tính tổng số lượng hoán vị trên các kích thước tập hợp con. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Tính toán một lần các giá trị giai thừa giảm | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được duy trì | 

Giải pháp này dễ dàng phù hợp với các ràng buộc vì n lên tới 100000 và thuật toán thực hiện một lần cập nhật liên tục theo thời gian cho mỗi giá trị của k. Việc sử dụng bộ nhớ là không đổi bất kể kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    for _ in range(n):
        input()

    ans = 0
    cur = 1
    for k in range(1, n + 1):
        cur = cur * (n - k + 1) % MOD
        ans = (ans + cur) % MOD

    return str(ans)

# provided sample
assert solve("3\naaa\naab\nbba\n") == "15"

# minimum case
assert solve("1\na\n") == "1"

# all equal first letters but irrelevant structure
assert solve("2\naaa\nbbb\n") == "4"

# n = 4 full check
assert solve("4\na\nb\nc\nd\n") == "64"

# larger sanity
assert solve("5\na\nb\nc\nd\ne\n") == str((5 + 20 + 60 + 120 + 120) % MOD)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 | 1 | trường hợp cơ sở đúng đắn | 
| n=2 | 4 | tập hợp con + tích lũy hoán vị | 
| n=4 | 64 | tổng giai thừa giảm đầy đủ | 
| n=5 | 325 | tính đúng đắn chung | 

## Vỏ cạnh 

Trường hợp một cạnh là khi n bằng 1. Cách xây dựng duy nhất có thể là chọn chuỗi đơn, do đó câu trả lời phải là 1. Thuật toán bắt đầu bằng k = 1 cho ra nPk = 1P1 = 1, khớp trực tiếp. 

Một trường hợp khác là khi tất cả các chuỗi đều có chung chữ cái đầu tiên. Một cách tiếp cận ngây thơ có thể cố gắng nhóm theo chữ cái đó và đếm quá mức hoặc hợp nhất các đóng góp không chính xác. Ví dụ: với hai chuỗi “aaa” và “bbb”, thuật toán vẫn coi chúng là các mục riêng biệt. Phép tính tạo ra 2 + 2 = 4, tương ứng với tất cả các lựa chọn có thứ tự và không có sự mơ hồ nào phát sinh từ các tiền tố giống hệt nhau. 

Trường hợp cạnh cuối cùng có n lớn, trong đó các giá trị giống giai thừa trung gian tăng nhanh. Nếu không có số học mô-đun ở mỗi phép nhân, lỗi tràn hoặc số học không chính xác sẽ xảy ra. Việc triển khai áp dụng mô-đun ở mọi bước, đảm bảo tính chính xác ngay cả khi các giá trị vượt quá giới hạn số nguyên thông thường.
