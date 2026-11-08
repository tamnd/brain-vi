---
title: "CF 1445B - Loại bỏ"
description: "Chúng tôi được yêu cầu tính tổng điểm tối thiểu có thể có cho vòng loại Olympic. Mỗi người tham gia thi hai phần thi và ban giám khảo chỉ nhớ một phần thông tin: điểm thứ 100 của mỗi phần thi và giới hạn dưới của điểm phần thi còn lại…"
date: "2026-06-11T04:00:47+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1445
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 680 (Div. 2, based on Moscow Team Olympiad)"
rating: 900
weight: 1445
solve_time_s: 79
verified: true
draft: false
---

[CF 1445B - Loại bỏ](https://codeforces.com/problemset/problem/1445/B) 

**Xếp hạng:** 900 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tính tổng điểm tối thiểu có thể có cho vòng loại Olympic. Mỗi người tham gia thi đấu ở hai cuộc thi và ban giám khảo chỉ nhớ một phần thông tin: điểm thứ 100 ở mỗi cuộc thi và giới hạn dưới về điểm của cuộc thi kia đối với 100 người tham gia cao nhất. Cụ thể, ở cuộc thi đầu tiên, người đứng thứ 100 đã ghi điểm`a`và tất cả 100 người tham gia hàng đầu đều có ít nhất`b`điểm ở cuộc thi thứ hai. Tương tự, ở cuộc thi thứ hai, người xếp thứ 100 ghi điểm`c`và tất cả 100 người tham gia hàng đầu đều có ít nhất`d`điểm trong cuộc thi đầu tiên Chúng tôi phải xác định tổng số điểm tối thiểu của người tham gia đứng ở vị trí thứ 100 khi xếp hạng theo tổng điểm. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp là bốn số nguyên`a, b, c, d`với những hạn chế`0 ≤ a, b, c, d ≤ 9`,`d ≤ a`Và`b ≤ c`. Các ràng buộc nhỏ, cho phép thực hiện các thao tác O(1) trên mỗi trường hợp thử nghiệm. Thách thức chính không phải là hiệu quả mà là lý luận về cách kết hợp điểm số để tạo ra mức giới hạn tối thiểu có thể. 

Một giải pháp đơn giản có thể cố gắng mô phỏng điểm số của tất cả người tham gia, nhưng điều đó nhanh chóng trở nên không thực tế, đặc biệt khi số lượng người tham gia chính xác không được xác định. Các trường hợp đặc biệt liên quan đến các tình huống trong đó vị trí thứ 100 của cuộc thi đầu tiên có số điểm thấp hơn mức tối thiểu trong cuộc thi thứ hai dành cho top 100 hoặc ngược lại. Ví dụ, nếu`a = 0, b = 0, c = 0, d = 0`, tổng số điểm tối thiểu rõ ràng là 0. Những cách tiếp cận bất cẩn có thể bỏ qua thực tế là chúng ta chỉ cần xem xét tổ hợp điểm trong trường hợp xấu nhất trong số 100 người đứng đầu chứ không phải danh sách đầy đủ người tham gia. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng liệt kê tất cả các phân phối điểm có thể có của những người tham gia tôn trọng các giới hạn nhất định và tìm ra tổng số điểm thứ 100. Mỗi người tham gia có thể nhận bất kỳ giá trị nào trong các ràng buộc, dẫn đến sự bùng nổ tổ hợp. Với 100 người tham gia, ngay cả với điểm số 0-9, số lượng hoạt động sẽ quá nhiều, mặc dù về mặt lý thuyết khả thi ở đây do giới hạn nhỏ. 

Điểm mấu chốt là chúng ta không cần phải mô phỏng tất cả những người tham gia. Chúng tôi chỉ quan tâm đến tổng số điểm thứ 100. Để giảm thiểu giới hạn, chúng tôi có thể giả định rằng tất cả những người tham gia khác có khả năng đẩy giới hạn lên cao hơn sẽ được chỉ định điểm thấp nhất cho phép phù hợp với dữ liệu đã cho. Do đó, tổng số điểm tối thiểu đạt được khi chúng ta chọn số tiền tối đa`a + b`Và`c + d`.`a + b`đại diện cho tổng điểm của người tham gia thứ 100 trong cuộc thi đầu tiên nếu họ đạt điểm tối thiểu trong cuộc thi thứ hai và`c + d`đại diện cho tổng điểm của người tham gia thứ 100 trong cuộc thi thứ hai nếu họ đạt điểm tối thiểu trong cuộc thi đầu tiên. Điểm giới hạn không thể thấp hơn một trong hai điểm này vì vị trí thứ 100 trong mỗi cuộc thi phải có ít nhất số điểm nhất định ở cuộc thi kia. 

Quan sát này thu gọn vấn đề thành một vấn đề đơn giản`max(a + b, c + d)`tính toán cho mỗi trường hợp thử nghiệm, tạo ra giải pháp O(1) cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(100^2) | O(100) | Quá mức cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test, hãy đọc các số nguyên`a, b, c, d`. 
3. Tính tổng điểm thứ 100 của cuộc thi đầu tiên và điểm cuộc thi thứ hai tối thiểu trong số 100 người đứng đầu, tức là:`sum1 = a + b`. 
4. Tính tổng điểm thứ 100 của cuộc thi thứ hai và điểm cuộc thi đầu tiên tối thiểu trong số 100 người đứng đầu, tức là:`sum2 = c + d`. 
5. Tận dụng tối đa`sum1`Và`sum2`. Đây là tổng số điểm nhỏ nhất mà người tham gia thứ 100 có thể có mà không vi phạm các ràng buộc nhất định. 
6. In mức tối đa này. 

Tại sao nó hoạt động: Bất kỳ kịch bản giả định nào có tổng điểm thấp hơn`max(a + b, c + d)`sẽ vi phạm các điều kiện để đạt vị trí thứ 100 trong ít nhất một cuộc thi.`a + b`đảm bảo người tham gia thứ 100 trong cuộc thi 1 đáp ứng mức tối thiểu cho cuộc thi 2 và`c + d`đảm bảo người tham gia thứ 100 trong cuộc thi 2 đáp ứng mức tối thiểu cho cuộc thi 1. Lấy mức tối đa đảm bảo tất cả các ràng buộc đều được tôn trọng trong khi giảm thiểu tổng số. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a, b, c, d = map(int, input().split())
    print(max(a + b, c + d))
```Mã này đọc số lượng trường hợp kiểm thử và lặp qua từng trường hợp kiểm thử. Đối với mỗi bộ bốn số nguyên, nó tính toán tổng số điểm có thể có cho vị trí thứ 100 và in ra điểm tối đa của chúng. sử dụng`sys.stdin.readline`đảm bảo I/O nhanh cho nhiều trường hợp thử nghiệm. các`map(int, input().split())`chuyển đổi bốn giá trị thành số nguyên một cách hiệu quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`1 2 2 1`| Biến | Giá trị | 
| --- | --- | 
| một | 1 | 
| b | 2 | 
| c | 2 | 
| d | 1 | 
| a + b | 3 | 
| c + d | 3 | 
| tối đa(a+b,c+d) | 3 | 

Đầu ra là 3, phù hợp với mẫu. Tổng số điểm tối thiểu của cả hai người tham gia thứ 100 đều trùng khớp. 

### Ví dụ 2 

đầu vào:`4 8 9 2`| Biến | Giá trị | 
| --- | --- | 
| một | 4 | 
| b | 8 | 
| c | 9 | 
| d | 2 | 
| a + b | 12 | 
| c + d | 11 | 
| tối đa(a+b,c+d) | 12 | 

Đầu ra là 12, xác nhận rằng điểm giới hạn bị chiếm ưu thế bởi vị trí thứ 100 của cuộc thi đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm yêu cầu một số lượng hoạt động không đổi. | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ, không phụ thuộc vào kích thước đầu vào. | 

Được cho`t ≤ 3025`và các thao tác tối thiểu cho mỗi trường hợp thử nghiệm, điều này phù hợp thoải mái trong giới hạn thời gian 1 giây và giới hạn bộ nhớ 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    t = int(input())
    for _ in range(t):
        a, b, c, d = map(int, input().split())
        print(max(a + b, c + d))
    
    return output.getvalue().strip()

# Provided samples
assert run("2\n1 2 2 1\n4 8 9 2\n") == "3\n12", "sample 1 & 2"

# Custom cases
assert run("1\n0 0 0 0\n") == "0", "all zeros"
assert run("1\n9 9 9 9\n") == "18", "all maximum values"
assert run("1\n5 5 5 5\n") == "10", "all equal"
assert run("1\n3 7 6 2\n") == "10", "mixed values"
assert run("1\n0 9 9 0\n") == "9", "max cross scenario"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 0 0 | 0 | trường hợp cạnh giá trị tối thiểu | 
| 9 9 9 9 | 18 | trường hợp cạnh giá trị tối đa | 
| 5 5 5 5 | 10 | kịch bản giá trị hoàn toàn bằng nhau | 
| 3 7 6 2 | 10 | kịch bản hỗn hợp chung | 
| 0 9 9 0 | 9 | đảm bảo tính toán tối đa chính xác | 

## Vỏ cạnh 

Nếu`a = d = 0`Và`b = c = 9`, sau đó`a + b = 0 + 9 = 9`Và`c + d = 9 + 0 = 9`. Tối đa là 9, đưa ra tổng số điểm tối thiểu cho vị trí thứ 100 một cách chính xác. Thuật toán xử lý chính xác các tình huống trong đó mức tối thiểu của cuộc thi đầu tiên kết hợp với mức tối thiểu của cuộc thi thứ hai, bất kể yếu tố nào chiếm ưu thế.
