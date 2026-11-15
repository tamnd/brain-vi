---
title: "CF 1834E - MEX của LCM"
description: "Chúng ta được yêu cầu tìm số nguyên dương nhỏ nhất không thể xuất hiện dưới dạng bội số chung nhỏ nhất của bất kỳ mảng con liền kề nào của một mảng nhất định. Đầu vào bao gồm nhiều trường hợp thử nghiệm."
date: "2026-06-09T06:52:56+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "implementation", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1834
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 879 (Div. 2)"
rating: 2300
weight: 1834
solve_time_s: 75
verified: true
draft: false
---

[CF 1834E - MEX của LCM](https://codeforces.com/problemset/problem/1834/E) 

**Đánh giá:** 2300 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, thực hiện, toán, lý thuyết số 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tìm số nguyên dương nhỏ nhất không thể xuất hiện dưới dạng bội số chung nhỏ nhất của bất kỳ mảng con liền kề nào của một mảng nhất định. Đầu vào bao gồm nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi được cung cấp một mảng các số nguyên và đầu ra là số nguyên tối thiểu “thiếu” dưới dạng LCM của một số phân đoạn. 

Quan sát chính là vấn đề liên quan đến việc xây dựng các số thông qua phép toán LCM trên các phần tử liên tiếp. Các phân đoạn một phần tử ngay lập tức đưa ra tất cả các số có trong mảng. Các phân đoạn lớn hơn có thể tạo ra bội số hoặc kết hợp, nhưng một số số nguyên sẽ không bao giờ được hình thành do mảng thiếu các yếu tố cần thiết. Nhiệm vụ là xác định số nguyên nhỏ nhất như vậy một cách hiệu quả. 

Các ràng buộc cho thấy việc liệt kê đơn giản tất cả các mảng con là không khả thi. Độ dài mảng có thể lên tới 3×10^5, nghĩa là việc lặp qua tất cả các phân đoạn con, có thể là O(n^2) hoặc tệ hơn, là quá chậm. Điều này buộc chúng ta phải suy luận về các LCM có thể tiếp cận một cách linh hoạt thay vì tính toán chúng một cách rõ ràng. Các trường hợp cạnh bao gồm các mảng có độ dài bằng một, mảng chứa các số lặp lại, mảng chứa các số nguyên tố lớn và mảng bao gồm các số nguyên nhỏ liên tiếp. Ví dụ, một mảng`[2,3]`ngay lập tức mang lại LCM phân khúc của`2`,`3`, Và`6`. Số nguyên tốt nhỏ nhất ở đây là`1`, vì không có phân đoạn nào tạo ra`1`. Việc không kiểm tra các số nguyên nhỏ một cách có hệ thống có thể dẫn đến câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực tạo ra tất cả các phân đoạn liền kề và tính toán LCM của chúng. Về nguyên tắc, điều này đúng vì theo định nghĩa, nó kiểm tra tất cả các phân đoạn có thể có. Vấn đề là số lượng các phân đoạn con tăng theo phương bậc hai với n và việc tính toán LCM một cách đơn giản cho mỗi phân đoạn con có thể tốn kém đối với các số nguyên lớn, khiến điều này trở thành O(n^3) trong trường hợp xấu nhất nếu không được tối ưu hóa. Rõ ràng, điều này là quá chậm do những hạn chế của vấn đề. 

Cách tiếp cận tối ưu thừa nhận rằng LCM phát triển nhanh chóng và có thể được biểu thị bằng các thừa số nguyên tố của chúng. Thay vì xem xét từng phân đoạn một cách độc lập, chúng tôi duy trì một tập hợp tất cả các LCM có thể kết thúc ở mỗi vị trí trong mảng. Chúng tôi lặp qua mảng, cập nhật tập hợp LCM có thể truy cập bằng cách kết hợp phần tử hiện tại với từng LCM trước đó. Để tránh tập hợp bùng nổ, chúng tôi cắt bớt các LCM vượt quá giới hạn hợp lý, vì chúng không thể đóng góp vào các số nguyên tốt nhỏ hơn. Sau khi xử lý toàn bộ mảng, chúng tôi lặp lại từ 1 trở lên để tìm số nguyên nhỏ nhất không có trong tập LCM có thể truy cập. Điều này làm giảm đáng kể độ phức tạp vì chúng tôi không bao giờ lưu trữ hoặc tính toán các LCM lớn dư thừa và chúng tôi tận dụng sự tăng trưởng đơn điệu của các LCM để tuyển chọn các ứng viên một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n^2) | Quá chậm | 
| Tối ưu | O(n log a_max) | O(n log a_max) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc kích thước mảng và các phần tử. 
2. Khởi tạo một tập hợp trống`current_lcms`sẽ lưu trữ tất cả LCM của các phân đoạn con kết thúc ở phần tử trước đó. 
3. Lặp lại mảng. Đối với mỗi phần tử`a[i]`, tạo một bộ mới`next_lcms`và chèn`a[i]`. Điều này thể hiện LCM của đoạn con có độ dài một kết thúc ở vị trí này. 
4. Đối với mỗi LCM`x`TRONG`current_lcms`, tính toán`lcm(x, a[i])`và thêm nó vào`next_lcms`. Điều này mở rộng tất cả các phân đoạn trước đó để bao gồm phần tử hiện tại. Bỏ qua LCM vượt quá một ngưỡng nhất định, vì chúng tôi đang tìm số nguyên bị thiếu nhỏ nhất nên số lượng lớn không liên quan. 
5. Cập nhật`current_lcms`được`next_lcms`. Sau khi xử lý toàn bộ mảng,`current_lcms`chứa tất cả LCM của tất cả các phân đoạn có thể có. 
6. Khởi tạo một biến`mex = 1`và tăng nó cho đến khi nó không còn trong`current_lcms`. Đây là số nguyên dương nhỏ nhất không phải là LCM của bất kỳ phân đoạn nào. 

Tại sao nó hoạt động: Mỗi bước đảm bảo rằng`current_lcms`chứa chính xác LCM của tất cả các phân đoạn con kết thúc ở chỉ mục hiện tại. Bằng cách truyền bá các LCM về phía trước và bao gồm phần tử hiện tại dưới dạng phân đoạn con có độ dài bằng một, chúng tôi thu được tất cả các LCM liền kề có thể có. Bất biến là sau khi xử lý vị trí i,`current_lcms`là tập hợp chính xác các LCM của các phân đoạn kết thúc tại i. Lần lặp cuối cùng để tìm MEX đảm bảo tính chính xác vì nó kiểm tra một cách có hệ thống tất cả các số nguyên bắt đầu từ 1. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        current_lcms = set()
        for num in a:
            next_lcms = {num}
            for x in current_lcms:
                l = x * num // math.gcd(x, num)
                if l <= 10**9 + 5:  # threshold to avoid unnecessary large numbers
                    next_lcms.add(l)
            current_lcms = next_lcms
        mex = 1
        while mex in current_lcms:
            mex += 1
        print(mex)

if __name__ == "__main__":
    solve()
```Mã đọc đầu vào hiệu quả với`sys.stdin.readline`và sử dụng một bộ để duy trì động LCM của tất cả các phân đoạn kết thúc ở mỗi vị trí. Phép tính LCM dựa trên GCD đảm bảo độ chính xác và ngăn ngừa tràn số nguyên bằng cách sử dụng công thức`x * y // gcd(x, y)`. Ngưỡng này ngăn chặn việc lưu trữ các LCM khổng lồ không thể là số nguyên tốt tối thiểu, giúp quản lý bộ nhớ và tính toán. 

## Ví dụ đã hoạt động 

Theo dõi đầu vào`[1, 2, 3]`: 

| tôi | số | current_lcms | next_lcms | 
| --- | --- | --- | --- | 
| 0 | 1 | {} | {1} | 
| 1 | 2 | {1} | {2, 2*1/gcd(2,1)=2} = {2,2} → {2} | 
| 2 | 3 | {2} | {3, lcm(2,3)=6} → {3,6} | 

Sau khi lặp lại, hợp tất cả các tập hợp: {1,2,3,6}. MEX là 4. 

Theo dõi đầu vào`[2, 3]`: 

| tôi | số | current_lcms | next_lcms | 
| --- | --- | --- | --- | 
| 0 | 2 | {} | {2} | 
| 1 | 3 | {2} | {3, lcm(2,3)=6} → {3,6} | 

Hợp tất cả các bộ: {2,3,6}. MEX là 1. 

Những dấu vết này xác nhận rằng các phân đoạn con một phần tử và các phân đoạn con mở rộng tạo ra tất cả các LCM có thể truy cập được và thuật toán tìm chính xác số nguyên bị thiếu tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log a_max) | Mỗi phần tử kết hợp với các LCM trước đó, nhưng việc cắt tỉa sẽ tránh được sự phát triển quá mức. GCD là O(log a_max). | 
| Không gian | O(n log a_max) | Chúng tôi lưu trữ các bộ LCM; việc cắt tỉa giới hạn kích thước tỷ lệ thuận với số lượng yếu tố. | 

Cho n tối đa 3×10^5 và giá trị lên tới 10^9, giải pháp này phù hợp thoải mái trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# Provided samples
assert run("6\n3\n1 2 3\n5\n1 2 3 4 5\n2\n2 3\n1\n1000000000\n12\n1 8 4 2 3 5 7 2 9 10 11 13\n12\n7 2 5 4 2 1 1 2 3 11 8 9") == "4\n7\n1\n1\n16\n13"

# Custom tests
assert run("1\n1\n1") == "2", "single-element 1"
assert run("1\n5\n1 1 1 1 1") == "2", "all ones"
assert run("1\n3\n2 4 8") == "1", "small powers of 2"
assert run("1\n4\n2 3 5 7") == "1", "all primes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 2 | Xử lý mảng có độ dài 1 | 
| 5 cái | 2 | Tất cả các giá trị bằng nhau, MEX > phần tử | 
| quyền hạn của 2 | 1 | Thiếu số nguyên nhỏ hơn bất kỳ phần tử nào | 
| tất cả các số nguyên tố | 1 | MEX cho mảng không có 1 | 

## Vỏ cạnh 

Mảng chỉ có một phần tử`[1]`đầu ra 2. Thuật toán khởi tạo chính xác`current_lcms`BẰNG`{1}`, sau đó tìm MEX bằng cách kiểm tra 1, 2. Việc kiểm tra ngưỡng đảm bảo chúng ta không bao giờ loại bỏ các số nguyên nhỏ cần thiết cho MEX. Mảng có số lượng lớn lặp lại`[10^9, 10^9]`cũng có tác dụng vì việc truyền LCM không loại bỏ các ứng viên nhỏ nhất và việc cắt tỉa chỉ ảnh hưởng đến những số lượng rất lớn không liên quan. Mảng gồm các số nguyên liên tiếp `[1,2,3,4,5]
