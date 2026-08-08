---
title: "CF 102535B - Làm việc với Khóa 2"
description: "Chúng tôi có một bộ sưu tập các số chìa khóa và một bộ sưu tập các số ổ khóa. Chìa khóa chỉ mở ổ khóa khi hai số liền kề nhau, nghĩa là hiệu tuyệt đối của chúng chính xác là một. Nhiệm vụ là đếm xem có bao nhiêu ổ khóa có ít nhất một chìa khóa có thể sử dụng được."
date: "2026-08-06T19:48:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102535
codeforces_index: "B"
codeforces_contest_name: "2020 UP ACM Algolympics Elimination Round"
rating: 0
weight: 102535
solve_time_s: 74
verified: true
draft: false
---

[CF 102535B - Làm việc với Khóa 2](https://codeforces.com/problemset/problem/102535/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ sưu tập các số chìa khóa và một bộ sưu tập các số ổ khóa. Chìa khóa chỉ mở ổ khóa khi hai số liền kề nhau, nghĩa là hiệu tuyệt đối của chúng chính xác là một. Nhiệm vụ là đếm xem có bao nhiêu ổ khóa có ít nhất một chìa khóa có thể sử dụng được. 

Đầu vào mô tả các chìa khóa mà Perry sở hữu và các ổ khóa xuất hiện trong những ngày hoạt động. Phần đầu tiên cung cấp các số khóa riêng biệt và phần thứ hai cung cấp các số khóa riêng biệt. Đầu ra là số ổ khóa có thể mở được bằng các phím có sẵn. 

Các giá trị được giới hạn ở 104, nhưng số lượng khóa và ổ khóa cũng có thể lên tới 104. Điều này có nghĩa là giải pháp kiểm tra mọi khóa với mọi khóa sẽ thực hiện tối đa 10^8 so sánh trong trường hợp xấu nhất. Con số này gần bằng giới hạn của những gì có thể vượt qua trong một số ngôn ngữ, nhưng ở đây nó không cần thiết vì phạm vi số nhỏ và mối quan hệ giữa chìa khóa và ổ khóa có cấu trúc rất đơn giản. Một cách tiếp cận tuyến tính là đủ nhanh. 

Các trường hợp cạnh chính xuất phát từ phần cuối của dãy số và do quên rằng một ổ khóa chỉ cần một chìa khóa khớp. Ví dụ, với đầu vào```
1
1
1
1
```câu trả lời là`0`. Chìa khóa số 1 không thể mở khóa 1 vì chênh lệch bằng 0 chứ không phải một. 

Một trường hợp ranh giới khác là:```
1
1
1
2
```Câu trả lời là`1`. Chiếc chìa khóa duy nhất cách ổ khóa đúng một chiếc, nên nó hoạt động được. Việc triển khai chỉ kiểm tra số trước đó để tìm khóa sẽ không thành công ở đây. 

Một trường hợp giới hạn trên tương tự là:```
1
104
1
103
```Câu trả lời là`1`. Khóa 104 có thể mở khóa 103, do đó giá trị cuối cùng trong phạm vi hoạt động giống như tất cả các giá trị khác. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là kiểm tra từng ổ khóa và so sánh nó với mọi chiếc chìa khóa. Đối với một khóa cụ thể, nếu bất kỳ khóa nào có sự khác biệt chính xác là một khóa thì khóa đó sẽ đóng góp một khóa vào câu trả lời. Phương pháp này đúng vì nó tuân theo định nghĩa của khóa hợp lệ. Tuy nhiên, với 10^4 phím và 10^4 ổ khóa, nó có thể thực hiện khoảng 10^8 lần kiểm tra, công việc này tốn nhiều công sức hơn mức cần thiết. 

Cấu trúc của bài toán cho chúng ta một quan sát đơn giản hơn. Ổ khóa có số x chỉ có thể mở được bằng chìa khóa x - 1 và x + 1. Chúng ta không bao giờ cần phải tìm kiếm tất cả các chìa khóa. Thay vào đó, chúng ta có thể lưu trữ các khóa có sẵn trong một bộ, sau đó có thể kiểm tra từng khóa bằng cách thực hiện hai truy vấn thành viên liên tục. 

Phương pháp brute-force hoạt động vì nó khám phá mọi cặp khóa có thể có, nhưng không thành công vì hầu hết các cặp đó đều không liên quan. Nhận xét rằng mỗi ổ khóa chỉ có hai chìa khóa hữu ích có thể làm giảm vấn đề xuống một chuỗi tra cứu đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(kL) | O(1) | Quá chậm đối với đầu vào lớn nhất | 
| Tối ưu | O(k + L) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các số chính và lưu chúng vào một bộ. Bộ này cho phép chúng tôi hỏi liệu một khóa cụ thể có tồn tại mà không cần quét toàn bộ bộ sưu tập hay không. 
2. Với mỗi số khóa x, kiểm tra xem x - 1 có tồn tại trong bộ khóa hay x + 1 có tồn tại trong bộ khóa hay không. Đây là hai chìa khóa duy nhất có thể mở được ổ khóa này. 
3. Tăng câu trả lời bất cứ khi nào có ít nhất một trong hai chìa khóa có thể có. 
4. In số lần khóa thành công cuối cùng. 

Lý do điều này có tác dụng là vì quy tắc mở chỉ phụ thuộc vào khoảng cách giữa chìa khóa và số ổ khóa. Đối với bất kỳ khóa x nào, mọi khóa ngoài x - 1 và x + 1 đều có khoảng cách khác nhau, vì vậy việc kiểm tra hai giá trị đó bao gồm mọi trường hợp thành công có thể xảy ra. 

Tại sao nó hoạt động: 

Bất biến được duy trì trong khi xử lý khóa là câu trả lời bằng số lượng khóa đã được xử lý có ít nhất một khóa liền kề trong bộ. Khi một ổ khóa x mới được kiểm tra, thuật toán sẽ kiểm tra chính xác bộ chìa khóa hoàn chỉnh có thể mở được nó. Nếu một tồn tại, số lượng sẽ tăng lên và nếu không tồn tại, số lượng sẽ không thay đổi. Vì vậy, mọi khóa đều được tính chính xác khi cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = sys.stdin.read().split()
    if not data:
        return

    idx = 0

    k = int(data[idx])
    idx += 1

    keys = set()
    for _ in range(k):
        keys.add(int(data[idx]))
        idx += 1

    L = int(data[idx])
    idx += 1

    answer = 0
    for _ in range(L):
        lock = int(data[idx])
        idx += 1
        if lock - 1 in keys or lock + 1 in keys:
            answer += 1

    print(answer)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên tạo một tập hợp từ các khóa vì kiểm tra tư cách thành viên là hoạt động trung tâm. Một bộ Python cho thời gian tra cứu trung bình O(1), biến mỗi lần kiểm tra khóa thành công việc liên tục. 

Vòng lặp khóa tuân theo thuật toán trực tiếp. Đối với giá trị khóa`lock`, các biểu thức`lock - 1`Và`lock + 1`đại diện cho các số khóa duy nhất có thể. Các ranh giới không yêu cầu xử lý đặc biệt vì việc tra cứu các giá trị bên ngoài phạm vi khóa hợp lệ chỉ trả về sai từ tập hợp. 

Việc triển khai đọc tất cả các mã thông báo đầu vào cùng một lúc vì kích thước đầu vào nhỏ và điều này tránh được việc phụ thuộc vào việc thẩm phán có cung cấp thêm khoảng trắng hay định dạng dòng hay không. Không có vấn đề tràn số nguyên trong Python và giá trị lớn nhất được kiểm tra chỉ là 105. 

## Ví dụ đã hoạt động 

Đối với mẫu được cung cấp:```
5
2 4 6 8 10
3
1 5 102
```Bộ khóa là`{2, 4, 6, 8, 10}`. 

| Khóa | Kiểm tra khóa - 1 | Kiểm tra khóa + 1 | Đếm sau khi xử lý | 
| --- | --- | --- | --- | 
| 1 | 0 không tìm thấy | 2 tìm thấy | 1 | 
| 5 | 4 được tìm thấy | 6 tìm thấy | 2 | 
| 102 | 101 không tìm thấy | 103 không tìm thấy | 2 | 

Khóa đầu tiên hoạt động vì phím 2 mở được nó. Ổ khóa thứ hai có thể có hai chìa khóa, cả hai đều tồn tại. Ổ khóa cuối cùng không có chìa khóa liền kề nên đáp án vẫn là 2. 

Một ví dụ thứ hai:```
3
1 50 104
4
2 49 103 104
```Bộ khóa là`{1, 50, 104}`. 

| Khóa | Kiểm tra khóa - 1 | Kiểm tra khóa + 1 | Đếm sau khi xử lý | 
| --- | --- | --- | --- | 
| 2 | 1 tìm thấy | 3 không tìm thấy | 1 | 
| 49 | 48 không tìm thấy | 50 được tìm thấy | 2 | 
| 103 | 102 không tìm thấy | 104 được tìm thấy | 3 | 
| 104 | 103 không tìm thấy | 105 không tìm thấy | 3 | 

Ví dụ này thực hiện cả hai hướng của quy tắc kề và xác nhận rằng một chìa khóa không thể mở được ổ khóa có cùng số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k + L) | Mỗi khóa được chèn một lần và mỗi khóa được kiểm tra bằng hai lần tra cứu đã đặt. | 
| Không gian | O(k) | Bộ này lưu trữ tất cả các khóa có sẵn. | 

Đầu vào tối đa chỉ chứa 10^4 khóa và 10^4 khóa, do đó giải pháp tuyến tính thực hiện khoảng vài chục nghìn thao tác. Nó thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

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

# provided sample
assert run("""5
2 4 6 8 10
3
1 5 102
""") == "2\n", "sample 1"

# minimum-size case
assert run("""1
1
1
1
""") == "0\n", "same key and lock do not work"

# lower boundary
assert run("""1
1
1
2
""") == "1\n", "key 1 opens lock 2"

# upper boundary
assert run("""1
104
1
103
""") == "1\n", "key 104 opens lock 103"

# larger mixed case
assert run("""4
2 10 50 104
5
1 3 9 51 100
""") == "4\n", "checks both sides of every lock"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chìa khóa đơn 1 và khóa 1 | 0 | Các số giống nhau không phải là cặp hợp lệ | 
| Chìa khóa 1 và khóa 2 | 1 | Liền kề ranh giới dưới | 
| Chìa khóa 104 và khóa 103 | 1 | Liền kề ranh giới trên | 
| Các giá trị hỗn hợp xung quanh một số khóa | 4 | Tính đúng đắn chung của cả hai hướng | 

## Vỏ cạnh 

Đối với trường hợp chìa và ổ khóa có cùng số hiệu, chẳng hạn:```
1
1
1
1
```thuật toán kiểm tra khóa 1 bằng cách tìm khóa 0 và 2. Cả hai đều không tồn tại nên câu trả lời vẫn là 0. Điều này tránh được sai lầm phổ biến khi coi đẳng thức là một kết quả khớp hợp lệ. 

Đối với ổ khóa được mở bằng chìa lớn hơn, chẳng hạn như:```
1
1
1
2
```thuật toán kiểm tra khóa 1 và 3 để tìm khóa 2. Khóa 1 tồn tại nên câu trả lời sẽ trở thành một. Điều này xác nhận rằng cả hai hướng của quy tắc sai phân đều được xử lý. 

Đối với giá trị biên cuối cùng:```
1
104
1
103
```thuật toán kiểm tra khóa 102 và 104 để tìm khóa 103. Khóa 104 tồn tại nên khóa được tính thành công. Các giá trị nằm ngoài phạm vi cho phép không gây ra vấn đề gì vì chúng đơn giản là không có trong tập hợp.
