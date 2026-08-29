---
title: "CF 104375B - Lưu trữ thùng"
description: "Chúng ta có một tình huống trong đó cà phê lần đầu tiên được đóng gói vào nhiều thùng nhỏ giống hệt nhau. Mỗi thùng nhỏ luôn chứa chính xác $K$ đơn vị cà phê và có $N$ thùng như vậy. Vì vậy, tổng số lượng cà phê chỉ đơn giản là $N nhân K$."
date: "2026-07-01T17:26:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104375
codeforces_index: "B"
codeforces_contest_name: "2023 ICPC Gran Premio de Mexico 1ra Fecha"
rating: 0
weight: 104375
solve_time_s: 64
verified: true
draft: false
---

[CF 104375B - Lưu trữ thùng](https://codeforces.com/problemset/problem/104375/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tình huống trong đó cà phê lần đầu tiên được đóng gói vào nhiều thùng nhỏ giống hệt nhau. Mỗi thùng nhỏ luôn đựng được chính xác$K$đơn vị cà phê, và có$N$những thùng chứa như vậy. Vì vậy, tổng lượng cà phê chỉ đơn giản là$N \times K$. 

Cà phê này sau đó phải được đóng gói lại vào các thùng chứa lớn hơn, trong đó mỗi thùng lớn có thể chứa tối đa$L$đơn vị. Mục đích là để xác định cần bao nhiêu thùng chứa lớn nếu chúng ta đóng gói một cách tối ưu, nghĩa là chúng ta đổ đầy từng thùng lớn nhất có thể trước khi sử dụng thùng tiếp theo. 

Vì vậy, nhiệm vụ giảm xuống còn tính toán bao nhiêu dung lượng-$L$thùng là cần thiết để lưu trữ tổng cộng$N \cdot K$đơn vị. 

Các hạn chế là nhỏ:$N, K, L \leq 1000$. Điều này ngay lập tức cho chúng ta biết rằng tổng lượng cà phê nhiều nhất là$10^6$, do đó, ngay cả việc mô phỏng theo đơn vị hoặc số học đơn giản cũng sẽ đủ nhanh. Bất cứ điều gì lên đến thời gian tuyến tính trong kích thước đầu vào hoặc thậm chí trong tổng lượng cà phê đều có thể chấp nhận được, nhưng rõ ràng chúng ta có thể hướng tới một$O(1)$giải thuật số học. 

Trường hợp cạnh tinh tế xuất hiện khi tổng số tiền chia hết cho$L$. Ví dụ, nếu$N=2, K=3, L=6$, tổng số là$6$, và chúng ta cần chính xác một thùng. Cách tiếp cận phân chia số nguyên đơn giản phải đảm bảo nó xử lý chính xác cả trường hợp chia hết và không chia hết bằng cách làm tròn số. 

Một trường hợp góc khác là khi$L = K$. Sau đó, mỗi thùng lớn chứa chính xác những gì thùng nhỏ chứa và câu trả lời sẽ trở thành chính xác.$N$. Nếu việc triển khai thực hiện nhầm phép chia số nguyên mà không xem xét tổng hợp, thì nó có thể thu gọn cấu trúc một cách không chính xác, nhưng ở đây lý do đúng vẫn hoàn toàn dựa trên tổng số. 

## Phương pháp tiếp cận 

Một cách đơn giản để nghĩ về điều này là mô phỏng quá trình đổ đầy từng thùng lớn một đơn vị. Chúng tôi lấy tổng số cà phê$N \cdot K$, trừ đi nhiều lần$L$, và đếm xem chúng ta làm như vậy bao nhiêu lần. Điều này đúng vì mỗi phép trừ tượng trưng cho việc đổ đầy một thùng lớn. 

Tuy nhiên, cách tiếp cận này trở nên không hiệu quả trong trường hợp xấu nhất. Nếu như$N \cdot K$lớn và$L = 1$, chúng tôi sẽ thực hiện lên đến$10^6$lần lặp lại. Mặc dù vẫn có thể chấp nhận được ở ranh giới nhưng điều đó là không cần thiết do cấu trúc của vấn đề. 

Quan sát quan trọng là về cơ bản chúng ta đang phân chia một lượng cố định thành các nhóm có kích thước$L$. Đây chính xác là phép chia số nguyên có làm tròn số. Thay vì mô phỏng, chúng tôi tính toán trực tiếp mức trần của$\frac{N \cdot K}{L}$. Điều này ghi lại cả nhóm đầy đủ và bất kỳ nhóm cuối cùng được lấp đầy một phần nào trong một biểu thức. 

Quá trình chuyển đổi từ mô phỏng sang số học xuất phát từ việc nhận ra rằng không có gì thay đổi trong suốt quá trình, chỉ có tổng số là quan trọng. Khi chúng tôi tổng hợp tất cả cà phê thành một giá trị duy nhất, cấu trúc bên trong về cách đóng gói ban đầu sẽ không còn liên quan. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(NK)$|$O(1)$| Quá chậm trong trường hợp xấu nhất | 
| Số học tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các giá trị$N$,$K$, Và$L$. Chúng xác định số lượng thùng chứa nhỏ, sức chứa của chúng và sức chứa của thùng chứa lớn. 
2. Tính tổng lượng cà phê là$T = N \cdot K$. Bước này thu gọn toàn bộ cấu hình ban đầu thành một số lượng duy nhất, vì chỉ có tổng khối lượng mới quan trọng đối với việc đóng gói lại. 
3. Tính xem có bao nhiêu thùng chứa lớn đầy$T$, đó là$T // L$. 
4. Kiểm tra xem có số dư không$T \bmod L$. Nếu còn thừa cà phê thì cần thêm một thùng lớn để đựng. 
5. Đầu ra$T // L + (1 \text{ if } T \bmod L \neq 0 \text{ else } 0)$. 

### Tại sao nó hoạt động 

Quá trình đổ đầy các thùng chứa lớn chỉ phụ thuộc vào tổng khối lượng. Mỗi thùng chứa độc lập chứa tối đa$L$đơn vị, vì vậy mọi chiến lược đóng gói tối ưu sẽ lấp đầy hoàn toàn các thùng chứa bất cứ khi nào có thể. Sau khi trích xuất đủ nhóm kích thước$L$càng tốt, số tiền dương còn lại phải chiếm đúng một thùng chứa bổ sung. Điều này đảm bảo công thức tính toán số lượng vùng chứa tối thiểu cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

N, K, L = map(int, input().split())

total = N * K

ans = total // L
if total % L != 0:
    ans += 1

print(ans)
```Giải pháp đầu tiên nén dữ liệu đầu vào thành một giá trị tổng duy nhất. Điều này tránh mô phỏng bất kỳ quá trình đóng gói nào. Phép chia số nguyên cung cấp số lượng thùng lớn được lấp đầy hoàn toàn, trong khi mức tăng có điều kiện xử lý trường hợp còn sót lại, đảm bảo hành vi trần chính xác. 

Một lỗi thường gặp là quên viết chữ còn lại và chỉ in`total // L`. Đó là tính thiếu bất cứ khi nào tổng số không phải là bội số chính xác của$L$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3 6
```Đây$T = 2 \times 3 = 6$. 

| Bước | Tổng T | T // L | T% L | Trả lời | 
| --- | --- | --- | --- | --- | 
| Tính tổng | 6 | - | - | - | 
| Chia cho L | 6 | 1 | 0 | 1 | 

Tổng số chính xác lấp đầy một thùng lớn, do đó không có phần dư nào xuất hiện. Điều này xác nhận rằng khả năng chia hết chính xác không tạo ra thêm nhóm nào. 

### Ví dụ 2 

đầu vào:```
5 3 4
```Đây$T = 5 \times 3 = 15$. 

| Bước | Tổng T | T // L | T% L | Trả lời | 
| --- | --- | --- | --- | --- | 
| Tính tổng | 15 | - | - | - | 
| Chia cho L | 15 | 3 | 3 | 4 | 

Ba thùng đầy cỡ 4 chiếm 12 đơn vị, còn lại 3 đơn vị, cần có thùng thứ tư. Điều này cho thấy hiệu ứng trần xuất hiện như thế nào khi có phần dư. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ một số phép tính số học được thực hiện bất kể kích thước đầu vào | 
| Không gian |$O(1)$| Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Các ràng buộc đủ nhỏ để ngay cả một mô phỏng cũng có thể vượt qua, nhưng công thức trực tiếp đảm bảo hành vi theo thời gian không đổi và tránh việc lặp lại không cần thiết. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    N, K, L = map(int, input().split())
    total = N * K
    ans = total // L
    if total % L != 0:
        ans += 1
    return str(ans)

# provided samples
assert run("2 3 6\n") == "1", "sample 1"
assert run("5 3 4\n") == "4", "sample 2"
assert run("1000 500 1000\n") == "500", "sample 3"

# custom cases
assert run("1 1 1\n") == "1", "minimum case"
assert run("1 1000 1000\n") == "1", "exact fit large bucket"
assert run("3 2 10\n") == "1", "fits into single bucket"
assert run("10 1 3\n") == "4", "remainder case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | 1 | trường hợp biên nhỏ nhất | 
| 1 1000 1000 | 1 | phân chia chính xác ở công suất tối đa | 
| 3 2 10 | 1 | nhiều thùng nhỏ vừa với một thùng lớn | 
| 10 1 3 | 4 | hành vi trần với phần còn lại | 

## Vỏ cạnh 

Khi nào$N = 1, K = 1, L = 1$, tổng số chính xác là 1 và thuật toán tính toán$1 // 1 = 1$, không có phần dư. Câu trả lời đúng sẽ trở thành 1, cho thấy trường hợp cơ sở hoạt động chính xác. 

Khi tổng số nhỏ hơn$L$, Ví dụ$N=3, K=2, L=10$, tổng cộng là 6. Phép chia cho 0 nhóm đầy đủ và dư 6, kích hoạt nhóm bổ sung. Thuật toán đưa ra 1, phù hợp với yêu cầu rằng ngay cả số lượng nhỏ vẫn cần một thùng chứa. 

Khi tổng số chia hết chính xác, chẳng hạn như$N=1000, K=500, L=1000$, tổng số là 500000. Phép chia cho 500 nhóm đầy đủ và không có phần dư nào, do đó không có thêm nhóm nào được thêm vào. Điều này xác nhận rằng thuật toán không đếm quá mức trong các trường hợp chia sạch.
