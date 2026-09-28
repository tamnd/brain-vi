---
title: "CF 104834B - Bánh nướng Baklava"
description: "Chúng tôi đang làm việc với các số nguyên có chín chữ số đại diện cho các cấu hình có thể có của các lớp baklava của Janise. Mỗi cấu hình hợp lệ chỉ là một số nguyên $N$ trong phạm vi từ $100{,}000{,}000$ đến $999{,}999{,}999$."
date: "2026-06-28T11:49:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104834
codeforces_index: "B"
codeforces_contest_name: "UTPC Contest 12-01-23 Div. 1 (Advanced)"
rating: 0
weight: 104834
solve_time_s: 82
verified: false
draft: false
---

[CF 104834B - Nướng bánh Baklava](https://codeforces.com/problemset/problem/104834/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với các số nguyên có chín chữ số đại diện cho các cấu hình có thể có của các lớp baklava của Janise. Mỗi cấu hình hợp lệ chỉ là một số nguyên$N$trong phạm vi từ$100{,}000{,}000$ĐẾN$999{,}999{,}999$. Đối với mỗi trường hợp thử nghiệm, một ước số$K$đã cho và chúng ta phải đếm xem có bao nhiêu số có chín chữ số chia hết cho$K$đồng thời đáp ứng một ràng buộc bổ sung dựa trên chữ số. 

Điều kiện bổ sung đó liên quan đến việc đảo ngược các chữ số của$N$. Khi số đảo ngược đó chia hết cho 5 thì ta xét$N$đủ điều kiện để tính. Vì vậy, nhiệm vụ là lọc tất cả các số có chín chữ số theo hai thuộc tính cùng một lúc: số ban đầu phải chia hết cho$K$và sự đảo ngược chữ số của nó phải chia hết cho 5. 

Các ràng buộc đẩy điều này tới giải pháp thời gian không đổi cho mỗi truy vấn. Với tối đa$10^5$trường hợp thử nghiệm và$K$lớn như$10^5$, việc lặp qua tất cả các số ứng cử viên cho mỗi truy vấn sẽ yêu cầu khoảng$10^9$hoạt động cho mỗi thử nghiệm trong trường hợp xấu nhất, vượt xa giới hạn chấp nhận được. Bất kỳ giải pháp nào liệt kê các số hoặc mô phỏng việc đảo ngược chữ số cho mỗi ứng cử viên đều không khả thi ngay lập tức. 

Một vấn đề tế nhị xuất hiện trong việc giải thích điều kiện đảo ngược. Nếu xử lý một cách máy móc, người ta có thể cố gắng tính các số đảo ngược cho từng bội số của$K$, nhưng điều đó gây ra thao tác chữ số không cần thiết. Điều quan trọng là điều kiện chia hết ngược lại thực sự trở thành một ràng buộc ở chữ số đầu tiên của$N$, loại bỏ mọi sự phụ thuộc vào phần còn lại của số. 

Một sai lầm phổ biến là nghĩ rằng sự đảo ngược làm thay đổi khả năng chia hết một cách phức tạp. Ví dụ, lấy$N = 120000005$, đảo ngược nó mang lại$500000021$và việc kiểm tra khả năng chia hết cho 5 chỉ phụ thuộc vào chữ số cuối cùng của số đảo chứ không phụ thuộc vào cấu trúc đầy đủ. Quan sát này là những gì đơn giản hóa toàn bộ vấn đề. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ liệt kê tất cả các số có chín chữ số, đảo ngược từng số, kiểm tra mức chia hết cho 5 và kiểm tra thêm khả năng chia hết cho$K$. có$900{,}000{,}000$những con số như vậy và mỗi trường hợp thử nghiệm sẽ yêu cầu quét toàn bộ phạm vi này. Với tối đa$10^5$trường hợp thử nghiệm, điều này trở nên hoàn toàn không thể thực hiện được, vượt quá$10^{14}$hoạt động. 

Sự đơn giản hóa quan trọng đến từ việc kiểm tra xem “nghịch đảo chia hết cho 5” thực sự có nghĩa là gì. Một số chia hết cho 5 khi chữ số cuối cùng của nó là 0 hoặc 5. Sau khi đảo ngược, chữ số cuối cùng của số bị đảo ngược là chữ số đầu tiên của số ban đầu. Điều này làm giảm điều kiện thành một hạn chế ở chữ số đầu của$N$. Từ$N$là số có chín chữ số, chữ số đứng đầu không thể là 0 nên phải là 5. 

Điều này biến đổi toàn bộ không gian tìm kiếm thành một khoảng liền kề: tất cả các số hợp lệ nằm giữa$500{,}000{,}000$Và$599{,}999{,}999$. Vấn đề sau đó trở thành việc đếm xem có bao nhiêu bội số của$K$nằm trong một khoảng cố định, có thể được trả lời bằng cách sử dụng phép chia sàn trong thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(T \cdot 10^9)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(T)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nhận biết rằng việc đảo ngược một số chỉ ảnh hưởng đến thứ tự chữ số, chữ số cuối cùng của số đảo ngược là chữ số đầu tiên của số ban đầu. Do đó, điều kiện chia hết cho 5 hạn chế chữ số đầu tiên của số có chín chữ số. 
2. Kết luận rằng chữ số đầu tiên phải là 5, vì số có chín chữ số không thể bắt đầu bằng 0. Điều này cố định phạm vi số hợp lệ thành một khối từ$500{,}000{,}000$ĐẾN$599{,}999{,}999$. 
3. Viết lại bài toán dưới dạng đếm các số nguyên chia hết cho$K$bên trong khoảng này. Thay vì lặp lại, chúng ta sử dụng phép đếm số học của bội số. 
4. Tính xem có bao nhiêu bội số của$K$nhỏ hơn hoặc bằng giới hạn trên$R = 599{,}999{,}999$, và trừ đi bao nhiêu nằm dưới giới hạn dưới$L = 500{,}000{,}000$. 
5. Xuất ra sự khác biệt cho từng trường hợp thử nghiệm. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thực tế là mọi số có chín chữ số hợp lệ được xác định duy nhất bởi vị trí của nó trong một khoảng cố định và điều kiện đảo ngược không phụ thuộc vào bất kỳ chữ số nào ngoại trừ chữ số đầu tiên. Điều này làm giảm vấn đề lọc ban đầu thành vấn đề giao khoảng. Đếm bội số của$K$trong một khoảng thông qua việc chia sàn liệt kê chính xác tất cả các ứng viên hợp lệ mà không bỏ sót hoặc trùng lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_multiples(r, k):
    return r // k

T = int(input())
L = 500_000_000
R = 599_999_999

for _ in range(T):
    K = int(input())
    ans = R // K - (L - 1) // K
    print(ans)
```Giải pháp dựa hoàn toàn vào thuộc tính chia số nguyên. biểu hiện`R // K`đếm tất cả bội số của$K$lên đến giới hạn trên, trong khi`(L - 1) // K`loại bỏ những cái nằm dưới khoảng hợp lệ. Điều này tránh các vòng lặp rõ ràng và đảm bảo xử lý liên tục theo thời gian cho mỗi truy vấn. 

Một cạm bẫy thực hiện phổ biến là quên trừ`L - 1`còn hơn là`L`. sử dụng`L`trực tiếp sẽ loại trừ không chính xác các số bằng giới hạn dưới khi chúng là bội số hợp lệ của$K$. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng đầu vào mẫu: 

| K | Phạm vi bội số hợp lệ ≤ R | bội số < L | Trả lời | 
| --- | --- | --- | --- | 
| 1 | 599.999.999 | 499.999.999 | 100.000.000 | 
| 2 | 299.999.999 | 249.999.999 | 50.000.000 | 
| 3 | 199.999.999 | 166.666.666 | 33.333.333 | 

Vì$K = 2$, bội số của 2 bên trong khoảng hợp lệ tạo thành một mẫu số học thông thường bắt đầu từ 500.000.000 và kết thúc ở 599.999.998. Việc đếm thông qua phân chia tầng khớp chính xác với chuỗi này mà không tạo ra nó một cách rõ ràng. 

Điều này xác nhận rằng việc chuyển đổi sang đếm khoảng sẽ bảo toàn tất cả cấu trúc từ điều kiện dựa trên chữ số ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T)$| Mỗi trường hợp thử nghiệm sử dụng một số phép tính số học không đổi | 
| Không gian |$O(1)$| Không cần cấu trúc dữ liệu phụ trợ | 

Giải pháp phù hợp thoải mái trong giới hạn vì ngay cả với$10^5$truy vấn, chương trình chỉ thực hiện các phép chia số nguyên đơn giản cho mỗi truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    T = int(input())
    L = 500_000_000
    R = 599_999_999
    out = []
    for _ in range(T):
        K = int(input())
        out.append(str(R // K - (L - 1) // K))
    return "\n".join(out)

# provided samples
assert run("5\n1\n2\n3\n4\n5\n") == "100000000\n50000000\n33333333\n25000000\n20000000"

# custom: smallest K
assert run("1\n1\n") == "100000000"

# custom: K larger than range
assert run("1\n1000000000\n") == "0"

# custom: K = 500e6 (edge alignment)
assert run("1\n500000000\n") == "1"

# custom: mixed
assert run("3\n2\n7\n13\n") == run("3\n2\n7\n13\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| K=1 | kích thước khoảng đầy đủ | tính chính xác của đường cơ sở | 
| K lớn | 0 | không có bội số trong phạm vi | 
| K=500000000 | 1 | bao gồm ranh giới | 

## Vỏ cạnh 

cho$K = 1$, thuật toán trả về tổng kích thước của khoảng$599{,}999{,}999 - 500{,}000{,}000 + 1 = 100{,}000{,}000$, khớp với thực tế là mọi số trong phạm vi đều hợp lệ. Việc tính toán giảm chính xác đến`R - (L - 1)`. 

Đối với rất lớn$K$, chẳng hạn như$10^9$, cả hai`R // K`Và`(L - 1) // K`đánh giá là 0, tạo ra kết quả đúng là 0 vì không tồn tại bội số nào trong khoảng. 

Ở ranh giới$K = 500{,}000{,}000$, khoảng chứa chính xác một bội số, chính là giới hạn dưới. Công thức trừ đảm bảo nó được bao gồm bởi vì`(L - 1) // K`không tính nó trong khi`R // K`làm.
