---
title: "CF 105380A - Ai Ghét Abhishek?"
description: "Chúng ta được yêu cầu xây dựng một loại hoán vị đặc biệt có kích thước n. Hoán vị ở đây có nghĩa là chúng ta sắp xếp các số từ 1 đến n đúng một lần. Điều khó khăn là hoán vị phải hoạt động giống như một sự tiến triển không có điểm cố định."
date: "2026-06-23T16:05:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105380
codeforces_index: "A"
codeforces_contest_name: "TSEC Round 1 (Div. 4)"
rating: 0
weight: 105380
solve_time_s: 64
verified: true
draft: false
---

[CF 105380A - Ai ghét Abhishek?](https://codeforces.com/problemset/problem/105380/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một loại hoán vị đặc biệt có kích thước`n`. Hoán vị ở đây có nghĩa là chúng ta sắp xếp các số từ`1`ĐẾN`n`mỗi cái đúng một lần. 

Điều khó khăn là hoán vị phải hoạt động giống như một sự tiến triển không có điểm cố định. Nói cách khác, nếu chúng ta nhìn vào giá trị tại vị trí`j`, gọi nó`a[j]`, sau đó áp dụng lại ánh xạ tương tự sẽ đưa chúng ta trở lại:`a[a[j]] = j`. Đồng thời, không có vị trí nào được phép ánh xạ tới chính nó, vì vậy`a[j] != j`cho mọi`j`. 

Cấu trúc này buộc mọi chỉ mục phải được ghép nối với chính xác một chỉ mục khác. Nếu như`a[x] = y`, sau đó tự động`a[y] = x`, và cũng không`x`cũng không`y`có thể bằng nhau. Vì vậy, hoán vị hoàn toàn được tạo thành từ 2 chu kỳ rời rạc. 

Kích thước đầu vào`n`có thể lớn như`100000`, vì vậy mọi giải pháp đều phải chạy theo thời gian tuyến tính. Một bậc hai hoặc thậm chí$O(n \log n)$việc xây dựng là tốt, nhưng bất cứ điều gì liên quan đến việc tìm kiếm hoặc so khớp lặp đi lặp lại sẽ không cần thiết và có nhiều rủi ro. 

Trường hợp cạnh khóa xuất hiện khi`n`thật kỳ quặc. Nếu mọi phần tử phải thuộc về một cặp thì số phần tử lẻ sẽ khiến một chỉ mục không được ghép đôi. Điều đó làm cho điều kiện không thể được thỏa mãn. 

Ví dụ, khi`n = 3`, chúng ta sẽ cần những cặp như`(1, 2)`Và`(3, ?)`, nhưng không còn đối tác nào cho`3`. Bất kỳ nỗ lực nào sẽ buộc một điểm cố định hoặc phá vỡ yêu cầu song song. 

Vì vậy, khó khăn thực sự không phải là xây dựng hoán vị khi có thể mà là nhận biết khi nào cấu trúc là không thể. 

## Phương pháp tiếp cận 

Một nỗ lực ngây thơ sẽ là thử xây dựng hoán vị bằng cách theo dõi những vị trí nào đã được sử dụng. Chúng ta có thể lặp qua các chỉ số từ`1`ĐẾN`n`và với mỗi chỉ mục không được sử dụng`i`, tìm kiếm chỉ mục khác chưa được sử dụng`j`và thiết lập`a[i] = j`,`a[j] = i`. Điều này đảm bảo tính chính xác vì chúng tôi thực thi các điều kiện một cách rõ ràng. 

Vấn đề là hiệu suất. Đối với mỗi chỉ mục, chúng tôi có thể quét qua các chỉ mục chưa sử dụng còn lại để tìm đối tác. Trong trường hợp xấu nhất, điều này trở thành xấp xỉ$n + (n-1) + (n-2) + \cdots$, đó là$O(n^2)$. Với`n = 100000`, điều này vượt xa giới hạn khả thi. 

Nhận xét quan trọng là điều kiện`a[a[j]] = j`Và`a[j] != j`buộc hoán vị phải khớp hoàn hảo trên các chỉ số. Chúng tôi không cần phải tìm kiếm đối tác một cách linh hoạt. Chúng ta chỉ cần đảm bảo mọi phần tử được ghép nối với chính xác một phần tử khác. 

Khi chúng tôi chấp nhận cách giải thích đó, việc xây dựng sẽ trở nên ngay lập tức: nếu chúng tôi liệt kê các số theo thứ tự và hoán đổi các cặp liền kề, chúng tôi sẽ tự động tạo ra 2 chu kỳ hợp lệ. Mỗi cặp`(1,2)`,`(3,4)`,`(5,6)`, v.v. thỏa mãn cả hai điều kiện. Ánh xạ là đối xứng và không có phần tử nào ánh xạ tới chính nó. 

Nếu như`n`là số lẻ, một phần tử không được ghép cặp, do đó không có cấu trúc hợp lệ nào tồn tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kết hợp lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Hoán đổi liền kề | O(n) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Kiểm tra xem`n`thật kỳ quặc. Nếu có thì xuất ngay`-1`. Điều này là do việc ghép nối yêu cầu các nhóm có kích thước chính xác là 2 và số lẻ khiến việc ghép nối đầy đủ là không thể. 
2. Khởi tạo một mảng`a`kích thước`n + 1`(sử dụng chỉ mục dựa trên 1 để thuận tiện). 
3. Lặp lại các chỉ số`i`từ`1`ĐẾN`n`trong các bước của`2`. 
4. Cho mỗi cặp`(i, i+1)`, giao phó`a[i] = i+1`Và`a[i+1] = i`. Điều này tạo ra sự ánh xạ lẫn nhau giữa các phần tử liền kề. 
5. Sau khi xử lý tất cả các chỉ số, xuất mảng từ`1`ĐẾN`n`. 

Lý do chúng tôi thực hiện từng bước 2 là vì mỗi thao tác sử dụng chính xác hai chỉ mục chưa sử dụng và chúng tôi đảm bảo không có sự trùng lặp giữa các cặp. 

### Tại sao nó hoạt động 

Hoán vị được xây dựng là sự kết hợp rời rạc của 2 chu kỳ. Mỗi chỉ số`i`được chỉ định chính xác một đối tác`i+1`hoặc`i-1`, do đó ánh xạ có tính chất phỏng đoán. Áp dụng hoán vị hai lần sẽ trả về chỉ mục ban đầu vì hoán đổi hai lần sẽ khôi phục vị trí ban đầu:`a[a[i]] = i`. Vì không có phần tử nào được ánh xạ tới chính nó nên điều kiện điểm cố định cũng được thỏa mãn. Không có sự tương tác giữa các cặp khác nhau, do đó thuộc tính được giữ trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    
    if n % 2 == 1:
        print(-1)
        return
    
    a = list(range(n + 1))
    
    for i in range(1, n + 1, 2):
        a[i], a[i + 1] = i + 1, i
    
    print(*a[1:])

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên xử lý tình trạng không thể xảy ra bằng cách kiểm tra tính chẵn lẻ. Đây là trường hợp duy nhất mà chúng ta có thể thất bại. 

Mảng được khởi tạo với chỉ số giả bằng 0 để việc lập chỉ mục dựa trên 1 phù hợp với định nghĩa vấn đề. Điều này tránh việc điều chỉnh chỉ số lặp đi lặp lại và giảm rủi ro từng cái một. 

Vòng lặp hoán đổi các chỉ số liền kề trong thời gian không đổi trên mỗi cặp. Mỗi lần lặp lại sẽ hoàn thiện vĩnh viễn hai vị trí, do đó không cần ghi sổ kế toán bổ sung. 

Cuối cùng là in`a[1:]`loại bỏ khe giả và xuất ra hoán vị. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`n = 6`Chúng ta bắt đầu với một mảng`[1, 2, 3, 4, 5, 6]`. 

| tôi | Hoạt động | Trạng thái mảng | 
| --- | --- | --- | 
| 1 | hoán đổi (1,2) | [2, 1, 3, 4, 5, 6] | 
| 3 | hoán đổi (3,4) | [2, 1, 4, 3, 5, 6] | 
| 5 | hoán đổi (5,6) | [2, 1, 4, 3, 6, 5] | 

Hoán vị cuối cùng thỏa mãn`a[a[i]] = i`bởi vì mỗi cặp đều đối xứng và không có phần tử nào cố định. 

### Ví dụ 2:`n = 4`Bắt đầu`[1, 2, 3, 4]`. 

| tôi | Hoạt động | Trạng thái mảng | 
| --- | --- | --- | 
| 1 | hoán đổi (1,2) | [2, 1, 3, 4] | 
| 3 | hoán đổi (3,4) | [2, 1, 4, 3] | 

Mỗi phần tử được ghép nối chính xác một lần, xác nhận cấu trúc hoạt động đồng đều`n`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mỗi chỉ mục được truy cập một lần và hoán đổi một lần | 
| Không gian | O(1) thêm | ngoài mảng đầu ra, chỉ có một vài biến được sử dụng | 

Cấu trúc tuyến tính nên có thể xử lý thoải mái`n = 100000`. Việc sử dụng bộ nhớ cũng ở mức tối thiểu vì chúng tôi chỉ lưu trữ chính hoán vị đó. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    # embedded solution
    def solve():
        n = int(sys.stdin.readline().strip())
        if n % 2 == 1:
            print(-1)
            return
        a = list(range(n + 1))
        for i in range(1, n + 1, 2):
            a[i], a[i + 1] = i + 1, i
        print(*a[1:])

    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue().strip()
    sys.stdout = old_stdout
    return out

# provided samples
assert run("6\n") == "2 1 4 3 6 5", "sample 1"
assert run("4\n") == "2 1 4 3", "sample 2"

# custom cases
assert run("1\n") == "-1", "minimum odd case"
assert run("2\n") == "2 1", "minimum even case"
assert run("3\n") == "-1", "small odd impossibility"
assert run("10\n") == "2 1 4 3 6 5 8 7 10 9", "larger even case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | -1 | trường hợp bất khả thi nhỏ nhất | 
| 2 | 2 1 | trao đổi hợp lệ nhỏ nhất | 
| 3 | -1 | thất bại có độ dài lẻ | 
| 10 | hoán đổi theo cặp | chính xác trên đầu vào chẵn lớn hơn | 

## Vỏ cạnh 

Khi nào`n = 1`, không có đối tác khả thi nào cho phần tử đơn lẻ. Thuật toán ngay lập tức trở lại`-1`, phù hợp với yêu cầu không cho phép có điểm cố định. 

Khi`n = 2`, vòng lặp chạy một lần và hoán đổi vị trí`1`Và`2`, sản xuất`[2, 1]`. Kiểm tra thủ công,`a[a[1]] = a[2] = 1`và tương tự cho chỉ mục`2`, xác nhận tính đúng đắn. 

Khi`n`có số lẻ nào như thế không`5`, thuật toán phát hiện tính chẵn lẻ trước khi bắt đầu xây dựng bất kỳ. Vì`n = 5`, trở về`-1`tránh cố gắng ghép nối một phần như`(1,2)`,`(3,4)`sẽ để lại chỉ mục`5`không ghép nối và buộc một điểm cố định hoặc ánh xạ không hợp lệ.
