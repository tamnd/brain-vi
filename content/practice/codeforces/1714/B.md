---
title: "CF 1714B - Xóa tiền tố"
description: "Chúng ta được cho một dãy số nguyên a có độ dài n, trong đó mỗi số nguyên nằm trong khoảng từ 1 đến n. Mục đích là làm cho chuỗi chỉ chứa các giá trị riêng biệt bằng cách liên tục loại bỏ các phần tử ngay từ đầu."
date: "2026-06-09T20:04:36+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1714
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 811 (Div. 3)"
rating: 800
weight: 1714
solve_time_s: 99
verified: true
draft: false
---

[CF 1714B - Xóa tiền tố](https://codeforces.com/problemset/problem/1714/B) 

**Đánh giá:** 800 
**Tas:** cấu trúc dữ liệu, tham lam, triển khai 
**Thời gian giải:** 1 phút 39 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên`a`chiều dài`n`, trong đó mỗi số nguyên nằm giữa`1`Và`n`. Mục đích là làm cho chuỗi chỉ chứa các giá trị riêng biệt bằng cách liên tục loại bỏ các phần tử ngay từ đầu. Đối với mỗi trường hợp thử nghiệm, chúng ta phải xác định số phần tử tối thiểu cần loại bỏ ở bên trái để chuỗi còn lại không chứa bản sao. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, tối đa 10^4 và tổng của tất cả các độ dài chuỗi không vượt quá 2×10^5. Điều này ngụ ý rằng bất kỳ giải pháp nào tuyến tính về độ dài của chuỗi cho mỗi trường hợp thử nghiệm sẽ phù hợp thoải mái trong giới hạn thời gian 2 giây, trong khi các giải pháp có O(n^2) cho mỗi trường hợp thử nghiệm sẽ quá chậm. 

Ví dụ, một trường hợp cạnh quan trọng xảy ra khi các bản sao xuất hiện ở đầu mảng`[1,1,1,1]`. Một cách tiếp cận đơn giản chỉ kiểm tra các bản sao từ đầu chuỗi có thể loại bỏ quá ít phần tử một cách không chính xác. Một trường hợp cạnh khác là khi chuỗi đã là duy nhất, chẳng hạn như`[1,2,3]`. Trong trường hợp đó, câu trả lời phải bằng 0. Một chuỗi có tất cả các phần tử riêng biệt nhưng theo thứ tự giảm dần, như`[6,5,4,3,2,1]`, là một trường hợp khác có thể gặp phải một thuật toán không theo dõi chính xác các bản sao từ cuối. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng việc loại bỏ các tiền tố. Bắt đầu từ việc loại bỏ các phần tử bằng 0 và kiểm tra xem chuỗi còn lại có phải là duy nhất hay không. Nếu không, hãy xóa thêm một phần tử, kiểm tra lại và lặp lại cho đến khi chuỗi là duy nhất. Điều này hiệu quả vì việc kiểm tra tính duy nhất trong một dãy con có thể được thực hiện bằng một tập hợp, nhưng trong trường hợp xấu nhất thì đây là O(n^2) vì chúng ta có thể kiểm tra tất cả các tiền tố và mỗi lần kiểm tra là O(n). Vì`n`lên tới 2×10^5, tốc độ này quá chậm. 

Thông tin chi tiết quan trọng là việc xóa các phần tử ngay từ đầu chỉ ảnh hưởng đến các phần tử trùng lặp ở bên trái. Khi sự trùng lặp xảy ra lần đầu tiên từ bên phải, chúng tôi có thể xóa mọi thứ trước khi nó xuất hiện sớm nhất. Điều này tự nhiên dẫn đến việc duyệt ngược lại: bắt đầu từ đầu bên phải của mảng và chèn các phần tử vào một tập hợp. Khi chúng tôi gặp một phần tử đã có trong tập hợp, chúng tôi dừng lại. Số phần tử trước điểm dừng này chính xác là tiền tố tối thiểu mà chúng ta phải loại bỏ để đạt được tính duy nhất trong chuỗi còn lại. Điều này làm giảm vấn đề xuống còn quét tuyến tính từ đầu, với thời gian O(n) cho mỗi trường hợp kiểm thử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một bộ`seen`để theo dõi những con số đã xuất hiện. 
2. Bắt đầu quét chuỗi từ phần tử ngoài cùng bên phải tới phần tử ngoài cùng bên trái. 
3. Với mỗi phần tử, hãy kiểm tra xem nó có nằm trong`seen`. 
4. Nếu phần tử đã có sẵn`seen`, ngừng quét. Đây là bản sao đầu tiên gặp phải từ bên phải. 
5. Nếu phần tử không có trong`seen`, thêm nó vào`seen`và tiếp tục di chuyển sang trái. 
6. Sau khi dừng, vị trí của phần tử ngoài cùng bên trái mà chúng ta quét tương ứng với tiền tố tối thiểu cần loại bỏ. Số phần tử cần loại bỏ chính xác là chỉ mục của phần tử này (lập chỉ mục dựa trên 0). 
7. In số phần tử cần loại bỏ cho từng test. 

**Tại sao nó hoạt động:** Bất biến là vậy`seen`luôn chứa tất cả các phần tử trong hậu tố của mảng bắt đầu từ phần tử ngoài cùng bên phải được quét cho đến nay. Phần tử đầu tiên sao chép một phần tử trong hậu tố này phải bị loại bỏ vì nó vi phạm tính duy nhất. Bằng cách quét từ bên phải, chúng tôi đảm bảo rằng chúng tôi đang giữ hậu tố duy nhất dài nhất và bất kỳ phần tử nào ở bên trái trùng lặp với phần tử trong hậu tố này phải thuộc về tiền tố đã bị xóa. Điều này đảm bảo loại bỏ tiền tố tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    seen = set()
    remove_count = 0
    # traverse from right to left
    for i in range(n-1, -1, -1):
        if a[i] in seen:
            break
        seen.add(a[i])
        remove_count += 1
    # the minimal prefix to remove
    print(n - remove_count)
```Giải pháp này khởi tạo một`set`để theo dõi các bản sao. Nó đếm số phần tử trong hậu tố duy nhất dài nhất. Trừ số này từ`n`cung cấp độ dài tiền tố tối thiểu để loại bỏ. Việc truyền tải từ phải sang trái đảm bảo chúng tôi nắm bắt được hậu tố duy nhất dài nhất một cách hiệu quả. Chúng tôi phá vỡ ngay lập tức khi gặp phải sự trùng lặp, tránh những công việc không cần thiết. 

## Ví dụ đã hoạt động 

**Ví dụ 1:`[3, 1, 4, 3]`** 

| tôi | một [tôi] | đã thấy | xóa_count | 
| --- | --- | --- | --- | 
| 3 | 3 | {} | 1 | 
| 2 | 4 | {3} | 2 | 
| 1 | 1 | {3,4} | 3 | 
| 0 | 3 | {1,4,3} | dừng lại | 

Hậu tố còn lại`[1,4,3]`, đã loại bỏ độ dài tiền tố = 4 - 3 = 1. 

**Ví dụ 2:`[1,2,1,7,1,2,1]`** 

| tôi | một [tôi] | đã thấy | xóa_count | 
| --- | --- | --- | --- | 
| 6 | 1 | {} | 1 | 
| 5 | 2 | {1} | 2 | 
| 4 | 1 | {1,2} | dừng lại | 

Hậu tố còn lại`[2,1]`, đã loại bỏ độ dài tiền tố = 7 - 2 = 5. 

Những dấu vết này cho thấy rằng việc quét từ phải sang trái sẽ thu được hậu tố duy nhất dài nhất và mang lại độ dài tiền tố tối thiểu cần loại bỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được thêm vào`seen`nhiều nhất một lần. Quá trình quét từ phải sang trái dừng ngay lập tức ở bản sao đầu tiên. | 
| Không gian | O(n) cho mỗi trường hợp thử nghiệm | các`seen`set có thể lưu trữ tất cả các số riêng biệt trong chuỗi. | 

Cho rằng tổng của`n`trên tất cả các trường hợp thử nghiệm không vượt quá 2×10^5, giải pháp phù hợp với giới hạn thời gian 2 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call solution
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        seen = set()
        remove_count = 0
        for i in range(n-1, -1, -1):
            if a[i] in seen:
                break
            seen.add(a[i])
            remove_count += 1
        print(n - remove_count)
    return output.getvalue().strip()

# Provided samples
assert run("5\n4\n3 1 4 3\n5\n1 1 1 1 1\n1\n1\n6\n6 5 4 3 2 1\n7\n1 2 1 7 1 2 1\n") == "1\n4\n0\n0\n5"

# Custom test cases
assert run("2\n3\n1 1 1\n3\n1 2 3\n") == "2\n0", "all-equal vs all-distinct"
assert run("1\n5\n5 4 3 2 1\n") == "0", "reverse-order unique"
assert run("1\n1\n1\n") == "0", "single element"
assert run("1\n6\n1 2 3 3 2 1\n") == "3", "multiple duplicates"
assert run("1\n7\n1 2 3 4 5 6 7\n") == "0", "already unique"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`| 2 | Xử lý đúng các phần tử lặp lại khi bắt đầu | 
|`1 2 3`| 0 | Trình tự đã là duy nhất | 
|`5 4 3 2 1`| 0 | Hậu tố dài nhất là toàn bộ mảng | 
|`1 2 3 3 2 1`| 3 | Loại bỏ tiền tố chính xác cho các bản sao nội bộ | 
|`1 2 3 4 5 6 7`| 0 | Đã là duy nhất, không cần xóa | 

## Vỏ cạnh 

cho`[1,1,1,1,1]`, quét từ bên phải, chúng ta thấy cái cuối cùng`1`đầu tiên, sau đó dừng lại ở phần trước`1`. Hậu tố là`[1]`, độ dài tiền tố bị loại bỏ = 5-1 = 4. Thuật toán xác định chính xác mức loại bỏ tối thiểu. 

Vì`[6,5,4,3,2,1]`, quét từ bên phải, mỗi phần tử là duy nhất cho đến phần tử ngoài cùng bên trái. Hậu tố là toàn bộ mảng, do đó mức loại bỏ tiền tố tối thiểu là 0. 

cho`[1]`, các chuỗi phần tử đơn luôn là duy nhất. Thuật toán quét từ bên phải, thêm`1`ĐẾN`seen`,
