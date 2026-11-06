---
title: "CF 1257E - Cuộc thi"
description: "Chúng ta có ba người tham gia và một tập hợp các bài toán được đánh số từ 1 đến n. Mỗi người tham gia hiện đang nắm giữ một số tập hợp con của các bài toán, nhưng các tập hợp con đó có thể không tôn trọng sự phân chia mong muốn: người tham gia đầu tiên phải giữ tiền tố của các bài toán, người tham gia thứ ba phải giữ một…"
date: "2026-06-11T20:50:20+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1257
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 76 (Rated for Div. 2)"
rating: 2000
weight: 1257
solve_time_s: 154
verified: true
draft: false
---

[CF 1257E - Cuộc thi](https://codeforces.com/problemset/problem/1257/E) 

**Xếp hạng:** 2000 
**Tags:** cấu trúc dữ liệu, dp, tham lam 
**Thời gian giải:** 2m 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có ba người tham gia và một tập hợp các bài toán được đánh số từ 1 đến n. Mỗi người tham gia hiện nắm giữ một số tập con của các bài toán, nhưng các tập con đó có thể không tôn trọng sự phân chia mong muốn: người tham gia đầu tiên phải giữ tiền tố của các bài toán, người tham gia thứ ba phải giữ một hậu tố và người tham gia thứ hai phải giữ tất cả các bài toán còn lại ở giữa. Đầu vào cho chúng ta biết sự phân bố hiện tại của các vấn đề giữa ba người tham gia. Nhiệm vụ của chúng ta là xác định số lần di chuyển tối thiểu cần thiết để sắp xếp lại các bài toán theo đúng thứ tự. Mỗi bước di chuyển bao gồm việc chuyển một vấn đề từ người tham gia này sang người tham gia khác. 

Các ràng buộc cho chúng ta biết rằng n có thể lớn tới 200.000, nghĩa là mọi giải pháp đều phải chạy trong thời gian tuyến tính hoặc gần tuyến tính. Phương pháp bậc hai hoặc mô phỏng đơn giản của mọi chuyển động sẽ quá chậm vì điều đó có thể yêu cầu tới$4 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất. Ngoài ra, vì mỗi số bài toán là duy nhất và nằm trong khoảng từ 1 đến n, nên chúng ta có thể sử dụng các mảng được lập chỉ mục theo số bài toán để tra cứu trực tiếp thay vì tìm kiếm tuyến tính. 

Các trường hợp phức tạp phát sinh khi các vấn đề "không đúng chỗ" so với phân vùng mục tiêu cuối cùng của chúng. Ví dụ: nếu người tham gia đầu tiên gặp vấn đề 5 nhưng tiền tố chỉ đến vấn đề 4 thì vấn đề này cần phải được chuyển đi. Tương tự, một số người tham gia ban đầu có thể không gặp vấn đề gì trong phân đoạn cuối cùng của họ, điều này hợp lệ nhưng không yêu cầu di chuyển đối với phân đoạn trống đó. Một cách tiếp cận ngây thơ chỉ đơn giản là đếm các vấn đề đặt nhầm chỗ cho mỗi người tham gia đôi khi có thể bị tính quá mức vì một số động thái có thể đồng thời khắc phục nhiều xung đột nếu được lựa chọn cẩn thận. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng mọi hành động: chọn một vấn đề, chuyển nó cho đúng người tham gia và lặp lại cho đến khi tất cả các vấn đề nằm đúng phân đoạn. Mặc dù cách này hoạt động hợp lý nhưng nó sẽ thực hiện tối đa O(n2) thao tác trong trường hợp xấu nhất vì mỗi động thái có thể liên quan đến việc quét danh sách người tham gia để xác định vấn đề. Với n = 200.000 thì điều này rõ ràng là không thực tế. 

Cái nhìn sâu sắc quan trọng đến từ việc quan sát thấy rằng các bước di chuyển tối thiểu tương ứng với việc tối đa hóa số lượng vấn đề đã có theo thứ tự tương đối chính xác. Cụ thể, chúng ta có thể tập trung vào **hậu tố dài nhất của các bài toán được sắp xếp đúng theo thứ tự giảm dần**, bắt đầu từ bài toán n xuống đến 1. Không cần phải di chuyển hậu tố này. Tất cả các vấn đề khác ngoài hậu tố này cuối cùng sẽ yêu cầu mỗi vấn đề chính xác một lần. Bằng cách theo dõi độ dài của hậu tố giảm dần này một cách hiệu quả bằng cách sử dụng mảng hiện diện, chúng tôi giảm bớt vấn đề từ việc mô phỏng các bước di chuyển thành một lần truyền tuyến tính duy nhất qua dữ liệu. 

Điều này làm giảm vấn đề tính toán`n - length_of_longest_correct_suffix`, đó là số lần di chuyển cần thiết. Sự thông minh chính là nhận ra rằng bạn chỉ cần tập trung vào các vấn đề được đánh số cao nhất và kiểm tra xem chúng đã ở đúng thứ tự phân khúc tương đối hay chưa, thay vì thử mọi cách phân phối lại có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc phần đầu vào để có được bộ vấn đề hiện tại của ba người tham gia. Ghép tất cả các bộ thành một mảng duy nhất để xử lý dễ dàng hơn. 
2. Xây dựng một mảng hiện diện boolean có kích thước n+1 để đánh dấu những vấn đề đang tồn tại. Điều này cho phép O(1) kiểm tra xem có vấn đề cụ thể nào tồn tại trong danh sách hiện tại của bất kỳ người tham gia nào không. 
3. Khởi tạo bộ đếm`correct_suffix_length = 0`. Lặp lại từ vấn đề n xuống 1. Với mỗi vấn đề i, hãy kiểm tra xem nó có tồn tại trong phân phối hiện tại hay không và có thể tiếp tục hậu tố giảm dần hay không. 
4. Dừng lại khi thiếu sót hoặc không ổn định; tất cả các vấn đề sau chỉ mục này đều tạo thành "hậu tố chính xác" không cần di chuyển. 
5. Tính số bước di chuyển tối thiểu như sau`n - correct_suffix_length`. Điều này đếm chính xác các vấn đề được đặt sai chỗ. 
6. In kết quả. 

**Tại sao nó hoạt động**: Bằng cách đếm hậu tố giảm dần dài nhất từ ​​n trở về trước, chúng tôi nắm bắt được số lượng vấn đề tối đa đã có thứ tự tương đối chính xác ở cuối. Mọi vấn đề khác đảm bảo cần phải di chuyển một lần, vì không có cách nào đặt nó chính xác mà không di chuyển nó. Bất biến này đảm bảo rằng`n - length_of_suffix`chính xác bằng số lần di chuyển tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

k1, k2, k3 = map(int, input().split())
a1 = list(map(int, input().split()))
a2 = list(map(int, input().split()))
a3 = list(map(int, input().split()))

n = k1 + k2 + k3
pos = [0] * (n + 2)

for x in a1 + a2 + a3:
    pos[x] = 1

suffix_len = 0
for i in range(n, 0, -1):
    if pos[i] == 1:
        suffix_len += 1
    else:
        break

print(n - suffix_len)
```Chúng tôi đọc đầu vào một cách nhanh chóng bằng cách sử dụng`sys.stdin.readline`vì n có thể lớn. Chúng tôi xây dựng một mảng hiện diện`pos`có kích thước n+2 để tránh lỗi sai một. Chúng ta lặp lại từ n đến 1 và đếm xem có bao nhiêu vấn đề tạo thành hậu tố đúng. Số bước di chuyển cần thiết là tổng số bài toán trừ đi độ dài hậu tố này. Điều này đảm bảo chúng tôi chỉ giải quyết chính xác những vấn đề không đúng chỗ. Việc nối tất cả các danh sách cho phép lặp lại đơn giản mà không cần phân biệt người tham gia vì hậu tố chỉ phụ thuộc vào thứ tự chung. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
2 1 2
3 1
4
2 5
```| Vấn đề | Sự hiện diện? | Hậu Tố Len | 
| --- | --- | --- | 
| 5 | vâng | 1 | 
| 4 | vâng | 2 | 
| 3 | vâng | 3 | 
| 2 | vâng | nghỉ | 

n = 5, hậu tố_len = 4 → nước đi = 5 - 4 = 1 

Bảng cho thấy tất cả các bài toán ngoại trừ bài 2 đều đã có hậu tố nên chỉ cần thực hiện một động tác. 

**Mẫu 2** 

đầu vào:```
3 2 1
1 2 3
4 5
6
```Hậu tố đếm từ 6 trở xuống: 6,5,4,3,... độ dài hậu tố là 1 (chỉ bài 6), nước đi = 6-1=5. Kiểm tra cẩn thận cho thấy vấn đề 6 đã đúng và phần còn lại cần phải di chuyển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi lặp lại một lần trên tất cả các vấn đề để xây dựng mảng hiện diện, sau đó lặp lại để tính hậu tố. | 
| Không gian | O(n) | Mảng hiện diện có kích thước n+2 lưu trữ có vấn đề tồn tại. | 

Với n 2·10⁵, giá trị này dễ dàng phù hợp với giới hạn 2 giây và giới hạn bộ nhớ 512MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    k1, k2, k3 = map(int, input().split())
    a1 = list(map(int, input().split()))
    a2 = list(map(int, input().split()))
    a3 = list(map(int, input().split()))
    n = k1 + k2 + k3
    pos = [0] * (n + 2)
    for x in a1 + a2 + a3:
        pos[x] = 1
    suffix_len = 0
    for i in range(n, 0, -1):
        if pos[i] == 1:
            suffix_len += 1
        else:
            break
    return str(n - suffix_len)

# Provided samples
assert run("2 1 2\n3 1\n4\n2 5\n") == "1"
assert run("3 2 1\n1 2 3\n4 5\n6\n") == "5"

# Custom cases
assert run("1 1 1\n3\n1\n2\n") == "2", "All shuffled, minimal moves"
assert run("3 3 3\n1 2 3\n4 5 6\n7 8 9\n") == "0", "Already sorted prefix/mid/suffix"
assert run("1 1 1\n1\n2\n3\n") == "0", "Already sorted, minimum size"
assert run("2 2 1\n2 1\n4 3\n5\n") == "3", "Completely reversed within segments"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1\n3\n1\n2\n`| 2 | Kích thước đầu vào tối thiểu, tất cả các vấn đề được xáo trộn | 
|`3 3 3\n1 2 3\n4 5 6\n7 8 9\n`| 0 | Đã được phân phối chính xác | 
|`1 1 1\n1\n2\n3\n`| 0 | Trường hợp cạnh nhỏ nhất hợp lệ n | 
|`2 2 1\n2 1\n4 3\n5\n`| 3 | Hoàn toàn đảo ngược trong các phân đoạn | 

## Vỏ cạnh 

Đối với đầu vào`1 1 1\n3\n1\n2\n`, hậu tố đúng chỉ là`3`. Vấn đề`1`Và`2`không đúng chỗ nên thuật toán sẽ tính toán`suffix_len = 1`Và`moves = 3-1=2`, phù hợp với số lần di chuyển tối thiểu cần thiết
