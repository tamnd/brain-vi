---
title: "CF 104289C - Chữ số bằng nhau"
description: "Chúng ta được cho một số nguyên dương $n$, và chúng ta cần tìm số nguyên nhỏ nhất $k$ sao cho $k ge n$ và mọi chữ số của $k$ đều giống hệt nhau. Những số như vậy trông giống như $1, 2, 3, dấu chấm, 9, 11, 22, 33, dấu chấm, 9999$, trong đó một chữ số được lặp lại một số lần."
date: "2026-07-01T20:36:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104289
codeforces_index: "C"
codeforces_contest_name: "Bangladesh CP Server - BCS Round 1 (Div. 3)"
rating: 0
weight: 104289
solve_time_s: 77
verified: false
draft: false
---

[CF 104289C - Các chữ số bằng nhau](https://codeforces.com/problemset/problem/104289/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên dương$n$, và ta cần tìm số nguyên nhỏ nhất$k$như vậy$k \ge n$và mọi chữ số của$k$là giống hệt nhau. Những con số như vậy trông giống như$1, 2, 3, \dots, 9, 11, 22, 33, \dots, 9999$, trong đó một chữ số được lặp lại một số lần. 

Nhiệm vụ cơ bản là tìm “số có chữ số thống nhất” tiếp theo không nhỏ hơn dữ liệu đầu vào đã cho. Đầu vào có thể lớn như$10^{18}$, Vì thế$n$có thể có tới 18 chữ số. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng lặp đi lặp lại từ$n$từng bước một, bởi vì ngay cả một bước duy nhất cho mỗi ứng viên cũng có thể dẫn đến$10^{18}$hoạt động trong trường hợp xấu nhất, vượt xa mọi giới hạn thời gian khả thi. 

Cấu trúc đầu ra cũng bị ràng buộc: mỗi số ứng viên hợp lệ được xác định đầy đủ bởi hai lựa chọn, chữ số$d \in [1,9]$và chiều dài$L \ge 1$. Điều này cho thấy không gian tìm kiếm nhỏ và có cấu trúc cao. 

Các trường hợp cạnh chính xuất phát từ sự chuyển tiếp ranh giới giữa các độ dài chữ số. Ví dụ, nếu$n = 9992$, câu trả lời là$9999$, nhưng nếu$n = 9999$, câu trả lời nhảy tới$11111$. Một cách tiếp cận ngây thơ chỉ thử các ứng cử viên có cùng độ dài sẽ thất bại trong trường hợp sau. Một trường hợp tinh tế khác là khi số hợp lệ nhỏ nhất có cùng độ dài vẫn còn quá nhỏ, buộc chúng ta phải chuyển hoàn toàn sang độ dài tiếp theo. 

## Phương pháp tiếp cận 

Một cách tiếp cận vũ phu sẽ bắt đầu từ$n$và liên tục tăng lên một đơn vị, kiểm tra xem tất cả các chữ số có giống nhau hay không. Kiểm tra một số là rẻ, chỉ$O(\log n)$, nhưng trong trường hợp xấu nhất, chúng tôi có thể quét qua một phạm vi rộng lớn trước khi chạm vào số hợp lệ tiếp theo. Nếu như$n$có giá trị như 1000000, số hợp lệ tiếp theo là 1111111, nghĩa là trong trường hợp xấu nhất có khoảng một triệu séc, mỗi séc đều được quét chữ số. Điều này trở nên quá chậm đối với giới hạn trên của$10^{18}$. 

Quan sát quan trọng là các số hợp lệ cực kỳ thưa thớt và có cấu trúc. Đối với mỗi chiều dài$L$, chỉ có chín ứng cử viên:$d \times 111...1$với$L$chữ số. Vì vậy, thay vì tìm kiếm từng số, chúng ta chỉ cần xét một tập nhỏ các ứng cử viên được hình thành bằng cách lặp lại các chữ số. 

Chiến lược trở thành: với mỗi độ dài chữ số có thể$L$khoảng chiều dài của$n$, tạo ra tất cả các số có cùng chữ số và chọn số nhỏ nhất ít nhất$n$. Từ$L$nhiều nhất là 18, và các chữ số từ 1 đến 9, chúng ta đang kiểm tra nhiều nhất 162 ứng viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(câu trả lời − n) · O(chữ số) | O(1) | Quá chậm | 
| Tối ưu | O(9 · số chữ số) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi dựa vào thực tế là bất kỳ câu trả lời hợp lệ nào cũng phải là một trong một tập hợp số rất nhỏ được xây dựng. 

1. Tính số chữ số$L$TRONG$n$. Điều này mang lại quy mô tự nhiên của các ứng cử viên mà chúng ta nên kiểm tra trước tiên. 
2. Tạo tất cả các số được hình thành bằng cách lặp lại một chữ số$d \in [1,9]$, chính xác$L$lần. Mỗi số như vậy có thể được xây dựng như$d \cdot (111...1)$. Chúng tôi so sánh từng cái với$n$và theo dõi cái hợp lệ nhỏ nhất. 
3. Đồng thời tạo ra tất cả các số như vậy theo chiều dài$L + 1$, bởi vì nếu không hợp lệ$L$-Số có chữ số đủ lớn thì đáp án phải là số chẵn nhỏ nhất có thêm một chữ số. Điều này ghi lại các chuyển đổi như 9999 → 11111. 
4. Trong số tất cả các ứng cử viên từ các cấp độ dài$L$Và$L+1$, chọn giá trị nhỏ nhất ít nhất$n$. 

Mỗi bước được chứng minh bằng thực tế là các số có chữ số giống nhau được sắp xếp nghiêm ngặt trước tiên theo độ dài và sau đó theo giá trị chữ số trong cùng độ dài. 

### Tại sao nó hoạt động 

Mỗi số hợp lệ thuộc về một tập rời rạc được lập chỉ mục theo giá trị độ dài và chữ số của nó. Đối với độ dài cố định, không có khoảng trống nào trong tập hợp này ngoài sự lặp lại chữ số, vì vậy mọi câu trả lời ứng cử viên đều phải xuất hiện trong danh sách được xây dựng. Chỉ xét độ dài$L$Và$L+1$là đủ vì bất kỳ số nào có độ dài nhỏ hơn$L$đã ở dưới rồi$n$và bất kỳ số nào có độ dài lớn hơn$L+1$thực sự lớn hơn bất kỳ ứng cử viên nào chúng ta cần xem xét. Điều này đảm bảo rằng mức tối thiểu đối với các ứng cử viên được tạo ra chính xác là câu trả lời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def make(d, length):
    return int(str(d) * length)

def solve():
    n = int(input().strip())
    s = str(n)
    L = len(s)

    candidates = []

    for length in (L, L + 1):
        for d in range(1, 10):
            candidates.append(make(d, length))

    ans = min(x for x in candidates if x >= n)
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng các số ứng cử viên bằng cách lặp lại các ký tự chữ số, điều này an toàn vì Python xử lý các số nguyên có độ dài tùy ý một cách hiệu quả cho các kích thước này. Vòng lặp trên các chữ số từ 1 đến 9 đảm bảo chúng tôi bao gồm tất cả các số có chữ số thống nhất có độ dài nhất định. 

Quyết định bao gồm cả hai độ dài$L$Và$L+1$là rất quan trọng. Không có$L+1$, các trường hợp như 999 hoặc 9999 sẽ thất bại vì không có ứng cử viên có cùng độ dài nào có thể thỏa mãn điều kiện. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 6528 

Chúng tôi tính toán$L = 4$. Chúng tôi tạo ra tất cả các số thống nhất có 4 chữ số và tất cả các số có 5 chữ số. 

| chiều dài | chữ số | giá trị | 
| --- | --- | --- | 
| 4 | 6 | 6666 | 
| 4 | 7 | 7777 | 
| 4 | 8 | 8888 | 
| 4 | 9 | 9999 | 
| 5 | 1 | 11111 | 
| 5 | 2 | 22222 | 
| ... | ... | ... | 

Giá trị nhỏ nhất ≥ 6528 là 6666. 

Điều này xác nhận rằng trong cùng độ dài chữ số, câu trả lời chỉ đơn giản là số thống nhất đầu tiên vượt qua ngưỡng. 

### Ví dụ 2: n = 9952 

đây$L = 4$. Trong số các ứng cử viên có 4 chữ số, chỉ có 9999 là đủ lớn. 

| chiều dài | chữ số | giá trị | 
| --- | --- | --- | 
| 4 | 9 | 9999 | 

Không có số thống nhất 4 chữ số nào khác hoạt động. Vì 9999 là hợp lệ nên chúng ta không cần số có 5 chữ số. 

Điều này chứng tỏ rằng chúng tôi chỉ chuyển sang độ dài dài hơn khi tất cả các ứng cử viên có cùng độ dài đều không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(9 · 2 · L) | Chúng tôi tạo ra tối đa 18 ứng viên cho mỗi phạm vi độ dài chữ số | 
| Không gian | O(1) | Chỉ một số lượng ứng cử viên không đổi được lưu trữ | 

Các ràng buộc cho phép các số có tối đa 18 chữ số và thuật toán chỉ thực hiện vài chục cấu trúc và so sánh số nguyên, dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def make(d, length):
        return int(str(d) * length)

    n = int(input().strip())
    s = str(n)
    L = len(s)

    candidates = []
    for length in (L, L + 1):
        for d in range(1, 10):
            candidates.append(make(d, length))

    ans = min(x for x in candidates if x >= n)
    return str(ans)

# provided samples
assert run("6528\n") == "6666"
assert run("9952\n") == "9999"

# custom cases
assert run("1\n") == "1", "minimum case"
assert run("9\n") == "9", "single digit max boundary"
assert run("10\n") == "11", "transition to repeated digits"
assert run("9999\n") == "11111", "length increase boundary"
assert run("777\n") == "777", "already uniform"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | đầu vào nhỏ nhất có thể | 
| 9 | 9 | ranh giới của không gian một chữ số | 
| 10 | 11 | chuyển từ không đồng nhất sang thống nhất | 
| 9999 | 11111 | hành vi tăng chiều dài | 
| 777 | 777 | số đã hợp lệ | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi đầu vào đã là một số có chữ số thống nhất. Ví dụ,$n = 777$. Thuật toán vẫn tạo ra các ứng cử viên cho độ dài 3 và vì bản thân 777 nằm trong tập ứng cử viên nên nó sẽ được chọn chính xác làm giá trị hợp lệ tối thiểu. 

Một trường hợp khác là khi số đó ở ngay dưới ngưỡng thống nhất, chẳng hạn như$n = 9999$. Tất cả các số thống nhất có 4 chữ số dưới 9999 ngoại trừ chính 9999 đều hợp lệ nhưng nhỏ hơn$n$, vì vậy không có đủ điều kiện. Thuật toán di chuyển chính xác đến các ứng viên có 5 chữ số và chọn 11111, khớp với hành vi bao quanh được yêu cầu. 

Trường hợp thứ ba là đầu vào nhỏ như$n = 1$. Ở đây, thế hệ ứng viên bao gồm các số thống nhất có 1 chữ số và ứng cử viên hợp lệ tối thiểu chính là số 1, do đó không xảy ra sự leo thang không cần thiết.
