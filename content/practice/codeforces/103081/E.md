---
title: "CF 103081E - Bánh ngọt"
description: "Chúng ta được cung cấp một công thức bao gồm một số nguyên liệu và với mỗi nguyên liệu, chúng ta biết hai con số: cần bao nhiêu nguyên liệu để nướng một chiếc bánh và bao nhiêu nguyên liệu hiện có sẵn trong bếp."
date: "2026-07-03T23:17:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103081
codeforces_index: "E"
codeforces_contest_name: "2020-2021 ICPC Southwestern European Regional Contest (SWERC 2020)"
rating: 0
weight: 103081
solve_time_s: 40
verified: true
draft: false
---

[CF 103081E - Bánh ngọt](https://codeforces.com/problemset/problem/103081/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải quyết:** 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một công thức bao gồm một số nguyên liệu và với mỗi nguyên liệu, chúng ta biết hai con số: cần bao nhiêu nguyên liệu để nướng một chiếc bánh và bao nhiêu nguyên liệu hiện có sẵn trong bếp. Nhiệm vụ là xác định số lượng bánh hoàn chỉnh tối đa mà chúng ta có thể nướng sao cho không hết nguyên liệu. 

Một chiếc bánh yêu cầu phải đáp ứng đồng thời tất cả các yêu cầu về nguyên liệu nên nếu thiếu nguyên liệu nào thì chúng ta không thể tăng thêm số lượng bánh. Mỗi thành phần đặt ra giới hạn trên một cách độc lập về số lượng bánh có thể được sản xuất. Do đó, câu trả lời cuối cùng bị chi phối bởi thành phần hạn chế nhất. 

Các ràng buộc là cực kỳ nhỏ, tối đa là 10 thành phần. Điều này ngay lập tức gợi ý rằng ngay cả những tính toán rất trực tiếp cũng là đủ, vì bất kỳ giải pháp nào thực hiện công việc liên tục cho mỗi thành phần sẽ chạy ngay lập tức. Không cần tối ưu hóa ngoài số học đơn giản. 

Một trường hợp phức tạp xuất hiện khi nhu cầu về thành phần lớn so với nguồn cung. Ví dụ: nếu một thành phần yêu cầu 10 đơn vị cho mỗi chiếc bánh và chỉ có 5 đơn vị, thì chỉ riêng thành phần đó buộc câu trả lời là không có bánh bất kể sự phong phú khác. Một trường hợp khác là khi tất cả nguyên liệu đều dồi dào ngoại trừ một hạn chế cực kỳ chặt chẽ, trở thành nút thắt cổ chai. 

Ví dụ: 

đầu vào:```
2 5
70 1000
```Thành phần đầu tiên cho phép tối đa 2 // 5 = 0 bánh nên đáp án là 0 mặc dù thành phần thứ hai rất dồi dào. Một cách tiếp cận ngây thơ cho rằng các tỷ lệ tính tổng hoặc trung bình sẽ không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng từng chiếc bánh nướng một. Đối với mỗi lần thử đếm bánh k, chúng tôi kiểm tra xem mọi thành phần có sẵn ít nhất k lần số lượng cần thiết hay không. Chúng tôi tăng k cho đến khi điều kiện này không thành công. Mặc dù đúng nhưng phương pháp này thực hiện tối đa kiểm tra O(câu trả lời × N) và trong trường hợp bệnh lý khi số lượng thành phần lớn, câu trả lời có thể đạt tới 10^4 trở lên, khiến nó lặp đi lặp lại một cách không cần thiết so với mức cần thiết. 

Điều quan trọng là mỗi thành phần sẽ giới hạn số lượng bánh một cách độc lập. Nếu một thành phần yêu cầu r đơn vị cho mỗi chiếc bánh và chúng ta có sẵn một đơn vị thì số lượng bánh được phép dùng riêng cho thành phần đó là a // r. Vì tất cả các thành phần phải được đáp ứng đồng thời nên câu trả lời tổng thể là mức tối thiểu của các giới hạn cho mỗi thành phần này. Điều này chuyển đổi vấn đề từ mô phỏng lặp đi lặp lại sang tính toán một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(N × câu trả lời) | O(1) | Quá chậm | 
| Tối thiểu cho mỗi thành phần | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số thành phần N, sau đó đọc từng cặp giá trị thể hiện lượng cần thiết và lượng sẵn có của từng thành phần. Điều này đặt ra những ràng buộc sẽ xác định khả năng sản xuất bánh. 
2. Đối với mỗi thành phần, hãy tính xem nó có thể hỗ trợ riêng bao nhiêu chiếc bánh bằng cách chia số lượng có sẵn cho số lượng yêu cầu bằng phép chia số nguyên. Bước này chuyển đổi các hạn chế về tài nguyên thô thành số liệu thống nhất về “công suất bánh”. 
3. Duy trì một biến được khởi tạo ở giá trị rất lớn, thể hiện giới hạn trên tốt nhất hiện tại của số lượng bánh. 
4. Đối với mỗi thành phần, hãy cập nhật biến này bằng cách lấy giá trị tối thiểu giữa giá trị hiện tại và công suất tính toán của thành phần đó. Điều này đảm bảo rằng kết quả cuối cùng tôn trọng ràng buộc chặt chẽ nhất. 
5. Sau khi xử lý tất cả nguyên liệu, in ra giá trị tối thiểu cuối cùng, thể hiện số lượng bánh tối đa có thể làm được mà không vi phạm bất kỳ ràng buộc nào. 

Tại sao nó hoạt động: mỗi thành phần áp đặt một giới hạn trên độc lập cho các giải pháp khả thi. Bất kỳ nghiệm hợp lệ nào cũng phải thỏa mãn đồng thời tất cả các giới hạn, vì vậy không gian nghiệm là giao của các ràng buộc này. Giao điểm của giới hạn trên được xác định bởi giới hạn nhỏ nhất, đảm bảo rằng không có thành phần nào bị lạm dụng quá mức đồng thời tối đa hóa số lượng bánh hoàn chỉnh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    ans = 10**18

    for _ in range(n):
        need, have = map(int, input().split())
        ans = min(ans, have // need)

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đọc đầu vào tăng dần và duy trì mức hoạt động tối thiểu. Chi tiết triển khai chính là phép chia số nguyên, giúp xác định chính xác số lượng bánh có thể có cho mỗi thành phần. Việc khởi tạo câu trả lời cho một số rất lớn sẽ đảm bảo thành phần đầu tiên đặt đúng đường cơ sở. 

Một lỗi thường gặp là đảo ngược thứ tự chia. Tính toán đúng là có // cần, không cần // có. Cái sau sẽ đảo ngược ràng buộc và tạo ra kết quả vô nghĩa. Một điểm tinh tế khác là đảm bảo phép chia số nguyên thay vì dấu phẩy động, vì bánh phân số không liên quan. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
100 500
2 5
70 1000
```| Bước | Thành phần | Cần | Có | Công suất | Tối thiểu hiện tại | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 100 | 500 | 5 | 5 | 
| 2 | 2 | 2 | 5 | 2 | 2 | 
| 3 | 3 | 70 | 1000 | 14 | 2 | 

Thành phần thứ hai là yếu tố hạn chế, chỉ sản xuất được 2 chiếc bánh. Điều này cho thấy một ràng buộc chặt chẽ duy nhất sẽ chi phối kết quả như thế nào ngay cả khi những người khác hào phóng. 

### Ví dụ 2 

đầu vào:```
2
3 10
4 5
```| Bước | Thành phần | Cần | Có | Công suất | Tối thiểu hiện tại | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 10 | 3 | 3 | 
| 2 | 2 | 4 | 5 | 1 | 1 | 

Thành phần thứ hai làm giảm câu trả lời xuống 1. Điều này khẳng định rằng ngay cả những khác biệt nhỏ về tỷ lệ cũng có thể xác định kết quả cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi thành phần được xử lý một lần với số học theo thời gian không đổi | 
| Không gian | O(1) | Chỉ có một biến tích lũy duy nhất được sử dụng | 

Với N ≤ 10, lời giải có tốc độ nhanh không đáng kể và thậm chí các ràng buộc lớn hơn cũng có thể nằm gọn trong giới hạn do độ phức tạp tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys
    from io import StringIO
    old_stdout = sys.stdout
    sys.stdout = StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# single ingredient
assert run("1\n3 10\n") == "3"

# bottleneck is zero
assert run("2\n5 4\n2 10\n") == "0"

# all equal ratios
assert run("3\n2 10\n3 15\n5 25\n") == "5"

# mixed constraints
assert run("4\n1 7\n2 8\n3 9\n4 10\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 thành phần | phân chia trực tiếp | trường hợp cơ sở | 
| một thành phần không có công suất | 0 | xử lý nút thắt cứng | 
| thành phần tỷ lệ | mở rộng quy mô nhất quán | tỷ lệ thống nhất | 
| ràng buộc hỗn hợp | lựa chọn tối thiểu đúng | tính đúng đắn chung | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi một nguyên liệu không đủ số lượng cho dù chỉ một chiếc bánh. 

đầu vào:```
1
5 3
```Thuật toán tính 3 // 5 = 0. Vì mức tối thiểu đang chạy bắt đầu lớn nên nó trở thành 0 ngay lập tức và duy trì ở mức 0 cho đến khi kết thúc. Điều này phản ánh chính xác rằng không có loại bánh nào có thể làm được. 

Một trường hợp khác là khi tất cả các thành phần đều có tỷ lệ giống hệt nhau. 

đầu vào:```
3
2 10
2 10
2 10
```Mỗi thành phần mang lại 10 // 2 = 5, vì vậy mức tối thiểu vẫn là 5 xuyên suốt. Thuật toán không nhân hoặc tính tổng các dung lượng một cách nhầm lẫn, điều này sẽ làm tăng kết quả một cách không chính xác.
