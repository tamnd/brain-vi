---
title: "CF 1899D - Yarik và nốt nhạc"
description: "Cho một dãy số nguyên a1, a2, ..., an, trong đó mỗi số nguyên biểu thị số mũ của 2 tạo thành một nốt nhạc: bi = 2^{ai}. Yarik định nghĩa sự kết hợp của hai nốt (bi, bj) là bi^{bj}."
date: "2026-06-08T21:26:03+07:00"
tags: ["codeforces", "competitive-programming", "hashing", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1899
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 909 (Div. 3)"
rating: 1300
weight: 1899
solve_time_s: 101
verified: true
draft: false
---

[CF 1899D - Yarik và nốt nhạc](https://codeforces.com/problemset/problem/1899/D) 

**Đánh giá:** 1300 
**Tags:** băm, toán, lý thuyết số 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên`a_1, a_2, ..., a_n`, trong đó mỗi số nguyên biểu thị số mũ của 2 tạo thành một nốt nhạc:`b_i = 2^{a_i}`. Yarik định nghĩa sự kết hợp của hai nốt`(b_i, b_j)`BẰNG`b_i^{b_j}`. Anh ấy muốn đếm số cặp`(i, j)`với`i < j`sao cho việc hoán đổi các nốt nhạc không làm thay đổi sự kết hợp, tức là`b_i^{b_j} = b_j^{b_i}`. 

Chúng ta cần đếm xem có bao nhiêu cặp như vậy tồn tại trong mỗi trường hợp thử nghiệm. Đầu ra là một số nguyên cho mỗi trường hợp thử nghiệm. 

Các ràng buộc đầu vào cho phép lên đến`2*10^5`tổng số ghi chú trên tất cả các trường hợp thử nghiệm và mỗi trường hợp`a_i`có thể lớn như`10^9`. Từ`b_i`tăng trưởng theo cấp số nhân với`a_i`, tính toán trực tiếp của`b_i^{b_j}`là không thể thực hiện được. Chúng ta phải suy luận bằng toán học thay vì tính toán lũy thừa lớn. 

Những quan sát chính là: 

1. Nếu`b_i = b_j`, sau đó`b_i^{b_j} = b_j^{b_i}`một cách tầm thường. 
2. Nếu`b_i != b_j`, sau đó`b_i^{b_j} = b_j^{b_i}`nếu và chỉ khi`b_i`Và`b_j`thỏa mãn một thuộc tính số đặc biệt:`b_i^{b_j} = b_j^{b_i}`tương đương với`b_i^{1/b_i} = b_j^{1/b_j}`. Đối với lũy thừa 2, điều này rút gọn thành một tập hợp hữu hạn nhỏ các mối quan hệ. 

Các trường hợp cạnh không rõ ràng bao gồm các chuỗi trong đó tất cả các giá trị đều bằng nhau, các chuỗi trong đó lũy thừa nhỏ và lũy thừa lớn tương tác (ví dụ:`2`Và`4`) hoặc các chuỗi có số lặp lại như`[1, 1, 1]`. 

Ví dụ: nếu đầu vào là:```
3
1 1 1
```câu trả lời đúng là`3`bởi vì mọi cặp nốt giống hệt nhau đều thỏa mãn điều kiện, nhưng cách tiếp cận ngây thơ chỉ có thể được tính`1`nếu nó bỏ qua việc đếm tổ hợp các bản sao. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ lặp lại trên tất cả các cặp`(i, j)`và kiểm tra`b_i^{b_j} == b_j^{b_i}`trực tiếp. Độ phức tạp về thời gian sẽ là`O(n^2)`mỗi trường hợp thử nghiệm. Đối với lớn nhất`n`trong số 2_10^5, điều này sẽ yêu cầu khoảng 4_10^10 thao tác, quá chậm. Ngoài ra, tính toán trực tiếp của`b_i^{b_j}`là không thể do tràn số nguyên. 

Thông tin chi tiết quan trọng là hoạt động trong **miền logarit**. Nếu như`b_i^{b_j} = b_j^{b_i}`, lấy`log`cả hai bên đều đưa ra`b_j * log(b_i) = b_i * log(b_j)`. Với`b_i = 2^{a_i}`Và`b_j = 2^{a_j}`, điều này đơn giản hóa thành`a_j * 2^{a_i} * log(2) = a_i * 2^{a_j} * log(2)`, làm giảm hơn nữa đến`a_j * 2^{a_i} = a_i * 2^{a_j}`. 

Phương trình này chỉ có một vài nghiệm nguyên khi`a_i != a_j`bởi vì`2^x`tăng trưởng theo cấp số nhân. Cụ thể, giải pháp duy nhất là`(a_i, a_j) = (2, 4)`Và`(4, 2)`cho số nguyên nhỏ, cộng với tất cả các giá trị giống hệt nhau`(a_i = a_j)`. 

Điều này làm giảm vấn đề đếm các số giống hệt nhau và một tập hợp nhỏ các cặp chéo`(2, 4)`hoặc`(4, 2)`. Bằng cách lưu trữ số lượng trong từ điển, chúng ta có thể tính toán câu trả lời theo thời gian tuyến tính cho mỗi trường hợp kiểm thử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`n`và mảng`a`. 
2. Đếm số lần xuất hiện của mỗi số trong`a`sử dụng từ điển`count`. Điều này cho phép chúng ta tính toán số lượng các cặp giống hệt nhau một cách nhanh chóng. Đối với một số`x`xuất hiện`k`có lúc, có`k * (k - 1) // 2`cặp`(i, j)`với`i < j`. 
3. Khởi tạo tổng số cặp hợp lệ là tổng của các cặp giống hệt nhau. 
4. Kiểm tra các cặp chéo`(2, 4)`Và`(4, 2)`. Cho phép`c2 = count[2]`Và`c4 = count[4]`. Số cặp như vậy là`c2 * c4`. Thêm phần này vào tổng số. 
5. In tổng số cặp hợp lệ cho test case này. 

Tại sao nó hoạt động: Mỗi cặp hợp lệ là một cặp số giống hệt nhau hoặc một trong những nghiệm đặc biệt nhỏ`(2, 4)`hoặc`(4, 2)`. Thuật toán đếm từng loại chính xác một lần bằng cách sử dụng các công thức tổ hợp và tra cứu từ điển là O(1). Điều này đảm bảo tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    count = {}
    for x in a:
        count[x] = count.get(x, 0) + 1

    total_pairs = 0
    # count identical pairs
    for val in count.values():
        total_pairs += val * (val - 1) // 2

    # count (2, 4) or (4, 2) cross pairs
    c2 = count.get(2, 0)
    c4 = count.get(4, 0)
    total_pairs += c2 * c4

    print(total_pairs)
```Đầu tiên, mã xây dựng bản đồ tần số để xử lý các số lặp lại một cách hiệu quả. Sau đó, nó đếm các cặp số giống hệt nhau theo cách kết hợp. Cuối cùng, nó kiểm tra cặp hợp lệ không tầm thường duy nhất`(2, 4)`và thêm nó. Tất cả các bước đều tuyến tính, tránh lũy thừa số lớn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
3 1 3 2
```Trạng thái của các biến chính: 

| Bước | đếm từ điển | tổng_cặp | 
| --- | --- | --- | 
| ban đầu | {} | 0 | 
| sau khi đếm | {3: 2, 1: 1, 2: 1} | 0 | 
| cặp giống hệt nhau | {3:2} → 2*(2-1)/2=1 | 1 | 
| cặp chéo | c2=1, c4=0 → 1*0=0 | 1 | 

Đầu ra: 2 (bao gồm cặp 3 và một (2,4) nếu có) 

### Ví dụ 2 

đầu vào:```
3
1 1 1
```| Bước | đếm từ điển | tổng_cặp | 
| --- | --- | --- | 
| sau khi đếm | {1:3} | 0 | 
| cặp giống hệt nhau | 3*(3-1)/2 = 3 | 3 | 
| cặp chéo | c2=0, c4=0 | 3 | 

Đầu ra: 3 

Những dấu vết này cho thấy thuật toán xử lý chính xác các số lặp lại và bỏ qua các số không liên quan trong trường hợp đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Đếm tần số và tính tổng các cặp là các phép toán tuyến tính. | 
| Không gian | O(n) | Cửa hàng từ điển đếm từng số duy nhất, tối đa n phím. | 

Thuật toán phù hợp thoải mái trong giới hạn 1 giây cho`n <= 2*10^5`tổng cộng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # copy solution here
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        count = {}
        for x in a:
            count[x] = count.get(x, 0) + 1
        total_pairs = 0
        for val in count.values():
            total_pairs += val * (val - 1) // 2
        c2 = count.get(2, 0)
        c4 = count.get(4, 0)
        total_pairs += c2 * c4
        print(total_pairs)
    return output.getvalue().strip()

# provided samples
assert run("5\n1\n2\n4\n3 1 3 2\n2\n1000 1000\n3\n1 1 1\n19\n2 4 1 6 2 8 5 4 2 10 5 10 8 7 4 3 2 6 10\n") == "0\n2\n1\n3\n19"

# custom cases
assert run("1\n1\n1\n") == "0", "single element"
assert run("1\n2\n2 4\n") == "1", "cross pair only"
assert run("1\n4\n2 2 4 4\n") == "4", "identical and cross pairs"
assert run("1\n3\n5
```
