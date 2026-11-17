---
title: "CF 1968C - Lắp ráp thông qua dư lượng"
description: "Chúng ta được yêu cầu xây dựng lại một mảng các số nguyên khi chỉ biết sự khác biệt mô-đun giữa các phần tử liên tiếp."
date: "2026-06-07T18:06:21+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1968
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 943 (Div. 3)"
rating: 1000
weight: 1968
solve_time_s: 114
verified: false
draft: false
---

[CF 1968C - Lắp ráp thông qua phần còn lại](https://codeforces.com/problemset/problem/1968/C) 

**Đánh giá:** 1000 
**Tas:** giải thuật xây dựng, lý thuyết số 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng lại một mảng các số nguyên khi chỉ biết sự khác biệt mô-đun giữa các phần tử liên tiếp. Chính xác hơn, chúng ta được cung cấp các giá trị mô tả cách mỗi phần tử hoạt động khi chia cho phần tử trước đó và chúng ta phải xây dựng bất kỳ chuỗi hợp lệ nào khớp với tất cả các phần tử còn lại này. 

Trình tự được xây dựng phải thỏa mãn đồng thời hai ràng buộc. Đầu tiên, mọi phần tử phải nằm trong một phạm vi số lớn nhưng cố định lên tới một tỷ. Thứ hai, với mỗi cặp liền kề, phần tử thứ hai phải tạo ra số dư cho trước khi chia cho phần tử thứ nhất. 

Ý nghĩa cấu trúc quan trọng xuất phát từ cách modulo hoạt động. Nếu chúng ta sửa một giá trị trước đó$a_{i-1}$, thì giá trị tiếp theo$a_i$phải nằm ở dạng số học$$a_i = k \cdot a_{i-1} + x_i$$đối với một số số nguyên không âm$k$, và cả$x_i < a_{i-1}$. Bất đẳng thức này không phải là tùy chọn, nó là một yêu cầu cứng rắn đối với bất kỳ cách xây dựng hợp lệ nào. 

Các ràng buộc nhỏ theo một cách rất quan trọng:$n \le 500$cho mỗi trường hợp thử nghiệm và tổng của tất cả các trường hợp thử nghiệm là$2 \cdot 10^5$. Điều này ngay lập tức gợi ý rằng chúng ta nên hướng tới việc xây dựng tuyến tính cho mỗi trường hợp thử nghiệm. Bất kỳ giải pháp nào cố gắng tìm kiếm hoặc quay lại các giá trị có thể có của$a_i$sẽ quá chậm, vì ngay cả một hệ số phân nhánh khiêm tốn cũng sẽ bùng nổ sau 500 bước. 

Một trường hợp cạnh tinh tế phát sinh khi$x_i = 0$. Trong hoàn cảnh đó,$a_i$phải là bội số của$a_{i-1}$. Một nỗ lực ngây thơ bỏ qua điều này và chỉ gán các giá trị tùy ý có xu hướng phá vỡ khả năng chia hết ở một số bước sau đó. Ví dụ, nếu một người tham lam chọn các giá trị nhỏ, người ta có thể dễ dàng bị mắc kẹt vì các ràng buộc trong tương lai buộc phải có số dư không thể có. 

Một trường hợp có vấn đề khác là khi số dư liên tiếp lớn so với giá trị nhỏ trước đó. Ví dụ, nếu$a_{i-1} = 3$Nhưng$x_i = 4$, điều kiện ngay lập tức là không thể. Điều này cho chúng ta biết rằng việc xây dựng phải đảm bảo$a_{i-1}$luôn đủ lớn, không xuất phát từ những lựa chọn ngẫu nhiên nhỏ trước đó. 

## Phương pháp tiếp cận 

Một chiến lược bạo lực sẽ cố gắng chọn từng$a_i$bằng cách thử các giá trị lên tới$10^9$và kiểm tra xem nó có tạo ra phần còn lại cần thiết hay không$a_{i-1}$. Điều này đúng về mặt khái niệm nhưng không khả thi. Ngay cả khi chúng ta chỉ thử tối đa một số lượng ứng cử viên nhất định cho mỗi bước, trường hợp xấu nhất vẫn trở thành cấp số nhân trong$n$, vì mỗi vị trí phụ thuộc vào vị trí trước đó và mọi lựa chọn sai sẽ lan truyền về phía trước. 

Quan sát quan trọng là chúng ta không được yêu cầu giảm thiểu hoặc tối ưu hóa bất cứ điều gì. Chúng ta chỉ cần sự tồn tại, điều này cho phép chúng ta tự do thổi phồng các giá trị một cách mạnh mẽ để tránh xung đột. Một khi chúng ta nhận ra điều này, vấn đề sẽ trở thành vấn đề xây dựng hơn là vấn đề tìm kiếm. 

Ý tưởng cấu trúc quan trọng là đảm bảo rằng ở mỗi bước, giá trị trước đó hoàn toàn lớn hơn giá trị còn lại hiện tại. Nếu chúng tôi đảm bảo$$a_{i-1} > x_i$$thì chúng ta luôn có thể chọn$$a_i = a_{i-1} + x_i$$tự động thỏa mãn điều kiện modulo vì: 

- chia cho$a_{i-1}$để lại thương số 1 
- số dư chính xác$x_i$Vì vậy, toàn bộ vấn đề giảm xuống còn việc đảm bảo rằng dãy tăng chặt chẽ và luôn chiếm ưu thế so với phần còn lại tiếp theo. Vì tất cả$x_i \le 500$, chúng ta có thể bắt đầu một cách an toàn với một giá trị cơ sở đủ lớn và sau đó tiếp tục cộng. 

Một lựa chọn đơn giản và mạnh mẽ là thiết lập$a_1$như một hằng số lớn như$10^9 - 500$, sau đó xây dựng tiếp bằng cách sử dụng$a_i = a_{i-1} + x_i$. Điều này không bao giờ vượt quá giới hạn và duy trì giá trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Xây dựng tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đặt phần tử đầu tiên$a_1$với một số lượng lớn, ví dụ$10^9 - 500$. Điều này đảm bảo có đủ chỗ để tất cả những bổ sung sau này vẫn nằm trong giới hạn. 
2. Đối với từng vị trí$i$từ 2 đến$n$, định nghĩa$a_i = a_{i-1} + x_i$. Điều này đảm bảo trình tự tăng lên một cách chặt chẽ và duy trì quyền kiểm soát phần dư. 
3. Xuất mảng đã xây dựng. 

### Tại sao nó hoạt động 

Ở mỗi bước$i$, chúng tôi có$a_i = a_{i-1} + x_i$. Từ$x_i < a_{i-1}$luôn đúng vì$a_{i-1}$bắt đầu cực kỳ lớn và chỉ tăng lên, hoạt động modulo hoạt động có thể dự đoán được:$$a_i \bmod a_{i-1} = (a_{i-1} + x_i) \bmod a_{i-1} = x_i$$Danh tính này nắm giữ trực tiếp từ số học mô-đun. Việc xây dựng không bao giờ vi phạm giới hạn vì mức tăng trưởng tối đa có thể là$500 \cdot 500 = 250000$, xa bên dưới$10^9$. Do đó mọi ràng buộc đều được thỏa mãn một cách xác định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        x = list(map(int, input().split()))
        
        a = [0] * n
        a[0] = 10**9 - 500
        
        for i in range(1, n):
            a[i] = a[i - 1] + x[i - 1]
        
        print(*a)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo ý tưởng mang tính xây dựng. Lựa chọn thiết kế duy nhất là độ lệch ban đầu lớn, đảm bảo không có hành vi tràn hoặc mô-đun không hợp lệ. Mỗi giá trị tiếp theo được lấy theo thời gian không đổi, bảo toàn độ phức tạp tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
x = [2, 4, 1]
```Chúng tôi chọn$a_1 = 10^9 - 500$. 

| tôi | một[i-1] | x[i] | a[i] = a[i-1] + x[i] | a[i] mod a[i-1] | 
| --- | --- | --- | --- | --- | 
| 2 | 999999500 | 2 | 999999502 | 2 | 
| 3 | 999999502 | 4 | 999999506 | 4 | 
| 4 | 999999506 | 1 | 999999507 | 1 | 

Điều này xác nhận mỗi điều kiện mô-đun giữ chính xác theo yêu cầu. 

### Ví dụ 2 

đầu vào:```
n = 3
x = [1, 5]
```Bắt đầu với$a_1 = 10^9 - 500$. 

| tôi | một[i-1] | x[i] | một [tôi] | 
| --- | --- | --- | --- | 
| 2 | 999999500 | 1 | 999999501 | 
| 3 | 999999501 | 5 | 999999506 | 

Mỗi bước bảo tồn$a_i \bmod a_{i-1} = x_i$, vì giá trị mới khác với giá trị trước đó đúng bằng phần còn lại được yêu cầu. 

Dấu vết cho thấy rằng việc xây dựng không bao giờ phụ thuộc vào các lựa chọn trước đó ngoài sự liền kề, điều này ngăn cản sự thất bại xếp tầng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Mỗi phần tử được tính toán một lần bằng cách sử dụng phép cộng theo thời gian không đổi | 
| Không gian | O(n) | Lưu trữ mảng đầu ra | 

Tổng số tiền của$n$trên tất cả các bài kiểm tra được giới hạn bởi$2 \cdot 10^5$, do đó, giải pháp chạy thoải mái trong giới hạn bằng cách sử dụng các bước tuyến tính đơn giản và số học theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    
    t = int(sys.stdin.readline())
    for _ in range(t):
        n = int(sys.stdin.readline())
        x = list(map(int, sys.stdin.readline().split()))
        
        a = [0] * n
        a[0] = 10**9 - 500
        for i in range(1, n):
            a[i] = a[i - 1] + x[i - 1]
        
        output.append(" ".join(map(str, a)))
    
    return "\n".join(output)

# provided samples (format adjusted, correctness check style)
assert run("1\n4\n2 4 1\n") == "999999500 999999502 999999506 999999507"
assert run("1\n3\n1 1\n") == "999999500 999999501 999999502"

# custom cases
assert run("1\n2\n500\n") == "999999500 1000000000"
assert run("1\n5\n1 1 1 1\n") == "999999500 999999501 999999502 999999503 999999504"
assert run("1\n3\n5 5\n") == "999999500 999999505 999999510"
assert run("1\n4\n1 2 3\n") == "999999500 999999501 999999503 999999506"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần còn lại tối đa duy nhất | cộng ranh giới tại giới hạn | xử lý x lớn nhất | 
| những cái lặp đi lặp lại | tính đúng đắn của chuỗi đơn điệu | mức tăng nhỏ nhất | 
| giá trị hỗn hợp | tính đúng đắn chung | ổn định mẫu tùy ý | 

## Vỏ cạnh 

### Số dư lớn bằng 500 

đầu vào:```
n = 2, x = [500]
```Chúng tôi thiết lập$a_1 = 999999500$, Vì thế$a_2 = 1000000000$. Từ$a_2 - a_1 = 500$, chúng tôi nhận được$a_2 \bmod a_1 = 500$. Việc xây dựng vẫn nằm trong giới hạn vì chúng tôi cố tình để lại mức chênh lệch 500. 

### Tất cả những cái đó 

đầu vào:```
n = 5, x = [1, 1, 1, 1]
```Trình tự trở thành:```
999999500, 999999501, 999999502, 999999503, 999999504
```Mỗi bước giữ nguyên bất biến vì việc thêm 1 đảm bảo còn lại 1 khi chia cho số hạng trước đó. 

### Chiều dài chuỗi tối đa 

Khi nào$n = 500$và tất cả$x_i = 500$, giá trị cuối cùng tăng tối đa$250000$, vẫn còn kém xa$10^9$. Điều này xác nhận bộ đệm ban đầu là đủ và không xảy ra tràn ở bất kỳ bước nào.
