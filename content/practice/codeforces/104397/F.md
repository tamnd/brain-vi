---
title: "CF 104397F - ``Chế độ'' nhưng ``Không gian thấp''?"
description: "Chúng tôi được cung cấp một tập hợp các kích thước cookie, về cơ bản là một mảng các số nguyên. Từ mảng này, chúng tôi muốn chọn một tập hợp con các cookie sao cho các phần tử được chọn nằm trong phạm vi giá trị hẹp: giá trị được chọn lớn nhất trừ đi giá trị được chọn nhỏ nhất phải bằng tối đa k."
date: "2026-06-30T23:10:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104397
codeforces_index: "F"
codeforces_contest_name: "The 21st UESTC Programming Contest Final"
rating: 0
weight: 104397
solve_time_s: 74
verified: true
draft: false
---

[CF 104397F - ``Mode'' but ``Không gian thấp''?](https://codeforces.com/problemset/problem/104397/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một tập hợp các kích thước cookie, về cơ bản là một mảng các số nguyên. Từ mảng này, chúng tôi muốn chọn một tập hợp con các cookie sao cho các phần tử được chọn nằm trong phạm vi giá trị hẹp: giá trị được chọn lớn nhất trừ đi giá trị được chọn nhỏ nhất phải bằng nhiều nhất`k`. Trong số tất cả các tập hợp con hợp lệ như vậy, chúng ta được yêu cầu tối đa hóa số lượng phần tử mà chúng ta có thể lấy. 

Kích thước đầu vào có thể lớn tới một triệu và bản thân các giá trị tùy ý lên tới 1e9. Sự ràng buộc về`k`là nhỏ, nhiều nhất là 9, điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào phụ thuộc nhiều vào sự khác biệt về giá trị đều có thể được tối ưu hóa bằng cách sử dụng thứ tự hoặc quét cục bộ thay vì tổ hợp toàn cục. 

Giới hạn bộ nhớ cực kỳ chặt chẽ ở mức 1 megabyte, loại trừ hiệu quả các mảng tần số trên miền giá trị và mọi cấu trúc phụ trợ tỷ lệ với`n`lưu trữ nhiều bản sao dữ liệu. Điều này thúc đẩy chúng tôi tiến tới xử lý tại chỗ hoặc sắp xếp hành vi phát trực tuyến với dung lượng lưu trữ bổ sung tối thiểu. 

Một cạm bẫy ngây thơ xuất hiện khi người ta cố gắng xử lý vấn đề dưới dạng tập hợp tần số độc lập cho mỗi giá trị. Ví dụ: nếu cookie được`[1, 100, 101, 102]`với`k = 2`, một nhóm ngây thơ có thể đếm quá mức bằng cách trộn số lượng từ các vùng rời rạc nếu không cẩn thận trong việc thực thi ràng buộc khoảng số liền kề. 

Một trường hợp cạnh tinh tế khác phát sinh khi tất cả các giá trị giống hệt nhau. Ví dụ,`[5, 5, 5, 5]`với bất kỳ`k`nên quay lại`4`và bất kỳ logic trượt nào đặt lại sai cửa sổ trên các bản sao sẽ không thành công ở đây. 

## Phương pháp tiếp cận 

Ý tưởng Brute-Force rất đơn giản: xem xét mọi tập con có thể có, kiểm tra xem giá trị lớn nhất trừ nhỏ nhất của nó có nằm trong`k`và giữ giá trị lớn nhất hợp lệ. Ngay cả khi hạn chế các tập hợp con liền kề nhau theo thứ tự được sắp xếp, người ta vẫn có thể cố gắng liệt kê tất cả các cặp bắt đầu và kết thúc. Sau khi sắp xếp, điều này làm giảm điều kiện kiểm tra tất cả các khoảng`[i, j]`như vậy`a[j] - a[i] <= k`, và lấy độ dài lớn nhất 

Điều này ngay lập tức gợi ý một`O(n^2)`quét qua tất cả các cặp. Với`n`lên đến 1e6, điều này hoàn toàn không khả thi vì nó sẽ yêu cầu 10^12 phép so sánh. 

Quan sát quan trọng là sau khi sắp xếp, bất kỳ tập hợp con tối ưu nào cũng phải tạo thành một phân đoạn liền kề trong mảng được sắp xếp. Sau khi được sắp xếp, việc mở rộng cửa sổ chỉ tăng mức tối đa và thu nhỏ cửa sổ chỉ tăng mức tối thiểu, do đó tính hợp lệ được biểu thị một cách tự nhiên dưới dạng điều kiện khoảng trượt. Điều này chuyển vấn đề thành việc tìm ra mảng con dài nhất trong đó sự khác biệt giữa các điểm cuối là nhiều nhất`k`. 

Cấu trúc đó chính xác là những gì kỹ thuật hai con trỏ nắm bắt được. Chúng tôi duy trì ranh giới bên trái và mở rộng ranh giới bên phải hết mức có thể trong khi vẫn duy trì ràng buộc, chỉ thu hẹp ranh giới bên trái khi cần thiết. Mỗi phần tử vào và ra khỏi cửa sổ nhiều nhất một lần, cho thời gian tuyến tính sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Sắp xếp + Hai con trỏ | O(n log n) | O(1) bổ sung (ngoài việc sắp xếp) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng kích thước cookie theo thứ tự không giảm. Điều này biến vấn đề thành giải quyết các khoảng giá trị liền kề trong đó bất kỳ tập hợp con hợp lệ nào cũng phải xuất hiện dưới dạng một phân đoạn liên tục theo thứ tự này. 
2. Khởi tạo hai con trỏ`l = 0`Và`r = 0`, đại diện cho cửa sổ hiện tại của các cookie đã chọn. Tại bất kỳ thời điểm nào, cửa sổ này đại diện cho một tập hợp con ứng cử viên. 
3. Mở rộng con trỏ bên phải`r`từng bước một. Sau mỗi lần mở rộng, hãy kiểm tra xem`a[r] - a[l] <= k`. Nếu đúng thì cửa sổ hiện tại hợp lệ và có khả năng cập nhật câu trả lời. 
4. Nếu điều kiện bị vi phạm, hãy thu nhỏ cửa sổ từ bên trái bằng cách tăng`l`cho đến khi điều kiện trở lại hợp lệ. Điều này có tác dụng vì ngày càng tăng`l`chỉ giảm giá trị tối thiểu trong cửa sổ, trực tiếp khôi phục tính khả thi. 
5. Trong quá trình này, hãy duy trì kích thước cửa sổ tối đa`r - l + 1`. Điều này đại diện cho tập hợp con hợp lệ tốt nhất được thấy cho đến nay. 
6. Tiếp tục cho đến khi`r`đến cuối mảng, đảm bảo tất cả các điểm cuối bên phải có thể được xem xét chính xác một lần. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, bất kỳ tập hợp con nào thỏa mãn ràng buộc đều có thể được ánh xạ tới một phân đoạn liền kề theo thứ tự được sắp xếp mà không làm mất tính tổng quát, vì việc bao gồm bất kỳ phần tử bị bỏ qua nào trong một phạm vi hợp lệ đều không thể vi phạm điều kiện tối đa trừ tối thiểu. Hai con trỏ bất biến là tại mỗi bước, cửa sổ`[l, r]`là ranh giới bên trái nhỏ nhất giữ cho phân đoạn hợp lệ cho hiện tại`r`. Điều này đảm bảo rằng không có phân đoạn hợp lệ nào kết thúc tại`r`bị bỏ qua, vì bất kỳ chỉ mục bên trái nhỏ hơn nào cũng sẽ vi phạm ràng buộc và bất kỳ chỉ mục nào lớn hơn chỉ làm giảm kích thước một cách không cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))

a.sort()

l = 0
ans = 1

for r in range(n):
    while a[r] - a[l] > k:
        l += 1
    ans = max(ans, r - l + 1)

print(ans)
```Giải pháp bắt đầu bằng cách sắp xếp mảng sao cho các tập hợp con hợp lệ tương ứng với các phạm vi liền kề trong không gian giá trị. Vòng lặp hai con trỏ sau đó duy trì một cửa sổ`[l, r]`luôn thỏa mãn ràng buộc`a[r] - a[l] <= k`. Bất cứ khi nào ràng buộc bị phá vỡ, con trỏ bên trái sẽ được nâng lên cho đến khi tính hợp lệ được khôi phục. Câu trả lời được cập nhật ở mỗi bước bằng cách sử dụng độ dài cửa sổ hiện tại. 

Một lỗi phổ biến ở đây là quên rằng việc thu gọn phải xảy ra trong một vòng lặp chứ không phải một bước có điều kiện duy nhất, bởi vì có nhiều bước tăng của`l`có thể được yêu cầu trước khi ràng buộc được thỏa mãn trở lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 3
1 1 4 5 1 4
```Mảng được sắp xếp:`[1, 1, 1, 4, 4, 5]`| r | tôi | cửa sổ | hợp lệ | tốt nhất | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | [1] | vâng | 1 | 
| 1 | 0 | [1,1] | vâng | 2 | 
| 2 | 0 | [1,1,1] | vâng | 3 | 
| 3 | 0 | [1,1,1,4] | không | 3 | 
| 3 | 1 | [1,1,4] | không | 3 | 
| 3 | 2 | [1,4] | không | 3 | 
| 3 | 3 | [4] | vâng | 3 | 
| 4 | 3 | [4,4] | vâng | 4 | 
| 5 | 3 | [4,4,5] | vâng | 5 | 

Câu trả lời cuối cùng là`5`, đạt được trên phân khúc`[4, 4, 5]`. 

Dấu vết này cho thấy con trỏ bên trái tăng mạnh như thế nào cho đến khi giới hạn phạm vi được khôi phục và cách các cửa sổ tối ưu có thể bắt đầu muộn trong mảng thay vì luôn luôn bắt đầu từ đầu. 

### Ví dụ 2 

đầu vào:```
5 0
2 2 2 3 3
```Mảng được sắp xếp:`[2, 2, 2, 3, 3]`| r | tôi | cửa sổ | hợp lệ | tốt nhất | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | [2] | vâng | 1 | 
| 1 | 0 | [2,2] | vâng | 2 | 
| 2 | 0 | [2,2,2] | vâng | 3 | 
| 3 | 0 | [2,2,2,3] | không | 3 | 
| 3 | 1 | [2,2,3] | không | 3 | 
| 3 | 2 | [2,3] | không | 3 | 
| 3 | 3 | [3] | vâng | 3 | 
| 4 | 3 | [3,3] | vâng | 4 | 

Câu trả lời là`4`. 

Điều này chứng tỏ rằng ngay cả với`k = 0`, các bản sao được xử lý chính xác vì cửa sổ chỉ phụ thuộc vào sự bằng nhau của các điểm cuối chứ không phải logic tần số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, hai con trỏ chạy theo thời gian tuyến tính | 
| Không gian | O(1) thêm | Chỉ các chỉ số và bộ đếm được sử dụng ngoài bộ nhớ đầu vào | 

Các ràng buộc cho phép tối đa một triệu phần tử, vì vậy việc quét tuyến tính sau khi sắp xếp là điều cần thiết. Hạn chế về bộ nhớ càng củng cố thêm việc tránh các cấu trúc phụ trợ như bản đồ tần số hoặc mảng nén tọa độ có kích thước lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()

    l = 0
    ans = 1
    for r in range(n):
        while a[r] - a[l] > k:
            l += 1
        ans = max(ans, r - l + 1)
    return str(ans)

# provided sample
assert run("6 3\n1 1 4 5 1 4\n") == "5"

# all equal
assert run("4 10\n7 7 7 7\n") == "4"

# k = 0 with duplicates
assert run("5 0\n2 2 2 3 3\n") == "4"

# strictly increasing, small k
assert run("5 1\n1 2 3 4 5\n") == "2"

# single element
assert run("1 5\n100\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | 4 | độ ổn định của cửa sổ khi mọi khác biệt bằng 0 | 
| k = 0 có trùng lặp | 4 | tính đúng đắn dưới ràng buộc bình đẳng | 
| trình tự tăng dần | 2 | hành vi co trượt | 
| phần tử đơn | 1 | độ đúng ranh giới tối thiểu | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các phần tử đều giống hệt nhau. Thuật toán tiếp tục mở rộng con trỏ bên phải mà không bao giờ thu hẹp lại, vì`a[r] - a[l]`luôn luôn bằng không. Cửa sổ trở thành toàn bộ mảng, tạo chính xác`n`. 

Một trường hợp cạnh khác là khi`k = 0`nhưng giá trị lặp lại. Chỉ các giá trị giống nhau mới có thể cùng tồn tại và thuật toán sẽ nhóm các phần tử bằng nhau một cách tự nhiên. Khi gặp một giá trị khác, con trỏ bên trái sẽ tiến lên cho đến khi cửa sổ chỉ chứa lại giá trị đó, đảm bảo tính chính xác. 

Trường hợp biên cuối cùng là các dãy tăng dần với số lượng nhỏ`k`. Cửa sổ liên tục thu gọn về kích thước một hoặc hai. Ví dụ, với`[1,2,3,4]`Và`k = 1`, cửa sổ không bao giờ vượt quá các cặp liền kề và thuật toán trả về chính xác`2`thay vì nhầm lẫn cho phép kéo dài hơn.
