---
title: "CF 1506D - Sự Chuyển Đổi Sử Thi"
description: "Chúng ta được cung cấp một mảng các số nguyên và chúng ta được phép loại bỏ nhiều lần các cặp phần tử khác nhau. Nhiệm vụ của chúng ta là xác định kích thước nhỏ nhất mà mảng có thể có sau khi áp dụng thao tác này bao nhiêu lần."
date: "2026-06-10T20:18:40+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1506
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 710 (Div. 3)"
rating: 1400
weight: 1506
solve_time_s: 141
verified: true
draft: false
---

[CF 1506D - Sự biến đổi hoành tráng](https://codeforces.com/problemset/problem/1506/D) 

**Đánh giá:** 1400 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 2m 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và chúng ta được phép loại bỏ nhiều lần các cặp phần tử khác nhau. Nhiệm vụ của chúng ta là xác định kích thước nhỏ nhất mà mảng có thể có sau khi áp dụng thao tác này bao nhiêu lần. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có mảng riêng. Đầu ra chỉ đơn giản là độ dài tối thiểu có thể có cho mỗi mảng sau khi loại bỏ cặp tối ưu. 

Nhìn vào các ràng buộc, mảng có thể lớn tới 2×10^5 phần tử trong một trường hợp thử nghiệm duy nhất và tổng số trên tất cả các trường hợp thử nghiệm được giới hạn ở mức 2×10^5. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng rõ ràng mọi loại bỏ cặp có thể xảy ra, bởi vì điều đó có thể liên quan đến các hoạt động O(n^2) trong trường hợp xấu nhất. Chúng ta cần một cách tiếp cận tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Mỗi giá trị phần tử có thể rất lớn, lên tới 10^9, do đó, bất kỳ giải pháp nào dựa vào việc tạo mảng tần số được lập chỉ mục theo chính giá trị đó đều không khả thi. Chúng ta cần làm việc với số lượng chứ không phải phạm vi giá trị. 

Các trường hợp cạnh cần được chú ý cẩn thận bao gồm các mảng trong đó tất cả các phần tử đều bằng nhau. Ví dụ,`[1, 1, 1]`không thể loại bỏ bất kỳ cặp nào, vì vậy kết quả là 3. Một tình huống khó khăn khác là khi số lượng thường xuyên nhất chiếm ưu thế trong mảng. Ví dụ,`[1, 1, 1, 2, 3]`chỉ cho phép loại bỏ hai cặp (`1-2`Và`1-3`), để lại một`1`phía sau. Một cách tiếp cận ngây thơ loại bỏ các cặp một cách tùy ý có thể tạo ra kích thước cuối cùng dưới mức tối ưu nếu nó không xem xét việc phân phối số lượng. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là mô phỏng việc loại bỏ hai số khác nhau bất kỳ nhiều lần. Bạn sẽ chọn các cặp, loại bỏ chúng và lặp lại cho đến khi không còn cặp nào nữa. Điều này đảm bảo sẽ tạo ra câu trả lời đúng nếu được thực hiện cẩn thận, nhưng độ phức tạp trong trường hợp xấu nhất là O(n^2), quá chậm đối với n = 2×10^5. 

Cái nhìn sâu sắc quan trọng là chúng ta chỉ cần biết tần số của mỗi số. Giả sử số xảy ra thường xuyên nhất`max_count`lần và tổng số phần tử còn lại`rest_count = n - max_count`. Nếu như`rest_count`lớn hơn hoặc bằng`max_count`, thì chúng ta có thể ghép mỗi lần xuất hiện của số thường xuyên nhất với một số phần tử khác nhau và giảm mảng thành 0 hoặc 1 phần tử tùy thuộc vào tính chẵn lẻ. Nếu như`rest_count`ít hơn`max_count`, thì ngay cả sau khi ghép nối mọi phần tử khác với phần tử thường xuyên nhất, sẽ có`max_count - rest_count`những phần tử còn sót lại, không thể loại bỏ được. Do đó, kích thước mảng tối thiểu là`max(1, max_count - rest_count)`nếu như`max_count > rest_count`Và`n % 2`nếu không thì. 

Quan sát này cho phép chúng tôi giải quyết từng trường hợp thử nghiệm trong thời gian O(n) bằng cách sử dụng bản đồ tần số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Phân tích tần số | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Với mỗi test case, hãy đọc độ dài mảng`n`và mảng`a`. 
3. Đếm số lần xuất hiện của từng số trong mảng. Điều này có thể được thực hiện bằng cách sử dụng từ điển hoặc`collections.Counter`. 
4. Xác định tần số lớn nhất`max_count`. 
5. Tính tổng tất cả các tần số khác`rest_count = n - max_count`. 
6. Nếu`max_count`lớn hơn`rest_count`, kích thước mảng tối thiểu là`max_count - rest_count`bởi vì những yếu tố này không thể được ghép nối với bất cứ điều gì khác nhau. 
7. Nếu`max_count`nhỏ hơn hoặc bằng`rest_count`, thì chúng ta có thể loại bỏ các cặp một cách tối ưu cho đến khi còn lại 0 hoặc 1 phần tử, do đó kích thước tối thiểu là`n % 2`. 
8. In kích thước tối thiểu cho mỗi test case. 

Bất biến ở đây là phần tử tần số tối đa xác định giới hạn dưới của các phần tử còn lại. Nếu các yếu tố khác đủ để ghép nối với nó, tất cả chúng đều có thể bị loại bỏ. Nếu không, phần dư thừa vẫn còn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import Counter

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    freq = Counter(a)
    max_count = max(freq.values())
    rest_count = n - max_count
    if max_count > rest_count:
        print(max_count - rest_count)
    else:
        print(n % 2)
```Mã đầu tiên đọc tất cả các đầu vào một cách hiệu quả.`Counter`xây dựng bản đồ tần số, cho phép chúng tôi nhanh chóng xác định phần tử phổ biến nhất.`max_count`Và`rest_count`nắm bắt hai đại lượng quan trọng cần thiết để quyết định kích thước mảng tối thiểu. Sau đó, chúng tôi áp dụng trực tiếp lý do từ thuật toán: nếu phần tử thường xuyên nhất chiếm ưu thế, chúng tôi sẽ đưa ra phần thừa; mặt khác, chúng tôi xuất ra tính chẵn lẻ của kích thước mảng sau khi ghép nối tối đa. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
6
1 6 1 1 4 4
```| Biến | Giá trị | 
| --- | --- | 
| tần số | {1:3, 6:1, 4:2} | 
| max_count | 3 | 
| nghỉ_đếm | 3 | 
| đầu ra | n % 2 = 6 % 2 = 0 | 

Giải thích: Chúng ta có thể ghép nối`1-6`,`1-4`,`1-4`để loại bỏ tất cả các yếu tố. Kích thước mảng tối thiểu là 0. 

### Đầu vào mẫu 2```
5
4 5 4 5 4
```| Biến | Giá trị | 
| --- | --- | 
| tần số | {4:3, 5:2} | 
| max_count | 3 | 
| nghỉ_đếm | 2 | 
| đầu ra | max_count - Rest_count = 3 - 2 = 1 | 

Giải thích: Chỉ có thể loại bỏ hai cặp:`4-5`Và`4-5`, để lại một`4`phía sau. Kích thước mảng tối thiểu là 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Tần số đếm là tuyến tính, việc tìm max trong một từ điển nhỏ là tuyến tính trong các phần tử duy nhất, tổng ≤ n. | 
| Không gian | O(n) | Chúng tôi lưu trữ bản đồ mảng và tần số; bản đồ tần số có tối đa n phím. | 

Với tổng số n 2×10^5 trong tất cả các trường hợp thử nghiệm, điều này rất phù hợp trong giới hạn thời gian 2 giây và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import Counter

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        freq = Counter(a)
        max_count = max(freq.values())
        rest_count = n - max_count
        if max_count > rest_count:
            output.append(str(max_count - rest_count))
        else:
            output.append(str(n % 2))
    return "\n".join(output)

# provided samples
assert run("5\n6\n1 6 1 1 4 4\n2\n1 2\n2\n1 1\n5\n4 5 4 5 4\n6\n2 3 2 1 3 1\n") == "0\n0\n2\n1\n0"

# custom cases
assert run("1\n1\n42\n") == "1"  # single element
assert run("1\n4\n7 7 7 7\n") == "4"  # all equal
assert run("1\n5\n1 2 3 4 5\n") == "1"  # all unique odd
assert run("1\n6\n1 1 2 2 3 3\n") == "0"  # all paired perfectly
assert run("1\n7\n1 1 1 2 2 2 3\n") == "1"  # one excess
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n42`| 1 | Phần tử đơn không thể được ghép nối | 
|`1\n4\n7 7 7 7`| 4 | Tất cả các yếu tố giống hệt nhau | 
|`1\n5\n1 2 3 4 5`| 1 | Tất cả số phần tử lẻ duy nhất | 
|`1\n6\n1 1 2 2 3 3`| 0 | Tất cả các phần tử có thể được ghép nối chính xác | 
|`1\n7\n1 1 1 2 2 2 3`| 1 | Phần tử dư thừa vẫn còn sau khi ghép nối tối đa | 

## Vỏ cạnh 

Đối với các mảng có tất cả các phần tử bằng nhau, thuật toán sẽ tính`max_count = n`,`rest_count = 0`và đầu ra`max_count - rest_count = n`. Ví dụ,`[7, 7, 7, 7]`kết quả đầu ra chính xác 4. Đối với các mảng có chính xác hai phần tử khác nhau xuất hiện với số lượng bằng nhau, như`[1, 1, 2, 2]`,`max_count = 2`,`rest_count = 2`, đầu ra là`n % 2 = 4 % 2 = 0`, xác nhận rằng tất cả có thể được gỡ bỏ. Giải pháp xử lý chính xác các mảng một phần tử, mảng có một số trội và mảng có cơ hội ghép nối hoàn hảo.
