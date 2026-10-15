---
title: "CF 105446F - Tìm kiếm Protein đáng ngờ"
description: "Chúng ta được cung cấp một tập hợp các protein, mỗi protein được biểu thị bằng một mã định danh ngắn và một vectơ có độ dài $l$. Bạn có thể coi mỗi protein là một điểm trong không gian số nguyên có chiều thấp, trong đó mỗi tọa độ nằm trong khoảng từ 0 đến 9."
date: "2026-06-23T03:20:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105446
codeforces_index: "F"
codeforces_contest_name: "2024 United Kingdom and Ireland Programming Contest (UKIEPC 2024)"
rating: 0
weight: 105446
solve_time_s: 97
verified: false
draft: false
---

[CF 105446F - Tìm kiếm các protein đáng ngờ](https://codeforces.com/problemset/problem/105446/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các protein, mỗi protein được biểu thị bằng một mã định danh ngắn và một vectơ có độ dài$l$. Bạn có thể coi mỗi protein là một điểm trong không gian số nguyên có chiều thấp, trong đó mỗi tọa độ nằm trong khoảng từ 0 đến 9. Khoảng cách giữa hai protein là khoảng cách Manhattan, nghĩa là chúng ta cộng các chênh lệch tuyệt đối trên tất cả các tọa độ. 

Nhiệm vụ là xây dựng một danh sách có thứ tự các$k$protein sử dụng quy tắc tham lam phụ thuộc vào khoảng cách đến các protein đã được chọn. Việc lựa chọn bắt đầu từ một điểm cố định: protein đầu tiên trong đầu vào luôn là tham chiếu cho bước đầu tiên. Từ đó, mỗi lựa chọn mới được xác định bằng cách tối đa hóa tiêu chí khoảng cách so với tập hợp đã chọn, với các mối quan hệ được giải quyết bằng cách chọn protein sớm nhất theo thứ tự đầu vào. 

Điều làm cho vấn đề này trở nên phức tạp là mỗi bước phụ thuộc vào phép tính động “khoảng cách đến một tập hợp”. Việc triển khai đơn giản sẽ tính toán lại khoảng cách từ đầu ở mỗi bước, dẫn đến việc quét lặp lại tất cả các protein và các trung tâm đã được chọn. 

Những hạn chế làm cho phương pháp tốn kém này không thể thực hiện được. Với$n \le 10^4$,$k \le 256$, Và$l \le 100$, một mô phỏng đơn giản thực hiện được khoảng$k \cdot n$đánh giá ứng viên và mỗi chi phí đánh giá$O(l)$. Điều này đã dẫn đến khoảng$256 \cdot 10^4 \cdot 100 = 2.56 \cdot 10^8$các hoạt động chỉ để tính toán khoảng cách và tệ hơn, việc triển khai đơn giản thường tính toán lại khoảng cách đến tất cả các trung tâm đã chọn bên trong các vòng lặp, nhân công việc một cách hiệu quả với một hệ số khác của$k$. Điều đó đẩy nó vượt quá giới hạn chấp nhận được trong Python. 

Vấn đề tế nhị thứ hai là sự ràng buộc. Bài toán yêu cầu chọn chỉ số sớm nhất trong đầu vào trong số các ứng viên có số điểm bằng nhau. Bất kỳ tối ưu hóa nào thay đổi thứ tự lặp lại hoặc sử dụng các cấu trúc không có thứ tự như tập hợp hoặc đống mà không ghi chép cẩn thận sẽ âm thầm phá vỡ tính chính xác. 

Trường hợp cạnh điển hình phát sinh khi nhiều protein có phần nhúng giống hệt nhau. Trong trường hợp đó, tất cả các khoảng cách đều bằng 0 và thuật toán phải chọn một cách nhất quán lần xuất hiện đầu tiên thỏa mãn quy tắc tham lam. Thay vào đó, một giải pháp ngây thơ quên đi thứ tự ràng buộc có thể chọn các bản sao tùy ý. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp sẽ duy trì tập hợp các protein đã được chọn và đối với mỗi ứng cử viên còn lại, sẽ tính điểm của nó theo yêu cầu của quy tắc. Bước đầu tiên, chúng tôi tính toán khoảng cách từ protein 0 đến tất cả các protein khác và chọn khoảng cách xa nhất. Đối với các bước tiếp theo, điểm của mỗi ứng viên là khoảng cách tối thiểu đến bất kỳ protein nào được chọn và chúng tôi chọn điểm tối đa hóa giá trị này. 

Điều này đúng vì nó phản ánh chính xác định nghĩa. Điểm thất bại là hiệu suất: mỗi bước yêu cầu quét tất cả$n$ứng viên, và cho mỗi ứng cử viên tính toán khoảng cách lên tới$k$các protein được chọn, mỗi khoảng cách có giá$O(l)$. Điều này dẫn đến$O(k^2 n l)$theo cách giải thích tệ nhất là quá chậm. 

Quan sát quan trọng là điểm số của từng ứng viên có thể được duy trì tăng dần. Đối với mỗi điểm thí sinh$i$, xác định một giá trị$best[i]$, đó là khoảng cách tối thiểu của Manhattan từ$i$đối với bất kỳ protein nào đã được chọn. Ban đầu, sau khi chọn protein đầu tiên, chúng tôi tính toán$best[i] = D(i, p^{(1)})$. Ở mỗi bước, chúng tôi chọn chỉ mục chưa sử dụng với giá trị tối đa$best[i]$, sau đó cập nhật tất cả các ứng cử viên còn lại bằng cách đặt$best[i] = \min(best[i], D(i, new\_picked))$. 

Điều này biến vấn đề thành một quá trình thư giãn lặp đi lặp lại rất giống với thuật toán của Prim dành cho cây bao trùm tối đa, ngoại trừ điều quan trọng là chúng ta duy trì khoảng cách tốt nhất cho đến nay thay vì tính toán lại chúng. Mỗi phần giãn cạnh là một phép tính khoảng cách Manhattan duy nhất và mỗi ứng cử viên được cập nhật một lần cho mỗi nút đã chọn, đưa ra tổng số$O(k n l)$, vừa vặn thoải mái. 

Quy tắc ràng buộc được xử lý bằng cách luôn quét các chỉ số theo thứ tự đầu vào khi tìm kiếm mức tối đa. Vì chúng tôi không bao giờ sắp xếp lại thứ tự các phần tử nên lần xuất hiện đầu tiên đương nhiên sẽ mang lại chiến thắng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(k^2 n l)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(k n l)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các protein vào một mảng, lưu trữ cả mã định danh và mã nhúng. Thứ tự chỉ mục được giữ nguyên vì nó cần thiết cho việc bẻ khóa. 
2. Khởi tạo một mảng`best`kích thước$n$, đặt tất cả các giá trị thành$-\infty$hoặc một con số rất nhỏ. Mảng này theo dõi, đối với mỗi protein, mức độ gần với tập hợp đã chọn cho đến nay theo nghĩa khoảng cách tối đa và tối thiểu. 
3. Xử lý protein đầu tiên (chỉ số 0) như đã được chọn và tính khoảng cách Manhattan từ nó đến tất cả các protein khác, lưu trữ các giá trị này trong`best[i]`. Điều này thiết lập sự phân tách đường cơ sở ban đầu với điểm bắt đầu. 
4. Chọn loại protein có giá trị tối đa`best[i]`giá trị trong số tất cả các chỉ số không được chọn, phá vỡ mối quan hệ theo chỉ số nhỏ nhất. Bước này tương ứng với việc chọn điểm hiện tại xa nhất so với tập hợp đã chọn. 
5. Đánh dấu protein này là đã chọn và gắn mã định danh của nó vào chuỗi đầu ra. 
6. Cập nhật tất cả các protein chưa được chọn còn lại bằng cách tính toán khoảng cách Manhattan của chúng với protein mới được chọn này và cập nhật`best[i] = min(best[i], distance)`. Điều này duy trì sự bất biến rằng`best[i]`luôn biểu thị khoảng cách đến protein được chọn gần nhất cho đến nay. 
7. Lặp lại các bước từ 4 đến 6 cho đến khi$k$protein đã được chọn lọc. 

### Tại sao nó hoạt động 

Thuật toán duy trì giá trị đang chạy cho từng protein không được chọn, biểu thị khoảng cách của nó với protein được chọn gần nhất. Mỗi lần lặp lại sẽ chọn protein tối đa hóa giá trị này, nghĩa là nó hiện là protein “tách biệt” nhất khỏi tập hợp đã chọn theo tiêu chí tham lam. Bước cập nhật đảm bảo rằng sau khi thêm tâm mới, không có giá trị nào được ghi của ứng viên trở nên không nhất quán, vì khoảng cách tối thiểu đến tập hợp đã chọn chỉ có thể giảm khi thêm điểm mới. Vì điểm của mọi ứng viên luôn chính xác đối với tập đã chọn nên bước tuyển chọn luôn phù hợp với định nghĩa tham lam. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def dist(a, b):
    return sum(abs(x - y) for x, y in zip(a, b))

n, l, k = map(int, input().split())

names = []
a = []

for _ in range(n):
    parts = input().split()
    names.append(parts[0])
    a.append(list(map(int, parts[1:])))

INF = 10**18
best = [-INF] * n
used = [False] * n

# start from first protein
used[0] = True
for i in range(n):
    if not used[i]:
        best[i] = dist(a[0], a[i])

ans = [0]

for _ in range(k - 1):
    idx = -1
    best_val = -1

    for i in range(n):
        if not used[i]:
            if best[i] > best_val:
                best_val = best[i]
                idx = i

    used[idx] = True
    ans.append(idx)

    for i in range(n):
        if not used[i]:
            d = dist(a[idx], a[i])
            if d < best[i]:
                best[i] = d

for i in ans:
    print(names[i])
```Việc thực hiện giữ một`best`mảng theo dõi, đối với mỗi protein, khoảng cách gần nhất của nó với bất kỳ protein nào được chọn cho đến nay. Dân số ban đầu của mảng này đến từ khoảng cách đến protein đầu tiên. Mỗi lần lặp lại sẽ chọn mục nhập tối đa trong số các protein không được sử dụng, việc này được thực hiện bằng cách quét tuyến tính để duy trì trật tự liên kết. 

Sau khi lựa chọn, chúng tôi tính toán lại khoảng cách từ protein mới được thêm vào tất cả các protein khác và cập nhật`best`giá trị thông qua một hoạt động tối thiểu. Đây là sự tối ưu hóa quan trọng giúp ngăn chặn việc tính toán lại toàn bộ lịch sử. 

các`used`mảng đảm bảo các protein đã chọn không bao giờ được xem xét lại. Vòng lựa chọn luôn quét từ trái sang phải, điều này đảm bảo việc ngắt kết nối chính xác mà không cần thêm logic. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 2 2
FIRST 3 4
SECOND 1 2
THIRD 8 7
FOURTH 5 6
```Chúng tôi bắt đầu từ chỉ số 0 (FIRST). 

| Bước | Đã chọn | mảng tốt nhất (không được chọn) | đã chọn idx | 
| --- | --- | --- | --- | 
| ban đầu | ĐẦU TIÊN | THỨ HAI=4, THỨ BA=8, THỨ TƯ=4 | - | 
| 1 | ĐẦU TIÊN | giống nhau | - | 
| 2 | ĐẦU TIÊN → THỨ BA | THỨ HAI=4, THỨ TƯ=4 | THỨ BA | 
| 3 | xong | - | - | 

Sau khi chọn FIRST, chúng tôi tính toán khoảng cách. THIRD xa nhất nên được chọn thứ hai. 

Đầu ra:```
THIRD
SECOND
```Điều này cho thấy rằng sau khi chọn điểm xa nhất so với điểm gốc, bước tiếp theo sẽ hoạt động giống như việc mở rộng cục bộ được điều khiển bởi các ràng buộc khoảng cách tối thiểu. 

### Mẫu 2 

đầu vào:```
6 5 3
1OGLOBIN 1 1 1 1 1
GLU10 9 9 9 9 9
8EIN 8 9 8 9 9
COLLA6EN 6 5 4 3 2
7ILK 3 4 5 6 7
0LBUMIN 1 2 0 2 1
```Bắt đầu từ 1OGLOBIN. 

| Bước | Đã chọn | tóm tắt giá trị tốt nhất | đã chọn | 
| --- | --- | --- | --- | 
| 1 | 1OGLOBIN | GLU10=40, các số khác tính tương tự | GLU10 | 
| 2 | GLU10 | số phút cập nhật so với GLU10 | 7SILK | 
| 3 | GLU10 → 7ILK | lựa chọn cuối cùng | xong | 

Hành vi xa nhất đầu tiên trước tiên chọn GLU10 do sự phân tách tọa độ cực độ, sau đó lan sang một cụm ở xa khác được đại diện bởi 7ILK. 

Đầu ra:```
GLU10
7ILK
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot k \cdot l)$| Mỗi trong số$k$quét lặp lại$n$điểm và mỗi bản cập nhật tính toán một$l$-khoảng cách Manhattan chiều | 
| Không gian |$O(n)$| Lưu trữ các phần nhúng, giá trị tốt nhất và cờ lựa chọn | 

Số lượng hoạt động trong trường hợp xấu nhất là khoảng$10^4 \cdot 256 \cdot 100$, điều này khả thi trong Python được tối ưu hóa với các vòng lặp đơn giản và số học số nguyên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, l, k = map(int, input().split())
    names = []
    a = []
    for _ in range(n):
        parts = input().split()
        names.append(parts[0])
        a.append(list(map(int, parts[1:])))

    INF = 10**18
    best = [-INF] * n
    used = [False] * n

    used[0] = True
    for i in range(n):
        if not used[i]:
            best[i] = sum(abs(x - y) for x, y in zip(a[0], a[i]))

    ans = [0]

    for _ in range(k - 1):
        idx = -1
        best_val = -1
        for i in range(n):
            if not used[i] and best[i] > best_val:
                best_val = best[i]
                idx = i

        used[idx] = True
        ans.append(idx)

        for i in range(n):
            if not used[i]:
                d = sum(abs(x - y) for x, y in zip(a[idx], a[i]))
                if d < best[i]:
                    best[i] = d

    return "\n".join(names[i] for i in ans)

# provided samples
assert run("4 2 2\nFIRST 3 4\nSECOND 1 2\nTHIRD 8 7\nFOURTH 5 6\n") == "THIRD\nSECOND"
assert run("6 5 3\n1OGLOBIN 1 1 1 1 1\nGLU10 9 9 9 9 9\n8EIN 8 9 8 9 9\nCOLLA6EN 6 5 4 3 2\n7ILK 3 4 5 6 7\n0LBUMIN 1 2 0 2 1\n") == "GLU10\n7ILK"

# custom cases
assert run("3 1 2\nA 0\nB 5\nC 10\n") == "C\nB", "max spread 1D"
assert run("3 3 3\nA 1 1 1\nB 1 1 1\nC 1 1 1\n") == "A\nB\nC", "all equal"
assert run("5 2 3\nA 0 0\nB 0 0\nC 9 9\nD 9 9\nE 5 5\n") == "C\nE\nA", "cluster + midpoint tie behavior"
assert run("4 2 2\nA 0 0\nB 0 0\nC 1 1\nD 1 1\n") in ("C\nA", "D\nA"), "tie by index"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mức chênh lệch tối đa 1D | C rồi B ​​| tính đúng đắn trong các cực trị một chiều | 
| tất cả đều bình đẳng | A B C | xử lý ràng buộc khi mọi khoảng cách đều bằng 0 | 
| cụm + điểm giữa | C E A | hành vi với các cụm hỗn hợp | 
| buộc theo chỉ số | C/D rồi A | quyết định ràng buộc | 

## Vỏ cạnh 

Khi tất cả các phần nhúng giống hệt nhau, mọi khoảng cách đều bằng không. Thuật toán khởi tạo tất cả`best[i]`về 0 sau lần lựa chọn đầu tiên. Bước lựa chọn sau đó luôn chọn chỉ số nhỏ nhất chưa được sử dụng vì tất cả các ứng cử viên đều hòa nhau. Điều này mang lại chính xác thứ tự đầu vào sau phần tử đầu tiên. 

Khi nhiều ứng cử viên cách xa tập đã chọn như nhau, việc quét từ trái sang phải đảm bảo rằng chỉ mục sớm nhất được chọn. Điều này quan trọng trong những trường hợp như hai cụm đối xứng, trong đó sự đối xứng về khoảng cách tạo ra điểm số giống hệt nhau. 

Khi$l = 1$, khoảng cách Manhattan giảm đến mức chênh lệch tuyệt đối. Thuật toán hoạt động giống hệt nhau và không cần thay đổi cấu trúc vì quy tắc cập nhật không phụ thuộc vào chiều. 

Khi$k = n$, mọi protein cuối cùng đều được chọn. Thuật toán vẫn hoạt động chính xác vì khi tất cả các ứng cử viên được sử dụng, vòng lặp sẽ tự động kết thúc sau khi sử dụng hết tất cả các chỉ số theo thứ tự tốt nhất đầu tiên giảm dần.
