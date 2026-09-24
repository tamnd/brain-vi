---
title: "CF 104804D - \u0420\u044b\u0446\u0430\u0440\u0438"
description: "Chúng tôi được cấp một túi chứa nhiều bộ mã thông báo hữu hạn. Trong số các token này, một số là token “hiệp sĩ” đặc biệt và số còn lại là token bình thường. Từ túi này, người chơi rút ngẫu nhiên một số lượng thẻ cố định một cách thống nhất mà không cần thay thế."
date: "2026-06-28T16:51:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104804
codeforces_index: "D"
codeforces_contest_name: "Central Russia Regional Contest, 2022, Qualification Contest"
rating: 0
weight: 104804
solve_time_s: 69
verified: true
draft: false
---

[CF 104804D - \u0420\u044b\u0446\u0430\u0440\u0438](https://codeforces.com/problemset/problem/104804/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một túi chứa nhiều bộ mã thông báo hữu hạn. Trong số các token này, một số là token “hiệp sĩ” đặc biệt và số còn lại là token bình thường. Từ túi này, người chơi rút ngẫu nhiên một số lượng thẻ cố định một cách thống nhất mà không cần thay thế. 

Nhiệm vụ là tính xác suất để ít nhất một trong số các quân bài được rút ra là hiệp sĩ. 

Nói một cách cụ thể hơn, chúng tôi đang lấy mẫu một tập hợp con có kích thước$m$từ một vũ trụ có kích thước$n$, chính xác là ở đâu$k$các phần tử được đánh dấu là thành công (hiệp sĩ). Mỗi tập hợp con có kích thước$m$có khả năng như nhau. Chúng tôi muốn xác suất để tập hợp con được chọn giao với tập hợp các hiệp sĩ. 

Những hạn chế rất nhỏ:$n \le 20$Và$m \le 20$. Điều này ngay lập tức báo hiệu rằng phép liệt kê tổ hợp hoặc tính toán trực tiếp sử dụng hệ số nhị thức là hoàn toàn đủ. Không cần đến phép tính gần đúng, mô phỏng hoặc tổ hợp tối ưu hóa tiệm cận. 

Trường hợp cạnh tinh tế xuất hiện khi$m \ge n$. Trong tình huống đó, mọi mã thông báo đều được rút ra, do đó xác suất trở thành 1 nếu$k > 0$, hoặc 0 nếu$k = 0$. Một trường hợp góc khác là$k = 0$, nơi không có hiệp sĩ nào tồn tại, làm cho xác suất bằng 0 một cách tầm thường bất kể$m$. Ở một thái cực khác, nếu$k \ge 1$Và$m = n$, đáp án chính xác là 1. 

Một mô phỏng Monte Carlo ngây thơ sẽ hội tụ quá chậm và gây ra các vấn đề về độ chính xác. Một sai lầm phổ biến khác là giả định tính độc lập của các lần rút thăm, điều này không chính xác vì việc lấy mẫu không thay thế. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là liệt kê tất cả các tập hợp con có kích thước$m$từ$n$phần tử, kiểm tra xem mỗi tập hợp con có chứa ít nhất một hiệp sĩ hay không và đếm xem có bao nhiêu phần tử hợp lệ. Tổng số tập hợp con là$\binom{n}{m}$, trong trường hợp xấu nhất là tối đa gần$n = 20, m = 10$, cho$\binom{20}{10} = 184,756$. Kích thước này đủ nhỏ để có thể trực tiếp sử dụng vũ lực, vì vậy tính đúng đắn là ngay lập tức. 

Tuy nhiên, sức mạnh vũ phu che giấu cấu trúc thực sự của vấn đề. Quan sát quan trọng là chúng ta không cần phải xem xét sự sắp xếp riêng lẻ mà chỉ tính xem có bao nhiêu hiệp sĩ được rút thăm. Thay vì đếm trực tiếp các tập hợp con thuận lợi, việc tính toán sự kiện bù sẽ đơn giản hơn: vẽ được 0 hiệp sĩ. Sự kiện đó tương ứng với việc chọn tất cả$m$mã thông báo từ$n-k$mã thông báo không phải hiệp sĩ. Điều này làm giảm vấn đề về một tỷ lệ tổ hợp duy nhất. 

Xác suất trở thành:$$P(\text{at least one knight}) = 1 - \frac{\binom{n-k}{m}}{\binom{n}{m}}$$với quy ước rằng$\binom{a}{b} = 0$khi$b > a$. 

Phép biến đổi này tránh liệt kê hoàn toàn các tập hợp con và thay thế bài toán bằng việc đánh giá một vài hệ số nhị thức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu |$O(\binom{n}{m} \cdot m)$|$O(m)$| Được chấp nhận nhưng không cần thiết | 
| Phần bù + Tổ hợp |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Tính toán từng bước 

1. Đọc số nguyên$n$,$k$, Và$m$. Chúng xác định tổng số nhóm, số lần thành công và cỡ mẫu tương ứng. 
2. Xử lý những trường hợp tầm thường trước. Nếu như$k = 0$, trả về 0 ngay lập tức vì không tồn tại phần tử thành công nào. Nếu như$m \ge n$, trả về 1 nếu$k > 0$, nếu không thì bằng 0. Điều này sẽ tránh được các biểu thức tổ hợp không hợp lệ sau này. 
3. Tính số lượng thẻ không phải hiệp sĩ, đó là$n - k$. Điều này đại diện cho nhóm “chỉ an toàn”. 
4. Tính xác suất chỉ rút được những người không phải hiệp sĩ là:$$\frac{\binom{n-k}{m}}{\binom{n}{m}}$$Tỷ lệ này phản ánh việc lựa chọn tất cả$m$các vật phẩm dành riêng cho những người không phải hiệp sĩ. 
5. Trừ giá trị này cho 1 để có xác suất có ít nhất một hiệp sĩ hiện diện. 
6. In kết quả với độ chính xác vừa đủ, thường có ít nhất 8 chữ số thập phân để đáp ứng yêu cầu về độ chính xác 1e-4. 

### Tại sao nó hoạt động 

Các thuật toán phân vùng đều có thể$m$-các tập hợp con có kích thước thành hai loại riêng biệt: loại có ít nhất một hiệp sĩ và loại không chứa hiệp sĩ nào. Hai sự kiện này bao phủ toàn bộ không gian mẫu mà không chồng chéo lên nhau. Sự kiện bổ sung “không có hiệp sĩ nào được chọn” dễ đếm hơn vì nó hạn chế lựa chọn hoàn toàn đối với một tập hợp con có kích thước giảm$n-k$. Vì tất cả các tập con có kích thước$m$có khả năng như nhau, xác suất chính xác là tỷ lệ của số lượng tập hợp con thuận lợi, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from math import comb

def solve():
    n, k, m = map(int, input().split())

    if k == 0:
        print("0.0")
        return

    if m >= n:
        print("1.0")
        return

    total = comb(n, m)
    if n - k < m:
        no_knight = 0
    else:
        no_knight = comb(n - k, m)

    ans = 1.0 - no_knight / total
    print(f"{ans:.10f}")

if __name__ == "__main__":
    solve()
```Việc triển khai dựa trực tiếp vào hàm tổ hợp tích hợp sẵn của Python, hàm này ổn định và chính xác đối với các ràng buộc này. Lối thoát sớm xử lý các trường hợp suy biến trong đó các hệ số nhị thức sẽ bằng 0 hoặc không xác định theo nghĩa đơn giản. 

Sự tinh tế duy nhất là đảm bảo xử lý đúng vụ việc$n-k < m$, nơi không thể lựa chọn$m$những người không phải hiệp sĩ, khiến xác suất “không hiệp sĩ” bằng không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 2 2
```Chúng tôi tính toán:$n = 5$,$k = 2$, vậy những người không phải hiệp sĩ = 3, và$m = 2$. 

| Bước | tổng C(5,2) | không hiệp sĩ C(3,2) | xác suất không hiệp sĩ | câu trả lời cuối cùng | 
| --- | --- | --- | --- | --- | 
| tính toán | 10 | 3 | 10/3 | 1 - 0,3 | 

Kết quả cuối cùng là$0.7$. 

Điều này xác nhận logic bổ sung: trường hợp thất bại duy nhất là chọn cả hai vật phẩm từ 3 người không phải hiệp sĩ. 

### Ví dụ 2 

đầu vào:```
8 2 4
```Đây$n = 8$,$k = 2$, vậy không phải hiệp sĩ = 6,$m = 4$. 

| Bước | tổng C(8,4) | không hiệp sĩ C(6,4) | xác suất không hiệp sĩ | câu trả lời cuối cùng | 
| --- | --- | --- | --- | --- | 
| tính toán | 70 | 15 | 15/70 | 1 - 0,2142857 | 

Kết quả cuối cùng là khoảng$0.78571429$. 

Ví dụ này cho thấy rằng ngay cả khi cỡ mẫu lớn, phần bù vẫn dễ tính toán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Đánh giá hệ số nhị thức theo thời gian không đổi cho n nhỏ | 
| Không gian |$O(1)$| Chỉ có một vài biến số nguyên được lưu trữ | 

Các ràng buộc là cực kỳ nhỏ, do đó, ngay cả các phép tính tổ hợp lặp đi lặp lại cũng không đáng kể. Giải pháp chạy thoải mái trong giới hạn và tránh mọi tính toán xác suất hoặc lặp lại. 

## Trường hợp thử nghiệm```python
import sys, io
from math import comb

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k, m = map(int, input().split())

    if k == 0:
        return "0.0000000000"
    if m >= n:
        return "1.0000000000"

    total = comb(n, m)
    no_knight = comb(n - k, m) if n - k >= m else 0
    ans = 1.0 - no_knight / total
    return f"{ans:.10f}"

# provided samples
assert abs(float(run("5 2 2")) - 0.7) < 1e-9
assert abs(float(run("8 2 4")) - 0.78571429) < 1e-6

# custom cases
assert run("5 0 3") == "0.0000000000"
assert run("5 5 2") == "1.0000000000"
assert run("5 1 5") == "1.0000000000"
assert run("6 3 1") == "0.5000000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 0 3 | 0,0 | Không có hiệp sĩ nào tồn tại | 
| 5 5 2 | 1.0 | Tất cả các mã thông báo đều là hiệp sĩ | 
| 5 1 5 | 1.0 | Rút thăm đầy đủ đảm bảo thành công | 
| 6 3 1 | 0,5 | Trường hợp xác suất rút thăm duy nhất | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi không có hiệp sĩ. Đối với đầu vào`5 0 3`, thuật toán ngay lập tức trả về 0 trước bất kỳ tổ hợp nào. Bất kỳ việc triển khai dựa trên công thức nào mà quên điều này vẫn có thể tính toán tỷ lệ nhưng tạo ra sự phân chia rủi ro hoặc tính toán không cần thiết. 

Một trường hợp khác là khi kích thước rút ra bằng kích thước túi, chẳng hạn như`5 2 5`. Xác suất bổ sung trở thành 0 vì không có cách nào để tránh chọn hiệp sĩ nếu có ít nhất một hiệp sĩ tồn tại. Thuật toán kích hoạt chính xác`m >= n`nhánh và trả về 1. 

Trường hợp thứ ba xảy ra khi có quá ít quân không phải hiệp sĩ để đáp ứng quy mô rút thăm. Ví dụ,`6 4 4`chỉ còn lại 2 người không phải hiệp sĩ. Sự tính toán`comb(2,4)`được coi là bằng 0, ngụ ý chính xác rằng mỗi lần rút hợp lệ phải bao gồm một quân mã, mang lại xác suất là 1.
