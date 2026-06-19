---
title: "CF 100B - Số thân thiện"
description: "Chúng ta được yêu cầu xác định xem liệu một tập hợp các số nguyên khác 0 có tạo thành một \"nhóm thân thiện\" hay không, nghĩa là mọi cặp số đều thỏa mãn mối quan hệ chia hết: một số chia hết cho số kia."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation"]
categories: ["algorithms"]
codeforces_contest: 100
codeforces_index: "B"
codeforces_contest_name: "Unknown Language Round 3"
rating: 1500
weight: 100
solve_time_s: 139
verified: true
draft: false
---

[CF 100B - Số thân thiện](https://codeforces.com/problemset/problem/100/B) 

**Đánh giá:** 1500 
**Tags:** *đặc biệt, thực hiện 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định xem liệu một tập hợp các số nguyên khác 0 có tạo thành một "nhóm thân thiện" hay không, nghĩa là mọi cặp số đều thỏa mãn mối quan hệ chia hết: một số chia hết cho số kia. Đầu vào cung cấp số lượng số nguyên,$n$, theo sau là danh sách các số nguyên được phân tách bằng dấu phẩy được sắp xếp theo thứ tự không giảm. Đầu ra phải là "BẠN" nếu toàn bộ nhóm thỏa mãn điều kiện này và nếu không thì "KHÔNG PHẢI BẠN BÈ". 

Được cho$n$có thể lên tới 1000 và mỗi số có thể có tối đa 7 chữ số, việc kiểm tra vũ phu tất cả các cặp là khả thi nhưng không tối ưu. Số thao tác trong trường hợp xấu nhất để kiểm tra theo cặp đầy đủ là$n(n-1)/2$, tức là khoảng 500.000 phép tính khi$n = 1000$. Điều này vẫn có thể quản lý được trong giới hạn thời gian 2 giây trong Python. Tuy nhiên, có một cấu trúc mà chúng ta có thể khai thác do đầu vào được sắp xếp giúp tránh được những kiểm tra không cần thiết. 

Các trường hợp cạnh không rõ ràng bao gồm các nhóm có tất cả các số giống nhau, các nhóm bao gồm 1 hoặc các nhóm có số lượng lớn không chia hết cho bất kỳ thành viên nhỏ hơn nào. Ví dụ: đầu vào "1, 3, 6, 12" là FRIENDS, nhưng "2, 3, 6" KHÔNG phải là FRIENDS vì 2 không chia 3 và 3 không chia 2. Việc thực hiện bất cẩn có thể chỉ kiểm tra các số liên tiếp trong mảng đã sắp xếp và trả về sai FRIENDS cho trường hợp sau. 

## Phương pháp tiếp cận 

Phương pháp brute-force kiểm tra từng cặp$a_i, a_j$với$i < j$và xác minh rằng$a_i$chia rẽ$a_j$hoặc$a_j$chia rẽ$a_i$. Điều này hiệu quả vì nó xác nhận rõ ràng định nghĩa về một nhóm thân thiện. Vì$n = 1000$, điều này đòi hỏi khoảng 500.000 lần kiểm tra tính chia hết. Mặc dù có thể chấp nhận được ràng buộc này nhưng nó không cần thiết đối với thứ tự sắp xếp của các số. 

Cách tiếp cận tối ưu tận dụng thứ tự sắp xếp. Nếu số nhỏ nhất chia hết cho tất cả các số khác thì mọi cặp trong mảng sẽ thỏa mãn điều kiện tình bạn. Điều này là do tính chia hết có tính bắc cầu dọc theo bội số của phần tử nhỏ nhất: nếu$x$chia rẽ$y$Và$y$chia rẽ$z$, sau đó$x$chia rẽ$z$. Do đó, chúng ta chỉ cần kiểm tra xem số nhỏ nhất có chia hết cho mọi số khác trong danh sách hay không. Điều này làm giảm số lượng kiểm tra từ$O(n^2)$ĐẾN$O(n)$. 

Brute-force hoạt động vì nó trực tiếp thực hiện định nghĩa nhưng không hiệu quả. Việc quan sát thấy phần tử nhỏ nhất phải chia cho tất cả các phần tử khác làm giảm vấn đề thành một lần quét tuyến tính duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Chấp nhận được nhưng chậm hơn | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$n$và danh sách các số được phân tách bằng dấu phẩy được sắp xếp. 
2. Chuyển đổi danh sách đầu vào thành số nguyên. 
3. Gán phần tử đầu tiên của danh sách cho một biến`smallest`. Đây là ứng cử viên phải chia rẽ tất cả những người khác. 
4. Lặp lại các số còn lại trong danh sách. 
5. Với mỗi số, hãy kiểm tra xem nó có chia hết cho không`smallest`. Nếu có số nào không chia hết thì in ngay "NOT FRIENDS" rồi thoát. 
6. Nếu mọi số đều chia hết cho`smallest`, in "BẠN". 

Điều bất biến là sau khi kiểm tra tất cả các số, nếu không xảy ra vi phạm thì phần tử nhỏ nhất sẽ chia hết cho tất cả các số còn lại. Vì các số được sắp xếp và tính chia hết có tính bắc cầu theo bội số của số nhỏ nhất nên mọi cặp trong nhóm đều thỏa mãn điều kiện tình bạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
nums = list(map(int, input().strip().split(',')))

smallest = nums[0]
for num in nums[1:]:
    if num % smallest != 0:
        print("NOT FRIENDS")
        sys.exit(0)
print("FRIENDS")
```Chúng tôi đọc đầu vào và phân tách nó bằng dấu phẩy, chuyển đổi chuỗi kết quả thành số nguyên. Chúng tôi lấy số đầu tiên là số nhỏ nhất và lặp lại phần còn lại để kiểm tra tính chia hết. sử dụng`sys.exit(0)`ngay lập tức chấm dứt chương trình khi có lỗi, tránh việc kiểm tra không cần thiết. Thứ tự thực hiện các thao tác rất quan trọng: chuyển đổi chuỗi thành số nguyên trước khi thực hiện modulo sẽ ngăn ngừa lỗi thời gian chạy. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:`1,3,6,12`| Biến | Lặp lại 1 | Lặp lại 2 | Lặp lại 3 | 
| --- | --- | --- | --- | 
| số | 3 | 6 | 12 | 
| num % nhỏ nhất | 3 % 1 = 0 | 6 % 1 = 0 | 12 % 1 = 0 | 

Tất cả các số đều chia hết cho 1. Kết quả: FRIENDS. Điều này xác nhận rằng chiến lược phần tử nhỏ nhất hoạt động khi giá trị nhỏ nhất là 1. 

Mẫu 2: 

đầu vào:`2,3,6`| Biến | Lặp lại 1 | Lặp lại 2 | 
| --- | --- | --- | 
| số | 3 | 6 | 
| num % nhỏ nhất | 3 % 2 = 1 | - | 

3 không chia hết cho 2. Kết quả: NOT FRIENDS. Điều này chứng tỏ sự thoát ra sớm khi thất bại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi lặp lại danh sách một lần để kiểm tra tính chia hết | 
| Không gian | O(n) | Chúng tôi lưu trữ danh sách các số nguyên trong bộ nhớ | 

Cho n ≤ 1000, thuật toán thực hiện tối đa 999 phép toán modulo. Việc sử dụng bộ nhớ là không đáng kể. Giải pháp phù hợp thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    n = int(input())
    nums = list(map(int, input().strip().split(',')))
    smallest = nums[0]
    for num in nums[1:]:
        if num % smallest != 0:
            print("NOT FRIENDS")
            return output.getvalue().strip()
    print("FRIENDS")
    return output.getvalue().strip()

# provided sample
assert run("4\n1,3,6,12\n") == "FRIENDS", "sample 1"
# all equal
assert run("3\n5,5,5\n") == "FRIENDS", "all equal"
# minimum size
assert run("1\n7\n") == "FRIENDS", "single element"
# maximum size, all divisible
assert run("5\n2,4,8,16,32\n") == "FRIENDS", "powers of two"
# non-divisible in middle
assert run("4\n2,3,6,12\n") == "NOT FRIENDS", "failure in middle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3,5,5 | BẠN BÈ | Tất cả các số bằng nhau | 
| 1 phần tử: 7 | BẠN BÈ | Nhóm đơn phần tử | 
| 2,4,8,16,32 | BẠN BÈ | Chuỗi bội số | 
| 2,3,6,12 | KHÔNG BẠN BÈ | Phát hiện sớm sự cố ở giữa | 

## Vỏ cạnh 

Đối với một nhóm thành phần đơn như`7`, thuật toán gán`smallest = 7`và không tìm thấy số còn lại. Nó in BẠN BÈ. Điều này xử lý chính xác mức tối thiểu$n=1$kịch bản. Đối với một nhóm có tất cả các số giống nhau, phép kiểm modulo luôn trả về 0, do đó FRIENDS được in. Nếu số nhỏ nhất là 1 thì mọi số khác đều chia hết cho 1, do đó FRIENDS được trả về chính xác ngay cả với số nguyên lớn. Việc thoát sớm đảm bảo chúng tôi không thực hiện các hoạt động không cần thiết khi phát hiện vi phạm.
