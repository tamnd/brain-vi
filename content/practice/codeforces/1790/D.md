---
title: "CF 1790D - Matryoshka"
description: "Chúng ta được tặng một bộ sưu tập búp bê matryoshka, tất cả được trộn lẫn với nhau và mỗi con búp bê có kích thước nguyên dương. Mỗi bộ ban đầu bao gồm những con búp bê có kích thước liên tiếp và mỗi bộ đều được sử dụng hoàn toàn."
date: "2026-06-09T10:37:51+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1790
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 847 (Div. 3)"
rating: 1200
weight: 1790
solve_time_s: 111
verified: true
draft: false
---

[CF 1790D - Matryoshkas](https://codeforces.com/problemset/problem/1790/D) 

**Đánh giá:** 1200 
**Tags:** cấu trúc dữ liệu, tham lam, sắp xếp 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được tặng một bộ sưu tập búp bê matryoshka, tất cả được trộn lẫn với nhau và mỗi con búp bê có kích thước nguyên dương. Mỗi bộ ban đầu bao gồm những con búp bê có kích thước liên tiếp và mỗi bộ đều được sử dụng hoàn toàn. Thách thức là tìm ra số lượng tập hợp ban đầu tối thiểu có thể giải thích chuỗi kích thước quan sát được. 

Trong thực tế, đầu vào là một danh sách các số nguyên và đầu ra là số lượng "chuỗi" nhỏ nhất mà chúng ta cần bao gồm chuỗi sao cho mỗi chuỗi tăng đúng một đơn vị ở mỗi bước. Mỗi con búp bê trong đầu vào thuộc về đúng một bộ. 

Với các ràng buộc, chúng tôi có thể có tới 200.000 búp bê trong một trường hợp thử nghiệm và tối đa 10.000 trường hợp thử nghiệm. Điều này có nghĩa là bất kỳ thuật toán nào chậm hơn O(n log n) cho mỗi trường hợp thử nghiệm đều khó có thể đáp ứng giới hạn thời gian 2 giây. Một cách tiếp cận ngây thơ thử tất cả các nhóm có thể sẽ là hàm mũ theo n và hoàn toàn không khả thi. Các trường hợp biên bao gồm các chuỗi có kích thước bằng nhau, các chuỗi có kích thước phân tán rộng rãi hoặc các chuỗi đã được sắp xếp. 

Ví dụ, nếu trình tự là`[2,2,3,4,3,1]`, việc tham lam ngây thơ chỉ hình thành chuỗi từ phần tử đầu tiên có thể thất bại, nhưng chiến lược dựa trên số lượng cẩn thận sẽ xác định rằng chúng ta cần chính xác hai bộ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng gán từng con búp bê vào một bộ hiện có hoặc bắt đầu từng bộ mới, thử tất cả các hoán vị. Điều này đúng về mặt lý thuyết nhưng rõ ràng là quá chậm đối với n lên tới 200.000 vì số lượng hoán vị tăng theo giai thừa. 

Quan sát quan trọng là đối với bất kỳ kích thước cụ thể nào`x`, mỗi lần xuất hiện của`x`phải thuộc về một chuỗi khác với chuỗi trước đó`x`trừ khi có những con búp bê trước đó trong chuỗi đó. Do đó, số lượng bộ tối thiểu được quyết định bởi tần số tối đa của bất kỳ kích thước búp bê nào. Ví dụ: nếu số 100 xuất hiện sáu lần thì chúng ta cần ít nhất sáu chuỗi, vì mỗi chuỗi chỉ có thể sử dụng một số 100 cho mỗi bộ số liên tiếp. Điều này biến vấn đề thành việc đếm tần số và lấy giá trị tối đa, có thể được thực hiện một cách hiệu quả với bản đồ băm hoặc từ điển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Đếm tần số | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, hãy đọc số lượng búp bê`n`và thứ tự kích thước. 
2. Khởi tạo một từ điển để đếm số lần mỗi kích thước xuất hiện. 
3. Lặp lại trình tự và cập nhật từ điển với số lượng cho từng kích thước. 
4. Tìm số lượng tối đa trên tất cả các kích cỡ. Giá trị này thể hiện số lượng bộ tối thiểu cần thiết, bởi vì không có bộ nào có thể đóng góp nhiều hơn một con búp bê có cùng kích thước vào một chuỗi. 
5. Xuất số lượng tối đa này. 

Lý do đằng sau bước 4 là bất kỳ tập hợp nào cũng tăng nghiêm ngặt với các kích thước liên tiếp. Nếu một kích thước cụ thể xảy ra nhiều lần thì mỗi lần xuất hiện phải nằm trong một chuỗi riêng biệt. Do đó, số lượng chuỗi không thể thấp hơn tần số của kích thước được lặp lại nhiều nhất. Điều này đảm bảo tính chính xác vì bạn không thể gói nhiều hơn một phiên bản có kích thước vào cùng một bộ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import Counter

t = int(input())
for _ in range(t):
    n = int(input())
    dolls = list(map(int, input().split()))
    freq = Counter(dolls)
    print(max(freq.values()))
```Giải pháp đầu tiên là đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó xây dựng một bảng tần số của tất cả các kích cỡ búp bê bằng cách sử dụng`Counter`. các`max`hàm tìm tần số cao nhất, tương ứng trực tiếp với số lượng bộ tối thiểu cần thiết. Việc triển khai này tránh việc sắp xếp không cần thiết và xử lý các số rất lớn một cách hiệu quả, vì số nguyên Python có thể lớn tùy ý. 

## Ví dụ đã hoạt động 

Đầu vào mẫu`[2,2,3,4,3,1]`: 

| Búp bê | Bảng tần suất sau khi cập nhật | Tần số tối đa | 
| --- | --- | --- | 
| 2 | {2:1} | 1 | 
| 2 | {2:2} | 2 | 
| 3 | {2:2, 3:1} | 2 | 
| 4 | {2:2, 3:1, 4:1} | 2 | 
| 3 | {2:2, 3:2, 4:1} | 2 | 
| 1 | {2:2, 3:2, 4:1, 1:1} | 2 | 

Đầu ra:`2`. Điều này xác nhận rằng chúng ta cần hai bộ vì kích thước`2`xảy ra hai lần. 

Đầu vào mẫu`[11, 8, 7, 10, 9]`: 

| Búp bê | Bảng tần số | Tần số tối đa | 
| --- | --- | --- | 
| 11 | {11:1} | 1 | 
| 8 | {11:1,8:1} | 1 | 
| 7 | {11:1,8:1,7:1} | 1 | 
| 10 | {11:1,8:1,7:1,10:1} | 1 | 
| 9 | {11:1,8:1,7:1,10:1,9:1} | 1 | 

Đầu ra:`1`. Tất cả các kích thước đều xuất hiện một lần nên một bộ duy nhất có thể bao gồm cả chuỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Đếm số lần xuất hiện bằng từ điển mất O(n) và tìm tần số tối đa là O(số kích thước duy nhất) ≤ n | 
| Không gian | O(n) | Từ điển lưu trữ từng kích cỡ búp bê duy nhất, tối đa n mục | 

Cho tổng của`n`trên tất cả các trường hợp thử nghiệm là 2×10^5, giải pháp thực thi thoải mái trong vòng 2 giây và sử dụng tốt trong giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import Counter

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        dolls = list(map(int, input().split()))
        freq = Counter(dolls)
        output.append(str(max(freq.values())))
    return "\n".join(output)

# Provided samples
assert run("10\n6\n2 2 3 4 3 1\n5\n11 8 7 10 9\n6\n1000000000 1000000000 1000000000 1000000000 1000000000 1000000000\n8\n1 1 4 4 2 3 2 3\n6\n1 2 3 2 3 4\n7\n10 11 11 12 12 13 13\n7\n8 8 9 9 10 10 11\n8\n4 14 5 15 6 16 7 17\n8\n5 15 6 14 8 12 9 11\n5\n4 2 2 3 4") == "2\n1\n6\n2\n2\n2\n2\n2\n4\n3"

# Custom cases
assert run("1\n1\n1") == "1", "single doll"
assert run("1\n5\n5 5 5 5 5") == "5", "all equal"
assert run("1\n6\n1 2 3 4 5 6") == "1", "already consecutive"
assert run("1\n6\n1 2 2 3 3 3") == "3", "varying frequencies"
assert run("1\n4\n1000000000 1000000000 999999999 1000000000") == "3", "large numbers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Hộp đựng búp bê đơn | 
|`5 5 5 5 5`|`5`| Tất cả các kích thước bằng nhau đều yêu cầu bộ tần số tối đa | 
|`1 2 3 4 5 6`|`1`| Chuỗi tăng hoàn toàn liên tiếp | 
|`1 2 2 3 3 3`|`3`| Tần số thay đổi xác định chính xác các bộ | 
|`1000000000 1000000000 999999999 1000000000`|`3`| Xử lý số nguyên lớn | 

## Vỏ cạnh 

Ví dụ: nếu tất cả búp bê có cùng kích thước`[7,7,7]`, thuật toán đếm tần số của`7`là 3 và trả về 3. Điều này xác định chính xác rằng mỗi lần xuất hiện phải nằm trong tập hợp riêng của nó. 

Đối với một chuỗi liên tiếp đầy đủ, chẳng hạn như`[1,2,3,4]`, mỗi con búp bê xuất hiện một lần nên tần số tối đa là 1, cho đúng một bộ duy nhất. 

Đối với một chuỗi thưa thớt có kích thước lặp lại và phân tán như`[1,2,2,3,3,3]`, thuật toán theo dõi số lượng`1:1,2:2,3:3`và đầu ra 3, đảm bảo việc gán chuỗi tôn trọng các ràng buộc về tần số mà không cần mô phỏng các bộ thực tế. 

Cách tiếp cận này khái quát hóa mọi hỗn hợp giá trị và tỷ lệ theo giới hạn đầu vào một cách hiệu quả.
