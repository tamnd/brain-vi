---
title: "CF 102535C - Làm việc với Khóa 3"
description: "Perry sở hữu một bộ sưu tập các chìa khóa được đánh số và mỗi chìa khóa chỉ có thể mở được những ổ khóa ngay cạnh số của chính nó. Các ổ khóa tạo thành một dòng đơn giản từ 1 đến 1.000.000.000."
date: "2026-08-07T05:00:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102535
codeforces_index: "C"
codeforces_contest_name: "2020 UP ACM Algolympics Elimination Round"
rating: 0
weight: 102535
solve_time_s: 219
verified: true
draft: false
---

[CF 102535C - Làm việc với Khóa 3](https://codeforces.com/problemset/problem/102535/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 39s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Perry sở hữu một bộ sưu tập các chìa khóa được đánh số và mỗi chìa khóa chỉ có thể mở được những ổ khóa ngay cạnh số của chính nó. Các ổ khóa tạo thành một dòng đơn giản từ 1 đến 1.000.000.000. Đối với mỗi ổ khóa xuất hiện trong danh sách đầu vào của những ngày hoạt động, chúng ta cần xác định xem liệu có ít nhất một trong các số lân cận của nó có tồn tại trong bộ sưu tập khóa của Perry hay không. Câu trả lời là số lượng ổ khóa đang hoạt động có thể mở được. 

Đầu vào đưa ra tập hợp các vị trí quan trọng đầu tiên. Bộ quan trọng thứ hai là tập hợp các vị trí khóa sẽ xuất hiện trong những ngày hoạt động. Chúng tôi không cần xử lý tất cả các khóa có thể có vì có thể có hàng tỷ khóa, chỉ những khóa cụ thể được liệt kê trong vấn đề đầu vào. 

Cả số lượng khóa và số lượng khóa được truy vấn có thể lên tới 100.000. Với giới hạn hai giây, giải pháp phải gần với thời gian tuyến tính. Việc kiểm tra từng chìa khóa với từng ổ khóa sẽ yêu cầu tới 10.000.000.000 so sánh, vượt xa những gì thực tế. Một giải pháp xung quanh O(k + L) hoặc O((k + L) log k) là phù hợp. 

Có một số trường hợp khó thực hiện có thể dễ dàng thất bại. 

Một ổ khóa ở vị trí 1 chỉ có một chìa khóa khả thi là chìa khóa 2. Việc coi nó như thể chìa khóa 0 tồn tại sẽ đưa ra câu trả lời sai. Ví dụ:```
1
0
1
1
```không phải là đầu vào hợp lệ vì các số khóa bắt đầu từ 1, nhưng trường hợp hợp lệ tương đương là:```
1
3
1
1
```Câu trả lời là 0 vì phím 3 không thể mở khóa 1. Việc thực hiện bất cẩn kiểm tra chênh lệch tuyệt đối không chính xác xung quanh các ranh giới có thể tạo ra kết quả sai. 

Một ổ khóa ở giữa có thể được mở từ hai bên và chỉ cần có một ổ khóa bên cạnh phù hợp là đủ. Ví dụ:```
1
4
1
5
```Câu trả lời là 1 vì khóa 4 mở khóa 5. Việc triển khai yêu cầu tồn tại cả hai khóa liền kề sẽ trả về 0 không chính xác. 

Không nên nhầm lẫn khóa với khóa đang được truy vấn. Ví dụ:```
1
10
1
10
```Câu trả lời là 0 vì khóa 10 không mở được khóa 10. Nó chỉ mở được khóa 9. Việc kiểm tra tư cách thành viên trực tiếp của khóa bên trong bộ khóa sẽ âm thầm thất bại trong trường hợp này. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là kiểm tra từng ổ khóa và so sánh nó với mọi chiếc chìa khóa. Đối với mỗi khóa được truy vấn, chúng tôi sẽ tính toán sự khác biệt giữa khóa đó và mọi khóa có sẵn và đếm nó nếu chênh lệch chính xác là một. Điều này đúng vì quy luật mở khóa chỉ phụ thuộc vào khoảng cách đó. 

Tuy nhiên, trường hợp xấu nhất chứa 100.000 chìa khóa và 100.000 ổ khóa. Phương pháp brute-force thực hiện khoảng 10 tỷ so sánh. Ngay cả với những thao tác rất nhanh, khối lượng công việc này cũng không thể hoàn thành trong thời gian giới hạn. 

Một quan sát hữu ích là một cái ổ khóa không quan tâm đến tất cả các chìa khóa. Nó chỉ quan tâm đến hai hàng xóm có thể có: số ngay trước nó và số ngay sau nó. Thay vì tìm kiếm trong toàn bộ bộ sưu tập khóa, chúng ta có thể lưu trữ tất cả các khóa trong một tập hợp băm và hỏi trực tiếp xem liệu hàng xóm có tồn tại hay không. 

Giải pháp brute-force hoạt động vì nó kiểm tra mọi mối quan hệ có thể có giữa chìa khóa và ổ khóa, nhưng nó lặp lại một lượng lớn công việc không cần thiết. Nhận xét rằng mỗi khóa chỉ có hai ứng viên sẽ giảm mỗi truy vấn thành hai lần tra cứu liên tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(kL) | O(1) | Quá chậm | 
| Tối ưu | O(k + L) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ mọi số khóa trong bộ băm. Một tập hợp được sử dụng vì chúng ta cần kiểm tra liên tục xem một số cụ thể có tồn tại hay không và tra cứu trung bình O(1) sẽ cho tốc độ cần thiết. 
2. Đọc từng khóa hoạt động một lần. Chúng ta không cần phải sắp xếp các ổ khóa hay cất giữ chúng vì mỗi ổ khóa có thể được trả lời độc lập. 
3. Đối với khóa hiện tại`x`, kiểm tra xem`x - 1`có nằm trong bộ khóa hay không`x + 1`nằm trong bộ khóa. Nếu một trong hai tra cứu thành công, hãy tăng câu trả lời. 
4. Xuất số đếm cuối cùng sau khi tất cả các khóa đã được xử lý. 

Tại sao nó hoạt động: mọi chìa khóa có thể mở được ổ khóa`x`phải có một số nhỏ hơn chính xác một hoặc lớn hơn đúng một`x`. Thuật toán kiểm tra chính xác hai khả năng này và không có khả năng nào khác. Nếu một trong số chúng tồn tại, khóa sẽ được tính và nếu không tồn tại thì không có khóa hợp lệ nào tồn tại. Vì mỗi khóa truy vấn được xử lý độc lập nên số lượng tích lũy chính xác là số ngày thành công. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    k = int(input())
    keys = set(map(int, input().split()))

    L = int(input())
    locks = list(map(int, input().split()))

    ans = 0

    for lock in locks:
        if lock - 1 in keys or lock + 1 in keys:
            ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Bộ khóa được xây dựng trước tiên vì mọi thao tác sau này đều là kiểm tra tư cách thành viên. Việc triển khai tập hợp của Python cung cấp khả năng tra cứu trung bình theo thời gian không đổi, đây là tính năng tối ưu hóa cốt lõi. 

Mỗi khóa được xử lý một lần. Mã kiểm tra trực tiếp hai số lân cận thay vì tìm kiếm qua tất cả các khóa. Các trường hợp ranh giới được xử lý một cách tự nhiên vì việc tra cứu một số không tồn tại chỉ trả về sai. Ví dụ, kiểm tra`lock - 1`khi`lock`là 1 bài kiểm tra 0, không thể xuất hiện trong bộ khóa. 

Số nguyên Python không tràn ở đây vì giá trị lớn nhất chỉ là 1.000.000.000, nằm trong cách biểu diễn số nguyên của Python. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
2 4 6 8 10
3
1 5 102
```Thuật toán xây dựng bộ khóa`{2, 4, 6, 8, 10}`. 

| Khóa | Kiểm tra hàng xóm bên trái | Kiểm tra ngay hàng xóm | Đếm | 
| --- | --- | --- | --- | 
| 1 | 0 vắng mặt | 2 có mặt | 1 | 
| 5 | 4 có mặt | 6 có mặt | 2 | 
| 102 | 101 vắng mặt | 103 vắng mặt | 2 | 

Dấu vết cho thấy chỉ cần một hàng xóm phù hợp. Khóa 5 được tính ngay cả khi cả hai hàng xóm đều tồn tại, trong khi khóa 102 không thành công vì không có khóa nào tồn tại. 

### Ví dụ tùy chỉnh 

đầu vào:```
3
1 7 100
4
2 6 7 101
```| Khóa | Kiểm tra hàng xóm bên trái | Kiểm tra ngay hàng xóm | Đếm | 
| --- | --- | --- | --- | 
| 2 | 1 hiện diện | 3 vắng mặt | 1 | 
| 6 | 5 vắng mặt | 7 có mặt | 2 | 
| 7 | 6 vắng mặt | 8 vắng mặt | 2 | 
| 101 | 100 có mặt | 102 vắng mặt | 3 | 

Dấu vết xác nhận rằng chìa khóa có cùng số với ổ khóa không giúp ích được gì. Khóa 7 không mở được bằng phím 7. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k + L) | Mỗi khóa được chèn một lần và mỗi khóa thực hiện hai lần tra cứu. | 
| Không gian | O(k) | Bộ băm lưu trữ tất cả các khóa có sẵn. | 

Đầu vào lớn nhất chứa 100.000 phím và 100.000 ổ khóa. Giải pháp tuyến tính chỉ thực hiện vài trăm nghìn thao tác, phù hợp thoải mái trong giới hạn thời gian và giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    import sys
    input = sys.stdin.readline

    k = int(input())
    keys = set(map(int, input().split()))
    L = int(input())
    locks = list(map(int, input().split()))

    ans = 0
    for lock in locks:
        if lock - 1 in keys or lock + 1 in keys:
            ans += 1

    print(ans)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("""5
2 4 6 8 10
3
1 5 102
""") == "2\n", "sample 1"

assert run("""1
1
1
2
""") == "1\n", "minimum values"

assert run("""4
1 3 5 7
5
2 4 6 8 10
""") == "4\n", "all locks except last have neighbors"

assert run("""3
999999998 999999999 1000000000
4
1 999999997 999999999 1000000000
""") == "3\n", "large boundary values"

assert run("""5
10 20 30 40 50
5
10 20 30 40 50
""") == "0\n", "same key and lock numbers do not match"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Giá trị khóa và khóa tối thiểu | 1 | Kiểm tra các vị trí nhỏ nhất có thể. | 
| Chìa khóa và ổ khóa thay thế | 4 | Xác nhận rằng mỗi khóa chỉ được đánh giá bằng các khóa lân cận. | 
| Số lượng rất lớn | 3 | Kiểm tra ranh giới trên gần 1.000.000.000. | 
| Vị trí khóa và khóa giống hệt nhau | 0 | Ngăn chặn việc đếm các số bằng nhau làm khóa hợp lệ. | 

## Vỏ cạnh 

Đối với trường hợp biên, hãy xem xét:```
1
3
1
1
```Bộ khóa chỉ chứa 3. Thuật toán kiểm tra khóa 1 bằng cách xem xét 0 và 2. Cả hai đều không tồn tại, vì vậy câu trả lời vẫn là 0. Thuật toán không bao giờ giả định rằng khóa có số 0 tồn tại. 

Đối với trường hợp một người hàng xóm:```
1
4
1
5
```Thuật toán kiểm tra khóa 5. Hàng xóm bên trái, 4, được tìm thấy trong tập hợp, vì vậy câu trả lời trở thành 1. Nó không yêu cầu tồn tại hàng xóm bên phải. 

Đối với trường hợp số bằng nhau:```
1
10
1
10
```Thuật toán kiểm tra 9 và 11. Không có số nào nên câu trả lời là 0. Bản thân số khóa không liên quan trừ khi nó cũng xuất hiện dưới dạng khóa lân cận, điều này là không thể.
