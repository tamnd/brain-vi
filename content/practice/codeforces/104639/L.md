---
title: "CF 104639L - KaChang!"
description: "Chúng ta được cung cấp một chương trình tham khảo có thời gian chạy cố định ở mức $T$, và một danh sách $n$ các chương trình khác có thời gian chạy đã biết."
date: "2026-06-29T16:58:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104639
codeforces_index: "L"
codeforces_contest_name: "The 2023 ICPC Asia EC Regionals Online Contest (I)"
rating: 0
weight: 104639
solve_time_s: 41
verified: true
draft: false
---

[CF 104639L - KaChang!](https://codeforces.com/problemset/problem/104639/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chương trình tham khảo có thời gian chạy cố định tại$T$, và một danh sách$n$các chương trình khác với thời gian chạy đã biết. Chúng tôi được yêu cầu chọn một số nguyên$k \ge 2$sao cho giới hạn thời gian là$k \cdot T$là đủ để mọi chương trình trong danh sách kết thúc, nghĩa là mọi$t_i \le k \cdot T$. Trong số tất cả các giá trị hợp lệ, chúng ta phải xuất ra giá trị nhỏ nhất có thể$k$. 

Viết lại điều kiện, mỗi chương trình áp đặt giới hạn dưới cho$k$: cho một cái nhất định$t_i$, chúng tôi cần$k \ge \frac{t_i}{T}$. Từ$k$phải là một số nguyên, ràng buộc thực sự sẽ trở thành$k \ge \left\lceil \frac{t_i}{T} \right\rceil$. Do đó, câu trả lời cuối cùng là mức tối đa đối với tất cả các chương trình theo yêu cầu của từng chương trình này, với ràng buộc bổ sung là$k \ge 2$. 

Những hạn chế$n \le 10^5$Và$t_i \le 10^9$đề nghị chúng ta phải tính toán câu trả lời theo thời gian tuyến tính. Bất kỳ cách tiếp cận nào liên quan đến việc sắp xếp đều không cần thiết và sẽ chậm hơn yêu cầu nhưng vẫn có thể chấp nhận được; tuy nhiên, một lần vượt qua là đủ. 

Một trường hợp tinh vi xuất hiện khi tất cả các chương trình đều chạy rất nhanh so với$T$. Khi đó mọi tỷ lệ đều nhỏ hơn 1, do đó mức tối đa được tính toán sẽ trở thành 1, nhưng vấn đề thực thi một cách rõ ràng$k \ge 2$. Ví dụ, nếu$T = 1000$và tất cả$t_i \le 500$, câu trả lời đúng là 2 mặc dù tất cả các chương trình đều đã vượt qua$k = 1$. 

Một trường hợp cạnh khác xảy ra khi một số$t_i$lớn hơn nhiều so với$T$, có thể đẩy câu trả lời lên tới$10^9$tỉ lệ. Vì phép nhân nằm trong phạm vi số nguyên 64 bit nên chúng tôi an toàn trong Python nhưng sẽ cần cẩn thận trong các ngôn ngữ có chiều rộng cố định. 

## Phương pháp tiếp cận 

Một cách trực tiếp để suy nghĩ về vấn đề là thử tất cả các giá trị có thể có của$k$, bắt đầu từ 2 trở lên và kiểm tra xem tất cả các chương trình có thỏa mãn$t_i \le kT$. Đối với mỗi$k$, điều này đòi hỏi phải quét tất cả$n$các chương trình, vì vậy mỗi lần kiểm tra đều tốn$O(n)$. Trong trường hợp xấu nhất,$k$có thể lớn lên khoảng$10^9$, điều này làm cho phương pháp này hoàn toàn không khả thi. 

Quan sát quan trọng là tính khả thi là đơn điệu trong$k$. Nếu nhất định$k$hoạt động, sau đó lớn hơn$k$cũng hoạt động. Điều này làm giảm vấn đề tìm số nguyên nhỏ nhất$k$thỏa mãn một tập hợp các giới hạn dưới, có thể được suy ra trực tiếp từ mỗi chương trình một cách độc lập. 

Mỗi chương trình đóng góp một yêu cầu$k \ge \lceil t_i / T \rceil$. Thay vì tìm kiếm khắp$k$, chúng tôi tính toán trực tiếp các yêu cầu này và lấy mức tối đa của chúng. Điều này thu gọn vấn đề từ tìm kiếm trong một khoảng thời gian thành tổng hợp một lượt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force hơn k |$O(n \cdot k)$|$O(1)$| Quá chậm | 
| Tỷ lệ tối đa trực tiếp |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$n$Và$T$. Những điều này xác định đường cơ sở chia tỷ lệ cho tất cả các so sánh. 
2. Khởi tạo một biến$k\_req = 2$, vì sự cố buộc hệ số nhân tối thiểu được phép là 2 bất kể dữ liệu. 
3. Đối với mỗi thời gian chương trình$t_i$, hãy tính số nhân nhỏ nhất có thể vượt qua. Đây là$\left\lceil \frac{t_i}{T} \right\rceil$. 
4. Cập nhật$k\_req = \max(k\_req, \lceil t_i / T \rceil)$. Điều này đảm bảo rằng tất cả các ràng buộc được thấy cho đến nay vẫn được thỏa mãn. Mức tối đa được sử dụng vì mọi chương trình đều ràng buộc một cách độc lập cùng một phạm vi toàn cầu$k$. 
5. Sau khi xử lý tất cả các chương trình, xuất ra$k\_req$. 

### Tại sao nó hoạt động 

Mỗi chương trình áp đặt một bất đẳng thức độc lập$kT \ge t_i$, có thể được sắp xếp lại thành$k \ge t_i / T$. Vì tất cả các ràng buộc phải được giữ đồng thời,$k$phải làm hài lòng người mạnh nhất trong số họ. Mức trần chuyển đổi các yêu cầu phân số thành các ràng buộc khả thi số nguyên. Việc lấy mức tối đa trên tất cả các chương trình sẽ bảo toàn chính xác ràng buộc chặt chẽ nhất, do đó giá trị cuối cùng vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, T = map(int, input().split())
    arr = list(map(int, input().split()))
    
    k_req = 2
    
    for t in arr:
        # compute ceil(t / T)
        k = (t + T - 1) // T
        if k > k_req:
            k_req = k
    
    print(k_req)

if __name__ == "__main__":
    main()
```Cốt lõi của việc thực hiện là tính toán trần số nguyên`(t + T - 1) // T`, giúp tránh các phép toán dấu phẩy động và đảm bảo tính chính xác cho các giá trị lớn. Việc khởi tạo thành 2 là cần thiết vì ngay cả khi tất cả các tỷ lệ gợi ý 1 thì ràng buộc vẫn không cho phép điều đó. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào giống như mẫu:```
5 1000
998 244 353 1111 2333
```Chúng tôi tính toán các số nhân cần thiết: 

| tôi | t_i | trần(t_i/T) | k_req | 
| --- | --- | --- | --- | 
| 1 | 998 | 1 | 2 | 
| 2 | 244 | 1 | 2 | 
| 3 | 353 | 1 | 2 | 
| 4 | 1111 | 2 | 2 | 
| 5 | 2333 | 3 | 3 | 

Câu trả lời cuối cùng là 3, vì chương trình cuối cùng yêu cầu ít nhất$3T = 3000$. 

Dấu vết này cho thấy rằng mặc dù hầu hết các chương trình đều có kích thước nhỏ so với$T$, một giá trị lớn duy nhất chiếm ưu thế trong kết quả. 

Bây giờ hãy xem xét trường hợp tất cả các chương trình đều nhỏ:```
3 100
10 20 30
```| tôi | t_i | trần(t_i/T) | k_req | 
| --- | --- | --- | --- | 
| 1 | 10 | 1 | 2 | 
| 2 | 20 | 1 | 2 | 
| 3 | 30 | 1 | 2 | 

Mặc dù tất cả các tỷ lệ đều dưới 1 nhưng ràng buộc buộc câu trả lời là 2. 

Điều này chứng tỏ rằng giới hạn dưới$k \ge 2$độc lập với dữ liệu đầu vào và phải được thực thi sau tất cả các so sánh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một lần vượt qua tất cả các thời gian của chương trình, công việc liên tục trên mỗi phần tử | 
| Không gian |$O(1)$| Chỉ có một số biến số nguyên được duy trì | 

Giải pháp phù hợp thoải mái trong các ràng buộc vì$n \le 10^5$và mỗi phép toán là một phép tính số nguyên theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    
    n, T = map(int, input().split())
    arr = list(map(int, input().split()))
    
    k_req = 2
    for t in arr:
        k_req = max(k_req, (t + T - 1) // T)
    
    return str(k_req)

# provided sample
assert solve("5 1000\n998 244 353 1111 2333\n") == "3"

# all small values, forces k=2
assert solve("3 100\n10 20 30\n") == "2"

# exact multiples
assert solve("3 10\n10 20 30\n") == "3"

# large single outlier
assert solve("4 1000\n1 2 3 1000000\n") == "1000"

# minimum case
assert solve("1 5\n1\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| giá trị nhỏ | 2 | thực thi giới hạn dưới k ≥ 2 | 
| bội số | 3 | phép chia chính xác đúng đắn | 
| ngoại lệ | 1000 | sự thống trị của ràng buộc tối đa | 
| phần tử đơn | 2 | xử lý ranh giới tối thiểu | 

## Vỏ cạnh 

Khi tất cả thời gian của chương trình nhỏ hơn nhiều so với$T$, mức trần tính toán luôn là 1. Ví dụ:```
2 100
1 2
```Thuật toán tính toán$k = 1$cho cả hai, nhưng vì khởi tạo là$k\_req = 2$, đầu ra cuối cùng vẫn là 2. Vòng lặp không bao giờ ghi đè lên nó, điều này thực thi chính xác ràng buộc của vấn đề. 

Khi một chương trình cực kỳ lớn:```
2 10
1 100000
```Việc tính toán tiến hành như sau. Giá trị đầu tiên mang lại$k = 1$, Vì thế$k\_req = 2$. Giá trị thứ hai cho$k = 10000$, Vì thế$k\_req$trở thành 10000. Điều này cho thấy rằng một ràng buộc duy nhất xác định đầy đủ câu trả lời và các giá trị trước đó chỉ đóng vai trò là giới hạn dưới.
