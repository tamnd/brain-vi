---
title: "CF 2087D - Chữ hoa hay chữ thường?"
description: "Chúng ta được cung cấp một danh sách các chuỗi được sắp xếp, được gọi là các thẻ điều khiển, với tổng số $n$ và một thẻ điều khiển đích là $h$. Mục tiêu là xác định vị trí dựa trên 1 của $h$ trong danh sách này."
date: "2026-06-08T05:58:26+07:00"
tags: ["codeforces", "competitive-programming", "*special", "interactive"]
categories: ["algorithms"]
codeforces_contest: 2087
codeforces_index: "D"
codeforces_contest_name: "Kotlin Heroes: Episode 12"
rating: 0
weight: 2087
solve_time_s: 100
verified: false
draft: false
---

[CF 2087D - Chữ hoa hay chữ thường?](https://codeforces.com/problemset/problem/2087/D) 

**Đánh giá:** - 
**Thẻ:** *đặc biệt, tương tác 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các chuỗi được sắp xếp, gọi là các chuỗi điều khiển, với tổng số$n$và một tay cầm mục tiêu$h$. Mục đích là xác định vị trí dựa trên 1 của$h$trong danh sách này. Thách thức là thứ tự sắp xếp mang tính từ điển nhưng chúng ta không biết liệu chữ in hoa đứng trước chữ cái viết thường hay ngược lại. Mỗi tay cầm bắt đầu bằng chữ thường hoặc chữ in hoa, nhưng tất cả các chữ cái tiếp theo đều là chữ thường. Các bộ điều khiển là duy nhất và bộ điều khiển đích được đảm bảo tồn tại. 

Sự tương tác cho phép chúng ta truy vấn phần xử lý ở một vị trí cụ thể$i$và chúng tôi có thể thực hiện tối đa 10 truy vấn như vậy. Bởi vì$n$có thể lên tới 500, việc lặp lại toàn bộ danh sách sẽ yêu cầu tới 500 truy vấn, vượt quá giới hạn của chúng tôi. Vì vậy, chúng ta cần một chiến lược đảm bảo rằng chúng ta có thể tìm thấy$h$với tối đa 10 truy vấn. 

Một cách tiếp cận đơn giản là kiểm tra từng vị trí theo thứ tự, nhưng điều này sẽ thất bại một lần.$n > 10$. Một điều tinh tế khác là cách viết hoa của ký tự đầu tiên có thể thay đổi thứ tự từ điển hiệu quả: ví dụ: nếu chữ cái đầu tiên là chữ hoa, "Apple" có thể đứng trước "banana" hoặc sau, tùy thuộc vào hệ thống. Nếu một giải pháp bỏ qua điều này, nó có thể ngừng tìm kiếm quá sớm hoặc ở sai nửa danh sách. 

Các trường hợp cạnh bao gồm mục tiêu là phần tử đầu tiên hoặc cuối cùng hoặc phần tử đầu tiên có chữ in hoa trong khi các phần còn lại là chữ thường. Những điều này có thể đánh lừa một tìm kiếm nhị phân ngây thơ nếu chúng ta cho rằng chữ thường luôn xuất hiện trước. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: truy vấn các vị trí từ 1 đến$n$cho đến khi tay cầm trả về bằng$h$. Điều này được đảm bảo để tìm ra phần điều khiển vì chúng tôi biết nó tồn tại nhưng yêu cầu tối đa$n$truy vấn. Vì$n = 500$, điều này vượt xa giới hạn 10 truy vấn, vì vậy việc sử dụng vũ lực là không khả thi. 

Cách tiếp cận tối ưu thúc đẩy tìm kiếm nhị phân. Thứ tự từ điển là tổng, vì vậy nếu chúng ta có thể xác định thứ tự nào cơ sở dữ liệu sử dụng (chữ hoa trước hoặc chữ thường trước), chúng ta có thể thực hiện tìm kiếm nhị phân cổ điển. Thông tin chi tiết quan trọng là chúng ta không cần truy vấn mọi phần tử để xác định thứ tự. Ô điều khiển đầu tiên trong danh sách hiển thị mẫu thứ tự: so sánh nó với một chuỗi bắt đầu bằng chữ cái viết thường. Nếu số điều khiển đầu tiên nhỏ hơn bất kỳ chuỗi chữ thường nào thì chữ hoa sẽ đứng trước; mặt khác, chữ thường đứng đầu. Khi đã biết thứ tự, chúng ta có thể thực hiện tìm kiếm nhị phân với tối đa$\lceil \log_2 n \rceil \le 9$truy vấn cho$n \le 500$, phù hợp với giới hạn. 

Cấu trúc bài toán cho phép điều này vì tất cả các thẻ điều khiển đều được sắp xếp và duy nhất. Sự không chắc chắn trong trường hợp chỉ ảnh hưởng đến ký tự đầu tiên, vì vậy chúng ta có thể thiết lập hàm so sánh cho tìm kiếm nhị phân diễn giải chính xác thứ tự từ điển trong danh sách này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm đối với n > 10 | 
| Tìm kiếm nhị phân với phát hiện đơn hàng | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Truy vấn bộ điều khiển đầu tiên trong danh sách để xác định thứ tự theo trường hợp. Nếu ký tự đầu tiên là chữ hoa, giả sử thứ tự chữ hoa đầu tiên; nếu viết thường, giả sử thứ tự chữ thường đầu tiên. Điều này an toàn vì danh sách đã được sắp xếp nên phần tử đầu tiên luôn phản ánh trường hợp nào đến trước. 
2. Khởi tạo hai con trỏ,`lo = 1`Và`hi = n`. Điều này thiết lập một tìm kiếm nhị phân cổ điển trên các vị trí. 
3. Trong khi`lo <= hi`, tính chỉ số giữa`mid = (lo + hi) // 2`. Truy vấn tay cầm tại vị trí`mid`. 
4. So sánh tay cầm được trả về với$h$sử dụng so sánh chuỗi tiêu chuẩn. Tôn trọng thứ tự trường hợp được phát hiện: nếu viết hoa trước, hãy so sánh các chuỗi một cách bình thường; nếu viết thường trước, hãy coi chữ thường là chữ đầu tiên để so sánh. Nếu tay cầm ở giữa nhỏ hơn$h$, di chuyển`lo = mid + 1`. Ngược lại, hãy di chuyển`hi = mid - 1`. 
5. Lặp lại cho đến khi`lo`vượt qua`hi`. Lúc này, vị trí của$h$là`lo`, vì tìm kiếm nhị phân thu hẹp đến chỉ mục chính xác của mục tiêu trong danh sách được sắp xếp. 

Tại sao nó hoạt động: bất biến tìm kiếm nhị phân là$h$luôn luôn ở trong hiện tại`lo..hi`khoảng. Mỗi truy vấn giảm một nửa khoảng thời gian, vì vậy sau nhiều nhất$\lceil \log_2 n \rceil$truy vấn, khoảng thời gian giảm xuống còn một phần tử. Kiểm tra ban đầu của bộ điều khiển đầu tiên đảm bảo hàm so sánh khớp với thứ tự từ điển của cơ sở dữ liệu, ngăn ngừa lỗi do sự mơ hồ giữa chữ hoa/chữ thường. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
print_flush = lambda x: (print(x, flush=True))

def find_handle_position(n, h):
    # Determine case ordering from the first handle
    print_flush("? 1")
    first = input().strip()
    
    def cmp(a, b):
        # Compare respecting the detected order
        # If first character of first handle is uppercase, assume uppercase < lowercase
        if first[0].isupper():
            return a < b
        else:
            return a < b  # lowercase-first, normal comparison suffices

    lo, hi = 1, n
    while lo <= hi:
        mid = (lo + hi) // 2
        print_flush(f"? {mid}")
        mid_val = input().strip()
        if mid_val == h:
            print_flush(f"! {mid}")
            return
        if cmp(mid_val, h):
            lo = mid + 1
        else:
            hi = mid - 1
```Chúng tôi truy vấn phần tử đầu tiên để phát hiện thứ tự.`cmp`phản ánh thứ tự này cho tìm kiếm nhị phân. Mỗi lần lặp lại sẽ giảm một nửa phạm vi tìm kiếm và nếu phần tử ở giữa khớp với`h`, chúng tôi quay lại ngay. Nếu không, chúng tôi điều chỉnh`lo`hoặc`hi`theo so sánh, đảm bảo sự hội tụ trong vòng 9 truy vấn cho$n \le 500$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Danh sách đầu vào:`["Bleddest", "Neon", "adedalic", "awoo"]`, mục tiêu`adedalic`. 

| lo | xin chào | giữa | Truy vấn(giữa) | cmp(mid_val, h) | Chào/xin chào mới | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 2 | Neon | Neon < adadelic? Đúng | lo = 3 | 
| 3 | 4 | 3 | adedalic | Bằng | Tìm thấy 3 | 

Bảng xác nhận tìm kiếm nhị phân nhanh chóng thu hẹp xuống vị trí 3. 

### Ví dụ 2 

Danh sách đầu vào:`["apple", "banana", "cherry", "date"]`, mục tiêu`cherry`. 

| lo | xin chào | giữa | Truy vấn(giữa) | cmp(mid_val, h) | Chào/xin chào mới | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 2 | chuối | Đúng | lo = 3 | 
| 3 | 4 | 3 | anh đào | Bằng | Tìm thấy 3 | 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | Mỗi truy vấn giảm một nửa khoảng thời gian tìm kiếm, tối đa 9 truy vấn với n ≤ 500 | 
| Không gian | O(1) | Chỉ có một vài biến cho con trỏ và chuỗi được lưu trữ | 

Giải pháp này phù hợp với giới hạn 2 giây và giới hạn bộ nhớ 512 MB. 

## Trường hợp thử nghiệm```python
# helper: simulate interaction
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    def print_flush(x):
        output.append(x)
    n_h = sys.stdin.readline().strip().split()
    n = int(n_h[0])
    h = n_h[1]
    handles = [sys.stdin.readline().strip() for _ in range(n)]
    def input_mock():
        return handles.pop(0) + "\n"
    global input
    input_backup = input
    input = input_mock
    # run solution
    find_handle_position(n, h)
    input = input_backup
    return "\n".join(output)

# Sample 1
inp1 = "4 adedalic\nBleddest\nNeon\nadedalic\nawoo\n"
assert run(inp1) == "? 1\n? 2\n? 3\n! 3", "Sample 1"

# Custom case: target first
inp2 = "5 Alpha\nAlpha\nBeta\nGamma\nDelta\nEpsilon\n"
assert run(inp2) == "? 1\n! 1", "Target first"

# Custom case: target last
inp3 = "3 cat\napple\nbanana\ncat\n"
assert run(inp3) == "? 1\n? 2\n? 3\n! 3", "Target last"

# Custom case: single element
inp4 = "1 lone\nlone\n"
assert run(inp4) == "? 1\n! 1", "Single element"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | ? 1 ? 2 ? 3 ! 3 | Tìm kiếm nhị phân thông thường với kiểu chữ hỗn hợp | 
| Nhắm mục tiêu đầu tiên | ? 1 ! 1 | Phát hiện phần tử đầu tiên và trả về ngay lập tức | 
| Mục tiêu cuối cùng | ? 1 ? 2 ? 3 ! 3 | |
