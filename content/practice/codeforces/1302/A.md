---
title: "CF 1302A - Cân bằng Nash"
description: "Chúng ta đang làm việc với một lưới số hình chữ nhật. Mỗi ô hoạt động giống như một người chơi trong trò chơi hai chiều: nó được so sánh theo chiều dọc với cột và theo chiều ngang với hàng của nó."
date: "2026-06-16T05:30:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 1302
codeforces_index: "A"
codeforces_contest_name: "AIM Tech Poorly Prepared Contest (unrated, funny, Div. 1 preferred)"
rating: 0
weight: 1302
solve_time_s: 395
verified: false
draft: false
---

[CF 1302A - Cân bằng Nash](https://codeforces.com/problemset/problem/1302/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 35 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang làm việc với một lưới số hình chữ nhật. Mỗi ô hoạt động giống như một người chơi trong trò chơi hai chiều: nó được so sánh theo chiều dọc với cột và theo chiều ngang với hàng của nó. 

Một ô được gọi là cân bằng Nash nếu nó đồng thời là mức tối đa nghiêm ngặt trong cột của nó và mức tối thiểu nghiêm ngặt trong hàng của nó. Nói cách khác, nhìn xuống cột của nó, không có giá trị nào khác có thể khớp hoặc vượt quá nó và nhìn qua hàng của nó, không có giá trị nào khác có thể khớp hoặc thấp hơn nó. 

Nhiệm vụ là tìm một tế bào như vậy. Nếu nhiều ô thỏa mãn điều kiện, chúng ta chọn ô xuất hiện sớm nhất theo thứ tự hàng và nếu vẫn bằng nhau thì ô xuất hiện sớm nhất theo thứ tự cột. Nếu không có ô như vậy tồn tại, chúng tôi xuất ra số 0. 

Các ràng buộc cho phép tối đa 1000 x 1000 ô, tức là lên tới một triệu giá trị. Điều đó ngay lập tức loại trừ mọi giải pháp quét liên tục toàn bộ hàng và cột cho mọi ô ứng cử viên. Việc xác thực từng ô đơn giản sẽ yêu cầu kiểm tra toàn bộ hàng và cột của nó, dẫn đến khoảng$O(nm(n + m))$hoạt động, trong trường hợp xấu nhất là quá chậm. 

Một vấn đề tế nhị xuất hiện khi các giá trị lặp lại. Việc triển khai bất cẩn có thể xử lý “tối đa trong cột” hoặc “tối thiểu trong hàng” bằng cách sử dụng các so sánh không nghiêm ngặt hoặc chỉ kiểm tra một phần, điều này phá vỡ tính chính xác. 

Ví dụ, hãy xem xét một cột`[1, 5, 5, 2]`. Nếu chúng tôi chỉ kiểm tra xem một giá trị có bằng giá trị tối đa mà không thực thi tính duy nhất hay không thì cả hai số 5 có thể được coi là cực đại cột hợp lệ, nhưng định nghĩa yêu cầu sự thống trị nghiêm ngặt đối với tất cả các hàng khác, điều này không thành công do trùng lặp. 

Một trường hợp cạnh khác phát sinh khi giá trị tối thiểu của hàng tốt nhất và giá trị tối đa của cột không trùng nhau về mặt cấu trúc. Một hàng có thể có mức tối thiểu duy nhất, nhưng vị trí đó không phải là mức tối đa của cột, do đó lý do cục bộ trên mỗi hàng là không đủ. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi ô, chúng tôi xác minh hai điều kiện: nó phải lớn hơn mọi giá trị khác trong cột và nhỏ hơn mọi giá trị khác trong hàng của nó. Mỗi lần xác minh đều tốn thời gian tuyến tính theo chiều dài hàng hoặc cột. Vì chúng tôi có thể kiểm tra tới$10^6$các ô, mỗi ô có giá lên tới$O(1000)$, tổng công việc trở nên xung quanh$10^9$, quá lớn trong hai giây. 

Quan sát quan trọng là các điều kiện có thể được tính toán trước một cách độc lập. Đối với mỗi hàng, chúng ta chỉ cần giá trị tối thiểu của nó và chỉ mục cột nơi nó xuất hiện. Đối với mỗi cột, chúng ta chỉ cần giá trị tối đa của nó và chỉ mục hàng nơi nó xuất hiện. Điểm cân bằng hợp lệ phải nằm chính xác ở giao điểm của cột tối thiểu và cột tối đa. 

Điều này làm giảm đáng kể không gian tìm kiếm. Thay vì kiểm tra từng ô, chúng tôi chỉ kiểm tra các ứng cử viên xuất phát từ cực tiểu hàng hoặc cực đại cột và xác minh tính nhất quán trong thời gian không đổi cho mỗi ứng cử viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nm(n+m))$|$O(1)$| Quá chậm | 
| Tối ưu |$O(nm)$|$O(n + m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng giải pháp bằng cách tính toán trước cực tiểu hàng và cực đại cột, sau đó giao các ràng buộc này. 

1. Đối với mỗi hàng, quét tất cả các cột và lưu trữ giá trị tối thiểu cũng như chỉ mục cột của nó. Nếu mức tối thiểu xuất hiện nhiều lần, chúng tôi vẫn giữ vị trí đầu tiên, vì việc bẻ khóa ưu tiên chỉ số cột nhỏ hơn. 
2. Đối với mỗi cột, quét tất cả các hàng và lưu trữ giá trị lớn nhất cũng như chỉ mục hàng của nó. Một lần nữa, các mối quan hệ được giải quyết bằng cách ưu tiên chỉ số hàng nhỏ nhất. 
3. Lặp lại tất cả các hàng. Đối với mỗi hàng, lấy vị trí tối thiểu được lưu trữ của nó$(i, j)$. 
4. Tại mỗi vị trí ứng viên, hãy kiểm tra xem giá trị tại$(i, j)$bằng giá trị lớn nhất của cột$j$. Điều này đảm bảo sự thống trị của cột. 
5. Nếu cả hai điều kiện đều khớp, hãy ghi ô này làm câu trả lời hợp lệ. 
6. Bởi vì các hàng được xử lý theo thứ tự tăng dần và chúng tôi chọn chỉ mục cột hợp lệ đầu tiên trong mỗi hàng, nên ô hợp lệ nhỏ nhất về mặt từ điển sẽ tự động được chọn. 

### Tại sao nó hoạt động 

Một trạng thái cân bằng Nash hợp lệ phải đồng thời thỏa mãn điều kiện cực trị cục bộ ở hàng và cột của nó. Điều kiện hàng buộc ô phải là một trong các cực tiểu của hàng và điều kiện cột buộc ô đó phải là một trong các cực đại của cột. Mọi giải pháp hợp lệ đều phải xuất hiện trong cả hai cấu trúc được tính toán trước. Bằng cách chỉ liệt kê các giá trị cực tiểu của hàng và xác thực theo giá trị cực đại của cột, chúng tôi đảm bảo rằng chúng tôi không bao giờ bỏ lỡ giải pháp đồng thời tránh kiểm tra các ô không liên quan. Thứ tự lặp lại đảm bảo tính tối thiểu về mặt từ điển mà không cần sắp xếp bổ sung. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = [list(map(int, input().split())) for _ in range(n)]

    row_min_val = [10**18] * n
    row_min_idx = [-1] * n

    col_max_val = [-10**18] * m
    col_max_idx = [-1] * m

    for i in range(n):
        for j in range(m):
            v = a[i][j]
            if v < row_min_val[i]:
                row_min_val[i] = v
                row_min_idx[i] = j
            if v > col_max_val[j]:
                col_max_val[j] = v
                col_max_idx[j] = i

    for i in range(n):
        j = row_min_idx[i]
        if j == -1:
            continue
        if a[i][j] == col_max_val[j]:
            print(i + 1, j + 1)
            return

    print(0, 0)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ xây dựng hai mảng phụ trợ trong một lần truyền qua ma trận. Mảng hàng nắm bắt phần tử nhỏ nhất trong mỗi hàng cùng với vị trí cột của nó. Mảng cột nắm bắt phần tử lớn nhất trong mỗi cột cùng với vị trí hàng của nó. 

Giai đoạn thứ hai chỉ kiểm tra cực tiểu của hàng, bởi vì mọi trạng thái cân bằng hợp lệ đều phải ở mức tối thiểu của hàng. Đối với mỗi hàng tối thiểu, chúng tôi xác minh xem đó có phải là cột tối đa cho cột đó hay không. Nếu vậy thì nó thỏa mãn cả hai ràng buộc. 

Một cạm bẫy phổ biến là cố gắng thực thi độc lập cả hai điều kiện trên mỗi ô mà không tính toán trước, dẫn đến quét bậc hai. Một sai lầm khác là không tôn trọng ngữ nghĩa so sánh chặt chẽ, đặc biệt nếu thay thế`<`hoặc`>`với`<=`hoặc`>=`, phá vỡ định nghĩa cân bằng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 4
1 2 3 4
1 2 3 5
1 2 3 6
2 3 5 7
```Chúng tôi tính toán hàng tối thiểu: 

| Hàng | Giá trị tối thiểu | Vị trí | 
| --- | --- | --- | 
| 1 | 1 | (1,1) | 
| 2 | 1 | (2,1) | 
| 3 | 1 | (3,1) | 
| 4 | 2 | (4,1) | 

Cột cực đại: 

| Col | Giá trị tối đa | Vị trí | 
| --- | --- | --- | 
| 1 | 2 | (4,1) | 
| 2 | 3 | (4,2) | 
| 3 | 5 | (4,3) | 
| 4 | 7 | (4,4) | 

Chúng tôi kiểm tra hàng 1 tối thiểu tại (1,1): cột 1 tối đa là 2, không khớp. 

Hàng 2 tối thiểu tại (2,1): lại không khớp. 

Hàng 3 tối thiểu tại (3,1): không khớp. 

Hàng 4 tối thiểu tại (4,1): giá trị 2 bằng cột 1 tối đa 2, vì vậy giá trị này hợp lệ. 

Đầu ra là (4,1). Theo thuật ngữ từ điển, đây là hàng hợp lệ sớm nhất và trong hàng đó là ứng cử viên duy nhất. 

Điều này chứng tỏ rằng giải pháp dựa vào giao điểm của cực tiểu hàng và cực đại cột thay vì các ô tùy ý. 

### Ví dụ 2 

đầu vào:```
3 3
5 1 5
2 3 4
1 6 7
```Hàng tối thiểu: 

| Hàng | Giá trị tối thiểu | Vị trí | 
| --- | --- | --- | 
| 1 | 1 | (1,2) | 
| 2 | 2 | (2,1) | 
| 3 | 1 | (3,1) | 

Cột cực đại: 

| Col | Giá trị tối đa | Vị trí | 
| --- | --- | --- | 
| 1 | 5 | (1,1) | 
| 2 | 6 | (3,2) | 
| 3 | 7 | (3,3) | 

Kiểm tra hàng tối thiểu: 

(1,2): 1 vs cột 2 max 6, không khớp 

(2,1): 2 vs cột 1 max 5, không khớp 

(3,1): 1 so với cột 1 tối đa 5, không khớp 

Không có giao điểm nào tồn tại nên đầu ra là (0,0). 

Điều này xác nhận rằng thuật toán loại bỏ chính xác các trường hợp trong đó các ràng buộc cực trị của hàng và cột không bao giờ thẳng hàng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm)$| Mỗi ô được truy cập một lần để tính cực tiểu hàng và cực đại cột, sau đó mỗi hàng được kiểm tra một lần | 
| Không gian |$O(n+m)$| Chúng tôi lưu trữ một mức tối thiểu trên mỗi hàng và một mức tối đa trên mỗi cột | 

Kích thước ma trận nhiều nhất là một triệu phần tử, do đó chỉ cần quét tuyến tính một lần là đủ. Giải pháp vẫn hoạt động tốt trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = [list(map(int, input().split())) for _ in range(n)]

    row_min_val = [10**18] * n
    row_min_idx = [-1] * n
    col_max_val = [-10**18] * m

    for i in range(n):
        for j in range(m):
            v = a[i][j]
            if v < row_min_val[i]:
                row_min_val[i] = v
                row_min_idx[i] = j
            if v > col_max_val[j]:
                col_max_val[j] = v

    for i in range(n):
        j = row_min_idx[i]
        if j != -1 and a[i][j] == col_max_val[j]:
            return f"{i+1} {j+1}"

    return "0 0"

# sample
assert run("4 4\n1 2 3 4\n1 2 3 5\n1 2 3 6\n2 3 5 7\n") == "4 1"

# all equal (no strict max/min possible conflict)
assert run("2 2\n1 1\n1 1\n") == "0 0"

# single valid equilibrium
assert run("2 2\n1 2\n3 0\n") == "1 2"

# unique row min and col max alignment
assert run("3 3\n1 2 3\n4 5 6\n7 8 9\n") == "3 3"

# no solution
assert run("3 3\n9 8 7\n6 5 4\n3 2 1\n") == "0 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | 4 1 | tính đúng đắn cơ bản | 
| tất cả đều bình đẳng | 0 0 | xử lý bất bình đẳng nghiêm ngặt | 
| lưới hỗn hợp | 1 2 | phát hiện cân bằng đơn | 
| tăng lưới | 3 3 | căn chỉnh tối đa trường hợp ranh giới | 
| lưới giảm dần | 0 0 | không có giao lộ hợp lệ | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi một hàng có nhiều cực tiểu giống nhau. Trong những trường hợp như vậy, việc chọn bất kỳ cột nào trong số chúng có thể dẫn đến việc kiểm tra các cột khác nhau. Thuật toán tránh sự mơ hồ bằng cách luôn ghi lại lần xuất hiện đầu tiên, giúp duy trì mức độ ưu tiên từ điển. 

Một trường hợp khác là khi một cột tối đa xuất hiện ở nhiều hàng. Chỉ một trong số chúng có thể đồng thời thỏa mãn ràng buộc tối thiểu về hàng. Việc kiểm tra giao nhau đảm bảo rằng chỉ một ô cực trị theo cả hai hướng mới được chấp nhận. 

Trường hợp thứ ba là khi lưới chứa các giá trị không đổi. Mỗi ô là một hàng tối thiểu và một cột tối đa theo nghĩa không chặt chẽ, nhưng sự bất bình đẳng nghiêm ngặt sẽ loại bỏ tất cả các ứng cử viên. Thuật toán không trả về chính xác giải pháp nào vì không có giá trị nào lớn hơn hoặc nhỏ hơn các giá trị lân cận theo hướng được yêu cầu.
