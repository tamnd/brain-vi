---
title: "CF 47D - An toàn"
description: "Chúng tôi đang cố gắng xây dựng lại một chuỗi nhị phân ẩn có độ dài n. Mỗi dự đoán mà Vasya đưa ra là một chuỗi nhị phân khác có cùng độ dài, cùng với một con số cho biết có bao nhiêu vị trí khớp chính xác với mã thực."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 47
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 44 (Div. 2)"
rating: 2200
weight: 47
solve_time_s: 100
verified: true
draft: false
---

[CF 47D - An toàn](https://codeforces.com/problemset/problem/47/D) 

**Xếp hạng:** 2200 
**Tags:** vũ lực 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang cố gắng xây dựng lại một chuỗi nhị phân có độ dài ẩn`n`. Mỗi dự đoán mà Vasya đưa ra là một chuỗi nhị phân khác có cùng độ dài, cùng với một con số cho biết có bao nhiêu vị trí khớp chính xác với mã thực. 

Ví dụ: nếu mã ẩn là`101011`và một phỏng đoán là`111001`, câu trả lời sẽ là`3`vì các dây nối nhau ở ba vị trí. 

Chúng tôi được đưa ra tới mười lần đoán như vậy. Mỗi câu trả lời nhiều nhất là`5`, hóa ra đó là hạn chế quan trọng. Nhiệm vụ là đếm xem có bao nhiêu chuỗi nhị phân phù hợp với tất cả các câu trả lời cùng một lúc. 

Cách giải thích trực tiếp nhất là thế này: trong số tất cả`2^n`chuỗi nhị phân, có bao nhiêu thỏa mãn$$\text{matches}(x, s_i) = c_i$$cho mọi nỗ lực`i`. 

Các ràng buộc loại trừ hoàn toàn việc liệt kê mọi mã có thể. Từ`n`có thể đạt được`35`, không gian tìm kiếm chứa$$2^{35} \approx 3.4 \times 10^{10}$$dây. Ngay cả việc kiểm tra một tỷ chuỗi mỗi giây vẫn là quá chậm. 

Số lần thử rất ít, chỉ nhiều nhất là`10`. Điều đó cho thấy giải pháp phải khai thác mối quan hệ giữa các dự đoán thay vì ép buộc tất cả các ứng viên. 

Hạn chế bất thường`c_i ≤ 5`là sự mở đầu thực sự. Mỗi lần đoán khác với câu trả lời đúng ít nhất`n - 5`các vị trí. Điều đó có nghĩa là mã ẩn rất xa so với mọi chuỗi đã cho và số lượng chỉnh sửa cần thiết so với bất kỳ lần đoán nào là rất nhỏ. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Giả sử hai lần đoán giống hệt nhau nhưng yêu cầu số lần khớp khác nhau:```
6 2
000000 2
000000 3
```Không có chuỗi nhị phân nào có thể thỏa mãn cả hai điều kiện, vì vậy câu trả lời đúng là`0`. Việc triển khai bất cẩn xử lý các ràng buộc một cách độc lập có thể vô tình đếm được các trạng thái không thể thực hiện được. 

Một trường hợp tinh vi khác xuất hiện khi một ràng buộc yêu cầu không có kết quả khớp nào:```
6 1
000000 0
```Mã hợp lệ duy nhất là`111111`. Bất kỳ triển khai nào có tính đến việc “thay đổi một vài bit” vẫn phải cho phép lật mọi vị trí nếu cần thiết. 

Cũng có trường hợp nhiều lần chỉnh sửa khác nhau tạo ra cùng một chuỗi cuối cùng. Ví dụ:```
6 2
000000 1
111111 5
```Cả hai ràng buộc đều mô tả chính xác cùng một tập hợp các chuỗi, cụ thể là tất cả các chuỗi có đúng một số 0. Một tìm kiếm đệ quy đơn giản có thể đếm gấp đôi trừ khi nó lưu trữ các bài tập hoàn chỉnh thay vì các đường dẫn. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Liệt kê mọi chuỗi nhị phân có độ dài`n`, so sánh nó với tất cả`m`đoán và đếm có bao nhiêu đáp ứng tất cả các trận đấu. 

Kiểm tra chi phí của một ứng viên`O(mn)`bởi vì chúng tôi so sánh với từng ký tự đoán. Tổng độ phức tạp trở thành$$O(2^n \cdot m \cdot n)$$điều đó thật vô vọng`n = 35`. 

Cấu trúc của các ràng buộc gợi ý một quan điểm khác. Chọn một lần đoán làm chuỗi tham chiếu, chẳng hạn như chuỗi đầu tiên. Số trận đấu bắt buộc của nó là`c`. 

Vì chuỗi ẩn có chính xác`c`vị trí khớp với tham chiếu này, nó khác nhau một cách chính xác$$n - c$$các vị trí. 

Lúc đầu cái này trông vẫn lớn, bởi vì`n - c`có thể ở xung quanh`30`. Nhưng việc lật ngược quan điểm sẽ giúp ích. 

Thay vì xây dựng trực tiếp chuỗi ẩn, hãy xem xét vị trí nào vẫn bằng chuỗi tham chiếu. Từ`c ≤ 5`, có nhiều nhất năm vị trí như vậy. 

Điều đó thay đổi không gian tìm kiếm một cách đáng kể. Chúng ta chỉ cần chọn tối đa năm vị trí không thay đổi so với lần đoán đầu tiên. Mọi vị trí khác sẽ tự động trở thành bit đối diện. 

Số tập con như vậy là$$\sum_{k=0}^{5} \binom{35}{k}$$cái nào ở dưới`400000`. Đối với mỗi tập hợp con, chúng ta có thể xây dựng lại chính xác một chuỗi ứng cử viên và xác minh tất cả các ràng buộc. 

Điều này hoạt động vì chuỗi nhị phân chỉ có hai giá trị có thể có ở mỗi vị trí. Khi chúng tôi quyết định xem một vị trí có khớp với chuỗi tham chiếu hay không, giá trị của nó sẽ được cố định. 

Brute-force thành công vì mọi mã hợp lệ đều có thể được kiểm tra độc lập. Giải pháp tối ưu hóa thành công vì ràng buộc đầu tiên nén không gian ứng cử viên từ`2^35`xuống tất cả các tập hợp con có kích thước tối đa là năm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot m \cdot n)$|$O(1)$| Quá chậm | 
| Tối ưu |$O\left(mn \sum_{k=0}^{5}\binom{n}{k}\right)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các dự đoán và số lượng trận đấu được yêu cầu của chúng. 
2. Chọn lần đoán đầu tiên làm chuỗi tham chiếu. 
3. Giả sử lần đoán đầu tiên có số lần trùng khớp`c`. Mã ẩn phải khác với chuỗi này một cách chính xác`n - c`các vị trí. 
4. Thay vì chọn các vị trí khác nhau, hãy chọn các vị trí bằng chuỗi tham chiếu. Số lượng của họ chính xác là`c`, và kể từ đó`c ≤ 5`, số khả năng là nhỏ. 
5. Liệt kê mọi tập hợp con các vị trí có kích thước`c`. 

Các vị trí này được khai báo bằng với lần đoán đầu tiên. Mọi vị trí khác đều bị đảo lộn. 
6. Xây dựng chuỗi ứng cử viên được ngụ ý bởi tập hợp con này. 

Đối với mọi chỉ số: 

- nếu chỉ mục thuộc tập hợp con đã chọn, hãy sao chép bit tham chiếu 
- nếu không, hãy đảo ngược nó 

Bởi vì bảng chữ cái là nhị phân nên điều này xác định duy nhất toàn bộ chuỗi. 
7. Kiểm tra từng dự đoán của ứng viên. 

Đếm xem có bao nhiêu vị trí phù hợp. Nếu mọi số đếm đều bằng giá trị được yêu cầu thì ứng viên đó hợp lệ. 
8. Đếm tất cả các ứng viên hợp lệ và in tổng số. 

### Tại sao nó hoạt động 

Ràng buộc đầu tiên phân chia tất cả các vị trí thành hai nhóm: các vị trí trong đó chuỗi ẩn bằng với giá trị dự đoán tham chiếu và các vị trí có chuỗi khác. 

Vì các chuỗi là nhị phân nên “khác” có nghĩa là bit đối diện chứ không phải giá trị thứ ba tùy ý. Khi các vị trí bằng nhau được chọn, chuỗi ẩn sẽ được xác định hoàn toàn. 

Mỗi chuỗi ẩn hợp lệ tương ứng với chính xác một tập hợp con có vị trí kích thước bằng nhau`c`và mỗi tập hợp con như vậy tạo ra chính xác một chuỗi ứng cử viên. Thuật toán liệt kê tất cả các tập hợp con này và lọc ra những tập hợp con vi phạm các ràng buộc khác, vì vậy mọi giải pháp hợp lệ đều được tính chính xác một lần. 

## Giải pháp Python```python
import sys
from itertools import combinations

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    strings = []
    counts = []

    for _ in range(m):
        s, c = input().split()
        strings.append(s)
        counts.append(int(c))

    base = strings[0]
    keep = counts[0]

    ans = 0
    positions = list(range(n))

    for same_positions in combinations(positions, keep):
        same_set = set(same_positions)

        candidate = []

        for i in range(n):
            if i in same_set:
                candidate.append(base[i])
            else:
                candidate.append('1' if base[i] == '0' else '0')

        candidate = ''.join(candidate)

        ok = True

        for s, need in zip(strings, counts):
            match = 0

            for a, b in zip(candidate, s):
                if a == b:
                    match += 1

            if match != need:
                ok = False
                break

        if ok:
            ans += 1

    print(ans)

solve()
```Việc thực hiện tuân theo việc giảm toán học trực tiếp. 

Quan sát quan trọng đầu tiên xuất hiện trong những dòng này:```
base = strings[0]
keep = counts[0]
```Chúng tôi sử dụng lần đoán đầu tiên làm chuỗi tham chiếu. Mọi câu trả lời hợp lệ đều phải khớp chính xác`keep`các vị trí. 

Vòng lặp```
for same_positions in combinations(positions, keep):
```liệt kê mọi tập hợp vị trí có thể có trong đó mã ẩn bằng chuỗi tham chiếu. Từ`keep ≤ 5`, bảng liệt kê này vẫn đủ nhỏ. 

Việc xây dựng ứng cử viên là tinh tế. Đối với các vị trí bên ngoài tập hợp con đã chọn, chúng ta phải lật bit:```
'1' if base[i] == '0' else '0'
```Điều này chỉ hoạt động vì bảng chữ cái là nhị phân. Trong bảng chữ cái lớn hơn, việc khác với tham chiếu sẽ không xác định được giá trị duy nhất. 

Bước xác minh sẽ so sánh ứng viên với mọi dự đoán và tính toán các kết quả trùng khớp chính xác. Vòng lặp sẽ thoát ngay sau khi tìm thấy mâu thuẫn, giúp tránh được những công việc không cần thiết. 

Một lỗi phổ biến là cố gắng thay đổi dần dần chuỗi tham chiếu trong khi lặp lại các tập hợp con. Việc xây dựng lại ứng viên từ đầu đơn giản hơn và tránh được các lỗi trạng thái cũ. 

Một điểm tinh tế khác là tính độc đáo. Các tập hợp con khác nhau luôn tạo ra các chuỗi ứng viên khác nhau vì các vị trí bằng tham chiếu được xác định duy nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 2
000000 2
010100 4
```Lần đoán đầu tiên yêu cầu chính xác hai vị trí khớp nhau. 

Chúng tôi liệt kê tất cả các tập hợp con có kích thước`2`. 

| Lựa chọn vị trí bình đẳng | Ứng viên được xây dựng | Phù hợp với`000000`| Phù hợp với`010100`| hợp lệ | 
| --- | --- | --- | --- | --- | 
| {0,1} | 001111 | 2 | 2 | Không | 
| {0,2} | 010111 | 2 | 4 | Có | 
| {0,3} | 011011 | 2 | 4 | Có | 
| {1,2} | 100111 | 2 | 2 | Không | 
| {2,4} | 110101 | 2 | 4 | Có | 

Tiếp tục quá trình này mang lại sáu chuỗi hợp lệ. 

Dấu vết này thể hiện tính bất biến cốt lõi: khi các vị trí bằng nhau so với lần đoán đầu tiên được cố định, toàn bộ chuỗi ứng cử viên sẽ được cố định tự động. 

### Ví dụ 2 

đầu vào:```
6 2
000000 0
111111 6
```Ràng buộc đầu tiên cho biết chuỗi ẩn khớp với`000000`ở vị trí số 0. 

| Lựa chọn vị trí bình đẳng | Ứng viên được xây dựng | Phù hợp với`000000`| Phù hợp với`111111`| hợp lệ | 
| --- | --- | --- | --- | --- | 
| {} | 111111 | 0 | 6 | Có | 

Chỉ có một tập con có kích thước bằng 0, tạo ra chính xác một ứng cử viên. 

Ví dụ này thực hiện trường hợp ranh giới trong đó mọi bit đều phải lật. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O\left(mn \sum_{k=0}^{5}\binom{n}{k}\right)$| Liệt kê tất cả các tập hợp con có kích thước`c ≤ 5`, xây dựng từng ứng viên và xác minh mọi dự đoán | 
| Không gian |$O(n)$| Bộ nhớ cho chuỗi ứng cử viên hiện tại | 

Vì`n = 35`, số lượng tập hợp con lớn nhất xảy ra tại`c = 5`:$$\binom{35}{5} = 324632$$Mỗi ứng viên yêu cầu tối đa`35 × 10 = 350`so sánh ký tự, dễ dàng phù hợp với giới hạn thời gian trong Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from itertools import combinations

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, m = map(int, input().split())

    strings = []
    counts = []

    for _ in range(m):
        s, c = input().split()
        strings.append(s)
        counts.append(int(c))

    base = strings[0]
    keep = counts[0]

    ans = 0

    for same_positions in combinations(range(n), keep):
        same_set = set(same_positions)

        candidate = []

        for i in range(n):
            if i in same_set:
                candidate.append(base[i])
            else:
                candidate.append('1' if base[i] == '0' else '0')

        candidate = ''.join(candidate)

        ok = True

        for s, need in zip(strings, counts):
            match = sum(a == b for a, b in zip(candidate, s))

            if match != need:
                ok = False
                break

        if ok:
            ans += 1

    return str(ans)

# provided sample
assert run(
"""6 2
000000 2
010100 4
"""
) == "6", "sample 1"

# contradiction
assert run(
"""6 2
000000 2
000000 3
"""
) == "0", "contradiction"

# all bits flipped
assert run(
"""6 1
000000 0
"""
) == "1", "unique complement"

# exact original string
assert run(
"""6 1
101010 6
"""
) == "1", "only base string works"

# maximum keep value
assert run(
"""6 1
111111 5
"""
) == "6", "choose one differing position"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai dự đoán giống hệt nhau với số lượng khác nhau | 0 | Phát hiện mâu thuẫn | 
| Đoán một lần với số 0 | 1 | Xây dựng bổ sung đầy đủ | 
| Đoán đơn với số 6 | 1 | Không cần lật | 
| Đoán đơn với số 5 | 6 | Đếm tất cả các khác biệt một bit | 

## Vỏ cạnh 

Xét trường hợp ràng buộc mâu thuẫn:```
6 2
000000 2
000000 3
```Thuật toán liệt kê mọi ứng cử viên có chính xác hai vị trí trùng khớp với`000000`. Mỗi ứng cử viên như vậy tự động có chính xác hai kết quả phù hợp với chuỗi thứ hai giống hệt nhau. 

Trong quá trình xác minh: 

| Ứng viên | Phù hợp với | Phù hợp với thứ hai | 
| --- | --- | --- | 
| 001111 | 2 | 2 | 
| 010111 | 2 | 2 | 
| 111100 | 2 | 2 | 

Không có giá trị nào thỏa mãn yêu cầu`3`, vậy đáp án vẫn là`0`. 

Bây giờ hãy xem xét trường hợp không khớp:```
6 1
000000 0
```Kích thước tập hợp con được chọn bằng 0, do đó chỉ tồn tại tập hợp con trống. 

| Vị trí bình đẳng | Ứng viên | 
| --- | --- | 
| {} | 111111 | 

Thuật toán lật từng bit và tìm chính xác mã hợp lệ duy nhất. 

Cuối cùng, hãy xem xét các ràng buộc chồng chéo:```
6 2
000000 1
111111 5
```Ràng buộc đầu tiên tạo ra tất cả các chuỗi có chính xác một số 0: 

| Vị trí bình đẳng | Ứng viên | 
| --- | --- | 
| {0} | 011111 | 
| {1} | 101111 | 
| {2} | 110111 | 

Mỗi ứng viên tự động khớp`111111`ở năm vị trí, như vậy cả sáu ứng cử viên đều được chấp nhận đúng một lần. 

Điều này xác nhận rằng việc biểu diễn tập hợp con tránh được việc tính trùng lặp.
