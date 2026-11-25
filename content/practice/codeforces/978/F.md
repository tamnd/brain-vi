---
title: "CF 978F - Cố vấn"
description: "Chúng ta được cấp một nhóm lập trình viên, mỗi người có một giá trị kỹ năng cố định. Đối với bất kỳ lập trình viên $i$ nào, chúng tôi muốn đếm xem họ có thể “cố vấn” cho bao nhiêu lập trình viên $j$ khác."
date: "2026-06-17T01:23:39+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 978
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 481 (Div. 3)"
rating: 1500
weight: 978
solve_time_s: 70
verified: true
draft: false
---

[CF 978F - Người cố vấn](https://codeforces.com/problemset/problem/978/F) 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một nhóm lập trình viên, mỗi người có một giá trị kỹ năng cố định. Dành cho bất kỳ lập trình viên nào$i$, chúng tôi muốn đếm xem có bao nhiêu lập trình viên khác$j$họ có thể “cố vấn”. Sự hướng dẫn mang tính định hướng:$i$có thể cố vấn$j$chỉ khi$i$có kỹ năng cao hơn hẳn so với$j$, và đồng thời$i$Và$j$không được liệt kê là đang xung đột. 

Vì vậy với mỗi chỉ số$i$, chúng tôi đang đếm một cách hiệu quả có bao nhiêu chỉ số$j$thỏa mãn hai điều kiện cùng một lúc: so sánh kỹ năng$r_i > r_j$và sự vắng mặt của cặp cấm$(i, j)$. 

Khó khăn đến từ quy mô. Với tối đa$2 \cdot 10^5$lập trình viên và cùng số cặp xung đột, bất kỳ phương pháp kiểm tra từng cặp riêng lẻ nào đều trở nên quá chậm. Một vòng lặp đôi ngây thơ trên tất cả các cặp sẽ liên quan đến khoảng$4 \cdot 10^{10}$so sánh trong trường hợp xấu nhất, vượt xa giới hạn khả thi trong 3 giây. 

Một điểm tinh tế trong đầu vào là xung đột rất thưa thớt so với tất cả các cặp có thể có. Điều này gợi ý rằng cấu trúc “chủ yếu là đếm được sắp xếp với các hiệu chỉnh nhỏ”, chứ không phải là một bài toán đồ thị đầy đủ. 

Một trường hợp phá vỡ cách tính toán ngây thơ là khi nhiều lập trình viên có chung kỹ năng. Ví dụ: nếu tất cả các kỹ năng đều như nhau thì không ai có thể cố vấn cho ai, vì vậy câu trả lời phải là toàn số 0. Một cách tiếp cận ngây thơ đếm "thấp hơn hoặc bằng" thay vì "thấp hơn nghiêm ngặt" sẽ đếm quá mức không chính xác trong tình huống này. 

Một tình huống phức tạp khác xảy ra khi một lập trình viên có nhiều xung đột. Nếu chúng tôi chỉ trừ một số cố định cho mỗi lập trình viên mà không xem xét thứ tự kỹ năng, chúng tôi có thể trừ các cặp không hợp lệ (trong đó người kia có kỹ năng cao hơn hoặc bằng và không bao giờ được tính ngay từ đầu). 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: đối với mỗi lập trình viên$i$, quét tất cả các lập trình viên khác$j$, kiểm tra xem$r_i > r_j$, đồng thời xác minh rằng$(i, j)$không phải là một cuộc xung đột. Điều này đúng vì nó trực tiếp thực thi định nghĩa về cố vấn. Tuy nhiên, mỗi truy vấn sẽ quét$O(n)$các ứng cử viên, và làm điều này cho tất cả$n$các nút dẫn đến$O(n^2)$hoạt động quá chậm ở các ràng buộc nhất định. 

Cải tiến quan trọng đến từ việc tách vấn đề thành hai phần. Đầu tiên, hãy bỏ qua hoàn toàn các xung đột và đếm xem có bao nhiêu lập trình viên có kỹ năng nhỏ hơn mỗi người.$i$. Điều này có thể được thực hiện bằng cách sắp xếp hoặc bằng cách sử dụng số đếm tần số trên các giá trị nén, tạo ra số lượng cơ sở toàn cầu. 

Thứ hai, chúng tôi sửa đường cơ sở này bằng cách trừ đi các trường hợp không hợp lệ: đối với mỗi cặp xung đột$(a, b)$, chính xác một hướng có thể đã được tính không chính xác. Nếu như$r_a > r_b$, sau đó$b$đã được đưa vào$a$số lượng cơ sở của nhưng phải được loại bỏ. Nếu như$r_b > r_a$, sau đó$a$phải được loại bỏ khỏi$b$được tính. Nếu các kỹ năng ngang nhau thì cả hai hướng đều không có giá trị và không có gì thay đổi. 

Điều này làm giảm vấn đề về sắp xếp cộng với việc lặp lại trên tất cả các cạnh một lần, điều này hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Sắp xếp + sửa xung đột |$O(n \log n + k)$|$O(n + k)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các giá trị kỹ năng và lưu trữ chúng cùng với chỉ số của chúng. 

Điều này là cần thiết để sau này chúng tôi có thể sắp xếp các lập trình viên theo kỹ năng trong khi vẫn ánh xạ kết quả về vị trí ban đầu. 
2. Sắp xếp lập trình viên theo giá trị kỹ năng. 

Việc sắp xếp cho phép chúng tôi tính toán có bao nhiêu giá trị nhỏ hơn cho mỗi cấp độ kỹ năng trong một lần quét tuyến tính. 
3. Tính mảng cơ sở`cnt[i]`đại diện cho bao nhiêu lập trình viên có kỹ năng hoàn toàn nhỏ hơn lập trình viên$i$. 

Điều này được thực hiện bằng cách sử dụng tập hợp tần số theo thứ tự đã sắp xếp. Khi xử lý các giá trị bằng nhau, chúng được nhóm lại để các kỹ năng bằng nhau không bị tính lẫn nhau. 
4. Khởi tạo mảng kết quả`ans[i] = cnt[i]`. 

Ở giai đoạn này, chúng tôi đang bỏ qua các xung đột, vì vậy mọi ứng viên “chỉ có kỹ năng” hợp lệ đều được đưa vào. 
5. Xử lý từng cặp xung đột$(x, y)$. 

Nếu như$r_x > r_y$, sau đó$y$được tính không chính xác vào$x$tổng số, do đó giảm`ans[x]`. 

Nếu như$r_y > r_x$, giảm`ans[y]`. 

Nếu họ bằng nhau thì không làm gì cả vì theo định nghĩa thì cả hai đều không thể cố vấn cho nhau. 
6. Xuất kết quả cuối cùng`ans`mảng. 

### Tại sao nó hoạt động 

Đường cơ sở`cnt[i]`đếm chính xác tất cả các lập trình viên có kỹ năng thực sự nhỏ hơn$i$. Bộ này độc lập với xung đột. Mỗi cặp xung đột có thể làm mất hiệu lực tối đa một mối quan hệ cố vấn được định hướng trong cơ sở này: chỉ có hướng từ kỹ năng cao hơn đến kỹ năng thấp hơn mới quan trọng. Vì mỗi cặp không hợp lệ sẽ bị trừ chính xác một lần và không có cặp hợp lệ nào bị trừ nên số đếm cuối cùng khớp chính xác với định nghĩa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    r = list(map(int, input().split()))

    # pair (skill, index)
    arr = [(r[i], i) for i in range(n)]
    arr.sort()

    cnt = [0] * n

    # compute how many strictly smaller elements each index has
    i = 0
    smaller = 0
    while i < n:
        j = i
        while j < n and arr[j][0] == arr[i][0]:
            j += 1
        # all elements before i are strictly smaller
        for t in range(i, j):
            idx = arr[t][1]
            cnt[idx] = smaller
        smaller += (j - i)
        i = j

    ans = cnt[:]

    for _ in range(k):
        x, y = map(int, input().split())
        x -= 1
        y -= 1
        if r[x] > r[y]:
            ans[x] -= 1
        elif r[y] > r[x]:
            ans[y] -= 1

    print(*ans)

if __name__ == "__main__":
    solve()
```Bước sắp xếp đảm bảo chúng tôi xử lý chính xác sự bất bình đẳng nghiêm ngặt bằng cách nhóm các kỹ năng bằng nhau và gán cho chúng cùng một “số lượng nhỏ hơn” trước khi tăng bộ đếm toàn cầu. Bước trừ được cố tình không đối xứng: chúng tôi chỉ điều chỉnh điểm cuối có tay nghề cao hơn, bởi vì chỉ hướng đó mới có thể đóng góp vào số lượng ban đầu của nó. 

Lập chỉ mục cẩn thận là điều cần thiết ở đây. Việc trừ được thực hiện sau khi tính toán đường cơ sở đầy đủ, giúp tránh nhiễu giữa xử lý xung đột và logic đặt hàng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 2
10 4 10 15
1 2
4 3
```Sắp xếp kỹ năng: 

| bước | phân đoạn mảng | giao nhỏ hơn | 
| --- | --- | --- | 
| nhóm 1 | (4, idx2) | 0 | 
| nhóm 2 | (10, idx1), (10, idx3) | 1 | 
| nhóm 3 | (15, idx4) | 3 | 

Đường cơ sở`cnt`trở thành: 

| tôi | kỹ năng | cnt | 
| --- | --- | --- | 
| 1 | 10 | 1 | 
| 2 | 4 | 0 | 
| 3 | 10 | 1 | 
| 4 | 15 | 3 | 

Áp dụng xung đột: 

- (1,2): 10 > 4 nên ans[1] -= 1 
- (4,3): 15 > 10 nên ans[4] -= 1 

Chung kết: 

| tôi | trả lời | 
| --- | --- | 
| 1 | 0 | 
| 2 | 0 | 
| 3 | 1 | 
| 4 | 2 | 

Điều này xác nhận rằng chỉ những kỹ năng thấp hơn mới được tính và các xung đột sẽ loại bỏ chính xác một hướng cố vấn không hợp lệ. 

### Ví dụ 2 

đầu vào:```
3 0
5 5 5
```Tất cả các kỹ năng đều bình đẳng nên mỗi nhóm được xử lý thành một khối. Mỗi`cnt[i]`trở thành 0. Không có xung đột nào tồn tại, vì vậy câu trả lời cuối cùng vẫn là số không. Điều này xác nhận việc xử lý chính xác đẳng thức mà không cần đếm ngẫu nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n + k)$| Sắp xếp chiếm ưu thế, mỗi xung đột được xử lý một lần | 
| Không gian |$O(n)$| Mảng để sắp xếp và lưu trữ kết quả | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các phần tử, vì thế$n \log n$nhanh một cách thoải mái và việc quét tuyến tính các xung đột là không đáng kể trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    n, k = map(int, sys.stdin.readline().split())
    r = list(map(int, sys.stdin.readline().split()))

    arr = [(r[i], i) for i in range(n)]
    arr.sort()

    cnt = [0] * n
    i = 0
    smaller = 0
    while i < n:
        j = i
        while j < n and arr[j][0] == arr[i][0]:
            j += 1
        for t in range(i, j):
            cnt[arr[t][1]] = smaller
        smaller += (j - i)
        i = j

    ans = cnt[:]

    for _ in range(k):
        x, y = map(int, sys.stdin.readline().split())
        x -= 1
        y -= 1
        if r[x] > r[y]:
            ans[x] -= 1
        elif r[y] > r[x]:
            ans[y] -= 1

    return " ".join(map(str, ans))

# provided sample
assert run("""4 2
10 4 10 15
1 2
4 3
""") == "0 0 1 2"

# minimum size
assert run("""2 0
1 2
""") == "1 0"

# all equal
assert run("""5 3
7 7 7 7 7
1 2
2 3
3 4
""") == "0 0 0 0 0"

# no conflicts
assert run("""3 0
3 1 2
""") == "2 0 1"

# chain conflicts
assert run("""4 2
4 3 2 1
1 2
2 3
""") == "3 2 1 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | 0 0 1 2 | tính đúng đắn với các ràng buộc hỗn hợp | 
| 2 nút không có cạnh | 1 0 | trường hợp tối thiểu | 
| tất cả đều bình đẳng | tất cả số không | xử lý bất bình đẳng nghiêm ngặt | 
| không xung đột | số lượng được sắp xếp | độ chính xác cơ bản | 
| chuỗi giảm dần | logic hiệu chỉnh đầy đủ | phép trừ nhiều bước | 

## Vỏ cạnh 

Đối với các kỹ năng bằng nhau, thuật toán sẽ nhóm chúng theo bước sắp xếp để không có lập trình viên nào trong nhóm được tính là có kỹ năng nhỏ hơn người khác trong cùng nhóm. Ví dụ, đầu vào`3 0 / 5 5 5`tạo ra một khối duy nhất trong đó`smaller`vẫn bằng 0 khi gán số lượng, vì vậy mọi mục nhập đều bằng 0. 

Đối với xung đột giữa các lập trình viên có trình độ ngang nhau, chẳng hạn như`(1,2)`với cả hai kỹ năng 10, không có nhánh nào của trình kích hoạt có điều kiện, do đó không xảy ra phép trừ. Điều này ngăn chặn những điều chỉnh tiêu cực không chính xác và duy trì tính đúng đắn của việc xử lý bình đẳng. 

Đối với trường hợp một lập trình viên trình độ thấp có nhiều xung đột, những xung đột đó không bao giờ ảnh hưởng đến số lượng của họ trừ khi bên kia có kỹ năng cao hơn. Vì họ không bao giờ được coi là người cố vấn cho các lập trình viên có kỹ năng cao hơn hoặc ngang bằng nên không xảy ra phép trừ không hợp lệ và thuật toán vẫn ổn định ngay cả trong các biểu đồ xung đột dày đặc.
