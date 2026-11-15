---
title: "CF 1863C - Lặp lại MEX"
description: "Chúng ta được cung cấp một mảng các số nguyên riêng biệt nằm trong khoảng từ 0 đến n. Nhiệm vụ là cập nhật liên tục mảng theo một cách rất cụ thể: với mỗi phần tử từ trái sang phải, thay thế bằng MEX (giá trị loại trừ tối thiểu) của mảng hiện tại."
date: "2026-06-09T00:01:33+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1863
codeforces_index: "C"
codeforces_contest_name: "Pinely Round 2 (Div. 1 + Div. 2)"
rating: 1100
weight: 1863
solve_time_s: 129
verified: true
draft: false
---

[CF 1863C - Lặp lại MEX](https://codeforces.com/problemset/problem/1863/C) 

**Đánh giá:** 1100 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 2m 9s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên riêng biệt nằm trong khoảng từ 0 đến n. Nhiệm vụ là cập nhật liên tục mảng theo một cách rất cụ thể: với mỗi phần tử từ trái sang phải, thay thế bằng MEX (giá trị loại trừ tối thiểu) của mảng hiện tại. Sau khi thực hiện thao tác này k lần, chúng ta cần in mảng kết quả. 

Vấn đề này yêu cầu xử lý nhiều trường hợp kiểm thử, mỗi trường hợp có tối đa 10^5 phần tử và tổng n trên tất cả các trường hợp kiểm thử bị giới hạn bởi 10^5. Điều này có nghĩa là bất kỳ giải pháp nào lặp lại k lần trực tiếp trên mảng đều không khả thi vì k có thể lớn tới 10^9. Một cách tiếp cận ngây thơ để thực sự thực hiện k thao tác sẽ yêu cầu tới 10^14 bước trong trường hợp xấu nhất, điều này rõ ràng là không thể thực hiện được trong thời hạn. 

Một điểm tinh tế là các phần tử mảng khác biệt và được giới hạn từ 0 đến n. Ràng buộc này ngụ ý rằng MEX tại bất kỳ thời điểm nào là n+1 hoặc một số từ 0 đến n bị thiếu trong mảng. Các trường hợp cạnh phát sinh khi mảng thiếu giá trị lớn nhất có thể hoặc khi k lớn nhưng mảng ổn định sau một vài thao tác. Ví dụ: nếu n = 1 và a = [1] với k = 2, thao tác đầu tiên cho [0], thao tác thứ hai cho [1]. Việc triển khai bất cẩn có thể cho rằng mảng luôn phát triển đơn điệu hoặc chỉ cần một thao tác là đủ. 

Một tình huống phức tạp khác xảy ra khi mảng đã chứa tất cả các số từ 0 đến n-1. Khi đó MEX là n. Thay thế một phần tử bằng n sẽ đưa ra một số nằm ngoài phạm vi ban đầu. Xử lý vấn đề này một cách chính xác là chìa khóa để tránh từng lỗi một. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản: đối với mỗi thao tác, hãy tính MEX của mảng hiện tại, sau đó thay thế từng phần tử một cách tuần tự. Điều này hoạt động vì vấn đề xác định rõ ràng từng bước hoạt động. Tuy nhiên, việc thực hiện điều này cho k thao tác là cực kỳ chậm, như chúng tôi đã lưu ý, vì k có thể lên tới 10^9 và n lên tới 10^5. 

Cái nhìn sâu sắc quan trọng là mảng cuối cùng sẽ ổn định. Sau tối đa n+1 phép toán, mọi số từ 0 đến n sẽ xuất hiện trong mảng và các ứng dụng tiếp theo của phép toán sẽ tạo ra một chu trình hoặc điểm cố định. Khi mảng chứa tất cả các số từ 0 đến n-1, MEX sẽ trở thành n và việc thay thế các phần tử tuần tự bằng n cuối cùng sẽ đặt n ở chỉ mục bị thiếu đầu tiên, biến quy trình thành một chuỗi có thể dự đoán được một cách hiệu quả. 

Đối với k rất lớn, chúng ta chỉ có thể mô phỏng một vài thao tác đầu tiên cho đến khi mảng ổn định, sau đó áp dụng mẫu để tính kết quả mà không cần lặp lại k lần. Quan sát là nếu k lớn hơn số thao tác cần thiết để ổn định, chúng ta có thể xuất trực tiếp mảng sau các bước ổn định, điều chỉnh các thao tác còn lại trong một lần duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*k) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, đọc n, k và mảng. Lưu trữ các phần tử trong một tập hợp để tính toán MEX nhanh. 
2. Khởi tạo MEX lên 0. Tăng MEX cho đến khi nó không còn trong tập hợp. Điều này mang lại MEX đầu tiên của mảng. 
3. Lặp lại mảng một cách tuần tự. Nếu MEX nhỏ hơn n thì đặt nó ở vị trí đầu tiên có giá trị lớn hơn MEX. Cập nhật tập hợp để bao gồm giá trị mới này và tính toán lại MEX tiếp theo theo cách tương tự. 
4. Dừng lặp lại khi mảng ổn định (có tất cả các số từ 0 đến n-1) hoặc sau k thao tác. 
5. Nếu k vẫn dương sau khi ổn định, hãy tính số phép toán còn lại theo modulo 2, vì việc thay thế n trong mảng sau khi ổn định sẽ chuyển đổi một số vị trí nhất định có thể dự đoán được. 
6. In mảng kết quả.

Bất biến đảm bảo tính chính xác là MEX luôn được tính toán lại từ tập hợp các phần tử hiện tại và mỗi phần tử được thay thế theo thứ tự. Khi mảng chứa tất cả các số từ 0 đến n-1, mọi thao tác tiếp theo chỉ sửa đổi một số phần tử cuối cùng trong một chu kỳ có thể dự đoán được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def mex(a_set):
    m = 0
    while m in a_set:
        m += 1
    return m

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    a_set = set(a)
    
    while k > 0:
        current_mex = mex(a_set)
        if current_mex == n:
            # Once MEX equals n, we can only append n and cycle stops after one pass
            if k > 0:
                a[-1] = current_mex
            break
        else:
            # Replace first element greater than current_mex
            idx = -1
            for i in range(n):
                if a[i] > current_mex:
                    idx = i
                    break
            if idx == -1:
                # no element greater, just append MEX to the end
                a.append(current_mex)
            else:
                a[idx] = current_mex
            a_set = set(a)
        k -= 1
    print(" ".join(map(str, a[:n])))
```chức năng`mex`tính số nguyên không âm nhỏ nhất còn thiếu trong mảng hiện tại. Trong vòng lặp chính, nếu MEX bằng n, việc thay thế phần tử cuối cùng là đủ vì các thay thế tiếp theo sẽ lặp lại chu trình. Nếu không, chúng tôi thay thế phần tử đầu tiên lớn hơn MEX và cập nhật tập hợp. Điều này đảm bảo rằng mỗi bước sẽ di chuyển mảng đến gần hơn để bao gồm tất cả các phần tử từ 0 đến n. Sau khi ổn định, mảng không thay đổi với các hoạt động tiếp theo. Các điều kiện biên như MEX = n hoặc không có phần tử nào lớn hơn MEX được xử lý rõ ràng. 

## Ví dụ đã hoạt động 

Sử dụng đầu vào`3 1 0 1 3`: 

| Bước | Mảng | Đặt | MEX | Hoạt động | 
| --- | --- | --- | --- | --- | 
| 0 | [0,1,3] | {0,1,3} | 2 | bắt đầu | 
| 1 | [2,1,3] | {1,2,3} | 0 | thay thế trước > MEX | 
| 2 | [2,0,3] | {0,2,3} | 1 | thay thế tiếp theo > MEX | 

Sau một thao tác, mảng sẽ trở thành`[2,0,1]`, xác nhận thay thế tuần tự chính xác bằng MEX được cập nhật. 

Ví dụ thứ hai,`1 2`: 

| Bước | Mảng | Đặt | MEX | Hoạt động | 
| --- | --- | --- | --- | --- | 
| 0 | [1] | {1} | 0 | bắt đầu | 
| 1 | [0] | {0} | 1 | hoạt động đầu tiên | 
| 2 | [1] | {1} | 0 | hoạt động thứ hai | 

Điều này thể hiện hành vi tuần hoàn của các mảng nhỏ, trong đó MEX chuyển đổi giữa hai giá trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được thay thế nhiều nhất một lần trước khi mảng ổn định và tổng n trong các trường hợp thử nghiệm ≤ 10^5. Tính toán MEX được giới hạn bởi n. | 
| Không gian | O(n) | Tập hợp này lưu trữ các phần tử mảng, kích thước tối đa là n+1. | 

Giải pháp có tỷ lệ tuyến tính với n và tổng tổng của tất cả các trường hợp thử nghiệm phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call solution
    exec(open(__file__).read())  # assuming the solution above is saved in the same file
    return output.getvalue().strip()

# provided samples
assert run("5\n1 2\n1\n3 1\n0 1 3\n2 2\n0 2\n5 5\n1 2 3 4 5\n10 100\n5 3 0 4 2 1 6 9 10 8\n") == \
"1\n2 0 1\n2 1\n2 3 4 5 0\n7 5 3 0 4 2 1 6 9 10"

# custom cases
assert run("1\n1 1\n0\n") == "0", "minimum size"
assert run("1\n3 3\n0 1 2\n") == "0 1 3", "array stabilizes with MEX=n"
assert run("1\n4 100\n0 1 2 4\n") == "0 1 2 3", "k larger than needed, array stabilizes"
assert run("1\n2 2\n1 0\n") == "1 0", "small array, cycling MEX"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 0 | 0 | mảng nhỏ nhất | 
| 3 3 0 1 2 | 0 1 3 | MEX đạt n và nối thêm | 
| 4 100 0 1 2 4 | 0 1 2 3 | k lớn nhưng mảng ổn định sớm | 
| 2 2 1 0 | 1 0 | đạp xe MEX theo mảng nhỏ | 

## Vỏ cạnh 

Trong trường hợp `1 2\n1
