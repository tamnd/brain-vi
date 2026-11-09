---
title: "CF 1454B - Đấu giá giá thầu duy nhất"
description: "Chúng tôi được yêu cầu xác định người chiến thắng trong \"Đấu giá giá thầu duy nhất\" đơn giản hóa. Mỗi người tham gia chọn một số và người chiến thắng là người tham gia đã chọn một số mà không ai khác chọn và đó là số nhỏ nhất trong số tất cả các số duy nhất như vậy."
date: "2026-06-11T02:51:52+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1454
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 686 (Div. 3)"
rating: 800
weight: 1454
solve_time_s: 83
verified: true
draft: false
---

[CF 1454B - Đấu giá giá thầu duy nhất](https://codeforces.com/problemset/problem/1454/B) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xác định người chiến thắng trong "Đấu giá giá thầu duy nhất" được đơn giản hóa. Mỗi người tham gia chọn một số và người chiến thắng là người tham gia đã chọn một số mà không ai khác chọn và đó là số nhỏ nhất trong số tất cả các số duy nhất như vậy. Nếu không có số duy nhất thì kết quả là -1. Đầu vào cung cấp nhiều trường hợp thử nghiệm, mỗi trường hợp có một số người tham gia và số được chọn của họ và chúng tôi phải trả về chỉ số dựa trên 1 của người tham gia chiến thắng cho mỗi trường hợp. 

Những hạn chế là rất quan trọng. Số lượng người tham gia tối đa là`n`, và có thể có tới`2·10^5`người tham gia trong tất cả các trường hợp thử nghiệm. Điều này loại trừ mọi giải pháp so sánh mọi cặp số (`O(n^2)`), vì trong trường hợp xấu nhất, điều đó có thể yêu cầu`4·10^10`hoạt động, vượt xa những gì có thể chạy trong một giây. Lý tưởng nhất là chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm`O(n)`. 

Các trường hợp có biên không rõ ràng bao gồm các tình huống trong đó tất cả các số đều trùng lặp, tồn tại một người tham gia hoặc số nhỏ nhất được lặp lại nhưng số lớn hơn một chút là duy nhất. Ví dụ, đối với đầu vào`[1,1,2]`, người chiến thắng là người tham gia đã chọn`2`, không`1`, bởi vì`1`không phải là duy nhất Tương tự, nếu tất cả các số đều`[1,1,2,2]`, không có người chiến thắng. Đây là những điều tinh tế vì cách tiếp cận ngây thơ chỉ tìm kiếm số nhỏ nhất mà không kiểm tra tính duy nhất sẽ thất bại. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: đối với mỗi số, hãy đếm số lần nó xuất hiện, sau đó quét tất cả các số để tìm số nhỏ nhất xuất hiện đúng một lần. Việc đếm có thể được thực hiện bằng các vòng lặp lồng nhau hoặc các lần quét lặp lại. Điều này hoạt động chính xác vì nó thực hiện rõ ràng định nghĩa vấn đề. Tuy nhiên, nó quá chậm. Đếm tần số của mỗi số bằng`O(n)`quét cho từng kết quả trường hợp thử nghiệm trong`O(n^2)`cho mỗi trường hợp thử nghiệm, điều này không thể chấp nhận được đối với các giới hạn trên của`n`. 

Thông tin chi tiết quan trọng là chúng tôi chỉ cần tần số của mỗi số chứ không phải vị trí của nó trong lần quét đầu tiên. Chúng ta có thể sử dụng từ điển để ánh xạ các số theo số lần xuất hiện của chúng. Sau khi có bản đồ tần số, chúng tôi có thể lọc các số xuất hiện chính xác một lần và chọn số tối thiểu trong số đó. Sau đó, một lần duyệt qua danh sách ban đầu sẽ xác định chỉ số dựa trên 1 của người chiến thắng. Cách tiếp cận này là tuyến tính: một lượt để xây dựng bản đồ đếm và một lượt để tìm chỉ mục,`O(n)`mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một từ điển trống để đếm số lần xuất hiện của mỗi số. 
2. Lặp lại các số của người tham gia, tăng dần số lượng của từng số trong từ điển. Điều này tạo ra một bản đồ tần số về số lần mỗi số xuất hiện. 
3. Xác định tất cả các số có số đếm chính xác là một. Nếu không có số nào như vậy thì trả về ngay -1 cho test case này. 
4. Tìm số nhỏ nhất trong số các số duy nhất đó. Đây là con số chiến thắng. 
5. Lặp lại danh sách các số ban đầu và tìm chỉ số xuất hiện đầu tiên của số trúng thưởng. Vì số trúng thưởng là duy nhất nên sẽ có đúng một lần xuất hiện như vậy. 
6. Xuất chỉ mục, điều chỉnh lập chỉ mục theo 1. 

Tại sao nó hoạt động: từ điển đảm bảo số lần đếm chính xác và việc quét số duy nhất tối thiểu đảm bảo rằng chúng ta đang tuân theo các quy tắc của vấn đề. Vì sau đó chúng tôi quét chỉ mục của số đó nên chúng tôi sẽ ánh xạ lại giá trị một cách chính xác cho người tham gia. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        count = {}
        for num in a:
            count[num] = count.get(num, 0) + 1
        # collect unique numbers
        unique_numbers = [num for num, freq in count.items() if freq == 1]
        if not unique_numbers:
            print(-1)
            continue
        winner_value = min(unique_numbers)
        # find 1-based index
        for i, num in enumerate(a):
            if num == winner_value:
                print(i + 1)
                break

if __name__ == "__main__":
    main()
```Đầu tiên, mã sẽ đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp, nó đọc số lượng người tham gia và số lượng của họ. các`count`từ điển lưu trữ tần số của mỗi số. Việc hiểu danh sách sẽ lọc ra các số duy nhất. Nếu không có số duy nhất, chúng ta xuất -1. Nếu không, chúng tôi sẽ tìm số duy nhất nhỏ nhất và quét mảng ban đầu để tìm chỉ mục của người tham gia. Việc sử dụng`enumerate`đảm bảo việc lập chỉ mục dựa trên 1 trở nên đơn giản bằng cách thêm một vào chỉ mục vòng lặp. 

## Ví dụ đã hoạt động 

### Mẫu 1:`[2,1,3]`| Bước | Biến | Giá trị | 
| --- | --- | --- | 
| Đếm tần số | đếm | {2:1,1:1,3:1} | 
| Số duy nhất | số_duy nhất | [2,1,3] | 
| Nhỏ nhất độc đáo | người chiến thắng_value | 1 | 
| Quét chỉ mục | tôi, số | i=0,num=2 → bỏ qua; i=1,num=1 → khớp → in 2 | 

Điều này cho thấy số duy nhất nhỏ nhất được tìm thấy chính xác và chỉ số dựa trên 1 chính xác được trả về. 

### Mẫu 2:`[1,1,5,5,4,4]`| Bước | Biến | Giá trị | 
| --- | --- | --- | 
| Đếm tần số | đếm | {1:2,5:2,4:2} | 
| Số duy nhất | số_duy nhất | [] | 
| Không có số duy nhất | - | in -1 | 

Điều này thể hiện việc xử lý trường hợp không có số duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt để đếm, một lượt để tìm mức tối thiểu, một lượt để xác định chỉ mục | 
| Không gian | O(n) | Từ điển tần số lưu trữ tối đa n khóa | 

Xét tổng của tất cả`n`không vượt quá`2·10^5`, giải pháp này chạy hiệu quả trong điều kiện hạn chế về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    main()
    return out.getvalue().strip()

# Provided samples
assert run("6\n2\n1 1\n3\n2 1 3\n4\n2 2 2 3\n1\n1\n5\n2 3 2 4 2\n6\n1 1 5 5 4 4\n") == "-1\n2\n4\n1\n2\n-1", "samples"

# Custom cases
assert run("1\n1\n1\n") == "1", "single participant"
assert run("1\n5\n5 5 5 5 5\n") == "-1", "all equal numbers"
assert run("1\n6\n1 2 3 4 5 6\n") == "1", "all unique numbers"
assert run("1\n5\n2 3 3 2 1\n") == "5", "smallest unique is last"
assert run("1\n7\n1 1 2 2 3 3 4\n") == "7", "last element unique"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 người tham gia`[1]`| 1 | số nhỏ nhất và duy nhất | 
| tất cả đều bình đẳng`[5 5 5 5 5]`| -1 | không có số duy nhất | 
| tất cả đều độc đáo`[1 2 3 4 5 6]`| 1 | chọn nhỏ nhất duy nhất một cách chính xác | 
| phần tử cuối cùng duy nhất`[2 3 3 2 1]`| 5 | tìm thấy chỉ mục chính xác ở cuối | 
| duy nhất cuối cùng`[1 1 2 2 3 3 4]`| 7 | xử lý tính duy nhất ở phần tử cuối cùng | 

## Vỏ cạnh 

Nếu đầu vào chỉ chứa một người tham gia, ví dụ:`[1]`, thuật toán đếm chính xác một lần xuất hiện, nhận ra nó là duy nhất và trả về chỉ số 1. Đối với các mảng hoàn toàn bằng nhau, ví dụ:`[5,5,5,5,5]`, bản đồ tần số không có giá trị với số đếm 1, kích hoạt đầu ra -1. Trong trường hợp số duy nhất nhỏ nhất nằm ở cuối mảng, ví dụ:`[2,3,3,2,1]`, thuật toán xác định chính xác`1`là người chiến thắng và tìm thấy chỉ mục của nó. Các trường hợp biên này xác nhận rằng thuật toán xử lý các ranh giới, phần tử đơn lẻ, bản sao và vị trí mảng một cách chính xác.
