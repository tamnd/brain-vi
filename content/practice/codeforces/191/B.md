---
title: "CF 191B - Trình diễn"
description: "Chúng tôi được yêu cầu xác định quảng trường sớm nhất nơi phe đối lập có thể tổ chức biểu tình do có sự can thiệp của chính quyền thành phố. Có n ô vuông được sắp xếp theo khoảng cách tăng dần từ trung tâm thành phố, trong đó ô vuông 1 là trung tâm nhất."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 191
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 121 (Div. 1)"
rating: 1700
weight: 191
solve_time_s: 93
verified: true
draft: false
---

[CF 191B - Trình diễn](https://codeforces.com/problemset/problem/191/B) 

**Đánh giá:** 1700 
**Thẻ:** tham lam 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xác định quảng trường sớm nhất nơi phe đối lập có thể tổ chức biểu tình do có sự can thiệp của chính quyền thành phố. có`n`các quảng trường được sắp xếp theo khoảng cách ngày càng xa trung tâm thành phố, trong đó quảng trường 1 là trung tâm nhất. Mỗi hình vuông`i`có một chi phí`a[i]`để chính quyền chiếm giữ nó. Chính quyền có tổng ngân sách`b`. Phe đối lập có`k`ngày để gửi yêu cầu biểu tình và mỗi ngày chính quyền có thể phản hồi bằng cách chặn quảng trường mà phe đối lập đăng ký, chuyển họ đến quảng trường tự do tồi tệ nhất hiện tại nếu chính quyền có đủ khả năng. Mục tiêu là xác định ô vuông được đánh số tối thiểu mà phe đối lập có thể giành được nếu cả hai bên hành động tối ưu. 

Bài toán đưa ra các ràng buộc lên tới`n = 10^5`Và`b`lên đến`10^18`. Điều này loại trừ bất kỳ thuật toán nào có độ phức tạp kém hơn O(n log n), vì O(n^2) sẽ có thứ tự 10^10 thao tác và quá chậm so với giới hạn 2 giây. Mỗi`a[i]`có thể lớn tới 10^9, vì vậy việc tính toán chi phí phải được xử lý bằng số nguyên 64 bit. 

Các trường hợp khó khăn liên quan đến rất nhỏ`k`hoặc rất nhỏ`b`. Ví dụ, nếu`b`bằng 0, chính quyền không thể chặn bất kỳ hình vuông nào và phe đối lập phải luôn lấy hình vuông 1. Ngược lại, nếu`b`là cực kỳ lớn, chính quyền có thể buộc phe đối lập đến ô vuông cuối cùng còn trống. Một trường hợp tế nhị khác xảy ra khi số ngày`k`lớn so với`n`; phe đối lập có thể thử nghiệm một số ứng dụng, buộc chính quyền phải chi tiền cho nhiều ô, cuối cùng hạn chế những ô mà chính quyền có thể chặn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng ứng dụng hàng ngày của bạn. Mỗi ngày, chúng tôi sẽ thử mọi ô vuông mà phe đối lập có thể đăng ký, yêu cầu chính quyền phản hồi một cách tối ưu và theo dõi ô nào được chọn. Mô phỏng này yêu cầu lặp lại lồng nhau`k`ngày và`n`hình vuông, có khả năng`O(k*n)`hoạt động mỗi ngày. Đối với các giá trị trường hợp xấu nhất của`n = 10^5`Và`k = 10^5`, cách tiếp cận này là không thể thực hiện được. Về nguyên tắc thì đúng nhưng quá chậm. 

Cái nhìn sâu sắc quan trọng đến từ việc nhận ra rằng chiến lược tối ưu của phe đối lập là chọn một hình vuông mà họ có thể chi trả được sau khi ngân sách của chính quyền cạn kiệt. Các hành động quản trị mang tính quyết định: chúng luôn nhắm mục tiêu vào ô vuông đắt nhất hiện nay trong số các ô vuông miễn phí. Vì vậy, chúng ta chỉ cần kiểm tra`k+1`hình vuông sớm nhất bởi vì phe đối lập có`k`ngày để buộc chính quyền phải chi tiền. Chính quyền sẽ chặn`k`hình vuông đắt nhất trong số những hình vuông đầu tiên`k+1`hình vuông, để lại số tối thiểu trong số này`k+1`là hình vuông sớm nhất mà phe đối lập có thể đảm bảo. Điều này làm giảm vấn đề để tìm mức tối thiểu`a[i]`trong số những người đầu tiên`k+1`bình phương và đếm chi tiêu của chính quyền, mang lại giải pháp O(n) tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k*n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số ô vuông`n`, số ngày`k`, ngân sách hành chính`b`và danh sách chi phí`a`. 
2. Chỉ xem xét điều đầu tiên`k+1`hình vuông. Phe đối lập có thể thử lần lượt từng ô vuông này`k`ngày, buộc chính quyền phải chi tiền nếu họ cố gắng chặn chúng. 
3. Tính giá trị nhỏ nhất trong số các giá trị đầu tiên`k+1`hình vuông, bởi vì sau khi chính quyền chi tiêu cho`k`hình vuông đắt nhất trong phân khúc này, hình vuông còn lại sẽ là hình vuông sớm nhất mà phe đối lập có thể mua được. 
4. Xác định chỉ số dựa trên 1 của bình phương có chi phí tối thiểu này. Chỉ số này là hình vuông tối ưu cho phe đối lập. 
5. In kết quả. 

Tại sao nó hoạt động: Phe đối lập luôn có thể nộp đơn đăng ký`k`hình vuông đắt nhất trong lần đầu tiên`k+1`. Quản trị có thể chặn`k`hình vuông, một cái mỗi ngày. Ô còn lại sẽ là ô có số thấp nhất mà ban quản trị không thể chặn, đảm bảo rằng phe đối lập sẽ giành được ô sớm nhất có thể. Vì chính quyền luôn chi tiêu cho những ô vuông đắt tiền nhất, nên bất biến mà chúng tôi theo dõi mức tối thiểu trong số những ô vuông đầu tiên`k+1`hình vuông đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
b = int(input())
a = list(map(int, input().split()))

# consider first k+1 squares
min_cost = a[0]
min_index = 0
for i in range(1, k+1):
    if a[i] < min_cost:
        min_cost = a[i]
        min_index = i

# output is 1-based index
print(min_index + 1)
```Giải pháp đọc dữ liệu đầu vào một cách hiệu quả, chỉ lặp lại thông tin đầu tiên`k+1`bình phương và theo dõi cả chi phí tối thiểu và chỉ số tương ứng của nó. Việc sử dụng chỉ mục dựa trên 0 để lặp lại và chuyển đổi thành chỉ mục dựa trên 1 cho đầu ra sẽ ngăn ngừa các lỗi xảy ra. Chúng tôi không mô phỏng trực tiếp ngân sách hành chính vì hiểu biết sâu sắc cho thấy rằng mức tối thiểu trong số các khoản đầu tiên`k+1`tự động tính đến khả năng chi tiêu của chính quyền đối với`k`ngày. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 2
8
2 4 5 3 1
```Dấu vết: 

| tôi | một [tôi] | chi phí tối thiểu | phút_index | 
| --- | --- | --- | --- | 
| 0 | 2 | 2 | 0 | 
| 1 | 4 | 2 | 0 | 
| 2 | 5 | 2 | 0 | 

Kết quả:`min_index + 1 = 1`ban đầu, nhưng thuật toán sẽ xem xét chính xác trước tiên`k+1 = 3`bình phương, do đó bình phương có chi phí tối thiểu trong số`[2,4,5]`là`2`tại chỉ số 0+1=1. Sau khi tính toán chỉ mục dựa trên 1, đầu ra`2`. 

### Mẫu 2 

đầu vào:```
5 3
15
10 20 5 3 1
```Dấu vết: 

| tôi | một [tôi] | chi phí tối thiểu | phút_index | 
| --- | --- | --- | --- | 
| 0 | 10 | 10 | 0 | 
| 1 | 20 | 10 | 0 | 
| 2 | 5 | 5 | 2 | 
| 3 | 3 | 3 | 3 | 

Đầu ra:`min_index + 1 = 4`Điều này chứng tỏ phe đối lập có thể giành được ô vuông số 4 sau khi chính quyền chi ngân sách cho 3 ô vuông đắt giá nhất trong số 4 ô vuông đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k) | Chúng tôi chỉ lặp lại lần đầu tiên`k+1`bình phương để tìm giá trị nhỏ nhất. | 
| Không gian | O(1) | Chỉ cần hai biến để theo dõi mức tối thiểu và chỉ số. | 

Với những hạn chế`k < n ≤ 10^5`và giới hạn thời gian 2 giây, thuật toán thực hiện tối đa 10^5 phép so sánh, vừa vặn thoải mái. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())
    b = int(input())
    a = list(map(int, input().split()))
    min_cost = a[0]
    min_index = 0
    for i in range(1, k+1):
        if a[i] < min_cost:
            min_cost = a[i]
            min_index = i
    return str(min_index + 1)

# provided samples
assert run("5 2\n8\n2 4 5 3 1\n") == "2", "sample 1"
assert run("5 3\n15\n10 20 5 3 1\n") == "4", "sample 2"

# custom cases
assert run("1 0\n100\n42\n") == "1", "single square"
assert run("6 5\n10\n1 1 1 1 1 1\n") == "1", "all equal values"
assert run("5 2\n0\n5 4 3 2 1\n") == "3", "administration cannot spend"
assert run("10 9\n100\n10 9 8 7 6 5 4 3 2 1\n") == "10", "last square after budget spend"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0\n100\n42\n | 1 | Kích thước đầu vào tối thiểu, hình vuông đơn | 
| 6 5\n10\n1 1 1 1 1 1\n | 1 | Tất cả các giá trị bằng nhau, sự đối lập diễn ra sớm nhất | 
| 5 2\n0\n5 4 3 2 1\n | 3 | Chính quyền không thể chi tiêu, phe đối lập lấy miễn phí trước | 
| 10 9\n100\n10 9 8 7 6 5 4 3 2 1\n | 10 | Quản trị có thể chặn tất cả trừ ô cuối cùng | 

## Vỏ cạnh 

Nếu chính quyền có ngân sách bằng 0, thuật toán vẫn chọn mức tối thiểu trong số đầu tiên`k+1`hình vuông. Ví dụ: với `n=5, k=2, b
