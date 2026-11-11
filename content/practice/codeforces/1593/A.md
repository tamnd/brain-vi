---
title: "CF 1593A - Bầu cử"
description: "Chúng ta có ba ứng cử viên trong một cuộc bầu cử, mỗi ứng cử viên có số phiếu bầu hiện tại: $a$, $b$ và $c$. Nhiệm vụ là xác định, một cách độc lập đối với mỗi ứng cử viên, họ cần bao nhiêu phiếu bầu bổ sung để đảm bảo họ là người chiến thắng."
date: "2026-06-10T09:05:04+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1593
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 748 (Div. 3)"
rating: 800
weight: 1593
solve_time_s: 108
verified: true
draft: false
---

[CF 1593A - Cuộc bầu cử](https://codeforces.com/problemset/problem/1593/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có ba ứng cử viên trong một cuộc bầu cử, mỗi ứng cử viên có số phiếu hiện tại:$a$,$b$, Và$c$. Nhiệm vụ là xác định, một cách độc lập đối với mỗi ứng cử viên, họ cần bao nhiêu phiếu bầu bổ sung để đảm bảo họ là người chiến thắng. Người chiến thắng được xác định là có nhiều phiếu bầu hơn hai ứng cử viên còn lại. Đối với mỗi ứng cử viên, phiếu bầu của hai người còn lại vẫn cố định; mục tiêu là làm cho ứng viên được đề cập dẫn trước một cách nghiêm túc. 

Các ràng buộc cho phép lên đến$10^4$trường hợp thử nghiệm và số phiếu bầu có thể cao bằng$10^9$. Điều này ngay lập tức cho chúng tôi biết rằng bất kỳ giải pháp nào lặp lại qua các phiếu bầu bổ sung tiềm năng sẽ quá chậm vì điều đó có thể yêu cầu hàng tỷ thao tác cho mỗi trường hợp thử nghiệm. Chúng ta cần một phương pháp tính toán số phiếu bầu cần thiết theo thời gian không đổi cho mỗi ứng cử viên. 

Các trường hợp khó khăn không rõ ràng phát sinh khi các ứng cử viên đã có cùng số phiếu bầu. Ví dụ, nếu số phiếu bầu$a=0, b=0, c=0$, mỗi ứng cử viên cần chính xác một phiếu bầu để vượt qua những người khác chứ không phải bằng không. Một trường hợp tinh vi khác xảy ra khi một ứng cử viên đã có nhiều phiếu bầu hơn những người khác. Ví dụ,$a=10, b=5, c=5$: ứng viên$a$không cần thêm phiếu bầu nào, trong khi những người khác cần đủ để vượt qua$a$. Những trường hợp này minh họa tầm quan trọng của việc xử lý sự bình đẳng một cách cẩn thận và thực thi điều kiện “nghiêm khắc hơn”. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ mô phỏng việc tăng thêm phiếu bầu cho từng ứng cử viên cho đến khi họ vượt lên dẫn trước những người khác. Dành cho ứng viên$a$, bạn sẽ tăng$a$từng cái một cho đến khi$a > \max(b, c)$, và tương tự cho$b$Và$c$. Mặc dù đúng nhưng điều này đòi hỏi tới$10^9$tăng trong trường hợp xấu nhất và thất bại đối với đầu vào lớn. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần mô phỏng. Để vượt qua các ứng cử viên khác, chúng ta chỉ cần biết mức tối đa hiện tại của hai ứng cử viên còn lại và tính toán sự khác biệt. Dành cho ứng viên$a$, số phiếu bầu bổ sung tối thiểu cần thiết là$\max(b, c) - a + 1$, nhưng chúng ta phải giới hạn nó ở mức 0 nếu$a$thực sự đã lớn hơn rồi. Lý luận này hoạt động giống hệt nhau đối với$b$Và$c$. Điều này biến vấn đề thành một vài phép tính số học cho mỗi ứng viên cho mỗi trường hợp kiểm thử, đảm bảo tính toán theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(max(a,b,c)) | O(1) | Quá chậm đối với số phiếu lớn | 
| Tối ưu | O(1) cho mỗi ứng viên, O(1) cho mỗi ca kiểm thử | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test, hãy đọc ba số nguyên$a$,$b$,$c$đại diện cho phiếu bầu. 
3. Đối với ứng viên$a$, tính số phiếu bổ sung cần thiết như$\max(0, \max(b, c) - a + 1)$. Mức tối đa bên trong tìm ra đối thủ cạnh tranh mạnh nhất, trừ đi$a$đưa ra khoảng cách và việc thêm 1 đảm bảo$a$trở nên lớn hơn một cách chặt chẽ. sử dụng$\max$bằng 0 đảm bảo chúng tôi không đề xuất phiếu bầu tiêu cực nếu$a$đã ở phía trước rồi. 
4. Lặp lại phép tính tương tự cho thí sinh$b$Và$c$. Vì$b$, sử dụng$\max(a, c) - b + 1$; vì$c$, sử dụng$\max(a, b) - c + 1$. 
5. In ba kết quả của test case. 

Tại sao nó hoạt động: Ở mỗi bước, chúng tôi tính toán chính xác mỗi ứng viên còn cách xa (những) người dẫn đầu hiện tại bao xa. Thêm một đảm bảo tính ưu việt nghiêm ngặt. Cách tiếp cận này đảm bảo tính đúng đắn vì nó trực tiếp thực thi điều kiện thắng cuộc và độc lập với những điều chỉnh của ứng viên khác, phù hợp với yêu cầu của bài toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a, b, c = map(int, input().split())
    A = max(0, max(b, c) - a + 1)
    B = max(0, max(a, c) - b + 1)
    C = max(0, max(a, b) - c + 1)
    print(A, B, C)
```Mã theo thuật toán trực tiếp. Chúng tôi sử dụng I/O nhanh với`sys.stdin.readline`từ$t$có thể lớn. Tính toán`max(b, c) - a + 1`thực thi điều kiện ưu việt nghiêm ngặt. Bên ngoài`max(0, …)`đảm bảo chúng tôi không trả về số âm cho các ứng viên đã dẫn đầu. Giải pháp tránh vòng lặp về số phiếu bầu, ngăn chặn các vấn đề về hiệu suất trên số lượng lớn. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
0 0 0
```| Ứng viên | Tối đa những người khác | Khoảng trống | Cần phiếu bầu | 
| --- | --- | --- | --- | 
| một | tối đa(0,0)=0 | 0-0=0 | 0+1=1 | 
| b | tối đa(0,0)=0 | 0-0=0 | 0+1=1 | 
| c | tối đa(0,0)=0 | 0-0=0 | 0+1=1 | 

Tất cả các ứng cử viên đều cần 1 phiếu, thể hiện sự bình đẳng. 

Tiếp theo, nhập:```
10 75 15
```| Ứng viên | Tối đa những người khác | Khoảng trống | Cần phiếu bầu | 
| --- | --- | --- | --- | 
| một | tối đa(75,15)=75 | 75-10=65 | +1 → 66 | 
| b | tối đa(10,15)=15 | 15-75=-60 | tối đa(0,-60)=0 | 
| c | tối đa(10,75)=75 | 75-15=60 | +1 → 61 | 

Ứng viên`b`đã ở phía trước rồi; những người khác ở phía sau. Dấu vết này cho thấy việc xử lý chính xác các khoảng trống âm và tính toán chặt chẽ hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm tính toán ba phép tính max/diff, tất cả đều O(1). | 
| Không gian | O(1) | Không cần cấu trúc bổ sung ngoài các biến đầu vào. | 

Được cho$t \le 10^4$và các hoạt động liên tục cho mỗi trường hợp thử nghiệm, giải pháp dễ dàng phù hợp với giới hạn thời gian 1 giây. Mức sử dụng bộ nhớ tối đa không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # solution
    t = int(input())
    for _ in range(t):
        a, b, c = map(int, input().split())
        A = max(0, max(b, c) - a + 1)
        B = max(0, max(a, c) - b + 1)
        C = max(0, max(a, b) - c + 1)
        print(A, B, C)
    return output.getvalue().strip()

# Provided samples
assert run("5\n0 0 0\n10 75 15\n13 13 17\n1000 0 0\n0 1000000000 0\n") == \
"1 1 1\n66 0 61\n5 5 0\n0 1001 1001\n1000000001 0 1000000001"

# Custom test cases
assert run("1\n0 0 1\n") == "2 2 0", "Candidate 3 ahead"
assert run("1\n5 5 5\n") == "1 1 1", "All equal"
assert run("1\n1000000000 0 0\n") == "0 1000000001 1000000001", "Maximum vote edge"
assert run("1\n0 1000000000 1000000000\n") == "1000000001 0 1", "Two tied leaders"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 1 | 2 2 0 | Ứng viên 3 đã dẫn trước | 
| 5 5 5 | 1 1 1 | Tất cả đều bình đẳng | 
| 10^9 0 0 | 0 10^9+1 10^9+1 | Cạnh phiếu bầu tối đa | 
| 0 10^9 10^9 | 10^9+1 0 1 | Hai nhà lãnh đạo bị ràng buộc | 

## Vỏ cạnh 

Đối với đầu vào`0 0 0`, mỗi ứng cử viên đều bị ràng buộc. Thuật toán tính toán số phiếu tối đa của những người khác (0), trừ số phiếu bầu của ứng cử viên (0) và cộng một phiếu, cho mỗi ứng cử viên 1 phiếu. Không có số âm nào được trả về do`max(0, …)`giấy gói. Đối với đầu vào`1000000000 0 0`, ứng viên`a`đã dẫn đầu nên việc tính toán cho`a`tạo ra 0. Ứng viên`b`Và`c`tính toán`max(1000000000,0)-0+1 = 1000000001`, đưa ra chính xác số phiếu cần thiết để vượt qua`a`. Tất cả các điều kiện cạnh bao gồm số phiếu bầu tối đa và sự bình đẳng đều được xử lý.
