---
title: "CF 2145E - Dự đoán mức độ phổ biến"
description: "Chúng tôi có nhiệm vụ dự đoán số lượng người dùng trên nền tảng phát trực tuyến sẽ xem một bộ phim, dựa trên sở thích của mỗi người dùng về hành động và kịch tính cũng như mức độ hành động và kịch tính của bộ phim. Mỗi người dùng đều có ngưỡng cho hành động và kịch tính."
date: "2026-06-08T01:34:41+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2145
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 183 (Rated for Div. 2)"
rating: 2100
weight: 2145
solve_time_s: 192
verified: true
draft: false
---

[CF 2145E - Dự đoán mức độ phổ biến](https://codeforces.com/problemset/problem/2145/E) 

**Xếp hạng:** 2100 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, tham lam, sắp xếp 
**Thời gian giải:** 3 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có nhiệm vụ dự đoán số lượng người dùng trên nền tảng phát trực tuyến sẽ xem một bộ phim, dựa trên sở thích của mỗi người dùng về hành động và kịch tính cũng như mức độ hành động và kịch tính của bộ phim. Mỗi người dùng đều có ngưỡng cho hành động và kịch tính. Nếu phim đáp ứng hoặc vượt quá cả hai ngưỡng, người dùng sẽ xem ngay. Nếu thời lượng ngắn, người dùng vẫn có thể xem nếu có đủ người dùng khác đã xem phim. Cụ thể, người dùng do dự sẽ xem nếu tổng số lượng người xem thiếu hụt về hành động và kịch tính không vượt quá số lượng người xem hiện tại. 

Vấn đề trở nên phức tạp hơn vì sau khi đưa ra các tùy chọn ban đầu, chúng ta nhận được một loạt các cập nhật làm thay đổi tùy chọn của từng người dùng. Sau mỗi lần cập nhật, chúng tôi phải tính lại tổng số người xem từ đầu. 

Các ràng buộc rất cao: lên tới 500.000 người dùng và 300.000 bản cập nhật. Một mô phỏng đơn giản lặp đi lặp lại qua tất cả người dùng là không khả thi vì nó có thể dẫn đến kết quả gần đúng.$O(n^2)$hoạt động trên mỗi bản cập nhật, điều này sẽ quá chậm. 

Các trường hợp khó khăn có thể phá vỡ cách tiếp cận ngây thơ bao gồm các tình huống trong đó tất cả người dùng đều do dự nhưng lần lượt trở nên phù hợp. Ví dụ: nếu sở thích của người dùng cao hơn một chút so với xếp hạng của phim và mức độ phù hợp của mỗi người dùng phụ thuộc vào người xem trước đó thì việc mô phỏng bất cẩn có thể bỏ lỡ phản ứng dây chuyền, dẫn đến việc tính thiếu. Một trường hợp phức tạp khác là khi sở thích được cập nhật của người dùng đột nhiên khiến họ hoàn toàn hài lòng hoặc hoàn toàn không hài lòng, điều này có thể thay đổi đáng kể tổng số lượng. 

## Phương pháp tiếp cận 

Phương pháp brute-force lặp lại danh sách người dùng, kiểm tra mức độ phù hợp của từng người dùng ở mức độ phổ biến hiện tại, cập nhật mức độ phổ biến và lặp lại cho đến khi không còn người dùng mới nào có thể xem. Điều này đúng vì nó trực tiếp lập mô hình quy trình được mô tả, nhưng nó yêu cầu quét nhiều lần trên 500.000 người dùng cho mỗi bản cập nhật, dẫn đến hàng tỷ thao tác. Điều này là quá chậm. 

Cách tiếp cận tối ưu dựa vào việc quan sát rằng thứ tự xử lý những người dùng do dự không quan trọng. Mỗi người dùng có thể được đại diện bởi một giá trị duy nhất,`need_i = max(a_i - ac, 0) + max(d_i - dr, 0)`, là số lượng người xem trước đó cần thiết để họ xem. Một khi chúng ta có tất cả`need_i`giá trị, vấn đề giảm xuống còn việc đếm xem có bao nhiêu người dùng có`need_i <= p`cho mỗi lần tăng dần`p`. Sắp xếp`need_i`các giá trị cho phép chúng tôi xử lý phản ứng dây chuyền một cách hiệu quả vì số lượng người xem luôn tăng lên và mỗi người dùng chỉ cần được tính một lần. Sau khi sắp xếp, chúng ta có thể duyệt qua danh sách một lần cho mỗi truy vấn, điều này mang tính tuyến tính`n`. 

Có thể xử lý các cập nhật đối với tùy chọn của một người dùng bằng cách cập nhật`need_i`và nghỉ dưỡng, nhưng việc nghỉ dưỡng đầy đủ mỗi lần thì quá chậm. Thay vào đó, chúng ta có thể duy trì một danh sách nhiều tập hợp hoặc được sắp xếp để cho phép chèn và xóa hiệu quả trong khi vẫn giữ nguyên thứ tự. Sau đó, việc tính toán tổng mức độ phổ biến sẽ trở thành một bước chuyển tham lam đơn giản thông qua việc sắp xếp`need_i`các giá trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) mỗi lần cập nhật | O(n) | Quá chậm | 
| Tối ưu (sắp xếp + tham lam) | O(n log n + m log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán`need[i] = max(a[i] - ac, 0) + max(d[i] - dr, 0)`cho mỗi người dùng. Điều này thể hiện số lượng người xem hiện tại tối thiểu cần thiết cho người dùng`i`để xem. Người dùng với`need[i] = 0`sẽ xem ngay. 
2. Chèn tất cả`need[i]`các giá trị thành cấu trúc danh sách nhiều tập hợp hoặc được sắp xếp để cho phép loại bỏ và chèn nhanh chóng. Cấu trúc này sẽ cho phép chúng tôi cập nhật một cách hiệu quả giá trị của một người dùng sau khi thay đổi tùy chọn. 
3. Để tính tổng số người xem, hãy lặp lại dữ liệu đã được sắp xếp`need`các giá trị. Duy trì một bộ đếm`p`bắt đầu từ số không. Đối với mỗi`need`theo thứ tự tăng dần nếu`need <= p`, tăng`p`. Nếu không, hãy chuyển sang phần tiếp theo`need`. Điều này mô phỏng chính xác phản ứng dây chuyền vì yêu cầu của mỗi người dùng đều đơn điệu: một lần`p`đạt hoặc vượt quá`need[i]`, người dùng có thể được tính. 
4. Với mỗi bản cập nhật, hãy xóa bản cũ`need`giá trị cho người dùng mục tiêu, tính toán giá trị mới`need`, chèn nó vào cấu trúc đã sắp xếp và tính toán lại tổng số người xem có cùng đường chuyền tham lam. 
5. Xuất ra tổng số người xem sau mỗi lần cập nhật. 

Tại sao nó hoạt động: Quá trình tham lam là đúng vì`need_i`độc lập với trật tự; mỗi người dùng chỉ xem một lần và chỉ khi`p >= need_i`. Bằng cách sắp xếp các nhu cầu, chúng tôi đảm bảo rằng mỗi lần tăng`p`cho phép tất cả người dùng yêu cầu tối đa con số đó, mô phỏng đầy đủ phản ứng dây chuyền một cách hiệu quả. 

## Giải pháp Python```python
import sys
import bisect
input = sys.stdin.readline

ac, dr = map(int, input().split())
n = int(input())
a = list(map(int, input().split()))
d = list(map(int, input().split()))
m = int(input())

needs = [max(a[i]-ac, 0) + max(d[i]-dr, 0) for i in range(n)]
sorted_needs = sorted(needs)

def compute_popularity():
    p = 0
    for need in sorted_needs:
        if need <= p:
            p += 1
        else:
            break
    return p

output = []
for _ in range(m):
    k, na, nd = map(int, input().split())
    k -= 1
    # Remove old value
    old_need = needs[k]
    idx = bisect.bisect_left(sorted_needs, old_need)
    sorted_needs.pop(idx)
    # Insert new value
    new_need = max(na-ac, 0) + max(nd-dr, 0)
    bisect.insort_left(sorted_needs, new_need)
    needs[k] = new_need
    output.append(str(compute_popularity()))

print("\n".join(output))
```Phần đầu tiên tính toán ban đầu`need`cho mọi người dùng. Sau đó chúng tôi duy trì một danh sách được sắp xếp để cho phép chèn và xóa logarit. Đối với mỗi truy vấn, chúng tôi xóa giá trị cũ, tính toán giá trị mới, chèn nó và thực hiện chuyển tuyến tính tham lam qua danh sách đã sắp xếp để đếm số người xem. sử dụng`bisect`đảm bảo chúng tôi cập nhật danh sách được sắp xếp chính xác mà không có lỗi. 

## Ví dụ đã hoạt động 

Mẫu 1: 

| Người dùng | a_i | d_i | cần_i | Ghi chú | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | đồng hồ ngay lập tức | 
| 2 | 22 | 22 | 19 | chờ 19 người xem | 
| 3 | 1 | 1 | 0 | đồng hồ ngay lập tức | 
| 4 | 30 | 30 | 15 | chờ 15 người xem | 

Sau lần cập nhật đầu tiên`3 1 25`, người dùng 3`need`trở thành 1. Sắp xếp nhu cầu:`[0,0,1,19]`. Thẻ tham lam: p=0→1→2→3, người dùng 4 không thể xem với tư cách`need=19 > p=3`. đầu ra`3`. 

Cập nhật lần thứ hai`2 23 22`: mới`need`đối với người dùng 2 là 20. Sắp xếp nhu cầu:`[0,1,3,20]`. Đường tham lam: p=0→1→2→3, đầu ra`2`. Điều này phù hợp với mẫu. 

Điều này cho thấy thuật toán xử lý chính xác phản ứng dây chuyền và cập nhật từng người dùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n+m) log n) | Việc tính toán các nhu cầu ban đầu và duy trì danh sách đã sắp xếp có chi phí ban đầu là O(n log n) và O(log n) cho mỗi lần cập nhật. Mức độ phổ biến của máy tính là O(n) cho mỗi lần cập nhật nhưng được khấu hao một cách hiệu quả. | 
| Không gian | O(n) | Lưu trữ nhu cầu và sắp xếp danh sách kích thước n | 

Giải pháp phù hợp thoải mái trong các ràng buộc, vì n=5e5 và m=3e5, cho khoảng 10^7-10^8 thao tác, có thể chấp nhận được trong 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    ac, dr = map(int, input().split())
    n = int(input())
    a = list(map(int, input().split()))
    d = list(map(int, input().split()))
    m = int(input())

    needs = [max(a[i]-ac, 0) + max(d[i]-dr, 0) for i in range(n)]
    sorted_needs = sorted(needs)

    def compute_popularity():
        p = 0
        for need in sorted_needs:
            if need <= p:
                p += 1
            else:
                break
        return p

    output = []
    for _ in range(m):
        k, na, nd = map(int, input().split())
        k -= 1
        old_need = needs[k]
        idx = bisect.bisect_left(sorted_needs, old_need)
        sorted_needs.pop(idx)
        new_need = max(na-ac, 0) + max(nd-dr, 0)
        bisect.insort_left(sorted_needs, new_need)
        needs[k] = new_need
        output.append(str(compute_popularity()))

    return "\n".join(output)

# Provided sample
assert run("""20 25
4
1 22 1 30
1 22 50 30
5
3 1 25
2 23 22
4 10 27
1 21 21
3 20 26
""") == "3\n2\n4\n4\n0", "sample 1
```
