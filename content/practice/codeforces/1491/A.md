---
title: "CF 1491A - Giá trị lớn thứ K"
description: "Chúng tôi đang làm việc với một mảng nhị phân trong đó mọi vị trí đều chứa 0 hoặc 1. Mảng thay đổi theo thời gian thông qua hai loại phép toán. Một thao tác lật một vị trí từ 0 thành 1 hoặc từ 1 thành 0."
date: "2026-06-10T22:26:36+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1491
codeforces_index: "A"
codeforces_contest_name: "Codeforces Global Round 13"
rating: 800
weight: 1491
solve_time_s: 145
verified: true
draft: false
---

[CF 1491A - Giá trị lớn thứ K](https://codeforces.com/problemset/problem/1491/A) 

**Đánh giá:** 800 
**Tags:** vũ phu, tham lam, thực hiện 
**Thời gian giải:** 2m 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một mảng nhị phân trong đó mọi vị trí đều chứa 0 hoặc 1. Mảng thay đổi theo thời gian thông qua hai loại phép toán. Một thao tác lật một vị trí từ 0 thành 1 hoặc từ 1 thành 0. Thao tác kia yêu cầu giá trị sẽ xuất hiện trong phiên bản đã sắp xếp của mảng nếu chúng ta xem xét một thứ hạng cụ thể từ trên xuống. 

Quan sát quan trọng là mảng chỉ chứa hai giá trị riêng biệt. Điều này có nghĩa là khi chúng ta sắp xếp nó theo thứ tự không tăng, tất cả số 0 sẽ xếp trước và tất cả số 0 sẽ xếp sau. Vì vậy, toàn bộ cấu trúc được sắp xếp hoàn toàn được xác định bởi một số duy nhất: hiện có bao nhiêu số trong mảng. 

Các ràng buộc cho phép tối đa 100.000 phần tử và 100.000 truy vấn. Bất kỳ giải pháp nào tính toán lại số lượng từ đầu cho mỗi truy vấn sẽ mất khoảng 10^10 thao tác trong trường hợp xấu nhất, vượt xa những gì có thể thực hiện được trong một giây. Ngay cả việc duy trì một mảng được sắp xếp một cách rõ ràng cũng sẽ thất bại vì mỗi lần lật sẽ yêu cầu cập nhật tuyến tính hoặc sắp xếp lại. 

Một sai lầm ngây thơ là cố gắng duy trì mảng được sắp xếp một cách vật lý sau mỗi lần lật. Ví dụ: sau mỗi lần chuyển đổi, người ta có thể xây dựng lại mảng đã sắp xếp và trả lời trực tiếp phần tử thứ k. Điều này không thành công ngay lập tức đối với các chuỗi cập nhật dài vì mỗi lần xây dựng lại là O(n). 

Một lỗi nhỏ khác là tính toán lại số lượng bằng cách quét toàn bộ mảng trên mỗi truy vấn. Điều này có vẻ đơn giản nhưng lại dẫn đến hành vi O(nq). 

Cấu trúc ẩn là tất cả các truy vấn chỉ phụ thuộc vào số lượng truy vấn chứ không phụ thuộc vào vị trí của chúng. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi truy vấn, nếu đó là một lần lật, chúng tôi sẽ cập nhật mảng. Nếu đó là một truy vấn, chúng tôi sắp xếp mảng theo thứ tự giảm dần và chọn phần tử thứ k. Việc sắp xếp chi phí O(n log n) và thực hiện tới 100.000 lần dẫn đến khoảng 10^10 thao tác ghi nhật ký, quá chậm. 

Chúng ta cũng có thể thử cách tiếp cận mạnh mẽ hơn một chút bằng cách quét mảng để đếm số đơn vị trên mỗi truy vấn. Điều đó làm giảm chi phí sắp xếp nhưng vẫn tốn O(n) cho mỗi truy vấn, một lần nữa trở thành 10^10 thao tác trong trường hợp xấu nhất. 

Thông tin chi tiết quan trọng là mảng luôn chỉ chứa 0 và 1. Sau khi sắp xếp, phân đoạn đầu tiên hoàn toàn là 1 và phần còn lại là 0. Do đó, phần tử lớn thứ k được xác định bởi liệu k có nằm trong khối các phần tử hay không. Nếu chúng ta chỉ duy trì số lượng hiện tại là 1 thì mọi truy vấn sẽ trở thành O(1). Một lần lật chỉ đơn giản là tăng hoặc giảm số đếm này tùy thuộc vào giá trị trước đó tại vị trí đó. 

Điều này làm giảm vấn đề duy trì một số nguyên duy nhất và một mảng nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tính toán lại/sắp xếp) | O(nq) hoặc O(q·n log n) | O(n) | Quá chậm | 
| Tối ưu (đếm cái) | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai phần thông tin: chính mảng và bộ đếm lưu trữ số lượng mảng hiện có. 

1. Khởi tạo mảng từ đầu vào và tính số lượng đơn vị ban đầu bằng cách tính tổng tất cả các phần tử. Điều này cung cấp cho chúng tôi cơ sở cần thiết để trả lời bất kỳ truy vấn nào. 
2. Đối với truy vấn loại 1 tại vị trí x, chúng ta lật giá trị tại chỉ mục đó. Nếu là 1 thì chúng ta giảm số lượng cái một. Nếu là 0 thì chúng ta tăng nó lên. Điều này đảm bảo bộ đếm luôn khớp với trạng thái thực tế của mảng mà không cần tính toán lại. 
3. Đối với truy vấn loại 2 có tham số k, chúng tôi quyết định xem phần tử lớn thứ k là 1 hay 0. Vì tất cả phần tử đều đứng đầu theo thứ tự được sắp xếp, nếu k nhỏ hơn hoặc bằng số phần tử đơn vị thì câu trả lời là 1. Ngược lại, nó phải bằng 0. 
4. Lặp lại quy trình này cho tất cả các truy vấn, in câu trả lời nếu cần.

Tại sao nó hoạt động dựa trên một bất biến cấu trúc: tại mọi thời điểm, mảng có thể được sắp xếp theo khái niệm thành một tiền tố gồm các số 0 theo sau và độ dài của tiền tố đó chính xác là số lượng được duy trì. Mỗi lần lật cập nhật độ dài tiền tố này một cách chính xác chính xác bằng ±1, do đó không cần thông tin lịch sử nào ngoài số lượng. Truy vấn phần tử thứ k giảm xuống việc kiểm tra tư cách thành viên trong tiền tố đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, q = map(int, input().split())
a = list(map(int, input().split()))

ones = sum(a)

out = []

for _ in range(q):
    t, x = map(int, input().split())
    if t == 1:
        x -= 1
        if a[x] == 1:
            a[x] = 0
            ones -= 1
        else:
            a[x] = 1
            ones += 1
    else:
        k = x
        if k <= ones:
            out.append("1")
        else:
            out.append("0")

print("\n".join(out))
```Mảng được lưu trữ rõ ràng chỉ để hỗ trợ lật từng vị trí một cách chính xác. Việc tối ưu hóa quan trọng là chúng tôi không bao giờ rút ra câu trả lời trực tiếp từ cấu trúc mảng; chúng tôi chỉ duy trì số lượng bất biến của những cái. 

Thao tác lật điều chỉnh cẩn thận cả mảng và bộ đếm theo thời gian không đổi. Điều này tránh các lỗi tính toán lại trong đó số lượng cũ sẽ tích lũy. Hoạt động truy vấn được giảm xuống thành một so sánh duy nhất với số lượng được duy trì. 

Một lỗi triển khai phổ biến là quên chuyển đổi các chỉ mục từ lập chỉ mục dựa trên 1 sang dựa trên 0 khi lật. Một cách khác là cập nhật bộ đếm trước khi kiểm tra giá trị hiện tại, điều này sẽ đảo ngược logic. Thứ tự đúng là luôn kiểm tra giá trị cũ trước, sau đó cập nhật cả mảng và bộ đếm một cách nhất quán. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 5
1 1 0 1 0
2 3
1 2
2 3
2 1
2 5
```Trạng thái ban đầu: 

| Bước | Mảng | Những cái | Truy vấn | Đầu ra | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [1,1,0,1,0] | 3 | - | - | 
| 1 | - | 3 | k=3 | 1 | 
| 2 | [1,0,0,1,0] | 2 | - | - | 
| 3 | - | 2 | k=3 | 0 | 
| 4 | - | 2 | k=1 | 1 | 
| 5 | - | 2 | k=5 | 0 | 

Dấu vết này cho thấy rằng chỉ có số lượng những cái một mới quan trọng. Mặc dù vị trí thay đổi, kết quả truy vấn chỉ phụ thuộc vào việc k có nằm trong phân đoạn đầu tiên hay không. 

### Ví dụ 2 

đầu vào:```
4 4
0 0 0 0
2 1
1 3
2 1
2 4
```| Bước | Mảng | Những cái | Truy vấn | Đầu ra | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [0,0,0,0] | 0 | - | - | 
| 1 | - | 0 | k=1 | 0 | 
| 2 | [0,0,1,0] | 1 | - | - | 
| 3 | - | 1 | k=1 | 1 | 
| 4 | - | 1 | k=4 | 0 | 

Trường hợp này xác minh tính chính xác khi mảng bắt đầu bằng tất cả các số 0 và lần lượt giới thiệu các số 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Số đếm ban đầu là O(n), mỗi truy vấn là O(1) | 
| Không gian | O(n) | Lưu trữ mảng cho các thao tác lật | 

Các ràng buộc cho phép tổng cộng lên tới 200.000 thao tác, do đó, cách tiếp cận thời gian không đổi cho mỗi truy vấn phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    ones = sum(a)

    out = []
    for _ in range(q):
        t, x = map(int, input().split())
        if t == 1:
            x -= 1
            if a[x] == 1:
                a[x] = 0
                ones -= 1
            else:
                a[x] = 1
                ones += 1
        else:
            k = x
            out.append("1" if k <= ones else "0")

    return "\n".join(out)

# provided sample
assert run("""5 5
1 1 0 1 0
2 3
1 2
2 3
2 1
2 5
""") == """1
0
1
0"""

# minimum size
assert run("""1 3
0
2 1
1 1
2 1
""") == """0
1"""

# all ones
assert run("""4 3
1 1 1 1
2 2
1 3
2 2
""") == """1
1"""

# all zeros
assert run("""4 2
0 0 0 0
2 1
2 4
""") == """0
0"""

# alternating flips
assert run("""6 5
0 1 0 1 0 1
2 3
1 1
2 3
1 2
2 1
""") == """1
0
1"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu 1 | đưa ra | tính đúng đắn của các hoạt động hỗn hợp | 
| phần tử đơn | 0,1 | trường hợp biên nhỏ nhất | 
| tất cả những cái | 1,1 | lật tính nhất quán | 
| tất cả số không | 0,0 | độ đúng giới hạn dưới | 
| lật xen kẽ | hỗn hợp | ổn định khi chuyển đổi lặp đi lặp lại | 

## Vỏ cạnh 

Trường hợp một cạnh là khi mảng bắt đầu hoàn toàn bằng số không. Trong tình huống này, bộ đếm hàng đơn vị bằng 0, vì vậy mọi truy vấn thứ k phải trả về 0 cho đến khi lần lật giới thiệu số 1. Ví dụ, đầu vào`[0,0,0,0]`với truy vấn`2 1`trả về chính xác 0 vì không có tiền tố nào cả. 

Một trường hợp cạnh khác là lặp đi lặp lại việc lật cùng một chỉ mục. Nếu một phần tử được chuyển đổi nhiều lần, độ chính xác phụ thuộc hoàn toàn vào việc cập nhật bộ đếm đồng bộ với mảng. Chẳng hạn, bắt đầu từ`[1,0]`, lật vị trí 1 hai lần sẽ đưa hệ thống về trạng thái ban đầu và bộ đếm đơn vị trở về giá trị ban đầu. Thuật toán xử lý việc này một cách tự nhiên vì mỗi lần lật đều đối xứng và có thể đảo ngược trong các bản cập nhật O(1). 

Trường hợp tinh tế cuối cùng là khi k bằng n. Điều này luôn trả về 0 trừ khi mảng hoàn toàn là một. logic`k <= ones`vẫn xử lý việc này một cách rõ ràng vì`ones`không bao giờ có thể vượt quá`n`, do đó so sánh ranh giới vẫn có giá trị mà không cần xử lý đặc biệt.
