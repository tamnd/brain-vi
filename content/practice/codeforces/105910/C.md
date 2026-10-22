---
title: "CF 105910C - \u6811\u54c8\u5e0c"
description: "Bài toán đưa ra số đỉnh phải xuất hiện ở mọi mức độ sâu của cây có gốc. Số đầu tiên mô tả chiều cao của cây và dãy sau mô tả có bao nhiêu đỉnh có chính xác 0 cạnh, 1 cạnh, 2 cạnh, v.v. tính từ gốc."
date: "2026-06-25T14:02:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105910
codeforces_index: "C"
codeforces_contest_name: "The 23rd Sichuan University Programming Contest"
rating: 0
weight: 105910
solve_time_s: 38
verified: true
draft: false
---

[CF 105910C - \u6811\u54c8\u5e0c](https://codeforces.com/problemset/problem/105910/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán đưa ra số đỉnh phải xuất hiện ở mọi mức độ sâu của cây có gốc. Số đầu tiên mô tả chiều cao của cây và dãy sau mô tả có bao nhiêu đỉnh có chính xác 0 cạnh, 1 cạnh, 2 cạnh, v.v. tính từ gốc. Nhiệm vụ là quyết định xem thông tin này có tạo ra hình dạng cây có gốc duy nhất hay không, hay liệu hai cây không đẳng hình khác nhau có thể đáp ứng cùng số lượng độ sâu hay không. Nếu cây không phải là duy nhất, chúng ta phải xây dựng hai mảng cha hợp lệ đại diện cho hai cây khác nhau. Đây là ý tưởng kinh điển đằng sau vấn đề “Cây băm” của Codeforces. 

Chiều cao có thể lớn, lên tới khoảng 100000 và tổng số đỉnh bị giới hạn. Điều đó có nghĩa là một giải pháp phải gần tuyến tính về số đỉnh. Việc xây dựng tất cả các cây có thể có hoặc so sánh nhiều cách sắp xếp có thể sẽ tăng quá nhanh vì số lượng các lựa chọn khác nhau của cha mẹ có thể trở nên rất lớn. Chúng ta cần tìm một đặc tính cấu trúc cho chúng ta biết chính xác khi nào sự mơ hồ xuất hiện. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. Nếu mỗi cấp độ sau gốc có đúng một đỉnh thì cây buộc phải là một đường đi duy nhất. Ví dụ, đầu vào```
2
1 1 1
```chỉ có một cây có thể, vì vậy kết quả đầu ra là```
perfect
```Một cách tiếp cận bất cẩn chỉ kiểm tra xem một số cấp độ có nhiều hơn một nút có thể gọi sai điều này là mơ hồ hay không. 

Một trường hợp quan trọng khác là khi hai cấp độ lân cận đều chứa nhiều đỉnh. Ví dụ,```
2
1 2 2
```là mơ hồ. Cấp độ đầu tiên có hai nút và cấp độ tiếp theo cũng có hai nút. Chúng ta có thể gắn cả hai nút sâu hơn dưới cùng một nút cha hoặc tách chúng ra giữa hai nút cha. Chúng cho hình dạng cây có rễ khác nhau. 

Đầu ra đúng bắt đầu bằng```
ambiguous
```theo sau là hai cây hợp lệ khác nhau. Một giải pháp chỉ tìm kiếm các giá trị lặp lại trong chuỗi mà không xem xét các mức liền kề sẽ bỏ qua tình huống này. 

## Phương pháp tiếp cận 

Một cách trực tiếp để giải quyết vấn đề là thử xây dựng mọi cây có thể phù hợp với số cấp độ. Đối với mỗi đỉnh ở một cấp độ, chúng ta sẽ chọn số lượng đỉnh con mà nó nhận được từ cấp độ tiếp theo. Điều này đúng vì quyền tự do duy nhất trong việc xây dựng một cây như vậy là sự phân bố con cái giữa cha và mẹ ở các cấp độ liên tiếp. Tuy nhiên, ngay cả một cấp độ duy nhất có nhiều đỉnh cũng tạo ra một số lượng phân phối khổng lồ. Với hàng trăm nghìn đỉnh, số lượng khả năng vượt xa những gì có thể khám phá. 

Quan sát quan trọng là lần duy nhất chúng ta có lựa chọn thay đổi cấu trúc cây là khi cả hai cấp độ liên tiếp đều có phân nhánh. Giả sử mức độ`i`chứa ít nhất hai đỉnh và mức`i + 1`cũng có ít nhất hai đỉnh. Những đứa trẻ ở trình độ`i + 1`có thể được tập trung dưới một cha mẹ hoặc phân phối giữa nhiều cha mẹ. Hai lựa chọn đó tạo ra các hình dạng cây con khác nhau. 

Nếu không có cặp cấp độ liền kề nào có thuộc tính này thì mỗi cấp độ phân nhánh sẽ được theo sau bởi một chuỗi duy nhất. Khi một cấp độ có nhiều đỉnh, cấp độ tiếp theo không thể phân chia thêm nữa, do đó không có cách nào sắp xếp lại các cấp độ con và tạo ra một cây không đẳng cấu khác. Cây được xác định duy nhất. 

Khi có sự mơ hồ, trước tiên chúng ta có thể xây dựng bất kỳ cây hợp lệ nào, sau đó thực hiện một sửa đổi cục bộ ở cặp cấp độ phân nhánh liên tiếp đầu tiên. Việc di chuyển một con từ cha mẹ đầu tiên sang cha mẹ thứ hai sẽ thay đổi hình dạng trong khi vẫn giữ nguyên tất cả số lượng độ sâu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chiều cao và số đỉnh ở mọi mức độ sâu. Theo dõi chỉ số bắt đầu của mọi cấp độ trong việc đánh số đỉnh cuối cùng. Gốc là đỉnh 1 và các đỉnh được gán theo thứ tự cấp độ. 
2. Xây dựng cây đầu tiên bằng cách gắn mọi đỉnh của cấp độ hiện tại vào đỉnh cuối cùng của cấp độ trước đó. Điều này luôn tạo ra một cây hợp lệ vì mọi đỉnh đều nhận được chính xác độ sâu cần thiết. 
3. Quét qua từng cặp cấp độ lân cận. Nếu một cấp độ và cấp độ tiếp theo đều chứa ít nhất hai đỉnh thì cây là mơ hồ. Lý do là trẻ em ở cấp độ tiếp theo có thể được sắp xếp theo nhiều cách. 
4. Nếu không tồn tại cặp như vậy, hãy in`perfect`bởi vì mọi cây hợp lệ đều phải có cấu trúc giống nhau. 
5. Nếu có sự mơ hồ ở các cấp độ`i`Và`i + 1`, hãy sử dụng cây được xây dựng đầu tiên làm câu trả lời đầu tiên. Đối với câu trả lời thứ hai, hãy thay đổi đỉnh gốc của một đỉnh theo cấp độ`i + 1`từ đỉnh đầu tiên của cấp độ`i`đến đỉnh thứ hai của cấp độ`i`. Điều này chỉ thay đổi kiểu kết nối chứ không thay đổi số đỉnh ở bất kỳ độ sâu nào. 

Tại sao nó hoạt động: 

Nút cha của nút chỉ có thể là đỉnh ở cấp độ trước đó. Nếu hai cấp độ liên tiếp đều có nhiều đỉnh, thì có ít nhất hai cách có thể để phân phối con và những cách đó có thể tạo ra số lượng con khác nhau cho các đỉnh ở cùng độ sâu. Sự khác biệt đó không thể được loại bỏ bằng cách đổi tên các đỉnh, do đó các cây không đẳng cấu. 

Nếu mọi cặp liền kề đều chứa một mức chỉ có một đỉnh thì mọi quyết định phân nhánh đều bắt buộc. Một cấp độ có một đỉnh chỉ có thể có một cấp độ gốc và cấp độ phân nhánh không thể được theo sau bởi một cấp độ phân nhánh khác có thể được sắp xếp lại. Toàn bộ hình dạng cây được cố định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    h = int(input())
    a = list(map(int, input().split()))

    start = [0] * (h + 1)
    cur = 1
    for i in range(h + 1):
        start[i] = cur
        cur += a[i]

    n = cur - 1

    parent1 = [0] * (n + 1)
    parent2 = [0] * (n + 1)

    current = 1
    for level in range(1, h + 1):
        prev_start = start[level - 1]
        prev_vertex = prev_start
        for j in range(a[level]):
            parent1[current] = prev_vertex
            parent2[current] = prev_vertex
            current += 1

        if level > 0 and a[level - 1] > 1 and a[level] > 1:
            pass

    for i in range(h):
        if a[i] > 1 and a[i + 1] > 1:
            child = start[i + 1]
            parent2[child] = start[i] + 1

            print("ambiguous")
            print(*parent1[1:])
            print(*parent2[1:])
            return

    print("perfect")

if __name__ == "__main__":
    solve()
```Mảng`start`lưu trữ nơi mỗi mức độ sâu bắt đầu bằng việc đánh số. Điều này giúp có thể xác định được đỉnh thứ nhất và đỉnh thứ hai của một mức trong thời gian không đổi. 

Cây đầu tiên được tạo ra một cách tham lam. Mỗi đỉnh ở cấp độ mới được gắn vào đỉnh đầu tiên của cấp độ trước đó. Điều này tạo ra một cây hợp lệ vì chỉ có độ sâu mới là vấn đề quan trọng trong quá trình xây dựng. 

Việc quét tìm sự mơ hồ tách biệt với việc xây dựng vì điều kiện chỉ phụ thuộc vào chuỗi đầu vào. Khi đã tìm thấy một cặp cấp độ phân nhánh liền kề, việc thay đổi cấp độ gốc của đỉnh đầu tiên ở cấp độ sâu hơn là đủ để tạo ra cây thứ hai. 

Chi tiết lập chỉ mục là nơi dễ mắc lỗi nhất. Gốc là đỉnh 1, không phải 0, vì vậy`start[i]`luôn đề cập đến một số đỉnh thực. Đầu ra yêu cầu cha mẹ cho các đỉnh theo thứ tự từ 1 đến`n`, đó là lý do tại sao các lát cắt cuối cùng bỏ qua chỉ số 0 không được sử dụng. 

## Ví dụ đã hoạt động 

Đối với đầu vào```
2
1 1 1
```quá trình thi công là: 

| Cấp độ | Đếm | Bắt đầu chỉ mục | Kiểm tra sự mơ hồ | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | không | 
| 1 | 1 | 2 | không | 
| 2 | 1 | 3 | không | 

Không có cặp mức liền kề nào chứa hai đỉnh. Cây bị ép buộc nên đáp án là:```
perfect
```Dấu vết này chứng tỏ trường hợp một chuỗi không có khả năng sắp xếp lại. 

Đối với đầu vào```
2
1 2 2
```các cấp độ là: 

| Cấp độ | Đếm | Bắt đầu chỉ mục | Kiểm tra sự mơ hồ | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | không | 
| 1 | 2 | 2 | vâng với cấp độ tiếp theo | 
| 2 | 2 | 4 | | 

Cây đầu tiên có thể được xây dựng như sau:```
0 1 1 2 2
```Cây thứ hai thay đổi đỉnh 4 để có cha 3:```
0 1 1 3 2
```Số lượng độ sâu giữ nguyên nhưng kích thước cây con khác nhau, do đó các cây không đẳng cấu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi đỉnh được xử lý một số lần không đổi trong khi xây dựng và kiểm tra cây. | 
| Không gian | O(n) | Mảng cha và thông tin cấp lưu trữ một giá trị trên mỗi đỉnh. | 

Thuật toán chỉ thực hiện công tuyến tính theo số đỉnh. Điều này phù hợp với các ràng buộc dự định vì tổng số đỉnh là yếu tố chi phối. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    
    h = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))

    start = [0] * (h + 1)
    cur = 1
    for i in range(h + 1):
        start[i] = cur
        cur += a[i]

    n = cur - 1
    p1 = [0] * (n + 1)
    p2 = [0] * (n + 1)

    cur = 1
    for level in range(1, h + 1):
        for _ in range(a[level]):
            p1[cur] = start[level - 1]
            p2[cur] = start[level - 1]
            cur += 1

    out = []
    for i in range(h):
        if a[i] > 1 and a[i + 1] > 1:
            p2[start[i + 1]] = start[i] + 1
            out.append("ambiguous")
            out.append(" ".join(map(str, p1[1:])))
            out.append(" ".join(map(str, p2[1:])))
            sys.stdin = old
            return "\n".join(out) + "\n"

    sys.stdin = old
    return "perfect\n"

assert run("2\n1 1 1\n") == "perfect\n", "single path"
assert run("2\n1 2 2\n").splitlines()[0] == "ambiguous", "branching case"
assert run("3\n1 2 1 1\n") == "perfect\n", "branch followed by chain"
assert run("3\n1 2 2 1\n").splitlines()[0] == "ambiguous", "two branching levels"
assert run("1\n1 5\n").splitlines()[0] == "perfect", "star tree"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 1 1 1`|`perfect`| Một chuỗi độc đáo | 
|`2 / 1 2 2`|`ambiguous`| Các cấp độ phân nhánh liền kề | 
|`3 / 1 2 1 1`|`perfect`| Cấp độ phân nhánh không thể sắp xếp lại | 
|`3 / 1 2 2 1`|`ambiguous`| Sự mơ hồ phân nhánh sâu hơn | 
|`1 / 1 5`|`perfect`| Root có nhiều con | 

## Vỏ cạnh 

Đối với trường hợp dây chuyền```
2
1 1 1
```thuật toán xây dựng cha mẹ`0 1 2`. Trong quá trình quét, số lượng không bao giờ lớn hơn một ở các cấp độ lân cận, do đó không thể sửa đổi. Nó in`perfect`, phù hợp với hình dạng duy nhất có thể. 

Đối với trường hợp khó hiểu```
2
1 2 2
```thuật toán thấy rằng cả cấp 1 và 2 đều có nhiều hơn một đỉnh. Cây thứ nhất gắn đỉnh 4 và 5 dưới đỉnh 2. Cây thứ 2 di chuyển đỉnh 4 xuống dưới đỉnh 3. Số cấp vẫn giữ nguyên`1, 2, 2`, nhưng một cây có một nút có hai con trong khi cây kia trải rộng chúng ra. 

Đối với trường hợp ranh giới như```
3
1 2 1 1
```nhánh cấp 1, nhưng cấp 2 chỉ chứa một đỉnh. Đỉnh đó phải có chính xác một đỉnh cha nên không có cấu trúc thay thế. Thuật toán bỏ qua cấp độ phân nhánh đơn một cách chính xác và in`perfect`. 

Vì```
3
1 2 2 1
```cấp 1 và 2 đều là nhánh. Đỉnh sâu hơn đầu tiên có thể được gắn vào một trong hai nút ở cấp 1. Thuật toán phát hiện cặp liền kề này và tạo ra hai cây hợp lệ khác nhau.
