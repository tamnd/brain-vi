---
title: "CF 106414N - Nguyên tố"
description: "Chúng ta cần xây dựng một mảng số nguyên với độ dài nhất định. Yêu cầu duy nhất là mỗi cặp vị trí khác nhau đều có tổng của một số nguyên tố. Bản thân các giá trị chỉ cần nằm trong phạm vi cho phép và mọi cấu trúc hợp lệ đều được chấp nhận."
date: "2026-06-25T09:49:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106414
codeforces_index: "N"
codeforces_contest_name: "Rutgers University Programming Contest Spring 2026 - Open Division"
rating: 0
weight: 106414
solve_time_s: 28
verified: true
draft: false
---

[CF 106414N - Primemas](https://codeforces.com/problemset/problem/106414/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 28s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một mảng số nguyên với độ dài nhất định. Yêu cầu duy nhất là mỗi cặp vị trí khác nhau đều có tổng của một số nguyên tố. Bản thân các giá trị chỉ cần nằm trong phạm vi cho phép và mọi cấu trúc hợp lệ đều được chấp nhận. 

Khó khăn chính là điều kiện áp dụng cho mọi cặp chứ không chỉ các phần tử lân cận. Với một mảng lớn, việc kiểm tra tất cả các cặp sẽ bao gồm quá nhiều thao tác, do đó, giải pháp phải tạo mảng theo cách có đặc tính rõ ràng mà không cần kiểm tra từng cặp. 

Chiều dài có thể đạt tới$2 \cdot 10^5$và tổng chiều dài trên các trường hợp thử nghiệm có cùng giới hạn. Điều này loại trừ bất kỳ cách tiếp cận nào so sánh các cặp, bởi vì điều đó sẽ đòi hỏi$O(n^2)$công việc. Ngay cả một trường hợp thử nghiệm đơn lẻ với 200000 phần tử cũng sẽ tạo ra khoảng 20 tỷ cặp, không thể đáp ứng được giới hạn thời gian. Chúng ta cần một công trình chỉ mất thời gian tuyến tính ở kích thước đầu ra. 

Những trường hợp phức tạp đều xuất phát từ tính chẵn lẻ. Việc xây dựng bất cẩn có thể vô tình tạo ra một cặp có tổng chẵn lớn hơn 2, không bao giờ có thể là số nguyên tố. Ví dụ: một mảng có đầu vào`3`không thể được xây dựng như`2 4 6`, bởi vì cặp`2 + 4 = 6`không phải là nguyên tố. Đầu ra đúng có thể là:```
3
```sản xuất:```
1 1 2
```Tổng số tiền là`1 + 1 = 2`,`1 + 2 = 3`, Và`1 + 2 = 3`, tất cả đều nguyên tố. 

Một trường hợp cạnh khác là chiều dài cho phép nhỏ nhất. Đối với đầu vào:```
2
```mảng chỉ có một cặp để thỏa mãn. Một công trình như:```
1 2
```hoạt động vì tổng duy nhất là`3`. Một giải pháp giả định phải có một số giá trị bằng nhau vẫn cần xử lý trường hợp này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng tạo ra một số số và xác minh tất cả các tổng cặp. Việc xác minh rất dễ dàng: với mỗi cặp chỉ số, hãy tính tổng và kiểm tra xem nó có phải là số nguyên tố hay không. Vấn đề là số lượng cặp. Vì$n$có những phần tử$\frac{n(n-1)}{2}$cặp, trở thành khoảng$2 \cdot 10^{10}$khi$n=200000$. Ngay cả trước khi xem xét kiểm tra cơ bản, điều này đã quá chậm. 

Quan sát hữu ích đến từ việc xem xét tính chẵn lẻ. Mọi số nguyên tố ngoại trừ 2 đều là số lẻ. Nếu hai số có cùng số chẵn lẻ thì tổng của chúng là số chẵn. Điều đó có nghĩa là hai số chẵn không thể xuất hiện cùng nhau, vì tổng của chúng sẽ là số chẵn lớn hơn 2. Hai số lẻ chỉ có tác dụng khi tổng của chúng đúng bằng 2, nghĩa là cả hai số đều phải bằng 1. 

Điều này để lại một cấu trúc rất đơn giản. Chúng ta có thể đặt càng nhiều`1`như chúng ta muốn, bởi vì mỗi cặp`1`s cho số nguyên tố 2. Chúng ta có thể cộng chính xác một số chẵn, vì nó chỉ cần tạo thành tổng nguyên tố với các số đó. Lựa chọn`2`hoạt động vì`1 + 2 = 3`, đó là số nguyên tố. 

Mảng cuối cùng luôn là một chuỗi các`n - 1`những cái theo sau là một`2`. Việc xây dựng không cần bất kỳ tìm kiếm, kiểm tra tính nguyên thủy hoặc xử lý đặc biệt nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) ngoài đầu ra | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc độ dài mảng yêu cầu`n`. 
2. Tạo`n - 1`bản sao của giá trị`1`. Bất kỳ hai phần tử nào trong số này có tổng bằng`2`, đó là số nguyên tố. 
3. Nối thêm một giá trị cuối cùng`2`. Mỗi tổng liên quan đến giá trị này và một`1`trở thành`3`, đó cũng là số nguyên tố. 
4. Xuất mảng đã xây dựng. 

Lý do công trình này hoạt động là vì nó tránh được tất cả các cặp nguy hiểm. Giá trị lặp lại duy nhất là`1`, và giá trị chẵn duy nhất là`2`, nên không có cặp nào có thể tạo ra một hợp số chẵn. 

Tại sao nó hoạt động: điều bất biến là mọi cặp trong mảng được tạo đều là hai`1`s hoặc một`1`và một`2`. Trường hợp đầu tiên luôn cho tổng`2`, và trường hợp thứ hai luôn cho tổng`3`. Vì cả hai giá trị đều là số nguyên tố nên mọi cặp có thể đều thỏa mãn điều kiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        ans.append(" ".join(["1"] * (n - 1) + ["2"]))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Mã trực tiếp theo sau việc xây dựng từ thuật toán. Đối với mỗi trường hợp thử nghiệm, nó tạo ra chính xác`n - 1`những cái đó và thêm một cái cuối cùng`2`. 

Logic nhân hoặc tính nguyên tố mà một giải pháp bạo lực cần có là hoàn toàn tránh được. Mảng hợp lệ theo cách xây dựng nên chương trình chỉ dành thời gian để tạo đầu ra theo yêu cầu. 

Kích thước danh sách được kiểm soát bởi`n`và số nguyên Python dễ dàng đủ lớn vì giá trị in lớn nhất chỉ`2`. Không có quyết định lập chỉ mục hoặc tính toán ranh giới nào có thể gây ra một lỗi nào ngoài việc đảm bảo số lượng lỗi là chính xác.`n - 1`. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
2
2
3
```Đối với trường hợp thử nghiệm đầu tiên: 

| Bước | n | Những cái đã tạo | Mảng hiện tại | 
| --- | --- | --- | --- | 
| Bắt đầu | 2 | 0 | [] | 
| Thêm cái | 2 | 1 | [1] | 
| Thêm giá trị cuối cùng | 2 | 1 | [1, 2] | 

Cặp duy nhất có tổng`1 + 2 = 3`, do đó việc xây dựng là hợp lệ. 

Đối với trường hợp thử nghiệm thứ hai: 

| Bước | n | Những cái đã tạo | Mảng hiện tại | 
| --- | --- | --- | --- | 
| Bắt đầu | 3 | 0 | [] | 
| Thêm cái | 3 | 2 | [1, 1] | 
| Thêm giá trị cuối cùng | 3 | 2 | [1, 1, 2] | 

Tổng của cặp là: 

| Cặp | Tổng hợp | 
| --- | --- | 
| Đầu tiên 1 và thứ hai 1 | 2 | 
| Đầu tiên 1 và 2 | 3 | 
| Thứ hai 1 và 2 | 3 | 

Tất cả các cặp có thể đều tạo ra số nguyên tố. Dấu vết này cho thấy tại sao các bản sao lại an toàn khi giá trị trùng lặp được`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được in được tạo một lần | 
| Không gian | O(n) | Mảng đầu ra cho mỗi trường hợp thử nghiệm chứa n giá trị | 

Tổng kích thước của tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$, do đó việc xây dựng tuyến tính dễ dàng phù hợp với các giới hạn. Giải pháp không thực hiện kiểm tra cặp và không tính toán số nguyên tố. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp: str) -> str:
    data = inp.strip().split()
    t = int(data[0])
    idx = 1
    out = []

    for _ in range(t):
        n = int(data[idx])
        idx += 1
        out.append(" ".join(["1"] * (n - 1) + ["2"]))

    return "\n".join(out)

# provided style samples
assert solve("2\n2\n3\n") == "1 2\n1 1 2"

# minimum size
assert solve("1\n2\n") == "1 2"

# all generated values must handle larger output sizes
assert len(solve("1\n10\n").split()) == 10

# repeated ones case
assert solve("1\n5\n") == "1 1 1 1 2"

# boundary style case
assert solve("1\n3\n") == "1 1 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2`|`1 2`| Độ dài hợp lệ tối thiểu | 
|`3`|`1 1 2`| Trường hợp đầu tiên có giá trị lặp lại | 
|`5`|`1 1 1 1 2`| Vị trí đúng của số chẵn duy nhất | 
|`10`| Mười số | Xây dựng tuyến tính cho kích thước lớn hơn | 

## Vỏ cạnh 

cho`n = 2`, thuật toán tạo ra một`1`và một`2`:```
Input:
1
2
```Đầu ra là:```
1 2
```Chỉ có một cặp và tổng của nó là`3`. Việc xây dựng không dựa vào việc có nhiều bản sao của`1`, do đó đầu vào nhỏ nhất được xử lý một cách tự nhiên. 

Đối với một số lượng lớn các phần tử, chẳng hạn như:```
1
6
```đầu ra là:```
1 1 1 1 1 2
```Mỗi cặp trong số năm yếu tố đầu tiên đều cho`2`và mỗi cặp chứa phần tử cuối cùng sẽ cho`3`. Thuật toán không bao giờ tạo ra hai giá trị chẵn, điều này tránh được lỗi chẵn lẻ phổ biến. 

Đối với các mảng có nhiều giá trị bằng nhau, chẳng hạn như:```
1
5
```kết quả là:```
1 1 1 1 2
```Việc triển khai ngây thơ có thể cho rằng sự trùng lặp là nguy hiểm, nhưng giá trị trùng lặp`1`chính xác là điều khiến công trình xây dựng thành công vì hai bản sao cộng lại bằng số nguyên tố nhỏ nhất.
