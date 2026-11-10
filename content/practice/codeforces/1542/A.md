---
title: "CF 1542A - Bộ Lẻ"
description: "Chúng ta được cung cấp một tập hợp các số có kích thước chẵn, cụ thể là 2n số nguyên cho mỗi trường hợp thử nghiệm. Nhiệm vụ là quyết định xem có thể chia các số này thành n cặp rời nhau sao cho mỗi cặp gồm hai số có tổng là số lẻ."
date: "2026-06-16T15:14:50+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1542
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 729 (Div. 2)"
rating: 800
weight: 1542
solve_time_s: 309
verified: true
draft: false
---

[CF 1542A - Bộ lẻ](https://codeforces.com/problemset/problem/1542/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 5 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các số có kích thước chẵn, cụ thể là 2n số nguyên cho mỗi trường hợp thử nghiệm. Nhiệm vụ là quyết định xem có thể chia các số này thành n cặp rời nhau sao cho mỗi cặp gồm hai số có tổng là số lẻ. 

Một tổng là số lẻ khi một số trong cặp là số chẵn và số kia là số lẻ. Vì vậy, mọi cặp hợp lệ phải khớp một số chẵn với một số lẻ. Không có cặp nào có thể chứa hai chẵn hoặc hai tỷ lệ cược. 

Từ các ràng buộc, n nhiều nhất là 100 và các giá trị nhỏ, do đó mỗi trường hợp thử nghiệm có tối đa 200 số. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ cách tiếp cận nào lên tới O(n²) cho mỗi trường hợp thử nghiệm đều đủ nhanh. Chúng tôi không bị buộc phải tối ưu hóa phức tạp, nhưng chúng tôi vẫn nên tránh việc mô phỏng các cặp đôi không cần thiết. 

Cấu trúc then chốt của bài toán không phải là về sự sắp xếp mà là về tính chẵn lẻ. Điều duy nhất quan trọng là tồn tại bao nhiêu số chẵn và số lẻ. 

Một sai lầm ngây thơ ở đây là cố gắng thực sự xây dựng các cặp một cách tham lam mà không kiểm tra tính khả thi trên toàn cầu. Ví dụ: việc ghép số lẻ đầu tiên với số chẵn đầu tiên có thể không thành công sau này mặc dù thứ tự ghép nối khác sẽ hoạt động. Một sai lầm khác là cho rằng có ít nhất một số lẻ và một số chẵn là đủ. Điều đó không thành công khi số lượng bị mất cân bằng. 

Một trường hợp thất bại cụ thể cho lối suy nghĩ ngây thơ là: 

đầu vào:```
n = 2
array = [1, 3, 2, 4]
```Có hai tỷ lệ cược và hai chẵn. Điều này hoạt động. Nhưng nếu chúng ta thay đổi nó một chút:```
n = 2
array = [1, 3, 5, 2]
```Bây giờ chúng ta có 3 tỷ lệ cược và 1 chẵn. Nỗ lực tham lam vẫn có thể ghép đôi (1,2), nhưng sau đó tỷ lệ cược vẫn không thể so sánh được nên thất bại. Câu trả lời đúng chỉ phụ thuộc vào số lượng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng liệt kê tất cả các cặp phần tử 2n. Số cách chia 2n phần tử thành từng cặp là (2n)! / (2^n n!), phát triển cực kỳ nhanh ngay cả với n = 10. Mỗi phân vùng sẽ cần xác thực tất cả các cặp để đảm bảo tính chẵn lẻ, khiến điều này hoàn toàn không khả thi. 

Điều quan trọng là mỗi cặp hợp lệ phải chứa chính xác một số chẵn và một số lẻ. Điều đó có nghĩa là mỗi số chẵn đóng góp chính xác một cặp với một số lẻ và ngược lại. Vì vậy, số chẵn phải bằng số chẵn. Vì tổng các phần tử là 2n nên điều kiện này tương đương với việc có chính xác n số chẵn và n số lẻ. 

Điều này làm giảm vấn đề từ việc ghép cặp tổ hợp sang việc kiểm tra tần số đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê cặp Brute Force | O((2n)! ) | O(2n) | Quá chậm | 
| Đếm chẵn lẻ | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết từng trường hợp thử nghiệm một cách độc lập bằng cách phân tích số lượng chẵn lẻ. 

1. Đọc n và danh sách 2n số nguyên. 
2. Đếm xem có bao nhiêu số chẵn và bao nhiêu số lẻ. 

Tính chẵn lẻ của một số được xác định bằng cách kiểm tra % 2. 
3. Kiểm tra xem số phần tử chẵn có bằng số phần tử lẻ hay không. 

Điều kiện này là cần thiết vì mỗi cặp hợp lệ đều tiêu thụ chính xác một số chẵn và một số lẻ. 
4. Nếu chúng bằng nhau thì in ra "Có". Ngược lại in ra "Không". 

### Tại sao nó hoạt động 

Mỗi cặp hợp lệ phải chứa một số chẵn và một số lẻ, vì vậy mỗi cặp tiêu thụ chính xác một phần tử từ mỗi lớp chẵn lẻ. Nếu số lượng khác nhau, ít nhất một phần tử sẽ bị bỏ lại mà không có đối tác hợp lệ có tính chẵn lẻ đối diện. Ngược lại, nếu số lượng bằng nhau, chúng ta luôn có thể ghép từng số chẵn với một số lẻ riêng biệt một cách tùy ý, vì không có ràng buộc bổ sung nào đối với các giá trị ngoài tính chẵn lẻ. Điều này thiết lập cả sự cần thiết và tính đầy đủ của số lượng chẵn lẻ bằng nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    arr = list(map(int, input().split()))
    
    even = 0
    odd = 0
    
    for x in arr:
        if x % 2 == 0:
            even += 1
        else:
            odd += 1
    
    print("Yes" if even == odd else "No")
```Giải pháp lặp lại qua từng trường hợp thử nghiệm một cách độc lập. Chi tiết triển khai chính là chúng tôi không bao giờ cố gắng xây dựng các cặp một cách rõ ràng. Chúng tôi chỉ theo dõi số lượng chẵn lẻ để tránh sự phức tạp không cần thiết và loại bỏ những lo ngại về thứ tự. 

Sự so sánh`even == odd`là đủ vì tổng kích thước được cố định ở mức 2n, do đó đẳng thức tự động ngụ ý rằng cả hai đều chính xác là n. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2
2
2 3 4 5
3
2 3 4 5 5 5
```Đối với trường hợp thử nghiệm đầu tiên: 

| Bước | Mảng | Đếm chẵn | Số Lẻ | Quyết định | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [2,3,4,5] | 0 | 0 | - | 
| Quy trình 2 | [2,3,4,5] | 1 | 0 | - | 
| Quy trình 3 | [2,3,4,5] | 1 | 1 | - | 
| Quy trình 4 | [2,3,4,5] | 2 | 1 | - | 
| Quy trình 5 | [2,3,4,5] | 2 | 2 | Có | 

Điều này xác nhận rằng có thể ghép nối vì số lượng chẵn lẻ khớp nhau. 

Đối với trường hợp thử nghiệm thứ hai: 

| Bước | Mảng | Đếm chẵn | Số Lẻ | Quyết định | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [2,3,4,5,5,5] | 0 | 0 | - | 
| Quy trình 2 | ... | 1 | 0 | - | 
| Quy trình 3 | ... | 1 | 1 | - | 
| Quy trình 4 | ... | 2 | 1 | - | 
| Quy trình 5 | ... | 2 | 4 | Không | 

Sự mất cân bằng về tính chẵn lẻ khiến cho mọi yếu tố không thể phù hợp. 

### Mẫu 2 

đầu vào:```
1
1
2 3
```| Bước | Mảng | Đếm chẵn | Số Lẻ | Quyết định | 
| --- | --- | --- | --- | --- | 
| Quy trình | [2,3] | 1 | 1 | Có | 

Điều này cho thấy cấu hình hợp lệ tối thiểu, trong đó chính xác một số chẵn và một số lẻ tạo thành một cặp hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi quét từng phần tử một lần để đếm tính chẵn lẻ | 
| Không gian | O(1) | Chỉ có hai quầy được duy trì | 

Kích thước đầu vào nhỏ nên việc quét tuyến tính dễ dàng nằm trong giới hạn ngay cả đối với số lượng trường hợp thử nghiệm tối đa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    
    t = int(input())
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        
        even = sum(1 for x in arr if x % 2 == 0)
        odd = 2 * n - even
        
        output.append("Yes" if even == odd else "No")
    
    return "\n".join(output)

# provided samples
assert run("""5
2
2 3 4 5
3
2 3 4 5 5 5
1
2 4
1
2 3
4
1 5 3 2 6 7 3 4""") == """Yes
No
No
Yes
No"""

# custom cases
assert run("""3
1
1 2
2
1 3 5 7
2
2 4 6 8""") == """Yes
No
No"""

assert run("""1
2
1 2 3 4""") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 / 1 3 5 7 / 2 4 6 8 | Có / Không / Không | chẵn lẻ hỗn hợp, tất cả lẻ, tất cả chẵn | 
| 1 2 3 4 | Có | trường hợp chẵn lẻ cân bằng | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các số có cùng tính chẵn lẻ. 

Đối với đầu vào:```
n = 2
array = [2, 4, 6, 8]
```Thuật toán đếm chẵn = 4, lẻ = 0. Vì chúng không bằng nhau nên nó cho kết quả là "Không". Điều này phù hợp với lý do vì không có phần tử lẻ nào tồn tại để tạo thành bất kỳ cặp hợp lệ nào. 

Một trường hợp khác là sự cân bằng hoàn hảo:```
n = 3
array = [1, 2, 3, 4, 5, 6]
```Ở đây chẵn = 3 và lẻ = 3. Thuật toán in "Có" và tồn tại một cặp hợp lệ, chẳng hạn như (1,2), (3,4), (5,6). Điều này khẳng định tính đầy đủ của điều kiện chẵn lẻ.
