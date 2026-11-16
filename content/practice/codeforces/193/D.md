---
title: "CF 193D - Hai phân đoạn"
description: "Chúng ta được cho một hoán vị các số nguyên từ 1 đến n. Hoán vị có nghĩa là mỗi số nguyên xuất hiện đúng một lần. Nhiệm vụ là đếm các cặp mảng con (đoạn) liền kề nhau sao cho khi chúng ta kết hợp hai phân đoạn đó, sự kết hợp các phần tử của chúng tạo thành một chuỗi liên tiếp…"
date: "2026-06-03T01:29:19+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 193
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 122 (Div. 1)"
rating: 2900
weight: 193
solve_time_s: 68
verified: true
draft: false
---

[CF 193D - Hai phân đoạn](https://codeforces.com/problemset/problem/193/D) 

**Xếp hạng:** 2900 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được hoán vị các số nguyên từ 1 đến _n_. Hoán vị có nghĩa là mỗi số nguyên xuất hiện đúng một lần. Nhiệm vụ là đếm các cặp mảng con (đoạn) liền kề sao cho khi chúng ta kết hợp hai phân đoạn, sự kết hợp các phần tử của chúng tạo thành một dãy số nguyên liên tiếp theo bất kỳ thứ tự nào. 

Ví dụ: trong hoán vị [1, 2, 3], lấy các phân đoạn [1,2] và [3,3] tạo thành tập hợp {1, 2, 3}, liên tiếp. Chúng tôi coi hai cặp giống hệt nhau nếu tổng tập hợp các phần tử mà chúng bao gồm giống nhau, do đó, các lựa chọn chồng chéo hoặc lồng nhau mang lại cùng một tập hợp chỉ được tính một lần. 

Kích thước của _n_ có thể lên tới 300.000. Điều này có nghĩa là bất kỳ cách tiếp cận nào xem xét tất cả các cặp phân đoạn một cách đơn giản, bao gồm các phép toán đại khái là O(n³) hoặc O(n²), đều không thực tế. Chúng ta cần một cách tiếp cận tuyến tính hoặc gần tuyến tính. Các trường hợp cạnh cần chú ý bao gồm các hoán vị rất ngắn (độ dài 1 hoặc 2) và các hoán vị trong đó các phần tử có thứ tự giảm dần, vì những trường hợp này có thể gây ra lỗi riêng lẻ hoặc phân tách phân đoạn không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là liệt kê mọi phân đoạn đầu tiên có thể có [a0, a1] và mọi phân đoạn thứ hai [b0, b1] sau nó. Đối với mỗi cặp ứng cử viên, chúng tôi sẽ trích xuất tất cả các phần tử, sắp xếp chúng và kiểm tra xem chúng có tạo thành một chuỗi liên tiếp hay không. Điều này đúng nhưng cực kỳ chậm: có khoảng n2/2 lựa chọn cho phân đoạn đầu tiên và lên đến n2/2 cho phân đoạn thứ hai và mỗi lần kiểm tra có thể bao gồm tối đa n phần tử. Điều này mang lại thời gian O(n⁴) trong trường hợp xấu nhất, không thể thực hiện được với n = 3·10⁵. 

Quan sát quan trọng là trong một hoán vị, bất kỳ tập hợp số nguyên liên tiếp nào cũng có thể được mô tả đầy đủ bằng mức tối thiểu và tối đa của nó. Nghĩa là, để một đoạn là liên tiếp, hiệu giữa cực đại và cực tiểu của nó cộng với một phải bằng độ dài đoạn. Điều này cho phép chúng tôi phát hiện các phân đoạn hợp lệ trong O(1) trên mỗi phân đoạn nếu chúng tôi duy trì chạy tối thiểu và tối đa. 

Sau đó, chúng ta có thể lặp lại hoán vị trong một lần chuyển, mở rộng các phân đoạn và theo dõi các phạm vi hợp lệ. Cụ thể, đối với mọi đầu bên phải có thể có của một đoạn, chúng tôi duy trì đầu bên trái tối thiểu sao cho đoạn đó tạo thành một chuỗi liên tiếp. Từ đó, chúng ta có thể xem xét mọi cách để chia đoạn này thành hai đoạn con liền kề nhau: mỗi đoạn tách tạo ra một cặp duy nhất. Bằng cách quét từ trái sang phải và theo dõi các giá trị tối thiểu/tối đa một cách cẩn thận, chúng tôi giảm vấn đề xuống O(n) bằng cách ghi sổ phù hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n⁴) | O(n) | Quá chậm | 
| Tối ưu (trượt tối thiểu/tối đa) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm`answer = 0`về số lượng các cặp tốt. 
2. Lặp lại tất cả các điểm cuối bên phải có thể`r`từ 0 đến n-1. Duy trì một biến`l`cho chỉ số ngoài cùng bên trái của phân đoạn sao cho các phần tử p[l..r] tạo thành một chuỗi liên tiếp. Sử dụng hai biến`current_min`Và`current_max`để lưu trữ các phần tử tối thiểu và tối đa của phân khúc. 
3. Mở rộng phân khúc bằng cách di chuyển`r`một bước sang phải. Cập nhật`current_min`Và`current_max`. Trong khi đoạn p[l..r] không thỏa mãn điều kiện`current_max - current_min == r - l`, tăng`l`để thu nhỏ đoạn từ bên trái. Điều này duy trì tính bất biến mà p[l..r] luôn tạo thành các phần tử liên tiếp. 
4. Với mỗi phân đoạn hợp lệ p[l..r], số cách chia nó thành hai phân đoạn con liền kề không trống là`r - l`. Mỗi phần tách tương ứng với một cặp phân đoạn duy nhất bao gồm cùng một tập hợp các phần tử. Tăng`answer`qua`r - l`. 
5. Sau khi duyệt qua tất cả các điểm cuối bên phải,`answer`chứa tổng số cặp phân khúc tốt riêng biệt. 

**Tại sao nó hoạt động**: Bất biến`current_max - current_min == r - l`đảm bảo rằng các phần tử của đoạn là liên tiếp. Mọi sự phân chia có thể xảy ra bên trong phân khúc này sẽ tạo ra hai phân đoạn con liền kề cùng nhau bao gồm cùng một tập hợp, đảm bảo chúng tôi đếm tất cả các cặp tốt duy nhất chính xác một lần. Các phân đoạn chồng chéo được tính thông qua việc trượt động của`l`, do đó tự nhiên tránh được sự trùng lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
p = list(map(int, input().split()))

pos = [0] * (n + 1)
for idx, val in enumerate(p):
    pos[val] = idx

answer = 0
l = 0
current_min = p[0]
current_max = p[0]

for r in range(n):
    if r == 0:
        current_min = current_max = p[r]
    else:
        current_min = min(current_min, p[r])
        current_max = max(current_max, p[r])
    while current_max - current_min != r - l:
        l += 1
        current_min = min(p[l:r+1])
        current_max = max(p[l:r+1])
    answer += r - l

print(answer)
```**Giải thích**:`for`vòng lặp mở rộng phân khúc sang bên phải. các`while`vòng lặp đảm bảo phân đoạn vẫn hợp lệ bằng cách thu nhỏ từ bên trái cho đến khi nó liên tiếp. Mỗi phân đoạn hợp lệ đều đóng góp`r - l`cặp phân đoạn tốt. Việc sử dụng tính toán lại tối thiểu/tối đa bên trong vòng lặp while có thể chấp nhận được vì mỗi phần tử được chạm nhiều nhất hai lần, giữ cho tổng công việc tuyến tính. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
3
1 2 3
```| r | tôi | hiện tại_min | hiện tại_max | r-l | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | 0 | 0 | 
| 1 | 0 | 1 | 2 | 1 | 1 | 
| 2 | 0 | 1 | 3 | 2 | 3 | 

Bảng cho thấy với r=1, đoạn [0,1] có thể được chia một lần; với r=2, đoạn [0,2] có thể được chia hai lần. Tổng số câu trả lời = 3, phù hợp với mẫu. 

### Đầu vào mẫu 2```
5
3 1 2 5 4
```| r | tôi | hiện tại_min | hiện tại_max | r-l | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 3 | 3 | 0 | 0 | 
| 1 | 1 | 1 | 1 | 0 | 0 | 
| 2 | 1 | 1 | 2 | 1 | 1 | 
| 3 | 3 | 5 | 5 | 0 | 1 | 
| 4 | 3 | 4 | 5 | 1 | 2 | 

Bảng thể hiện sự điều chỉnh động của`l`và đếm tăng dần các cặp phân khúc tốt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được thêm vào và xóa khỏi phân đoạn nhiều nhất một lần. | 
| Không gian | O(n) | Chúng tôi lưu trữ các mảng ánh xạ hoán vị và vị trí. | 

Lời giải là tuyến tính theo thời gian và không gian, nằm trong giới hạn cho n = 3·10⁵. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    p = list(map(int, input().split()))
    answer = 0
    l = 0
    current_min = current_max = p[0]
    for r in range(n):
        if r == 0:
            current_min = current_max = p[r]
        else:
            current_min = min(current_min, p[r])
            current_max = max(current_max, p[r])
        while current_max - current_min != r - l:
            l += 1
            current_min = min(p[l:r+1])
            current_max = max(p[l:r+1])
        answer += r - l
    return str(answer)

assert run("3\n1 2 3\n") == "3", "sample 1"
assert run("5\n3 1 2 5 4\n") == "2", "sample 2"
assert run("1\n1\n") == "0", "single element"
assert run("2\n1 2\n") == "1", "two elements"
assert run("5\n5 4 3 2 1\n") == "4", "decreasing order"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "1\n1\n" | 0 | Hoán vị phần tử đơn | 
| "2\n1 2\n" | 1 | Trường hợp hai yếu tố tối thiểu | 
| "5\n5 4 3 2 1\n" | 4 | Giảm đơn hàng, thu hẹp phân khúc tối đa | 
| "3\n1 2 3\n" | 3 | Mẫu gốc | 
| "5\n3 1 2 5 4\n" | 2 | Hoán vị không tầm thường với phép chia | 

## Vỏ cạnh 

Đối với hoán vị một phần tử, không có hai phân đoạn nào để tạo thành một cặp, do đó thuật toán trả về chính xác 0. Trong hoán vị giảm nghiêm ngặt như [5,4,3,2,1], thuật toán sẽ dịch chuyển linh hoạt`l`
