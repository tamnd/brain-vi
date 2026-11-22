---
title: "CF 34C - Số trang"
description: "Chúng tôi được yêu cầu xử lý chuỗi số trang do người dùng nhập để in. Đầu vào là một chuỗi các số nguyên dương được phân tách bằng dấu phẩy, chẳng hạn như 1,2,3,1,1,2,6,6,2. Một số số có thể lặp lại, có thể không liên tiếp."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "expression-parsing", "implementation", "sortings", "strings"]
categories: ["algorithms"]
codeforces_contest: 34
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 34 (Div. 2)"
rating: 1300
weight: 34
solve_time_s: 67
verified: true
draft: false
---
[CF 34C - Số trang](https://codeforces.com/problemset/problem/34/C) 

**Đánh giá:** 1300 
**Thẻ:** phân tích biểu thức, triển khai, sắp xếp, chuỗi 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xử lý chuỗi số trang do người dùng nhập để in. Đầu vào là một chuỗi các số nguyên dương được phân tách bằng dấu phẩy, chẳng hạn như`1,2,3,1,1,2,6,6,2`. Một số số có thể lặp lại, có thể không liên tiếp. Mục tiêu của chúng tôi là tạo ra một cách trình bày cô đọng, chuẩn mực: các số liên tiếp được hợp nhất thành các phạm vi, các số trùng lặp được loại bỏ và các trang đơn được trình bày không có dấu gạch ngang. 

Chẳng hạn, từ`1,2,3,1,1,2,6,6,2`, sau khi loại bỏ các bản sao, chúng tôi nhận được`1,2,3,6`, rồi các số liên tiếp`1,2,3`được sáp nhập vào`1-3`, đưa ra kết quả cuối cùng`1-3,6`. 

Đầu vào có tối đa 100 số, mỗi số không vượt quá 1000. Những ràng buộc này có nghĩa là chúng tôi không cần cấu trúc dữ liệu được tối ưu hóa cao; ngay cả các giải pháp O(n log n) cũng sẽ chạy thoải mái trong giới hạn 2 giây. Một thuật toán đơn giản quét mảng nhiều lần cũng sẽ hoạt động, nhưng chúng tôi vẫn muốn cẩn thận với các phạm vi trùng lặp và liên tiếp. 

Các trường hợp phức tạp bao gồm các chuỗi trong đó tất cả các số đều giống nhau, các chuỗi có khoảng trống và các chuỗi có các bản sao lặp lại xen kẽ với các số khác. Ví dụ,`5,5,5`nên xuất ra`5`, Và`1,3,2`sau khi loại bỏ trùng lặp và sắp xếp sẽ là`1-3`vì đầu ra phải theo thứ tự tăng dần. Việc triển khai bất cẩn có thể bỏ sót việc loại bỏ trùng lặp hoặc định dạng các số đơn không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là lặp lại dữ liệu đầu vào, kiểm tra từng số với tất cả các số trước đó để loại bỏ các số trùng lặp, sắp xếp danh sách kết quả, sau đó lặp lại để hợp nhất các số liên tiếp. Cách này phù hợp với những ràng buộc nhất định nhưng đòi hỏi phải quét và sắp xếp nhiều lần. Số lượng hoạt động trong trường hợp xấu nhất là khoảng`O(n^2)`để loại bỏ trùng lặp cộng`O(n log n)`để sắp xếp, có thể chấp nhận được với n ≤ 100 nhưng không phù hợp. 

Thông tin chi tiết quan trọng là chúng ta có thể duy trì tính duy nhất trong khi vẫn giữ nguyên thứ tự bằng cách sử dụng một tập hợp để theo dõi các số đã thấy và một danh sách để duy trì thứ tự chèn. Khi chúng ta có danh sách bị trùng lặp, việc hợp nhất các số liên tiếp rất đơn giản. Điều này giúp giảm bớt công việc dư thừa, dễ thực hiện và phân biệt rõ ràng hai mối quan tâm: loại bỏ các bản sao và phạm vi định dạng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Được chấp nhận với n ≤ 100, nhưng không hiệu quả | 
| Tối ưu | O(n log n) | O(n) | Được chấp nhận, sạch sẽ và có thể bảo trì | 

## Hướng dẫn thuật toán 

1. Phân tích chuỗi đầu vào bằng cách phân tách bằng dấu phẩy và chuyển đổi từng phần thành số nguyên. Điều này đưa ra một danh sách các số nguyên theo thứ tự được nhập. 
2. Lặp lại danh sách này trong khi vẫn duy trì một`seen`bộ. Đối với mỗi số, nếu nó không có trong`seen`, thêm nó vào danh sách mới`unique_pages`và thêm nó vào`seen`. Điều này bảo tồn thứ tự xuất hiện đầu tiên và loại bỏ các bản sao. 
3. Sắp xếp`unique_pages`theo thứ tự tăng dần. Việc sắp xếp là cần thiết vì phạm vi chỉ có ý nghĩa đối với các chuỗi có thứ tự. 
4. Khởi tạo danh sách trống`ranges`và một con trỏ`start`ở phần tử đầu tiên. Lặp lại danh sách đã sắp xếp từ phần tử thứ hai trở đi. Đối với mỗi phần tử, nếu nó liên tiếp với phần tử trước đó thì tiếp tục. Nếu nó không liên tiếp, hãy đóng phạm vi hiện tại bằng cách nối thêm một trong hai`start`hoặc`start-end`ĐẾN`ranges`, sau đó bắt đầu một phạm vi mới ở phần tử hiện tại. 
5. Sau vòng lặp, nối thêm dải ô cuối cùng theo cách tương tự. 
6. Tham gia`ranges`danh sách bằng dấu phẩy và in. Điều này tạo ra định dạng chuẩn được yêu cầu. 

Tại sao nó hoạt động: ở mỗi bước,`unique_pages`chứa mỗi số đúng một lần và danh sách được sắp xếp. Mỗi chuỗi liên tiếp được hợp nhất thành một phạm vi duy nhất, trong khi các số không liên tiếp tự nhiên tạo thành các phạm vi riêng biệt. Tính bất biến đó là`ranges`luôn chứa các phạm vi được định dạng chính xác bao gồm tất cả các số được thấy cho đến nay. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

pages = list(map(int, input().strip().split(',')))

seen = set()
unique_pages = []
for p in pages:
    if p not in seen:
        unique_pages.append(p)
        seen.add(p)

unique_pages.sort()

ranges = []
start = unique_pages[0]
end = unique_pages[0]

for p in unique_pages[1:]:
    if p == end + 1:
        end = p
    else:
        if start == end:
            ranges.append(str(start))
        else:
            ranges.append(f"{start}-{end}")
        start = end = p

if start == end:
    ranges.append(str(start))
else:
    ranges.append(f"{start}-{end}")

print(','.join(ranges))
```Đầu tiên, mã phân tích cú pháp đầu vào thành số nguyên, loại bỏ các bản sao trong khi vẫn giữ nguyên thứ tự, sau đó sắp xếp danh sách. Sau đó nó xây dựng các phạm vi bằng cách theo dõi các số liên tiếp với`start`Và`end`. Câu lệnh in cuối cùng định dạng danh sách một cách chính xác. Một điểm tinh tế là xử lý phạm vi cuối cùng sau vòng lặp, điều này rất dễ quên và dẫn đến thiếu các trang cuối cùng. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:`1,2,3,1,1,2,6,6,2`| Bước | trang_duy nhất | được sắp xếp | bắt đầu | kết thúc | phạm vi | 
| --- | --- | --- | --- | --- | --- | 
| phân tích/khảo sát | [1,2,3,6] | [1,2,3,6] | 1 | 1 | [] | 
| Yếu tố thứ 2 | - | - | 1 | 2 | [] | 
| yếu tố thứ 3 | - | - | 1 | 3 | [] | 
| yếu tố thứ 4 | - | - | 6 | 6 | ['1-3'] | 
| cuối cùng | - | - | 6 | 6 | ['1-3','6'] | 

Dấu vết này cho thấy rằng các số trùng lặp được loại bỏ trước tiên, sau đó các số liên tiếp tạo thành một phạm vi duy nhất và số đơn cuối cùng tạo thành mục nhập riêng của nó. 

Đầu vào mẫu 2:`5,5,5,5`| Bước | trang_duy nhất | được sắp xếp | bắt đầu | kết thúc | phạm vi | 
| --- | --- | --- | --- | --- | --- | 
| phân tích/khảo sát | [5] | [5] | 5 | 5 | [] | 
| cuối cùng | - | - | 5 | 5 | ['5'] | 

Điều này chứng tỏ rằng các số giống hệt nhau lặp đi lặp lại sẽ thu gọn chính xác thành một mục duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Loại bỏ trùng lặp với một tập hợp là O(n), sắp xếp mất O(n log n), phạm vi hợp nhất là O(n) | 
| Không gian | O(n) | Việc lưu trữ các trang và phạm vi duy nhất yêu cầu không gian tuyến tính về số lượng đầu vào | 

Với n 100, O(n log n) là không đáng kể và mức sử dụng bộ nhớ thấp hơn nhiều so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    pages = list(map(int, input().strip().split(',')))
    seen = set()
    unique_pages = []
    for p in pages:
        if p not in seen:
            unique_pages.append(p)
            seen.add(p)
    unique_pages.sort()
    ranges = []
    start = unique_pages[0]
    end = unique_pages[0]
    for p in unique_pages[1:]:
        if p == end + 1:
            end = p
        else:
            if start == end:
                ranges.append(str(start))
            else:
                ranges.append(f"{start}-{end}")
            start = end = p
    if start == end:
        ranges.append(str(start))
    else:
        ranges.append(f"{start}-{end}")
    return ','.join(ranges)

# provided sample
assert run("1,2,3,1,1,2,6,6,2") == "1-3,6", "sample 1"
# all equal
assert run("5,5,5,5") == "5", "all equal"
# single element
assert run("42") == "42", "single element"
# max value
assert run(",".join(map(str, range(1,101)))) == "1-100", "consecutive max"
# gaps
assert run("1,3,2,5,6") == "1-3,5-6", "gaps"
# unordered input
assert run("10,1,3,2,7,6,8") == "1-3,6-8,10", "unordered input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5,5,5,5 | 5 | trùng lặp | 
| 42 | 42 | phần tử đơn | 
| 1..100 | 1-100 | chuỗi lớn liên tiếp | 
| 1,3,2,5,6 | 1-3,5-6 | xử lý lỗ hổng | 
| 10,1,3,2,7,6,8 | 1-3,6-8,10 | đầu vào không có thứ tự | 

## Vỏ cạnh 

đầu vào:`5,5,5`

`unique_pages = [5]`, được sắp xếp`[5]`, bắt đầu = kết thúc = 5, phạm vi = [] ban đầu. Sau lần nối cuối cùng, phạm vi = ['5']. Đầu ra là`5`. Thuật toán thu gọn chính xác tất cả các bản sao thành một mục duy nhất. 

đầu vào:`1,3,2``trang_duy nhất = [1,3
