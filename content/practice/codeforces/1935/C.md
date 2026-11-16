---
title: "CF 1935C - Messenger trong MAC"
description: "Chúng ta được cung cấp một tập hợp các tin nhắn, mỗi tin nhắn có hai đặc điểm: ai, thời gian đọc cơ sở cho tin nhắn và bi, tọa độ ảnh hưởng đến chi phí chuyển tiếp giữa các tin nhắn liên tiếp."
date: "2026-06-08T18:04:37+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "constructive-algorithms", "data-structures", "dp", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1935
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 932 (Div. 2)"
rating: 1800
weight: 1935
solve_time_s: 89
verified: true
draft: false
---

[CF 1935C - Messenger trong MAC](https://codeforces.com/problemset/problem/1935/C) 

**Đánh giá:** 1800 
**Tags:** tìm kiếm nhị phân, sức mạnh vũ phu, thuật toán xây dựng, cấu trúc dữ liệu, dp, tham lam, sắp xếp 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các thông điệp, mỗi thông điệp có hai đặc điểm:`a_i`, thời gian đọc cơ sở cho tin nhắn và`b_i`, tọa độ ảnh hưởng đến chi phí chuyển tiếp giữa các tin nhắn liên tiếp. Tổng thời gian để đọc một tập hợp con các tin nhắn là tổng của`a_i`giá trị cộng với tổng chênh lệch tuyệt đối của`b_i`giá trị giữa các tin nhắn liên tiếp theo thứ tự đã chọn. Mục tiêu là, trong tổng thời gian đọc được phép nhất định`l`, để tìm số lượng tin nhắn tối đa có thể đọc được mà không vượt quá`l`. Một tập hợp có thể trống, trong trường hợp đó thời gian đọc là`0`. 

Những hạn chế là rất quan trọng. Mỗi ca kiểm thử có`n`lên tới 2000, nhưng tổng số tiền`n^2`trên tất cả các trường hợp thử nghiệm được giới hạn ở mức 4 triệu. Điều này cho thấy chúng ta có thể cung cấp các thuật toán có độ phức tạp trong trường hợp xấu nhất`O(n^2)`mỗi trường hợp thử nghiệm, nhưng bất cứ điều gì`O(n^3)`hoặc tệ hơn có thể sẽ hết thời gian. Giới hạn thời gian là 3 giây, phù hợp với giải pháp bậc hai nếu được thực hiện hiệu quả. 

Các trường hợp cạnh phát sinh khi tất cả`a_i`các giá trị lớn hơn`l`, do đó câu trả lời phải bằng 0, hoặc khi tất cả`b_i`các giá trị giống hệt nhau, làm cho chi phí chuyển đổi bằng không. Một cách tiếp cận tham lam ngây thơ chọn những tin nhắn nhỏ nhất`a_i`có thể thất bại vì chi phí chuyển tiếp cao giữa các tin nhắn có thể đẩy tổng chi phí vượt quá`l`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các tập hợp con của thông báo và tất cả các hoán vị của các tập hợp con đó để tính toán thời gian đọc. Đối với mỗi tập hợp con, chúng tôi sẽ tính tổng`a_i`và theo cặp`|b_i - b_j|`chuyển tiếp. Điều này rõ ràng là không khả thi vì số lượng hoán vị tăng theo giai thừa với kích thước tập hợp con. Ngay cả các tập hợp con cũng có`2^n`, nó quá lớn đối với`n = 2000`. 

Cái nhìn sâu sắc quan trọng là giảm vấn đề xuống một dạng mà chúng tôi không xem xét các hoán vị tùy ý. Sắp xếp tin nhắn theo`b_i`cho phép chúng ta xem xét trình tự trong`b_i`thứ tự, bởi vì tổng của sự khác biệt tuyệt đối`|b_i - b_{i+1}|`được giảm thiểu khi các số liên tiếp theo thứ tự được sắp xếp. Sau khi sắp xếp, chúng ta có thể tập trung vào các chuỗi con theo thứ tự này, điều này sẽ chuyển vấn đề thành bài toán lập trình động hoặc cửa sổ trượt nơi chúng ta tích lũy`a_i`giá trị và chi phí chuyển đổi một cách hiệu quả. 

Cách tiếp cận tối ưu lặp lại qua từng thông báo như một điểm bắt đầu tiềm năng của chuỗi và mở rộng chuỗi sang bên phải trong`b_i`thứ tự, theo dõi thời gian đọc tích lũy. Nếu tổng thời gian vượt quá`l`, chúng ta ngừng kéo dài và ghi lại độ dài. Điều này có thể được thực hiện trong`O(n^2)`cho mỗi trường hợp thử nghiệm, có thể chấp nhận được trong các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * n) | O(n) | Quá chậm | 
| Sắp xếp + Trượt/DP | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`Và`l`và sau đó đọc danh sách tin nhắn theo cặp`(a_i, b_i)`. 
3. Sắp xếp tin nhắn theo`b_i`giá trị. Điều này đảm bảo rằng bất kỳ chuỗi tiếp theo nào của các tin nhắn liên tiếp theo thứ tự này sẽ có chi phí chuyển đổi tối thiểu. 
4. Khởi tạo`max_size`về không. Điều này sẽ chứa tập hợp con lớn nhất được tìm thấy thỏa mãn giới hạn thời gian. 
5. Lặp lại từng chỉ mục tin nhắn bắt đầu có thể`i`. Bộ`total_time`ĐẾN`a[i]`Và`current_size`đến 1. 
6. Mở rộng chuỗi sang bên phải từ chỉ mục`i+1`ĐẾN`n-1`. Đối với mỗi tin nhắn mới`j`, thêm vào`a[j]`cộng thêm`|b[j] - b[j-1]|`ĐẾN`total_time`. Nếu như`total_time`vượt quá`l`, ngừng mở rộng. Ngược lại thì tăng`current_size`. 
7. Cập nhật`max_size`nếu như`current_size`lớn hơn mức tối đa trước đó. 
8. Sau khi kiểm tra tất cả các điểm bắt đầu, xuất ra`max_size`. 

Tại sao nó hoạt động: Sắp xếp theo`b_i`đảm bảo rằng chi phí chuyển đổi cho bất kỳ chuỗi con liên tiếp nào là tối thiểu, vì việc chèn các thông báo không theo thứ tự sẽ chỉ làm tăng tổng các khác biệt tuyệt đối. Việc mở rộng các chuỗi từ mỗi chỉ mục bắt đầu đảm bảo rằng tất cả các chuỗi con liền kề đều được xem xét và`max_size`theo dõi chính xác trình tự khả thi lớn nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, l = map(int, input().split())
        messages = [tuple(map(int, input().split())) for _ in range(n)]
        messages.sort(key=lambda x: x[1])
        max_size = 0
        
        for i in range(n):
            total_time = messages[i][0]
            if total_time > l:
                continue
            current_size = 1
            for j in range(i + 1, n):
                total_time += messages[j][0] + abs(messages[j][1] - messages[j-1][1])
                if total_time > l:
                    break
                current_size += 1
            max_size = max(max_size, current_size)
        
        print(max_size)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách đọc dữ liệu đầu vào một cách hiệu quả và sắp xếp các tin nhắn. Việc sắp xếp đảm bảo chi phí chuyển tiếp được giảm thiểu. Vòng lặp lồng nhau mở rộng mỗi thông báo bắt đầu thành một chuỗi liên tiếp và`break`ngăn chặn việc tính toán không cần thiết khi vượt quá giới hạn thời gian. Các trường hợp cạnh, chẳng hạn như`total_time > l`khi bắt đầu được xử lý ngay để bỏ qua những trình tự không khả thi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 8
4 3
1 5
2 4
4 3
2 3
```Sau khi sắp xếp theo`b_i`:```
(4,3), (4,3), (2,4), (1,5), (2,3)
```Hãy xem xét bắt đầu từ chỉ số 0: 

| Chỉ mục | a_i | b_i | Tổng Thời Gian | Kích thước hiện tại | 
| --- | --- | --- | --- | --- | 
| 0 | 4 | 3 | 4 | 1 | 
| 1 | 4 | 3 | 4+4+0=8 | 2 | 
| 2 | 2 | 4 | 8+2+1=11 > 8 | dừng lại | 

Kích thước tối đa từ lần bắt đầu này là 2. Các tính toán tương tự từ những lần bắt đầu khác mang lại kích thước tối đa cuối cùng là 3. 

Dấu vết này cho thấy cách sắp xếp theo`b_i`và tích lũy`a_i + |b_i - b_{i-1}|`cho phép kiểm tra hiệu quả các trình tự khả thi. 

### Ví dụ 2 

đầu vào:```
3 8
17 17
5 14
15 3
```Sắp xếp theo`b_i`:```
15 3, 5 14, 17 17
```Chỉ số bắt đầu 0: 

| Chỉ mục | a_i | b_i | Tổng Thời Gian | Kích thước hiện tại | 
| --- | --- | --- | --- | --- | 
| 0 | 15 | 3 | 15 > 8 | dừng lại | 

Bắt đầu chỉ số 1: 

| Chỉ mục | a_i | b_i | Tổng Thời Gian | Kích thước hiện tại | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 14 | 5 | 1 | 
| 2 | 17 | 17 | 5+17+3=25 > 8 | dừng lại | 

Kích thước tối đa là 1. 

Điều này xác nhận việc xử lý cạnh: trình tự vượt quá`l`ngay lập tức được bỏ qua. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) cho mỗi trường hợp thử nghiệm | Sắp xếp là O(n log n), vòng lặp mở rộng lồng nhau là trường hợp xấu nhất O(n^2). | 
| Không gian | O(n) | Lưu trữ tin nhắn và các biến tạm thời. | 

Với tổng ràng buộc là n^2 trên tất cả các trường hợp thử nghiệm 4*10^6, giải pháp phù hợp thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("""5
5 8
4 3
1 5
2 4
4 3
2 3
1 6
4 10
3 12
4 8
2 1
2 12
5 26
24 7
8 28
30 22
3 8
17 17
5 14
15 3
1000000000 998244353
179 239
228 1337
993 1007
""") == "3\n1\n2\n1\n0"

# custom test cases
assert run("1\n3 5\n2 1\n2 2\n2 3") == "2", "subsequence selection"
assert run("1\n4 3\n4 4\n4 4\n4 4\n4 4") == "0", "all a_i > l"
assert run("1\n5 100\n10 1\n20 1\n30 1\n40 1\n50 1") == "5", "all b_i same"
assert run("1\n5 15\n5 1\n5 2\n5 3\n5 4\n5 5") ==
```
