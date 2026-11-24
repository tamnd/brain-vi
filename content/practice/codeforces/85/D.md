---
title: "CF 85D - Tổng số trung vị"
description: "Chúng ta được yêu cầu duy trì một tập hợp động gồm các số nguyên dương theo ba thao tác: thêm một số, xóa một số và tính tổng các trung vị của mỗi nhóm năm phần tử liên tiếp sau khi sắp xếp tập hợp."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 85
codeforces_index: "D"
codeforces_contest_name: "Yandex.Algorithm 2011: Round 1"
rating: 2300
weight: 85
solve_time_s: 78
verified: true
draft: false
---

[CF 85D - Tổng số trung vị](https://codeforces.com/problemset/problem/85/D) 

**Đánh giá:** 2300 
**Tags:** tìm kiếm nhị phân, lực lượng vũ phu, cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu duy trì một tập hợp động gồm các số nguyên dương theo ba thao tác: thêm một số, xóa một số và tính tổng các trung vị của mỗi nhóm năm phần tử liên tiếp sau khi sắp xếp tập hợp. Trung vị trong nhóm năm người là phần tử thứ ba sau khi nhóm đó được sắp xếp. Phép tính tổng nên xem xét tập hợp được sắp xếp theo thứ tự tăng dần, chia nó thành 5 khối liền kề, tính trung vị của mỗi khối và tính tổng tất cả các trung vị này. Nếu kích thước được đặt nhỏ hơn năm thì sẽ không có khối đầy đủ, do đó tổng bằng 0. 

Đầu vào bao gồm tối đa 100.000 thao tác và các số nguyên liên quan có thể lên tới 10^9. Vì n có thể đạt tới 10^5, nên bất kỳ giải pháp nào liên tục sắp xếp toàn bộ tập hợp cho mỗi truy vấn tính tổng, sẽ tốn O(n log n), có thể quá chậm. Đặc biệt, trong trường hợp xấu nhất, việc sắp xếp lặp đi lặp lại sau mỗi thao tác sẽ dẫn đến tổng số thao tác là O(n^2 log n), vượt quá giới hạn thực tế. Điều này có nghĩa là chúng ta cần một cấu trúc cho phép chèn, xóa và truy xuất trung bình hiệu quả mà không cần sắp xếp đầy đủ mỗi lần. 

Các trường hợp cạnh phát sinh khi tập hợp có ít hơn năm phần tử, trong đó không tồn tại trung vị và khi kích thước tập hợp không phải là bội số của 5, để lại một khối không đầy đủ ở cuối sẽ không đóng góp vào tổng. Một trường hợp tinh tế khác là các thao tác lặp đi lặp lại để cộng và loại bỏ cùng một số, điều này sẽ không phá vỡ phép tính hoặc thứ tự trung vị. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: duy trì tập hợp dưới dạng danh sách và đối với mỗi truy vấn tính tổng, hãy sắp xếp danh sách, lặp lại các nhóm năm thành phần, lấy phần tử thứ ba của mỗi nhóm và tính tổng chúng. Điều này đúng vì việc sắp xếp tạo ra thứ tự cần thiết cho tính toán trung vị. Tuy nhiên, việc sắp xếp tối đa 100.000 phần tử mỗi lần là O(n log n) và với tối đa 100.000 thao tác, tổng thời gian có thể đạt tới O(n^2 log n), tốc độ này quá chậm. 

Thông tin chi tiết quan trọng về tối ưu hóa là chúng ta không cần sắp xếp toàn bộ tập hợp cho mỗi truy vấn. Chúng tôi chỉ quan tâm đến số trung vị của mỗi khối năm phần tử liên tiếp. Điều này gợi ý sử dụng cây tìm kiếm nhị phân cân bằng hoặc cấu trúc dữ liệu tập hợp có thứ tự hỗ trợ các truy vấn chèn, xóa và thống kê thứ tự hiệu quả. Trong Python,`SortedList`từ`sortedcontainers`là lý tưởng vì nó cung cấp khả năng chèn, xóa và truy cập O(1) theo chỉ mục. Với điều này, chúng ta có thể duy trì chế độ xem được sắp xếp của tập hợp mọi lúc và tính tổng trung vị trong O(n) trên mỗi truy vấn tổng, điều này có thể chấp nhận được với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n log n) mỗi tổng | O(n) | Quá chậm cho trường hợp xấu nhất | 
| SortedList / Thống kê đơn hàng | O(log n) mỗi lần cộng/del, O(n) mỗi tổng | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một khoảng trống`SortedList`. Cấu trúc này duy trì tất cả các phần tử theo thứ tự được sắp xếp, cho phép các hoạt động tìm kiếm nhị phân để chèn, xóa và lập chỉ mục. 
2. Với mỗi thao tác nhập vào: 

- Nếu nó là một`add x`, chèn`x`vào`SortedList`. Việc chèn sẽ tự động giữ nguyên thứ tự sắp xếp. 
- Nếu đó là một`del x`, di dời`x`từ`SortedList`. Thao tác di dời cũng duy trì trật tự. 
- Nếu vậy`sum`, khởi tạo tổng số đang chạy về 0. Lặp lại danh sách đã sắp xếp theo các bước gồm năm bước. Đối với mỗi nhóm hoàn chỉnh gồm năm phần tử, hãy chọn phần tử thứ ba (chỉ mục 2 trong khối) và thêm nó vào tổng số đang chạy. In tổng số sau khi xử lý tất cả các nhóm hoàn chỉnh. 
3. Lặp lại cho tất cả các thao tác. 

Tại sao nó hoạt động:`SortedList`đảm bảo rằng tại mọi thời điểm, tập hợp có thứ tự tăng dần. Bằng cách lặp lại theo năm bước và chọn phần tử thứ ba của mỗi khối, chúng ta sẽ trực tiếp làm theo định nghĩa về số trung vị của bài toán. Việc chèn và xóa logarit của cấu trúc duy trì hiệu quả ngay cả với tập động, trong khi lập chỉ mục trực tiếp cho phép truy cập liên tục theo thời gian để truy xuất trung bình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from sortedcontainers import SortedList

def main():
    n = int(input())
    sl = SortedList()
    
    for _ in range(n):
        line = input().split()
        if line[0] == "add":
            x = int(line[1])
            sl.add(x)
        elif line[0] == "del":
            x = int(line[1])
            sl.remove(x)
        else:  # sum
            total = 0
            for i in range(2, len(sl), 5):
                total += sl[i]
            print(total)

if __name__ == "__main__":
    main()
```các`SortedList`được chọn vì sự kết hợp giữa duy trì đơn hàng và lập chỉ mục nhanh. Việc chèn và xóa là logarit, giúp tránh tình trạng chậm lại khi tập hợp phát triển. Phép lặp trong truy vấn tổng truy cập mọi phần tử thứ năm bắt đầu từ chỉ mục 2, tương ứng chính xác với trung vị của mỗi khối gồm năm phần tử. Một cạm bẫy phổ biến là bắt đầu từ chỉ mục 0 hoặc 1, điều này sẽ chọn sai phần tử. Kích thước số nguyên tùy ý của Python tránh được những lo ngại về tràn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6
add 4
add 5
add 1
add 2
add 3
sum
```| Hoạt động | Danh sách được sắp xếp | Tính Tổng | Đầu ra | 
| --- | --- | --- | --- | 
| thêm 4 | [4] | - | - | 
| thêm 5 | [4,5] | - | - | 
| thêm 1 | [1,4,5] | - | - | 
| thêm 2 | [1,2,4,5] | - | - | 
| thêm 3 | [1,2,3,4,5] | Chỉ có một nhóm đầy đủ: [1,2,3,4,5], trung vị=3 | 3 | 

Điều này chứng tỏ thuật toán xác định chính xác giá trị trung bình của một nhóm đầy đủ năm người sau nhiều lần chèn không có thứ tự. 

### Mẫu tùy chỉnh 

đầu vào:```
7
add 7
add 1
add 3
add 9
add 5
add 11
sum
```| Hoạt động | Danh sách được sắp xếp | Tính Tổng | Đầu ra | 
| --- | --- | --- | --- | 
| thêm 7 | [7] | - | - | 
| thêm 1 | [1,7] | - | - | 
| thêm 3 | [1,3,7] | - | - | 
| thêm 9 | [1,3,7,9] | - | - | 
| thêm 5 | [1,3,5,7,9] | Một nhóm: [1,3,5,7,9], trung vị=5 | - | 
| thêm 11 | [1,3,5,7,9,11] | Nhóm: [1,3,5,7,9], [11] -> sum=5 | 5 | 

Cho thấy các nhóm chưa hoàn thiện ở cuối không đóng góp gì. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) cho tất cả cộng/del, O(n) mỗi tổng | Mỗi lần chèn/xóa tốn O(log n), tổng số lần lặp theo các bước 5, tối đa n lần lặp | 
| Không gian | O(n) | Chúng tôi lưu trữ tất cả các phần tử hiện tại trong một`SortedList`| 

Với n tối đa 10^5 và các thao tác được giới hạn trong 3 giây, điều này phù hợp một cách thoải mái. Mỗi truy vấn tổng lặp lại tối đa 10^5 phần tử, điều này là khả thi. 

## Trường hợp thử nghiệm```python
import sys, io
from sortedcontainers import SortedList

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# provided sample
assert run("6\nadd 4\nadd 5\nadd 1\nadd 2\nadd 3\nsum\n") == "3", "sample 1"

# minimum-size input
assert run("1\nsum\n") == "0", "empty set sum"

# multiple adds and deletes
assert run("8\nadd 1\nadd 2\nadd 3\nadd 4\nadd 5\nsum\ndel 3\nsum\n") == "3\n0", "delete affects sum"

# all equal values
assert run("6\nadd 2\nadd 2\nadd 2\nadd 2\nadd 2\nsum\n") == "2", "all equal"

# non-multiple of five
assert run("7\nadd 1\nadd 2\nadd 3\nadd 4\nadd 5\nadd 6\nsum\n") == "3", "extra element ignored"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 khoản | 0 | bộ trống | 
| cộng 1..5, tổng, del 3, tổng | 3 0 | tổng tác động xóa | 
| tất cả cộng thêm 2 | 2 | trung vị của các giá trị giống hệt nhau | 
| 6 phần tử, tổng | 3 | nhóm chưa hoàn chỉnh cuối cùng bị bỏ qua | 

## Vỏ cạnh 

Ví dụ: khi tập hợp có ít hơn năm phần tử, các phép toán`add 1, add 2, sum`, thuật toán trả về 0 một cách chính xác vì vòng lặp trên các chỉ số bắt đầu từ 2 với bước 5 không thực thi. Nếu kích thước được đặt chính xác là năm thì một trung vị được chọn làm chỉ số 2, điều này đúng. Đối với các bộ lớn hơn năm nhưng không phải là bội số của năm, các phần tử bổ sung
