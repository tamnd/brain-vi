---
title: "CF 104758I - ICPC Thạc sĩ"
description: "Chúng tôi được xếp một hàng những người tham gia cần được chia thành các đội. Ban tổ chức luôn cố gắng thành lập càng nhiều đội hoàn chỉnh càng tốt, trong đó mỗi đội hoàn chỉnh có chính xác $K$ người."
date: "2026-06-28T22:34:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104758
codeforces_index: "I"
codeforces_contest_name: "The 2023 ICPC Masters Mexico Regional #ICPCMX2023 Edition"
rating: 0
weight: 104758
solve_time_s: 49
verified: true
draft: false
---

[CF 104758I - Bậc thầy ICPC](https://codeforces.com/problemset/problem/104758/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được xếp một hàng những người tham gia cần được chia thành các đội. Ban tổ chức luôn cố gắng thành lập càng nhiều đội hoàn chỉnh càng tốt, trong đó mỗi đội hoàn chỉnh bao gồm chính xác$K$mọi người. Sau khi thành lập các đội đầy đủ này, có thể còn lại một số người tham gia không thể điền vào một nhóm đầy đủ khác.$K$. Những người tham gia còn lại được xếp vào một đội bổ sung. 

Điều này có nghĩa là cấu trúc cuối cùng được xác định hoàn toàn bằng cách chia$N$mọi người thành từng khối có kích thước$K$, với đoạn cuối cùng có thể nhỏ hơn$K$. Nhiệm vụ là xác định quy mô của nhóm nhỏ nhất sẽ được thành lập. 

Giới hạn đầu vào rất nhỏ:$N \le 1000$Và$K \le 10$. Điều này ngay lập tức loại trừ mọi lo ngại về hiệu quả; thậm chí một mô phỏng liên tục trừ đi$K$sẽ đủ nhanh. Thử thách thực sự không phải là thời gian tính toán mà là tránh hiểu sai về cách thành lập đội cuối cùng. 

Một cạm bẫy phổ biến xuất hiện khi$N$là bội số của$K$. Trong trường hợp đó, không còn nhóm nào còn sót lại. Ví dụ, nếu$N = 8$Và$K = 2$, chúng tôi thành lập bốn đội có quy mô 2, vì vậy đội nhỏ nhất vẫn là 2. Một cách giải thích ngây thơ rằng “luôn có một đội còn sót lại” sẽ giả định không chính xác một nhóm cuối cùng trống hoặc không có quy mô. 

Một trường hợp tế nhị khác là khi$K > N$. Ví dụ, nếu$N = 5$Và$K = 10$, không thể thành lập một nhóm đầy đủ, vì vậy mọi người ở cùng nhau trong một nhóm có quy mô 5. Bất kỳ giải pháp nào giả định tồn tại ít nhất một nhóm đầy đủ sẽ bỏ qua kịch bản này một cách không chính xác. 

## Phương pháp tiếp cận 

Cách mạnh mẽ nhất để nghĩ về điều này là mô phỏng rõ ràng việc phân nhóm từng người tham gia. Chúng tôi tiếp tục hình thành các nhóm có quy mô$K$, đếm xem mỗi nhóm có bao nhiêu người. Mỗi lần chúng tôi đạt tới$K$, chúng tôi hoàn thiện đội đó và bắt đầu một đội mới. Cuối cùng, nếu có một nhóm được lấp đầy một phần, chúng tôi sẽ đưa nhóm đó vào nhóm cuối cùng. 

Điều này hoạt động vì nó phản ánh chính xác quá trình được mô tả trong tuyên bố. Tuy nhiên, việc ghi chép đó là không cần thiết: chúng ta liên tục trừ đi$K$từ$N$, thực hiện một cách hiệu quả$O(N/K)$các bước, nhiều nhất là 1000 thao tác ở đây, vì vậy ngay cả sức mạnh vũ phu cũng không đáng kể. Sự kém hiệu quả mang tính khái niệm hơn là thực tế. 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần phải mô phỏng việc phân nhóm. Mỗi đội đầy đủ đều có quy mô$K$, và có nhiều nhất một đội còn sót lại có quy mô chính xác bằng$N \bmod K$. Vì vậy, đội nhỏ nhất chỉ đơn giản là kích thước còn lại nếu nó tồn tại, nếu không thì đó là$K$. Nếu không còn lại thì tất cả các đội đều bằng nhau. 

Điều này làm giảm toàn bộ vấn đề trong việc tính toán số dư và xử lý trường hợp 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(N/K)$|$O(1)$| Đã chấp nhận | 
| Số học mô-đun |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số dư$r = N \bmod K$, đại diện cho số lượng người tham gia còn lại sau khi thành lập các đội đầy đủ. Điều này trực tiếp nắm bắt liệu một nhóm chưa hoàn chỉnh cuối cùng có tồn tại hay không. 
2. Nếu$r = 0$, nó có nghĩa là$N$hoàn toàn chia hết cho$K$, vậy mỗi đội có chính xác$K$thành viên và không có nhóm nhỏ hơn. 
3. Nếu$r \neq 0$, thì đội cuối cùng bao gồm chính xác những người còn lại$r$người tham gia và tất cả các đội khác đều có quy mô$K$, vậy đội nhỏ nhất là$r$. 

### Tại sao nó hoạt động 

Phân vùng của$N$thành các nhóm có tính chất quyết định: phép trừ lặp đi lặp lại của$K$tạo ra các khối có kích thước đầy đủ giống hệt nhau$K$và nhiều nhất là một khối dư có kích thước nhỏ hơn$K$. Không có mô hình nhóm nào khác có thể thực hiện được theo các quy tắc. Điều này đảm bảo rằng quy mô nhóm tối thiểu phải là$K$hoặc phần còn lại, không có giá trị trung gian nào được đưa vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

N, K = map(int, input().split())

r = N % K
if r == 0:
    print(K)
else:
    print(r)
```Giải pháp đọc hai số nguyên và tính toán phần còn lại trực tiếp. Điểm quyết định duy nhất là liệu phần còn lại có bằng không hay không. Nếu có, chúng tôi xuất ra$K$, vì mọi đội đều đầy đủ. Mặt khác, chúng tôi xuất phần còn lại, vì đó là phần duy nhất nhỏ hơn-$K$nhóm. 

Một điểm tinh tế là xử lý các trường hợp$K > N$. Trong hoàn cảnh đó,$N \% K = N$, điều này phản ánh chính xác rằng không tồn tại một đội đầy đủ và một đội duy nhất chứa tất cả những người tham gia. 

## Ví dụ đã hoạt động 

### Ví dụ 1: Đầu vào`5 3`Chúng tôi tính toán cách người tham gia được nhóm lại. 

| Bước | Còn lại N | Các đội đầy đủ được thành lập | Phần còn lại | 
| --- | --- | --- | --- | 
| Bắt đầu | 5 | 0 | 5 | 
| Sau khi nhóm | 5 | 1 đội 3 người | 2 | 

Số còn lại là 2 nên các đội gồm một nhóm 3 người và một nhóm 2 người. Đội nhỏ nhất là 2. Điều này khẳng định rằng những người tham gia còn sót lại sẽ trực tiếp thành lập đội cuối cùng. 

### Ví dụ 2: Nhập liệu`21 2`| Bước | Còn lại N | Các đội đầy đủ được thành lập | Phần còn lại | 
| --- | --- | --- | --- | 
| Bắt đầu | 21 | 0 | 21 | 
| Sau khi nhóm | 21 | 10 đội 2 người | 1 | 

Phần còn lại là 1, do đó có mười đội đầy đủ cỡ 2 và một đội cuối cùng có cỡ 1. Do đó, quy mô đội tối thiểu là 1. Điều này cho thấy phần còn lại nhỏ chiếm ưu thế như thế nào trong câu trả lời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ thực hiện mô-đun và kiểm tra có điều kiện | 
| Không gian |$O(1)$| Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Các ràng buộc cho phép các phương pháp thậm chí còn chậm hơn, nhưng giải pháp số học trực tiếp là thời gian không đổi và phù hợp một cách tầm thường trong các giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    N, K = map(int, input().split())
    r = N % K
    if r == 0:
        return str(K)
    return str(r)

# provided samples
assert run("5 3\n") == "2", "sample 1"
assert run("21 2\n") == "1", "sample 2"
assert run("8 5\n") == "3", "sample 3"

# custom cases
assert run("2 10\n") == "2", "K > N case"
assert run("10 5\n") == "5", "perfect division"
assert run("1 1\n") == "1", "minimum boundary"
assert run("1000 3\n") == str(1000 % 3), "large N stress case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 10 | 2 | K lớn hơn N, nhóm đơn | 
| 10 5 | 5 | chia chính xác, không dư | 
| 1 1 | 1 | đầu vào nhỏ nhất có thể | 
| 1000 3 | 1 | độ chính xác còn lại trên giới hạn lớn hơn | 

## Vỏ cạnh 

Khi nào$K > N$, ví dụ như đầu vào`2 10`, thuật toán tính toán$2 \bmod 10 = 2$. Vì phần còn lại khác 0 nên nó trả về 2. Điều này khớp với nhóm thực tế mà không có đội đầy đủ nào có thể thành lập. 

Khi$N$chia hết cho$K$, chẳng hạn như`10 5`, phần còn lại trở thành 0. Thuật toán chuyển sang xuất$K$, cho điểm 5. Điều này phản ánh rằng tất cả các đội đều có quy mô đầy đủ và không tồn tại nhóm nhỏ hơn. 

Khi$K = 1$, mỗi người tham gia thành lập đội của riêng mình. Số dư luôn là 0 nên đáp án luôn là 1, phù hợp với thực tế là đội nào cũng có size 1.
