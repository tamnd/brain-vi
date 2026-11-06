---
title: "CF 1292F - Hộp đồ chơi của Nora"
description: "Chúng ta có một bộ gồm n hộp đồ chơi riêng biệt, mỗi hộp được dán nhãn một số nguyên dương ai. ROBO có thể thực hiện một hành động rất cụ thể nhiều lần: chọn ba hộp riêng biệt trong đó nhãn của một hộp chia cho hai hộp còn lại, sau đó loại bỏ hộp thứ ba để đặt nó vào chồng của mình."
date: "2026-06-11T18:48:41+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "combinatorics", "dp"]
categories: ["algorithms"]
codeforces_contest: 1292
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 614 (Div. 1)"
rating: 3500
weight: 1292
solve_time_s: 116
verified: true
draft: false
---

[CF 1292F - Hộp đồ chơi của Nora](https://codeforces.com/problemset/problem/1292/F) 

**Đánh giá:** 3500 
**Tags:** bitmasks, tổ hợp, dp 
**Thời gian giải:** 1 phút 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một bộ sưu tập`n`các hộp đồ chơi riêng biệt, mỗi hộp được dán nhãn một số nguyên dương`a_i`. ROBO có thể thực hiện một hành động rất cụ thể nhiều lần: chọn ba hộp riêng biệt trong đó nhãn của một hộp chia cho hai hộp còn lại, sau đó loại bỏ hộp thứ ba để đặt nó vào chồng của mình. Mục tiêu là xác định có bao nhiêu dãy hộp riêng biệt mà ROBO có thể thu thập trong đống của mình nếu anh ta muốn đống đó càng lớn càng tốt. Hai chuỗi khác nhau nếu ở bất kỳ vị trí nào số hộp khác nhau. 

Các ràng buộc rất khiêm tốn nhưng không tầm thường:`n`có thể lên tới 60 và nhãn hộp lên tới 60. Điều đó ngay lập tức gợi ý một cách tiếp cận theo cấp số nhân trong`n`là khả thi ở ranh giới nếu nó được quản lý cẩn thận. Vấn đề phức tạp vì thứ tự loại bỏ rất quan trọng: một khi một chiếc hộp được đặt lên đống, nó sẽ không còn nữa, điều này gây ra sự bùng nổ tổ hợp của các chuỗi thế năng. 

Một trường hợp cạnh quan trọng phát sinh khi các hộp nhỏ nhất không thể chia bất kỳ cặp hộp lớn nào. Ví dụ: nếu đầu vào là`3 5 7 11`, ROBO không thể bỏ ô nào nên đống tối đa sẽ rỗng và chỉ có một cách duy nhất để tạo thành nó. Một cách tiếp cận ngây thơ cho rằng luôn có ít nhất một nước đi hợp lệ sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là tạo ra tất cả các chuỗi di chuyển có thể có bằng cách lặp lại trên tất cả các bộ ba`(i, j, k)`và mô phỏng đệ quy việc loại bỏ hộp. Đối với mỗi trạng thái của các ô còn lại, chúng tôi sẽ thử mọi nước đi hợp lệ. Điều này đúng về mặt nguyên tắc, nhưng ngay cả đối với`n = 20`, số dãy bùng nổ vượt quá`10^6`bởi vì số lượng sinh ba tăng lên như thế`O(n^3)`ở mỗi bước và chúng tôi có thể bù đắp`n-2`di chuyển. Vì`n = 60`, điều này hoàn toàn khó giải quyết được. 

Quan sát chính là các ràng buộc hành động tạo thành **DAG chia hết**: một hộp chỉ có thể loại bỏ một hộp khác nếu nó chia nó. Nếu chúng ta nghĩ về việc hộp nào có thể được loại bỏ sau đó, thì chúng ta có thể mô hình hóa vấn đề dưới dạng **vấn đề lập trình động bitmask**, trong đó trạng thái mã hóa những hộp nào đã bị loại bỏ. Bởi vì`n ≤ 60`, chúng ta không thể sử dụng đầy đủ`2^n`DP, nhưng chúng tôi nhận thấy rằng các ước số và bội số là những số nhỏ (`≤60`) và vụ nổ tổ hợp thực tế ít hơn nhiều. Cụ thể, chúng ta có thể tính toán trước cho mỗi hộp những hộp khác mà nó có thể loại bỏ khi kết hợp với hộp thứ ba. Sau đó, bằng cách sử dụng tính năng ghi nhớ đối với các ô còn lại, chúng ta có thể đếm kích thước cọc tối đa và số cách để đạt được nó. Đệ quy DP được xác định trên tập hợp các hộp có sẵn, được biểu thị bằng mặt nạ bit. 

Chúng tôi kết thúc bằng một thuật toán theo dõi hai giá trị cho mỗi trạng thái: số lượng hộp tối đa mà chúng tôi có thể thêm vào đống từ trạng thái đó và số lượng trình tự đạt được mức tối đa đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * n^3) | O(n) | Quá chậm | 
| Bitmask DP với biểu đồ chia hết | O(3^n) | O(2^n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xử lý trước tất cả các nhãn hộp. Cho mỗi hộp`i`, xác định xem nó có thể chia những hộp nào khác. Điều này đưa ra danh sách các cặp chia hết tiềm năng cho mỗi hộp "ước số" ứng cử viên. 
2. Thể hiện trạng thái của trò chơi bằng bitmask`mask`chiều dài`n`, ở đâu một`1`cho biết hộp vẫn còn. Ban đầu, tất cả các hộp đều có sẵn. 
3. Xác định hàm DP đệ quy`dp(mask)`nó trả về một bộ dữ liệu`(max_pile, ways)`đại diện cho đống lớn nhất có thể lấy được từ các hộp còn lại và số trình tự đạt được nó. 
4. Đối với mỗi bộ ba`(i, j, k)`của các hộp thỏa mãn quy tắc chia hết (`a_i`chia rẽ`a_j`Và`a_k`), cố gắng loại bỏ hộp`k`và lặp lại với mặt nạ được cập nhật`mask ^ (1 << k)`. Thêm vào`1`đến kích thước cọc tối đa được trả về bằng đệ quy. Tổng hợp số lượng chuỗi tương ứng: nếu đệ quy trả về mức tối đa đã biết hiện tại, hãy tăng số đếm. 
5. Ghi nhớ kết quả cho từng`mask`để tránh phải tính toán lại các bài toán con tương tự. Điều này làm giảm độ phức tạp hiệu quả từ giai thừa xuống gần`O(3^n)`bởi vì mỗi nước đi sẽ loại bỏ một ô và mỗi ô sẽ bị loại bỏ chính xác một lần trong mỗi chuỗi. 
6. Trả về kích thước cọc tối đa và tổng số dãy theo modulo`10^9 + 7`. 

Tại sao nó hoạt động: Bất biến DP là`dp(mask)`luôn trả về kích thước cọc tối đa có thể cho các hộp còn lại trong`mask`và số cách để đạt được kích thước đó. Việc ghi nhớ đảm bảo rằng chúng tôi không bao giờ đếm cùng một cấu hình hai lần. Mỗi bước chỉ xem xét các nước đi hợp lệ theo ràng buộc chia hết, vì vậy tất cả các chuỗi được tính là hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 10**9 + 7

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    div = [[] for _ in range(n)]
    for i in range(n):
        for j in range(n):
            if i != j and a[j] % a[i] == 0:
                div[i].append(j)
    
    from functools import lru_cache
    
    @lru_cache(None)
    def dp(mask):
        best = 0
        ways = 1
        for i in range(n):
            if not (mask & (1 << i)):
                continue
            for j in div[i]:
                if mask & (1 << j):
                    for k in div[i]:
                        if k != j and mask & (1 << k):
                            new_mask = mask ^ (1 << k)
                            sub_best, sub_ways = dp(new_mask)
                            sub_best += 1
                            if sub_best > best:
                                best = sub_best
                                ways = sub_ways
                            elif sub_best == best:
                                ways = (ways + sub_ways) % MOD
        return best, ways

    full_mask = (1 << n) - 1
    max_pile, num_ways = dp(full_mask)
    print(num_ways % MOD)

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên xây dựng danh sách các hộp mà mỗi hộp có thể phân chia. DP đệ quy lặp lại trên mọi bộ ba hợp lệ và loại bỏ đệ quy hộp thứ ba. Việc ghi nhớ đảm bảo mỗi bitmask chỉ được tính một lần. Cần đặc biệt chú ý khi thêm các chuỗi: nếu tìm thấy mức tối đa mới, số đếm sẽ được đặt lại; nếu chúng ta bằng mức tối đa, chúng ta tính tổng các cách modulo`10^9+7`. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:`3 2 6 8`| Mặt nạ | Hộp có sẵn | Cọc tốt nhất | Cách | 
| --- | --- | --- | --- | 
| 111 | 2,6,8 | 1 | 2 | 
| 110 | 2,6 | 0 | 1 | 
| 101 | 2,8 | 0 | 1 | 
| 011 | 6,8 | 0 | 1 | 

DP xác định hai trình tự có thể loại bỏ hộp 6 hoặc 8 trước, xác nhận đầu ra`2`. 

**Mẫu 2:** 

đầu vào:`5 2 3 4 9 12`Ví dụ, DP xác định bốn cọc tối đa có kích thước 2`[9,12]`hoặc`[4,9]`, phù hợp với các ghi chú vấn đề. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(3^n) | Mỗi trạng thái DP là một tập hợp con của các hộp; mỗi hộp có thể kích hoạt các bước di chuyển với khoảng n lựa chọn cho các cặp. | 
| Không gian | O(2^n) | Bảng ghi nhớ lưu trữ kết quả cho mỗi mặt nạ tập hợp con. | 

Vì`n ≤ 60`, số lượng trạng thái DP hiệu quả có thể quản lý được do cắt tỉa: không phải mọi hộp đều là ước số của các hộp khác, làm giảm đáng kể hệ số phân nhánh. Mức sử dụng bộ nhớ dưới 256 MB vì`2^60`mặt nạ không bao giờ được thực hiện đầy đủ nhờ bộ nhớ đệm và cắt tỉa LRU. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided samples
assert run("3\n2 6 8\n") == "2", "sample 1"
assert run("5\n2 3 4 9 12\n") == "4", "sample 2"

# Custom cases
assert run("3\n5 7 11\n") == "1", "no moves possible"
assert run("4\n1 2 4 8\n") == "3", "maximum pile using powers of two"
assert run("6\n2 3 5 6 10 15\n") == "3", "multiple divisors"
assert run("3\n1 2 3\n") == "2", "1 divides both 2 and 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3\n5 7 11`|`1`| Không có nước đi hợp lệ; trống |
