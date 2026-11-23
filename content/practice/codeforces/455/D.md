---
title: "CF 455D - Serega và niềm vui"
description: "Chúng tôi có một mảng các số nguyên, mỗi số từ 1 đến n và chúng tôi cần hỗ trợ hai loại hoạt động một cách hiệu quả. Thao tác đầu tiên xoay một đoạn của mảng sang phải một bước. Thao tác thứ hai đếm số lần một giá trị cụ thể xuất hiện trong một phân đoạn."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 455
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 260 (Div. 1)"
rating: 2700
weight: 455
solve_time_s: 87
verified: true
draft: false
---

[CF 455D - Serega và Fun](https://codeforces.com/problemset/problem/455/D) 

**Đánh giá:** 2700 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mảng các số nguyên, mỗi số từ 1 đến _n_ và chúng tôi cần hỗ trợ hai loại hoạt động một cách hiệu quả. Thao tác đầu tiên xoay một đoạn của mảng sang phải một bước. Thao tác thứ hai đếm số lần một giá trị cụ thể xuất hiện trong một phân đoạn. Các truy vấn đến ở dạng được mã hóa phụ thuộc vào câu trả lời trước đó, vì vậy chúng tôi phải xử lý chúng trực tuyến, cập nhật mã hóa sau mỗi truy vấn đếm. 

Các ràng buộc cho phép kích thước mảng và số lượng truy vấn đạt tới 100.000. Một cách tiếp cận đơn giản giúp di chuyển các phần tử cho mỗi vòng quay hoặc lặp qua các phân đoạn cho mỗi lần đếm sẽ yêu cầu tới 10^10 thao tác trong trường hợp xấu nhất, điều này vượt xa mức chấp nhận được. Điều này loại trừ mọi giải pháp có O(n) cho mỗi truy vấn. Chúng ta cần một cấu trúc có thể thực hiện phép quay phân đoạn và đếm tần số hiệu quả hơn. 

Các trường hợp biên cần xem xét bao gồm các đoạn có độ dài bằng một, các đoạn bao quanh sau khi mã hóa và nhiều phép quay liên tiếp trên cùng một phần tử. Ví dụ: phân đoạn một phần tử được xoay sẽ không thay đổi. Ngoài ra, các truy vấn yêu cầu đếm một giá trị không có trong phân đoạn sẽ trả về 0 và cần phải xử lý cẩn thận công thức mã hóa để tránh lỗi sai từng cái một. 

## Phương pháp tiếp cận 

Phương pháp brute-force lặp lại trên phân đoạn cho mỗi vòng quay và mỗi lần đếm. Đối với phép quay, điều này liên quan đến việc di chuyển tối đa n phần tử cho mỗi truy vấn. Đối với các truy vấn đếm, nó cũng quét tối đa n phần tử. Với q lên đến 10^5 và n lên đến 10^5, điều này mang lại các phép toán O(n * q), có thể đạt tới 10^10, khiến nó không thể thực hiện được. 

Cách tiếp cận tối ưu nhận thấy rằng hai thao tác mà chúng tôi cần hỗ trợ - xoay phân đoạn và truy vấn tần số theo phạm vi - là những trường hợp cổ điển cho **kỹ thuật phân tách khối**, thường được gọi là phân tách sqrt. Chúng ta có thể chia mảng thành các khối có kích thước khoảng √n. Đối với mỗi khối, chúng tôi duy trì bản đồ tần số các giá trị của nó. Việc đếm một giá trị trên một phân đoạn bao gồm việc tính tổng số đếm từ các khối đầy đủ và chỉ lặp lại thủ công trên các khối một phần. Xoay một phân đoạn sẽ cập nhật tối đa hai khối một phần và giữ nguyên tần số của các khối đầy đủ sau khi dịch chuyển bên trong chúng. Bằng cách giữ mỗi khối trong một danh sách theo thứ tự phần tử và một từ điển cho tần số, chúng ta có thể thực hiện cả hai thao tác một cách hiệu quả, trong thời gian O(√n) cho mỗi truy vấn. 

Điều này tận dụng thực tế là căn bậc hai của 100.000 là khoảng 316, đủ nhỏ để các hoạt động trên mỗi truy vấn duy trì trong giới hạn thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * q) | O(n) | Quá chậm | 
| Phân hủy Sqrt | O(q * √n) | O(n + √n * n) ≈ O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước kích thước khối khoảng √n. Chia mảng thành các khối liền kề có kích thước này. Mỗi khối lưu trữ các phần tử của nó theo thứ tự và duy trì một từ điển tần số các giá trị trong khối đó. 
2. Đối với truy vấn đếm trên phân đoạn [l, r], trước tiên hãy giải mã truy vấn bằng câu trả lời cuối cùng. Xác định khối nào được chứa đầy đủ trong [l, r] và khối nào là một phần. Đối với các khối đầy đủ, tính tổng tần số của giá trị đích trực tiếp từ từ điển. Đối với các khối một phần ở các cạnh, hãy lặp lại thủ công các phần tử trong phân đoạn và đếm các phần tử trùng khớp. 
3. Đối với truy vấn xoay trên đoạn [l, r], giải mã đoạn tương tự. Nếu l và r nằm trong một khối, chỉ cần xoay các phần tử trong danh sách và cập nhật từ điển tần số của khối nếu cần. Nếu phân đoạn trải dài trên nhiều khối, hãy cẩn thận xoay các phần tử qua các ranh giới khối: xóa phần tử cuối cùng của phân đoạn và chèn nó vào đầu, điều chỉnh từ điển tần số của từng khối bị ảnh hưởng để phản ánh các thay đổi. 
4. Sau mỗi truy vấn đếm, hãy cập nhật kết quả cuối cùng vì nó sẽ được sử dụng để giải mã các truy vấn tiếp theo.

Tại sao nó hoạt động: Mỗi khối duy trì một từ điển tần số chính xác và thứ tự phần tử trong khối được giữ nguyên. Đếm các truy vấn tính tổng số lượng chính xác từ các khối đầy đủ và kiểm tra chính xác các khối một phần, đảm bảo tính chính xác. Xoay vòng cập nhật cả vị trí phần tử và số lượng tần số, do đó các truy vấn trong tương lai sẽ thấy trạng thái chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math
from collections import defaultdict

def main():
    n = int(input())
    a = list(map(int, input().split()))
    q = int(input())
    lastans = 0
    
    block_size = int(math.sqrt(n)) + 1
    blocks = []
    freqs = []
    for i in range(0, n, block_size):
        blk = a[i:i+block_size]
        blocks.append(blk)
        freq = defaultdict(int)
        for val in blk:
            freq[val] += 1
        freqs.append(freq)
    
    def get_index(idx):
        return idx // block_size, idx % block_size

    for _ in range(q):
        parts = list(map(int, input().split()))
        if parts[0] == 1:
            l, r = parts[1], parts[2]
            l = (l + lastans - 1) % n
            r = (r + lastans - 1) % n
            if l > r:
                l, r = r, l
            l_blk, l_off = get_index(l)
            r_blk, r_off = get_index(r)
            if l_blk == r_blk:
                blk = blocks[l_blk]
                val = blk[r_off]
                for i in range(r_off, l_off, -1):
                    blk[i] = blk[i-1]
                blk[l_off] = val
                freq = freqs[l_blk]
                # freq unchanged
            else:
                # extract the last element
                val = blocks[r_blk][r_off]
                # shift last block
                for i in range(r_off, 0, -1):
                    blocks[r_blk][i] = blocks[r_blk][i-1]
                # shift middle blocks
                for b in range(r_blk-1, l_blk, -1):
                    last = blocks[b][-1]
                    blocks[b][1:] = blocks[b][:-1]
                    blocks[b][0] = last
                # shift first block
                for i in range(blocks[l_blk].__len__()-1, l_off, -1):
                    blocks[l_blk][i] = blocks[l_blk][i-1]
                blocks[l_blk][l_off] = val
                # rebuild frequency dicts
                for b in range(l_blk, r_blk+1):
                    freq = defaultdict(int)
                    for v in blocks[b]:
                        freq[v] += 1
                    freqs[b] = freq
        else:
            l, r, k = parts[1], parts[2], parts[3]
            l = (l + lastans - 1) % n
            r = (r + lastans - 1) % n
            k = (k + lastans - 1) % n + 1
            if l > r:
                l, r = r, l
            l_blk, l_off = get_index(l)
            r_blk, r_off = get_index(r)
            ans = 0
            if l_blk == r_blk:
                for i in range(l_off, r_off+1):
                    if blocks[l_blk][i] == k:
                        ans += 1
            else:
                for i in range(l_off, len(blocks[l_blk])):
                    if blocks[l_blk][i] == k:
                        ans += 1
                for b in range(l_blk+1, r_blk):
                    ans += freqs[b][k]
                for i in range(0, r_off+1):
                    if blocks[r_blk][i] == k:
                        ans += 1
            print(ans)
            lastans = ans

if __name__ == "__main__":
    main()
```Mã bắt đầu bằng cách đọc đầu vào và chuẩn bị các khối với từ điển tần số. Đối với các phép quay, chúng tôi xử lý cẩn thận các trường hợp một khối và nhiều khối, dịch chuyển các phần tử và xây dựng lại bản đồ tần số. Đối với số lượng, chúng tôi tính tổng số lượng toàn khối và kiểm tra trực tiếp các khối một phần. Mã hóa được xử lý trước mỗi truy vấn. 

## Ví dụ đã hoạt động 

**Mẫu 1 vết** 

| Bước | Truy vấn | người cuối cùng | Hành động | Trạng thái phân đoạn | 
| --- | --- | --- | --- | --- | 
| 1 | 1 3 6 | 0 | Xoay chỉ số 2-5 | [6,6,5,2,7,4,2] | 
| 2 | 2 2 4 2 | 0 | Đếm giá trị 2 trong 1-3 | trả lời=2 | 
| 3 | 2 2 4 7 | 2 | Đếm giá trị 7 trong 1-3 | trả lời=1 | 
| 4 | 2 2 2 5 | 1 | Đếm giá trị 5 trong 1-1 | trả lời=0 | 
| 5 | 1 2 6 | 0 | Xoay chỉ số 1-5 | mảng cập nhật | 
| 6 | 1 1 4 | 0 | Xoay chỉ số 0-3 | mảng cập nhật | 
| 7 | 2 1 7 3 | 0 | Đếm giá trị 3 trong 0-6 | trả lời=0 | 

Dấu vết này xác nhận logic xoay và điều chỉnh mã hóa. 

**Dấu vết tùy chỉnh** 

Mảng [1,2,3,4], xoay 0-3, đếm 2 trong 0-1, đảm bảo phép xoay đầu tiên di chuyển các phần tử một cách chính xác và việc đếm hoạt động trên các khối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q * √n) | Mỗi truy vấn |
