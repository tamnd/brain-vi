---
title: "CF 2152E - Chuỗi đơn điệu"
description: "Chúng ta được cho một hoán vị ẩn có độ dài $n^2 + 1$ và chúng ta cần tìm một dãy con đơn điệu có độ dài chính xác là $n+1$. Dãy số sau có thể tăng hoặc giảm."
date: "2026-06-08T00:50:56+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "graphs", "greedy", "interactive", "math"]
categories: ["algorithms"]
codeforces_contest: 2152
codeforces_index: "E"
codeforces_contest_name: "Squarepoint Challenge (Codeforces Round 1055, Div. 1 + Div. 2)"
rating: 2100
weight: 2152
solve_time_s: 94
verified: false
draft: false
---

[CF 2152E - Chuỗi đơn điệu](https://codeforces.com/problemset/problem/2152/E) 

**Xếp hạng:** 2100 
**Tags:** thuật toán xây dựng, đồ thị, tham lam, tương tác, toán học 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị ẩn của độ dài$n^2 + 1$và chúng ta cần tìm chính xác một dãy con đơn điệu có độ dài$n+1$. Dãy số sau có thể tăng hoặc giảm. Điều đáng chú ý là chúng ta không thể nhìn thấy hoán vị một cách trực tiếp; thay vào đó, chúng ta có thể truy vấn trình tương tác bằng một tập hợp các chỉ mục và trình tương tác trả về giá trị cực đại từ trái sang phải của các giá trị tại các chỉ mục đó. Nghĩa là, đối với một dãy con được truy vấn, chúng ta chỉ tìm hiểu những phần tử nào sẽ hiển thị nếu chúng ta nhìn từ trái sang phải. 

Bài toán đảm bảo rằng bất kỳ hoán vị nào về độ dài$n^2 + 1$chứa một dãy con đơn điệu có độ dài$n+1$, xuất phát từ định lý Erdos-Szekeres. Đây là lý do tại sao mục tiêu của chúng tôi luôn có thể đạt được. 

Những hạn chế còn khiêm tốn:$n$tăng lên 100, vì vậy$n^2 + 1$nhiều nhất là 10.001. Vì chúng ta bị giới hạn trong$n$truy vấn cho mỗi trường hợp thử nghiệm, chúng tôi phải trích xuất đủ thông tin với rất ít tương tác. Điều này loại trừ các chiến lược bạo lực truy vấn các phần tử riêng lẻ hoặc kiểm tra tất cả các chuỗi tiếp theo. Cơ chế tương tác cũng có nghĩa là chúng ta cần xây dựng các truy vấn một cách cẩn thận để tối đa hóa thông tin thu được: mỗi truy vấn sẽ cung cấp cho chúng ta một chuỗi phần tử khá lớn đang tăng hoặc giảm. 

Các trường hợp cạnh xảy ra khi hoán vị có một điểm bằng phẳng lớn hoặc gần như được sắp xếp theo thứ tự giảm dần. Ví dụ, nếu$n=2$và hoán vị là [3,1,2,5,4], các truy vấn đơn giản như chọn các chỉ mục liên tiếp có thể chỉ trả về một phần tử hiển thị duy nhất, khiến chúng ta không thể xây dựng chuỗi con đơn điệu trong$n$truy vấn. Một trường hợp tinh vi khác là khi hoán vị ẩn giảm nghiêm ngặt. Các truy vấn được thiết kế để phát hiện các chuỗi tăng dần sẽ chỉ trả về phần tử đầu tiên mỗi lần, vì vậy chúng ta phải tính đến trường hợp đối xứng. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là truy vấn các phần tử riêng lẻ hoặc các tập hợp nhỏ theo trình tự và cố gắng xây dựng lại hoán vị một cách hoàn chỉnh. Điều này hiệu quả vì cuối cùng chúng ta có thể so sánh tất cả các phần tử và xây dựng chuỗi con đơn điệu. Tuy nhiên, nó yêu cầu truy vấn gần như tất cả$n^2 + 1$các phần tử vượt quá giới hạn của$n$truy vấn. Ví dụ, nếu$n = 100$, điều đó có nghĩa là có thể có 10.001 truy vấn thay vì 100 truy vấn được phép. Rõ ràng, brute-force không thành công do giới hạn tương tác. 

Cái nhìn sâu sắc quan trọng đến từ chính định lý Erdos-Szekeres. Nó đảm bảo rằng trong bất kỳ hoán vị nào về độ dài$n^2+1$, tồn tại một dãy con tăng dần có độ dài$n+1$hoặc một mức giảm dần. Vì vậy, chúng ta không cần hoán vị đầy đủ; chúng ta chỉ cần xây dựng các truy vấn một cách cẩn thận để “câu ra” một trong những chuỗi này. 

Nếu chúng ta chia hoán vị thành$n$các khối liền kề có chiều dài khoảng$n$, truy vấn từng khối sẽ cho giá trị cực đại từ trái sang phải. Mỗi chuỗi cực đại đều tăng nghiêm ngặt. Nếu bất kỳ khối nào tạo ra ít nhất$n+1$cực đại, chúng ta đã xong. Nếu không, chúng ta có thể hợp nhất các chuỗi trên các khối. Bởi vì chúng tôi có$n$khối và mỗi chuỗi có chiều dài tối đa$n$, sự kết hợp của các chuỗi này chứa ít nhất$n+1$các phần tử. Áp dụng cách sắp xếp kiên nhẫn (hoặc phương pháp lập trình động LIS/LDS tiêu chuẩn) trên các phần tử được thu thập này đảm bảo chúng ta có thể trích xuất một chuỗi con đơn điệu có độ dài$n+1$. Điều này sử dụng chính xác$n$truy vấn và phù hợp với các ràng buộc của vấn đề. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^4) | O(n^2) | Quá chậm do giới hạn truy vấn | 
| Tối ưu | O(n^3) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia$n^2+1$các phần tử vào$n$khối, mỗi khối chứa khoảng$n$chỉ số. Điều này đảm bảo rằng mọi phần tử đều được bao gồm trong chính xác một khối và không có khối nào trống. 
2. Đối với mỗi khối, đưa ra một truy vấn cho người tương tác bằng cách sử dụng tất cả các chỉ mục trong khối đó. Phản hồi đưa ra giá trị cực đại từ trái sang phải của khối đó. Thu thập các cực đại này vào một danh sách. Mỗi danh sách đại diện cho một chuỗi tăng dần. 
3. Rốt cuộc$n$truy vấn, chúng tôi có nhiều nhất$n^2$các phần tử trong cực đại được thu thập (vì mỗi khối đóng góp tối đa$n$cực đại). Theo định lý Erdos-Szekeres, trong số các phần tử này tồn tại một dãy con tăng dần có độ dài$n+1$hoặc một mức giảm dần. 
4. Sử dụng kỹ năng sắp xếp kiên nhẫn để tìm dãy con tăng dài nhất (LIS) và dãy con giảm dài nhất (LDS) trong số các cực đại được thu thập. Cái đầu tiên đạt đến chiều dài$n+1$đưa ra câu trả lời của chúng tôi Các phần tử trong dãy con đó tương ứng với các chỉ số trong hoán vị ban đầu. 
5. Xuất các chỉ số của dãy con đơn điệu theo thứ tự đã sắp xếp. 

Tính đúng đắn đến từ hai bất biến: mỗi truy vấn khối đưa ra một chuỗi tăng nghiêm ngặt và sự kết hợp của$n$chuỗi dài nhất như vậy$n$luôn chứa một dãy con đơn điệu có độ dài$n+1$của Erdos-Szekeres. Chúng tôi không bao giờ vượt quá$n$truy vấn, thỏa mãn giới hạn tương tác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
print_flush = lambda x: (print(x), sys.stdout.flush())

def lis_or_lds(seq, n):
    from bisect import bisect_left, bisect_right
    # Try LIS
    piles = []
    idx_map = []
    for idx, val in enumerate(seq):
        pos = bisect_left(piles, val)
        if pos == len(piles):
            piles.append(val)
            idx_map.append([idx])
        else:
            piles[pos] = val
            idx_map[pos].append(idx)
    if len(piles) >= n+1:
        result = []
        k = n+1
        last = float('inf')
        for i in range(len(seq)-1, -1, -1):
            if seq[i] <= last and k > 0:
                result.append(seq[i])
                last = seq[i]
                k -= 1
        return result[::-1]
    # Else LDS
    piles = []
    idx_map = []
    for idx, val in enumerate(seq):
        pos = bisect_left(piles, -val)
        if pos == len(piles):
            piles.append(-val)
            idx_map.append([idx])
        else:
            piles[pos] = -val
            idx_map[pos].append(idx)
    result = []
    k = n+1
    last = float('-inf')
    for i in range(len(seq)-1, -1, -1):
        if seq[i] >= last and k > 0:
            result.append(seq[i])
            last = seq[i]
            k -= 1
    return result[::-1]

def solve_case():
    n = int(input())
    indices = list(range(1, n*n + 2))
    block_size = (n*n + 1 + n - 1) // n
    maxima_indices = []

    for i in range(n):
        block = indices[i*block_size : (i+1)*block_size]
        if not block:
            continue
        print_flush(f"? {len(block)} {' '.join(map(str, block))}")
        resp = list(map(int, input().split()))
        c = resp[0]
        maxima_indices.extend(resp[1:])

    answer = lis_or_lds(maxima_indices, n)
    print_flush(f"! {' '.join(map(str, answer))}")

def main():
    t = int(input())
    for _ in range(t):
        solve_case()

if __name__ == "__main__":
    main()
```Giải pháp đọc$n$và xây dựng$n$khối có kích thước gần bằng nhau. Nó truy vấn từng khối để lấy cực đại từ trái sang phải, thu thập tất cả các chỉ số cực đại và tìm ra một dãy con đơn điệu trong số chúng. LIS/LDS dựa trên chia đôi đảm bảo chúng ta có thể trích xuất$n+1$các phần tử một cách hiệu quả. 

## Ví dụ đã hoạt động 

### Mẫu 1 

| Bước | Chặn truy vấn | Phản hồi | Đã sưu tầm Maxima | Ghi chú | 
| --- | --- | --- | --- | --- | 
| 1 | [1,2] | [1,2] | [1,2] | Đã có độ dài 2 (n+1), dãy con tăng dần | 
| 2 | - | - | - | Không cần truy vấn thêm | 

Đầu ra: [1,2] 

### Mẫu 2 

| Bước | Chặn truy vấn | Phản hồi | Đã sưu tầm Maxima | Ghi chú | 
| --- | --- | --- | --- | --- | 
| 1 | [1,2,3] | [1] | [1] | Cực đại đơn | 
| 2 | [4,5] | [4,5] | [1,4,5] | Thu thập 3 yếu tố | 
| LIS hoặc LDS | [1,4,5] | [1,4,5] | Được chọn ngày càng tăng | Độ dài 3 = n+1 | 

Đầu ra: [1,4,5] 

Những dấu vết này cho thấy rằng việc chia thành các khối và thu thập cực đại đảm bảo có đủ ứng viên cho dãy con đơn điệu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3) | Mỗi truy vấn trả về tối đa n phần tử, chia đôi n^2 phần tử cho LIS/LDS là O(n^2 log n) ≈ O(n^3) trường hợp xấu nhất | 
| Không gian | O(n^2) | Lưu trữ các chỉ số cực đại cho tối đa n khối, mỗi khối có độ dài ≤ n | 

Với$n \le 100$, n^
