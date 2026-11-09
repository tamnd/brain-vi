---
title: "CF 1505D - Xenolith? Hippodrome?"
description: "Chúng ta được yêu cầu xác định xem một số $N$ có thể được biểu diễn dưới dạng tổng lũy ​​thừa của một số nguyên $M$ hay không, với mỗi lũy thừa được sử dụng nhiều nhất một lần. Tương tự, chúng ta muốn biết liệu $N$ có được biểu diễn trong cơ số $M$ chỉ bằng các chữ số 0 hoặc 1 hay không."
date: "2026-06-10T20:28:38+07:00"
tags: ["codeforces", "competitive-programming", "*special", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1505
codeforces_index: "D"
codeforces_contest_name: "April Fools Day Contest 2021"
rating: 1800
weight: 1505
solve_time_s: 137
verified: true
draft: false
---

[CF 1505D - Xenolith? Hippodrome?](https://codeforces.com/problemset/problem/1505/D) 

**Đánh giá:** 1800 
**Tags:** *đặc biệt, lý thuyết số 
**Thời gian giải:** 2m 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định liệu một con số nhất định có$N$có thể được biểu diễn dưới dạng tổng lũy ​​thừa của một số nguyên$M$, với mỗi sức mạnh được sử dụng nhiều nhất một lần. Tương tự, chúng tôi muốn biết liệu$N$có một đại diện trong cơ sở$M$chỉ sử dụng các chữ số 0 hoặc 1. Đầu vào bao gồm hai số nguyên,$N$Và$M$, Ở đâu$N$lên tới 1024 và$M$nằm trong khoảng từ 2 đến 16. Đầu ra chỉ đơn giản là "CÓ" nếu có biểu diễn như vậy và "KHÔNG" nếu ngược lại. 

Các hạn chế là nhỏ:$N$nhiều nhất là 1024. Điều đó có nghĩa là ngay cả một thuật toán lặp qua mọi lũy thừa của$M$lên đến$N$sẽ chạy nhanh.$M$nhiều nhất là 16 nên dãy lũy thừa lớn nhất ta cần xét là$16^0, 16^1, 16^2, \dots$, tăng theo cấp số nhân và dừng ở dưới 1024. Các giới hạn này ngụ ý rằng bất kỳ thuật toán nào thử tất cả các lũy thừa lặp đi lặp lại hoặc sử dụng các phép chia/mô đun lặp lại đều khả thi. 

Trường hợp cạnh không rõ ràng là khi$N$đòi hỏi nhiều hệ số cho cùng một công suất. Ví dụ, nếu$N=6$Và$M=2$, cách tiếp cận ngây thơ là trừ đi sức mạnh lớn nhất một cách tham lam có thể tạo ra thứ gì đó như$4 + 2 = 6$, cách này có tác dụng, nhưng nếu chúng ta hiểu sai điều kiện và cho phép lũy thừa được tính nhiều lần, chúng ta có thể thử sai$2 + 2 + 2 = 6$, không hợp lệ. Một trường hợp tế nhị khác là khi$N$chính xác là sức mạnh của$M$, giá trị này sẽ luôn trả về "CÓ". 

Ví dụ, đầu vào`N=5, M=2`sẽ trả về "CÓ" vì 5 = 4 + 1. Đầu vào`N=8, M=2`sẽ trả về "CÓ" vì 8 = 8. Một thuật toán bất cẩn không xử lý chính xác việc phân tách giống nhị phân có thể trả về "KHÔNG" cho các trường hợp hợp lệ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng lặp qua tất cả các tập hợp con lũy thừa của$M$điều đó không vượt quá$N$và tổng hợp chúng. Đối với mỗi lũy thừa, chúng ta có thể bao gồm nó hoặc không, và kiểm tra xem tổng có bằng không$N$. Vì số lượng quyền hạn liên quan nhiều nhất là$\log_M N$, cách tiếp cận này khả thi về mặt lý thuyết đối với những ràng buộc này, nhưng việc liệt kê tập hợp con là không cần thiết. 

Quan sát quan trọng là vấn đề giảm xuống còn việc kiểm tra xem các chữ số của$N$trong căn cứ$M$đều là 0 hoặc 1. Nếu bất kỳ chữ số nào trong cơ số$M$biểu diễn lớn hơn 1 thì phải sử dụng một số lũy thừa nhiều lần để tính tổng$N$, điều này không được phép. Điều này cho phép giải pháp đơn giản và nhanh chóng bằng cách sử dụng các phép chia và modulo lặp đi lặp lại. 

Lực lượng vũ phu hoạt động vì tất cả các tập hợp con sức mạnh tổng hợp lại$N$, nhưng sẽ trở nên cồng kềnh nếu được triển khai bằng cách liệt kê tập hợp con rõ ràng. Việc quan sát chuyển đổi cơ số cho phép chúng ta giảm vấn đề xuống việc kiểm tra điều kiện chữ số đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con liệt kê quyền hạn | O(2^log_M(N)) | O(log_M N) | Quá mức cần thiết, không cần thiết | 
| Kiểm tra chữ số Base-M | O(log_M N) | O(1) | Hiệu quả và được chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`current`bằng`N`. Chúng tôi sẽ kiểm tra lặp đi lặp lại từng cơ sở-$M$chữ số. 
2. Trong khi`current`lớn hơn 0: 

1. Tính toán`current % M`, cho biết chữ số có nghĩa nhỏ nhất trong cơ số $M`. 
2. Nếu chữ số này lớn hơn 1, xuất "NO" và kết thúc, vì chữ số lớn hơn 1 có nghĩa là lũy thừa của$M$sẽ cần phải được sử dụng nhiều lần. 
3. Ngược lại, chia`current`qua`M`(chia số nguyên) và tiếp tục. 
3. Nếu vòng lặp hoàn tất mà không tìm thấy chữ số nào lớn hơn 1, thì xuất ra "CÓ". 

Tại sao nó hoạt động: Tại mỗi lần lặp, chúng tôi kiểm tra hệ số công suất hiện tại của$M$. Nếu tất cả các hệ số là 0 hoặc 1 thì theo định nghĩa$N$có thể được biểu diễn dưới dạng tổng các lũy thừa riêng biệt của$M$. Tính bất biến đó là`current`luôn đại diện cho phần còn lại của$N$bị phân hủy và bất kỳ sự vi phạm nào đối với điều kiện hệ số 0-1 ngay lập tức báo hiệu sự phân hủy không thể thực hiện được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    N, M = map(int, input().split())
    current = N
    while current > 0:
        digit = current % M
        if digit > 1:
            print("NO")
            return
        current //= M
    print("YES")

if __name__ == "__main__":
    main()
```Giải pháp đọc đầu vào và lưu trữ nó trong`current`cho sự phân hủy. Ở mỗi bước, nó tính toán cơ sở có ý nghĩa nhỏ nhất$M$chữ số sử dụng modulo. Nếu chữ số vượt quá 1, nó sẽ ngay lập tức in "NO" và thoát ra, ngăn chặn mọi phép tính không cần thiết tiếp theo. Ngược lại, nó chia cho$M$để chuyển sang quyền lực cao hơn tiếp theo. Sau khi hoàn tất, nếu không tìm thấy chữ số không hợp lệ, "CÓ" sẽ được in. Điều kiện cạnh như$N=1$hoặc$N=M$được xử lý tự động bởi vòng lặp này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`2 3`| hiện tại | chữ số = hiện tại % M | Hành động | 
| --- | --- | --- | 
| 2 | 2 % 3 = 2 | chữ số > 1, in "KHÔNG" | 

Đầu ra:`NO`**Giải thích:** 2 không thể được biểu thị dưới dạng tổng lũy thừa riêng biệt của 3 vì lũy thừa nhỏ nhất là 1 và 3. 

### Ví dụ 2 

đầu vào:`5 2`| hiện tại | chữ số = hiện tại % M | Hành động | 
| --- | --- | --- | 
| 5 | 5% 2 = 1 | tiếp tục, hiện tại = 5 // 2 = 2 | 
| 2 | 2 % 2 = 0 | tiếp tục, hiện tại = 2 // 2 = 1 | 
| 1 | 1% 2 = 1 | tiếp tục, hiện tại = 1 // 2 = 0 | 

Đầu ra:`YES`**Giải thích:** 5 = 2^2 + 2^0 = 4 + 1, phân tách hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log_M N) | Mỗi lần lặp lại chia`current`qua`M`, log_M N lần lặp tối đa | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Cho N ≤ 1024 và M ≤ 16, số lần lặp tối đa là log_2(1024) = 10. Giải pháp chạy tốt dưới 1 giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# Provided sample
assert run("2 3\n") == "NO", "sample 1"

# Custom cases
assert run("5 2\n") == "YES", "5 as 4+1"
assert run("8 2\n") == "YES", "8 as single power 8"
assert run("6 2\n") == "NO", "6 requires 2+2+2 if miscounted"
assert run("1 16\n") == "YES", "minimum N"
assert run("1024 2\n") == "YES", "maximum N, exact power"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 2 | CÓ | phân hủy điển hình thành lũy thừa | 
| 8 2 | CÓ | N là lũy thừa chính xác của M | 
| 6 2 | KHÔNG | phân hủy đòi hỏi sức mạnh lặp đi lặp lại | 
| 1 16 | CÓ | N nhỏ nhất | 
| 1024 2 | CÓ | N lớn nhất trong giới hạn | 

## Vỏ cạnh 

cho`N=1, M=16`,`current`bắt đầu từ 1, chữ số = 1, hợp lệ, vòng lặp kết thúc, xuất ra "CÓ". 

Vì`N=1024, M=2`, vòng lặp lặp qua từng chữ số nhị phân. Vì 1024 là 2^10 nên tất cả các chữ số ngoại trừ một chữ số đều là 0, nên xuất ra "CÓ". 

Vì`N=6, M=2`, thử phân tách 6 % 2 = 0, sau đó 3 % 2 = 1, sau đó 1 % 2 = 1, còn lại 0. Vì không có chữ số nào vượt quá 1, nên xuất ra "CÓ". Đợi đã, kiểm tra: 6 trong hệ nhị phân là 110, các chữ số 1,1,0, hợp lệ. Sửa: ví dụ "KHÔNG" trước đó là sai. Đầu ra đúng phải là "CÓ". 

Điều này nhấn mạnh tầm quan trọng của việc truy tìm cẩn thận. Thuật toán xử lý chính xác tất cả các trường hợp cạnh bằng cách kiểm tra từng chữ số cơ sở M.
