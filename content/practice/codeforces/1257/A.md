---
title: "CF 1257A - Hai học sinh đối thủ"
description: "Chúng ta có một hàng học sinh được đánh số từ 1 đến n. Trong đó có hai học sinh là đối thủ của nhau, nằm ở vị trí a và b. Giáo viên thể dục muốn tối đa hóa khoảng cách giữa hai học sinh này, trong đó khoảng cách đơn giản là sự khác biệt tuyệt đối về vị trí của họ."
date: "2026-06-11T20:47:45+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1257
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 76 (Rated for Div. 2)"
rating: 800
weight: 1257
solve_time_s: 121
verified: true
draft: false
---

[CF 1257A - Hai học sinh đối thủ](https://codeforces.com/problemset/problem/1257/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hàng học sinh được đánh số từ 1 đến n. Trong đó có hai học sinh là đối thủ của nhau, nằm ở vị trí a và b. Giáo viên thể dục muốn tối đa hóa khoảng cách giữa hai học sinh này, trong đó khoảng cách đơn giản là sự khác biệt tuyệt đối về vị trí của họ. Chúng ta được phép thực hiện tối đa x lần hoán đổi và mỗi lần hoán đổi sẽ trao đổi hai học sinh liền kề. Nhiệm vụ là xác định khoảng cách lớn nhất có thể đạt được giữa các đối thủ sau những lần hoán đổi này. 

Nhìn vào các ràng buộc, n nhiều nhất là 100, x nhiều nhất là 100 và có tới 100 trường hợp thử nghiệm. Giá trị này đủ nhỏ để ngay cả giải pháp O(n) hoặc O(1) cho mỗi trường hợp thử nghiệm cũng cực kỳ nhanh. Không cần cấu trúc dữ liệu phức tạp hoặc thủ thuật tối ưu hóa. Tuy nhiên, chúng ta vẫn cần suy luận cẩn thận xem nước đi nào thực sự làm tăng khoảng cách. 

Một điểm tinh tế là việc hoán đổi chỉ di chuyển học sinh theo một vị trí tại một thời điểm. Vì vậy, chúng ta không thể đặt ngay một học sinh vào một vị trí tùy ý; mức tăng khoảng cách tối đa bị giới hạn bởi số lượng giao dịch hoán đổi có sẵn. Ví dụ: nếu n = 5, x = 1, a = 3, b = 2, một cách tiếp cận đơn giản là chỉ đặt học sinh vào vị trí 1 và 5 sẽ là không thể vì chỉ được phép hoán đổi một lần. Chúng ta phải tính đến giới hạn hoán đổi khi tính toán khoảng cách tối đa. 

Một trường hợp cạnh khác xảy ra khi x = 0. Khi đó không thể chuyển động được và khoảng cách chỉ là hiệu tuyệt đối hiện tại |a - b|. Tương tự, nếu học sinh đã ở cuối và x lớn, chúng ta không bao giờ có thể tăng khoảng cách vượt quá n - 1 vì đó là khoảng cách vật lý tối đa trên một đường thẳng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng tất cả các chuỗi hoán đổi có thể xảy ra. Đối với mỗi trường hợp kiểm tra, chúng ta có thể thử di chuyển một học sinh sang trái hoặc phải và học sinh kia sang phải hoặc sang trái, thử mọi cách kết hợp các chuỗi hoán đổi cho đến x lần hoán đổi. Sau mỗi chuỗi, chúng tôi tính toán khoảng cách. Cách tiếp cận này đúng nhưng cực kỳ kém hiệu quả: số lượng chuỗi tăng theo cấp số nhân với x. Với x = 100, điều này rõ ràng là không thực tế. 

Quan sát chính là chúng tôi không quan tâm đến vị trí trung gian chính xác của học sinh. Chúng tôi chỉ quan tâm đến khoảng cách giữa hai đối thủ và việc hoán đổi sẽ di chuyển học sinh từng bước cho mỗi thao tác. Do đó, khoảng cách tối đa mà chúng ta có thể đạt được bị giới hạn bởi hai yếu tố: số lượng giao dịch hoán đổi có sẵn x và khoảng cách vật lý tối đa trong đường dây là n - 1. Nếu khoảng cách ban đầu giữa a và b là d = |a - b|, thì chúng ta có thể tăng nó lên tối đa x, nhưng không thể vượt quá n - 1. Điều này đưa ra một công thức đơn giản cho giải pháp tối ưu:```
max_distance = min(n - 1, |a - b| + x)
```Điều này có hiệu quả vì mỗi lần hoán đổi có thể tăng khoảng cách lên tối đa là 1. Chúng ta chỉ cần xem xét việc di chuyển học sinh về phía hai đầu đối diện của đường thẳng. Không cần phải mô phỏng các giao dịch hoán đổi riêng lẻ, tạo ra giải pháp O(1) cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^x) | O(n) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case t. Mỗi trường hợp thử nghiệm bao gồm bốn số nguyên: n, x, a, b. 
2. Đối với mỗi trường hợp thử nghiệm, hãy tính khoảng cách hiện tại giữa các đối thủ như sau:`d = abs(a - b)`. 
3. Tính khoảng cách tối đa có thể đạt được sau khi đổi chỗ. Mỗi lần hoán đổi có thể tăng khoảng cách tối đa là 1, vì vậy hãy thêm x vào khoảng cách hiện tại:`new_distance = d + x`. 
4. Khoảng cách lớn nhất tuyệt đối trên một đoạn thẳng có độ dài n là n - 1. Nếu`new_distance`vượt quá mức này, giới hạn nó:`max_distance = min(new_distance, n - 1)`. 
5. Đầu ra`max_distance`cho từng trường hợp thử nghiệm. 

Tại sao nó hoạt động: Điều bất biến là mỗi lần hoán đổi sẽ tăng khoảng cách giữa các đối thủ tối đa là 1. Bắt đầu từ vị trí ban đầu, khoảng cách tối đa có thể đạt được chính xác là khoảng cách ban đầu cộng với số lần hoán đổi, giới hạn bởi độ dài đường. Điều này đảm bảo rằng công thức mang lại kết quả tối ưu trong mọi tình huống. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, x, a, b = map(int, input().split())
    d = abs(a - b)
    max_distance = min(n - 1, d + x)
    print(max_distance)
```Trong giải pháp này, trước tiên chúng tôi đọc số lượng trường hợp thử nghiệm. Sau đó với mỗi trường hợp, chúng ta tính khoảng cách tuyệt đối`d`giữa hai đối thủ. Chúng tôi thêm số lượng giao dịch hoán đổi`x`và giới hạn kết quả với`n - 1`để tôn trọng chiều dài dòng. Việc in kết quả trực tiếp đảm bảo O(1) hoạt động cho mỗi trường hợp thử nghiệm. Sự chú ý cẩn thận được dành cho việc sử dụng`abs(a - b)`Và`min(...)`để xử lý tất cả các trường hợp cạnh một cách chính xác. 

## Ví dụ đã hoạt động 

**Mẫu 1:**```
n = 5, x = 1, a = 3, b = 2
```| Bước | d | d + x | khoảng cách tối đa | 
| --- | --- | --- | --- | 
| Ban đầu | 1 | 2 | 2 | 

Giải thích: Khoảng cách ban đầu là 1. Một lần hoán đổi sẽ tăng khoảng cách lên 2, nhỏ hơn n - 1 = 4. Thuật toán đưa ra chính xác 2. 

**Mẫu 2:**```
n = 100, x = 33, a = 100, b = 1
```| Bước | d | d + x | khoảng cách tối đa | 
| --- | --- | --- | --- | 
| Ban đầu | 99 | 132 | 99 | 

Giải thích: Khoảng cách ban đầu là 99. Thêm các giao dịch hoán đổi sẽ cho 132, nhưng mức tối đa vật lý là n - 1 = 99. Thuật toán giới hạn nó ở mức 99. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ cần tính sai phân tuyệt đối, phép cộng và phép tính min | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu bổ sung, chỉ có một vài số nguyên | 

Với t 100, n 100 và x 100, thuật toán chạy trong thời gian không đáng kể, thoải mái trong giới hạn 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n, x, a, b = map(int, input().split())
        d = abs(a - b)
        max_distance = min(n - 1, d + x)
        print(max_distance)
    return output.getvalue().strip()

# Provided samples
assert run("3\n5 1 3 2\n100 33 100 1\n6 0 2 3\n") == "2\n99\n1"

# Custom cases
assert run("1\n2 0 1 2\n") == "1", "minimum size input"
assert run("1\n10 5 4 5\n") == "6", "moderate swaps increasing distance"
assert run("1\n10 100 1 2\n") == "9", "swaps exceed maximum distance"
assert run("1\n7 3 3 7\n") == "6", "rivals initially at distance 4, increase by swaps"
assert run("1\n100 0 50 51\n") == "1", "zero swaps, distance unchanged"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 0 1 2 | 1 | Mảng kích thước tối thiểu, không hoán đổi | 
| 10 5 4 5 | 6 | Hoán đổi tăng khoảng cách trong giới hạn | 
| 10 100 1 2 | 9 | Hoán đổi vượt quá khoảng cách tối đa, giới hạn ở n - 1 | 
| 7 3 3 7 | 6 | Đối thủ ban đầu xa cách, áp dụng hoán đổi | 
| 100 0 50 51 | 1 | Không hoán đổi, khoảng cách vẫn giữ nguyên | 

## Vỏ cạnh 

Đối với trường hợp x = 0, thuật toán trả về đúng khoảng cách ban đầu. Ví dụ,`n = 6, x = 0, a = 2, b = 3`cho`abs(2-3) = 1`. Không thể thực hiện hoán đổi nên khoảng cách không thể tăng lên. Hàm min không làm thay đổi giá trị vì 1 < n - 1 = 5. 

Đối với trường hợp vị trí ban đầu ở cực trị,`n = 100, x = 50, a = 1, b = 100`, khoảng cách đã đạt cực đại là 99. Việc cộng x sẽ cho kết quả là 149, nhưng hàm min giới hạn nó ở mức 99. Điều này chứng tỏ thuật toán tôn trọng các ràng buộc vật lý của đường thẳng. 

Đối với một dòng tối thiểu,`n = 2, x = 0, a = 1, b = 2`, khoảng cách là 1. Không thể hoán đổi và khoảng cách không thể vượt quá n - 1 = 1. Kết quả đầu ra phù hợp với mong đợi. 

Tất cả những kiểm tra này hiển thị công thức`min(n - 1, abs(a - b) + x)`xử lý chính xác các trường hợp cạnh mà không cần logic điều kiện đặc biệt.
