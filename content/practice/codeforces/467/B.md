---
title: "CF 467B - Fedor và trò chơi mới"
description: "Chúng ta được cho một trò chơi với nhiều người chơi, mỗi người điều khiển một đội quân gồm nhiều loại lính khác nhau. Mỗi đội quân được biểu diễn dưới dạng một số nguyên không âm trong đó biểu diễn nhị phân mã hóa sự hiện diện của các loại binh sĩ: bit được đặt thành 1 có nghĩa là quân đội bao gồm loại đó."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "constructive-algorithms", "implementation"]
categories: ["algorithms"]
codeforces_contest: 467
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 267 (Div. 2)"
rating: 1100
weight: 467
solve_time_s: 79
verified: true
draft: false
---

[CF 467B - Fedor và trò chơi mới](https://codeforces.com/problemset/problem/467/B) 

**Đánh giá:** 1100 
**Tags:** bitmasks, vũ phu, thuật toán xây dựng, triển khai 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một trò chơi với nhiều người chơi, mỗi người điều khiển một đội quân gồm nhiều loại lính khác nhau. Mỗi đội quân được biểu diễn dưới dạng một số nguyên không âm trong đó biểu diễn nhị phân mã hóa sự hiện diện của các loại binh sĩ: bit được đặt thành 1 có nghĩa là quân đội bao gồm loại đó. Fedor là người chơi cuối cùng và chúng ta được yêu cầu xác định xem có bao nhiêu người chơi khác có quân đội khác với anh ta về nhiều nhất _k_ loại binh lính. Nói cách khác, khoảng cách Hamming giữa quân của Fedor và quân của người chơi khác tối đa là _k_. 

Dữ liệu đầu vào bao gồm _n_, tổng số loại lính, _m_, số lượng người chơi khác và _k_, ngưỡng tình bạn đối với các loại lính khác nhau. Mỗi đội quân sau đó được liệt kê dưới dạng một con số, bao gồm cả đội quân của Fedor ở cuối. Đầu ra là một số nguyên duy nhất, đếm những người bạn tiềm năng của Fedor. 

Các ràng buộc cung cấp cho chúng tôi _n_ tối đa 20, _m_ lên tới 1000 và _k_ lên đến _n_. Với _n_ 20, mỗi đội quân có thể được biểu diễn dưới dạng số 20 bit, do đó các thao tác trên từng bit riêng lẻ sẽ nhanh chóng. Giới hạn trên của _m_ có nghĩa là chúng ta có thể kiểm tra từng người chơi khác một cách riêng lẻ. Một cách tiếp cận đơn giản so sánh mỗi người chơi với Fedor bằng cách sử dụng các thao tác bitwise sẽ thực hiện kiểm tra tối đa 20 × 1000 = 20.000 bit, cũng nằm trong giới hạn thời gian. Các trường hợp đặc biệt bao gồm khi _k_ = 0, trong đó chỉ có quân đội giống hệt với số lượng của Fedor và khi _k_ = _n_, trong đó mọi người chơi đều là bạn bất kể sự khác biệt. 

Việc triển khai bất cẩn có thể cố gắng so sánh trực tiếp các số thập phân thay vì biểu diễn nhị phân của chúng hoặc sử dụng so sánh nhị phân dựa trên chuỗi. Ví dụ: nếu quân đội của Fedor là 15 (1111 ở dạng nhị phân) và một người chơi khác có 14 (1110 ở dạng nhị phân), so sánh thập phân ngây thơ sẽ bỏ sót rằng chúng khác nhau ở chính xác một loại, điều này được phép nếu _k_ ≥ 1. 

## Phương pháp tiếp cận 

Giải pháp đơn giản nhất là dùng vũ lực: đối với mỗi người chơi _m_ đầu tiên, hãy tính số lượng bit khác nhau giữa quân đội của họ và quân đội của Fedor. Điều này tương đương với việc lấy XOR theo bit của hai số và đếm số 1 trong kết quả. Mỗi số 1 tương ứng với một loại lính mà quân đội khác nhau. Nếu số lượng nhiều nhất là _k_, hãy tăng bộ đếm tình bạn. 

Lực lượng vũ phu này hoạt động vì độ dài bit tối đa _n_ nhỏ, do đó việc đếm các bit khác nhau cho mỗi người chơi sẽ rẻ. Không cần các kỹ thuật nâng cao hơn như tiền tính toán hoặc tối ưu hóa tổ hợp. Thông tin chuyên sâu quan trọng giúp đơn giản hóa việc triển khai là sử dụng thao tác XOR, thao tác này chuyển đổi vấn đề đếm các loại lính khác nhau thành việc đếm số 1 trong một số nguyên duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m × n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(m × n) | O(1) | Đã chấp nhận | 

“Tối ưu” ở đây về cơ bản giống như brute-force, nhưng cái nhìn sâu sắc là nhận ra hoạt động XOR và đếm bit là một cách rõ ràng, hiệu quả để triển khai nó. 

## Hướng dẫn thuật toán 

1. Đọc các số nguyên _n_, _m_ và _k_ từ đầu vào. Những điều này xác định số lượng loại lính, số lượng người chơi khác và sự khác biệt tối đa được phép đối với tình bạn. 
2. Đọc các dòng _m + 1_ tiếp theo thành một dãy số nguyên biểu thị các quân đội. Mục cuối cùng trong mảng này là quân đội của Fedor. 
3. Khởi tạo bộ đếm`friends = 0`để theo dõi những người bạn tiềm năng. 
4. Đối với mỗi đội quân _m_ đầu tiên, hãy tính XOR theo bit với đội quân của Fedor. Điều này tạo ra một số nguyên trong đó mỗi bit được đặt thành 1 đại diện cho một loại binh sĩ khác nhau giữa hai đội quân. 
5. Đếm số 1 trong kết quả XOR. Điều này có thể được thực hiện bằng cách sử dụng tính năng tích hợp sẵn`bin(x ^ y).count('1')`bằng Python. Nếu số này nhỏ hơn hoặc bằng _k_, hãy tăng giá trị`friends`quầy tính tiền. 
6. Sau khi lặp qua tất cả những người chơi khác, hãy in`friends`quầy tính tiền. 

Tại sao nó hoạt động: XOR tách biệt chính xác các loại lính trong đó các đội quân khác nhau. Đếm 1 giây trong kết quả XOR sẽ cho ra khoảng cách Hamming. Vì tình bạn được xác định bằng cách có nhiều nhất _k_ sự khác biệt, nên việc so sánh số lượng này với _k_ sẽ xác định chính xác những người bạn tiềm năng. Mỗi người chơi được xem xét độc lập và không có giá trị gần đúng nào, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, k = map(int, input().split())
armies = [int(input()) for _ in range(m + 1)]
fedor = armies[-1]

friends = 0
for i in range(m):
    diff = bin(armies[i] ^ fedor).count('1')
    if diff <= k:
        friends += 1

print(friends)
```Giải pháp đầu tiên đọc đầu vào một cách hiệu quả bằng cách sử dụng`sys.stdin.readline`để xử lý _m_ lớn hơn. Các giá trị quân đội được lưu trữ trong một danh sách để dễ dàng truy cập và quân đội của Fedor được truy cập ở phần tử cuối cùng. Vòng lặp chỉ lặp lại trên _m_ người chơi đầu tiên, đảm bảo chúng ta không so sánh Fedor với chính anh ta. Hoạt động XOR cung cấp một cách ngắn gọn để nắm bắt sự khác biệt và đếm số bit thông qua`bin(...).count('1')`tận dụng biểu diễn nhị phân tích hợp của Python. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
7 3 1
8
5
111
17
```| Người chơi | Quân đội | XOR với Fedor | Đếm 1 giây | Bạn bè? | 
| --- | --- | --- | --- | --- | 
| 1 | 8 | 8 ^ 17 = 25 | 3 | Không | 
| 2 | 5 | 5 ^ 17 = 20 | 2 | Không | 
| 3 | 111 | 111^17 = 126 | 6 | Không |`friends = 0`, phù hợp với đầu ra dự kiến. 

**Mẫu tùy chỉnh 2** 

đầu vào:```
4 3 2
3
5
6
7
```| Người chơi | Quân đội | XOR với Fedor | Đếm 1 giây | Bạn bè? | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 3 ^ 7 = 4 | 1 | Có | 
| 2 | 5 | 5 ^ 7 = 2 | 1 | Có | 
| 3 | 6 | 6 ^ 7 = 1 | 1 | Có | 

Cả ba đều là bạn bè,`friends = 3`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m × n) | Mỗi người trong số m người chơi được so sánh với Fedor bằng XOR và đếm tối đa n bit. | 
| Không gian | O(m) | Lưu trữ danh sách quân đội của m+1 người chơi. | 

Với _m_ 1000 và _n_ 20, tối đa 20.000 bit so sánh được thực hiện, tức là trong giới hạn thời gian 1 giây. Việc sử dụng bộ nhớ là không đáng kể đối với các kích thước đầu vào này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m, k = map(int, input().split())
    armies = [int(input()) for _ in range(m + 1)]
    fedor = armies[-1]
    friends = 0
    for i in range(m):
        if bin(armies[i] ^ fedor).count('1') <= k:
            friends += 1
    return str(friends)

# provided sample
assert run("7 3 1\n8\n5\n111\n17\n") == "0", "sample 1"

# minimum input
assert run("1 1 1\n1\n1\n") == "1", "minimum input"

# all equal armies
assert run("5 3 5\n31\n31\n31\n31\n") == "3", "all equal"

# maximum difference allowed
assert run("4 2 4\n1\n2\n15\n") == "2", "maximum difference"

# no friends
assert run("3 2 0\n1\n2\n3\n") == "0", "no friends"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 7 3 1 … | 0 | Mẫu được cung cấp | 
| 1 1 1 … | 1 | Kích thước đầu vào tối thiểu | 
| 5 3 5 … | 3 | Tất cả quân đội đều giống nhau, k = n | 
| 4 2 4 … | 2 | Chênh lệch tối đa cho phép của k | 
| 3 2 0 … | 0 | k = 0, chỉ tính quân giống nhau | 

## Vỏ cạnh 

Khi _k_ = 0, chỉ những đội quân giống hệt Fedor mới là bạn. Đối với đầu vào:```
3 2 0
5
6
5
```Kết quả XOR: 5^5 = 0, 6^5 = 3. Số 1: 0 và 2. Chỉ người chơi đầu tiên là bạn bè nên đầu ra là 1. Thuật toán nắm bắt chính xác điều này bằng cách so sánh số bit với _k_. 

Khi _k_ = n, mọi người chơi đều là bạn. Ví dụ:```
3 2
```
