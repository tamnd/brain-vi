---
title: "CF 440A - Tập bị lãng quên"
description: "Chúng ta được yêu cầu xác định tập nào Polycarpus chưa xem trong một phần của chương trình truyền hình. Anh ấy đã xem n - 1 tập trong tổng số n tập, mỗi tập được đánh số liên tục từ 1 đến n."
date: "2026-06-07T03:19:27+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 440
codeforces_index: "A"
codeforces_contest_name: "Testing Round 10"
rating: 800
weight: 440
solve_time_s: 61
verified: true
draft: false
---

[CF 440A - Tập bị lãng quên](https://codeforces.com/problemset/problem/440/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định tập nào Polycarpus chưa xem trong một phần của chương trình truyền hình. Anh ấy đã xem _n_ - 1 tập trong tổng số _n_, mỗi tập được đánh số liên tục từ 1 đến _n_. Dữ liệu đầu vào cung cấp cho chúng tôi _n_, tổng số tập và danh sách _n_ - 1 số nguyên đại diện cho các tập mà anh ấy đã xem. Nhiệm vụ là tìm số tập còn thiếu. 

Cho rằng _n_ có thể lớn tới 100.000, bất kỳ giải pháp nào có tỷ lệ kém hơn O(n) đều có thể trở nên quá chậm. Ví dụ: quét liên tục danh sách để tìm từng số từ 1 đến _n_ sẽ là O(n²) trong trường hợp xấu nhất, tốc độ này chậm đến mức không thể chấp nhận được đối với n ≈ 10⁵. Chúng ta cần một giải pháp thực hiện truyền tuyến tính hoặc sử dụng thuộc tính toán học để giảm bớt công việc. 

Trường hợp đặc biệt không rõ ràng xảy ra khi tập bị thiếu là tập đầu tiên (1) hoặc tập cuối cùng (n). Ví dụ: nếu n = 3 và các tập đã xem là [2, 3] thì tập bị thiếu là 1. Bất kỳ giải pháp nào giả định tập bị thiếu nằm ở đâu đó ở giữa danh sách sẽ không thành công. Tương tự, nếu các tập đã xem là [1, 2] với n = 3 thì tập bị thiếu là 3, tập này có thể bị bỏ qua nếu giới hạn mảng bị xử lý sai. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ lặp qua các số từ 1 đến _n_, kiểm tra từng số xem nó có tồn tại trong danh sách đã theo dõi hay không. Điều này có hiệu quả vì danh sách chứa các số nguyên riêng biệt trong phạm vi từ 1 đến _n_, vì vậy bất kỳ số nào bị thiếu đều là câu trả lời. Tuy nhiên, việc kiểm tra tư cách thành viên trong danh sách có kích thước _n_ - 1 là O(n), vì vậy cách tiếp cận ngây thơ này sẽ thực hiện các thao tác O(n²). Với n = 100.000, đây là khoảng 10¹⁰ thao tác, quá chậm so với giới hạn thời gian 1 giây. 

Cách tiếp cận tối ưu dựa trên một tính chất toán học đơn giản. Tổng của n số tự nhiên đầu tiên là n*(n+1)/2. Nếu chúng ta tính tổng số này và trừ tổng của tất cả các tập đã xem thì sự khác biệt phải là tập bị thiếu. Điều này hiệu quả vì các tập đã xem bao gồm mọi số khác đúng một lần. Cả phép tính tổng và phép trừ đều là O(n), do đó tổng thời gian chạy là tuyến tính và vừa vặn trong các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tổng chênh lệch | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tổng số tập, n. Điều này cho chúng ta biết phạm vi số tập hợp lệ. 
2. Đọc danh sách n - 1 tập đã xem. Mỗi số được đảm bảo là khác biệt và nằm trong khoảng từ 1 đến n. 
3. Tính tổng của tất cả n số tập bằng công thức n*(n+1)//2. Đây là số tiền chúng tôi sẽ có nếu không có tập nào bị thiếu. 
4. Tính tổng số tập đã xem. Điều này ghi lại tất cả những con số chúng ta đã thấy. 
5. Trừ tổng số tập đã xem. Kết quả là số tập Polycarpus quên xem. 
6. In số tập còn thiếu này. 

Thuộc tính quan trọng đảm bảo tính chính xác là tổng của tất cả các số riêng biệt từ 1 đến n trừ đi tổng của bất kỳ n - 1 nào trong số chúng để lại chính xác một số bị thiếu. Tính năng này hoạt động bất kể tập nào bị thiếu, kể cả tập đầu tiên hay tập cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
watched = list(map(int, input().split()))

total_sum = n * (n + 1) // 2
watched_sum = sum(watched)

missing_episode = total_sum - watched_sum
print(missing_episode)
```Mã ghi n và danh sách các tập đã xem. Nó tính tổng của n số tự nhiên đầu tiên bằng phép chia số nguyên để tránh các vấn đề về dấu phẩy động. Sau đó, nó tính tổng số tập đã xem và trừ đi tổng số đó. Cuối cùng, nó in tập còn thiếu. Một lỗi phổ biến là quên phép chia số nguyên hoặc sử dụng phép chia dấu phẩy động, điều này có thể cho kết quả không chính xác. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
10
3 8 10 1 7 9 6 5 2
```| Biến | Giá trị | 
| --- | --- | 
| n | 10 | 
| đã xem | [3, 8, 10, 1, 7, 9, 6, 5, 2] | 
| tổng_tổng | 55 | 
| đã xem_sum | 51 | 
| thiếu_tập | 4 | 

Bảng cho thấy tập 4 còn thiếu chính xác là chênh lệch giữa tổng số tiền 55 và tổng số tiền đã xem là 51. 

**Ví dụ tùy chỉnh** 

đầu vào:```
3
2 3
```| Biến | Giá trị | 
| --- | --- | 
| n | 3 | 
| đã xem | [2, 3] | 
| tổng_tổng | 6 | 
| đã xem_sum | 5 | 
| thiếu_tập | 1 | 

Điều này xác nhận rằng thuật toán xác định chính xác tập đầu tiên bị thiếu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi tính tổng của n số bằng công thức, sau đó tính tổng n - 1 phần tử của danh sách. | 
| Không gian | O(n) | Danh sách các tập đã xem sử dụng bộ nhớ O(n). | 

Thời gian và không gian tuyến tính nằm trong giới hạn 1 giây và 256 MB cho n lên tới 100.000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    watched = list(map(int, input().split()))
    total_sum = n * (n + 1) // 2
    watched_sum = sum(watched)
    return str(total_sum - watched_sum)

# provided sample
assert run("10\n3 8 10 1 7 9 6 5 2\n") == "4", "sample 1"

# custom cases
assert run("3\n2 3\n") == "1", "missing first episode"
assert run("3\n1 2\n") == "3", "missing last episode"
assert run("5\n1 2 3 4\n") == "5", "small sequence, last missing"
assert run("2\n1\n") == "2", "minimum size"
assert run("4\n1 3 4\n") == "2", "missing middle episode"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3\n2 3 | 1 | tập đầu tiên bị thiếu | 
| 3\n1 2 | 3 | tập cuối bị thiếu | 
| 5\n1 2 3 4 | 5 | chuỗi nhỏ, thiếu cuối cùng | 
| 2\n1 | 2 | đầu vào kích thước tối thiểu | 
| 4\n1 3 4 | 2 | tập giữa bị thiếu | 

## Vỏ cạnh 

Nếu tập bị thiếu là tập đầu tiên, ví dụ: n = 3, đã xem = [2, 3], Total_sum = 6, đã xem_sum = 5, thì thuật toán trả về 1, điều này đúng. Nếu tập bị thiếu là tập cuối cùng, ví dụ: n = 3, đã xem = [1, 2], Total_sum = 6, đã xem_sum = 3, thì thuật toán trả về 3. Trong mọi trường hợp, tổng chênh lệch sẽ ghi lại chính xác số bị thiếu bất kể vị trí của nó trong danh sách.
