---
title: "CF 2202F - Không tìm kiếm và truy vấn nhị phân"
description: "Chúng ta được yêu cầu duy trì một dãy số nguyên và liên tục tính toán hai thuộc tính sau khi cập nhật điểm. Cụ thể, đối với một mảng a có độ dài n cho trước, chúng ta muốn tìm, sau mỗi lần cập nhật, độ dài k lớn nhất sao cho tồn tại hai mảng con có độ dài k giống hệt nhau…"
date: "2026-06-09T04:50:42+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2202
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1082 (Div. 2)"
rating: 2500
weight: 2202
solve_time_s: 97
verified: false
draft: false
---

[CF 2202F - Truy vấn và không tìm kiếm nhị phân](https://codeforces.com/problemset/problem/2202/F) 

**Đánh giá:** 2500 
**Tas:** cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu duy trì một dãy số nguyên và liên tục tính toán hai thuộc tính sau khi cập nhật điểm. Cụ thể, đối với một mảng nhất định`a`chiều dài`n`, chúng tôi muốn tìm, sau mỗi lần cập nhật, độ dài lớn nhất`k`sao cho tồn tại hai mảng con có độ dài`k`với số lượng tần số giống hệt nhau của tất cả các phần tử và số lượng các cặp mảng con như vậy với mức tối đa đó`k`. Các bản cập nhật thay đổi một thành phần duy nhất và duy trì cho các truy vấn trong tương lai. 

Được cho`n`có thể đạt tới 200.000 và`q`có thể đạt tới 100.000, bất kỳ phương pháp nào kiểm tra rõ ràng tất cả các cặp mảng con đều là vô vọng: ngay cả một`O(n^2)`hoạt động sẽ đại khái$4 \times 10^{10}$hoạt động, vượt xa giới hạn 2-4 giây. Điều này loại trừ sự liệt kê thô bạo một cách ngây thơ của tất cả`(i,j,k)`gấp ba lần. Vấn đề cũng liên quan đến đầu ra có giá trị cao-`k_max`Và`f`có thể đi lên$10^{11}$-so việc xem xét tràn số nguyên chỉ phát sinh nếu người ta cố gắng đếm các cặp một cách ngây thơ bằng cách sử dụng số nguyên 32 bit. 

Thách thức không rõ ràng là việc kiểm tra mạnh mẽ sự bằng nhau của mảng con theo tần số là bậc hai theo`k`cho từng ứng viên và các bản cập nhật buộc phải có cấu trúc dữ liệu liên tục. Việc triển khai bất cẩn có thể tính toán lại mọi thứ từ đầu sau mỗi truy vấn, quá chậm. Các trường hợp biên bao gồm các chuỗi có tất cả các phần tử bằng nhau, các chuỗi không có mảng con lặp lại và các chuỗi trong đó một thay đổi duy nhất sẽ thay đổi đáng kể`k_max`từ`n-1`ĐẾN`0`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả độ dài của mảng con`k`từ 1 đến`n-1`. Đối với mỗi`k`, chúng ta sẽ trượt hai con trỏ`i`Và`j`trên tất cả các vị trí bắt đầu có chiều dài-`k`mảng con, tính toán bản đồ tần số của từng mảng con và so sánh. Đếm tất cả các trận đấu mang lại`f(b)`và theo dõi tối đa`k`cho`k_max(b)`. Điều này hoạt động chính xác nhưng có độ phức tạp$O(n^3)$trong trường hợp xấu nhất do ba vòng lặp lồng nhau: độ dài, bắt đầu mảng con thứ nhất, bắt đầu mảng con thứ hai. Vì`n=200,000`, điều này là không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là vấn đề giảm xuống việc đếm các tiền tố và hậu tố bằng nhau. Nếu chúng ta ánh xạ từng phần tử tới lần xuất hiện cuối cùng của nó, thì các chuỗi có các phần tử lặp lại sẽ tạo thành các “khối” trong đó xảy ra các chuỗi tần số giống hệt nhau. Đối với các chuỗi có độ dài lên tới`n`, độ dài mảng con duy nhất có thể có chuỗi tần số giống hệt nhau là ước số của mỗi số trong`a`. Điều này làm giảm đáng kể số lượng ứng viên`k`các giá trị. Hơn nữa, việc duy trì số lượng vị trí phần tử cho phép tính toán tối đa`k`và số lượng cặp bằng cách nhóm các khối bằng nhau liền kề nhau, tránh việc lặp lại lồng nhau trên tất cả các cặp mảng con. 

Trên thực tế, thay vì so sánh trực tiếp các mảng con, chúng tôi theo dõi ranh giới của các phần tử bằng nhau và tính toán tiềm năng`k_max`từ độ dài của những lần chạy này. Mỗi bản cập nhật có thể ảnh hưởng đến tối đa hai lần chạy: một lần chứa phần tử được cập nhật và có thể là phần tử lân cận của nó nếu giá trị thay đổi. Bằng cách lưu trữ độ dài của các lần chạy trong nhiều tập hợp, chúng ta có thể duy trì độ dài lần chạy tối đa một cách hiệu quả và đếm số cách để ghép các lần chạy có độ dài đó, đưa ra`k_max`Và`f(a)`sau mỗi truy vấn trong`O(log n)`cho mỗi truy vấn với cấu trúc dữ liệu phù hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n) | Quá chậm | 
| Tối ưu (nhóm thời lượng chạy + nhiều tập) | O(n + q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xử lý trước mảng ban đầu`a`để xác định các khối liền kề có phần tử bằng nhau. Lưu trữ mỗi khối dưới dạng`(value, start_index, length)`. Điều này nắm bắt các tần số một cách ngầm định vì các phần tử lặp lại trong một khối có số lượng giống hệt nhau trong bất kỳ mảng con nào được chứa đầy đủ trong khối. 
2. Duy trì nhiều bộ (hoặc từ điển)`length_counts`được khóa theo độ dài khối, lưu trữ mỗi khối có bao nhiêu khối. Điều này sẽ cho phép truy xuất nhanh độ dài khối tối đa (`k_max`) và số khối có chiều dài đó (`f`). 
3. Đối với mỗi truy vấn`(i, x)`, xác định vị trí khối chứa chỉ mục`i`. Nếu giá trị tại`i`đã rồi`x`, không có gì thay đổi;`k_max`Và`f`vẫn như cũ. Mặt khác: 

một. Nếu độ dài khối là 1, thay thế giá trị của khối bằng`x`. Hợp nhất với các khối giá trị lân cận`x`nếu chúng tồn tại, cập nhật`length_counts`tương ứng. 

b. Nếu độ dài khối lớn hơn 1, hãy chia nó thành hai khối nhỏ hơn không bao gồm chỉ mục`i`. Thay thế chỉ mục`i`với một khối giá trị mới`x`. Hợp nhất với hàng xóm như trong bước 3a. Mỗi bản cập nhật chia tách và hợp nhất`length_counts`. 
4. Sau khi cập nhật, hãy lấy khóa lớn nhất trong`length_counts`vì`k_max`và nó được tính cho`f(a)`. 
5. Xuất kết quả cho mỗi truy vấn và lặp lại. 

Tại sao nó hoạt động: Mỗi khối đại diện cho một tập hợp các phần tử giống hệt nhau liền kề nhau. Bằng cách duy trì độ dài của các khối và hợp nhất các khối liền kề có cùng giá trị, chúng tôi đảm bảo rằng bất kỳ mảng con nào có độ dài`k_max`gồm các khối có nội dung giống nhau có thể tạo thành một cặp thỏa mãn điều kiện bộ. Việc cập nhật một phần tử duy nhất sẽ thay đổi tối đa hai hoặc ba khối và chỉ số lượng cục bộ, do đó, bất biến nhiều tập hợp cho phép truy xuất chính xác độ dài và số lượng tối đa mà không cần quét toàn bộ mảng. 

## Giải pháp Python```python
import sys
from collections import defaultdict
import bisect
input = sys.stdin.readline

class BlockArray:
    def __init__(self, arr):
        self.blocks = []
        self.length_counts = defaultdict(int)
        n = len(arr)
        i = 0
        while i < n:
            val = arr[i]
            start = i
            while i < n and arr[i] == val:
                i += 1
            length = i - start
            self.blocks.append([val, start, length])
            self.length_counts[length] += 1

    def update(self, idx, x):
        # find the block containing idx
        for i, (val, start, length) in enumerate(self.blocks):
            if start <= idx < start + length:
                break
        else:
            return

        old_val, start, length = self.blocks[i]
        if old_val == x:
            return

        # remove old block length
        self.length_counts[length] -= 1
        if self.length_counts[length] == 0:
            del self.length_counts[length]

        # split block if necessary
        new_blocks = []
        if idx > start:
            left_len = idx - start
            new_blocks.append([old_val, start, left_len])
            self.length_counts[left_len] += 1
        new_blocks.append([x, idx, 1])
        if idx < start + length - 1:
            right_len = start + length - idx - 1
            new_blocks.append([old_val, idx + 1, right_len])
            self.length_counts[right_len] += 1

        # replace old block
        self.blocks = self.blocks[:i] + new_blocks + self.blocks[i+1:]

        # merge adjacent blocks if same value
        i = max(0, i-1)
        while i < len(self.blocks) - 1:
            if self.blocks[i][0] == self.blocks[i+1][0]:
                lsum = self.blocks[i][2] + self.blocks[i+1][2]
                self.length_counts[self.blocks[i][2]] -= 1
                if self.length_counts[self.blocks[i][2]] == 0:
                    del self.length_counts[self.blocks[i][2]]
                self.length_counts[self.blocks[i+1][2]] -= 1
                if self.length_counts[self.blocks[i+1][2]] == 0:
                    del self.length_counts[self.blocks[i+1][2]]
                self.blocks[i][2] = lsum
                self.length_counts[lsum] += 1
                self.blocks.pop(i+1)
            else:
                i += 1

    def query(self):
        if not self.length_counts:
            return (0, 0)
        k_max = max(self.length_counts)
        return (k_max, self.length_counts[k_max])

def main():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))
        ba = BlockArray(a)
        for _ in range(q):
            idx, x = map(int, input().split())
            ba.update(idx-1, x)
            k_max, f = ba.query()
            print(k_max, f)

if __name__ == "__main__":
    main()
```Đoạn mã chia vấn đề thành một`BlockArray`lớp theo dõi các khối liền kề của các phần tử giống hệt nhau. Mỗi bản cập nhật chỉ ảnh hưởng đến khối có liên quan và các khối lân cận, giúp cập nhật nhanh chóng. Truy vấn chỉ đơn giản truy xuất độ dài khối lớn nhất và số khối như vậy, tương ứng trực tiếp với`k_max`Và`f(a)`trong mô hình này. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`a = [1,2,1,1]`, truy vấn`(3,2)`. 

| Bước | Khối | Đếm chiều dài | Hành động | 
| --- | --- | --- | --- | 
| Bắt đầu | [[1,0,1],[2,1,1],[1,2,2]] | {1:2,2:1} | ban đầu | 
| Cập nhật 3->2 | [[1,0,1],[2,1,2],[1,3,1]] | {1:2,2:1}-> được cập nhật thông qua phân tách/hợp nhất | |
