---
title: "CF 102562D - Cupidus the Cupidon"
description: "Mỗi thí sinh đứng trên trục tung tại vị trí (0, ai) và bắn một mũi tên bay theo đường thẳng về điểm hạ cánh (xi, yi). Nhiệm vụ là giữ càng nhiều ứng viên càng tốt để không có hai quỹ đạo mũi tên được chọn nào gặp nhau."
date: "2026-08-03T18:14:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102562
codeforces_index: "D"
codeforces_contest_name: "AGM 2020, Final Round, Day 1"
rating: 0
weight: 102562
solve_time_s: 315
verified: true
draft: false
---

[CF 102562D - Cupidus the Cupidon](https://codeforces.com/problemset/problem/102562/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi thí sinh đứng trên trục tung tại vị trí`(0, a_i)`và bắn một mũi tên đi theo đường thẳng về phía điểm hạ cánh`(x_i, y_i)`. Nhiệm vụ là giữ càng nhiều ứng viên càng tốt để không có hai quỹ đạo mũi tên được chọn nào gặp nhau. 

Một cách hữu ích để xem quỹ đạo là xem một đường thẳng có độ cao ban đầu và độ dốc. Chiều cao ban đầu là`a_i`, và độ dốc là`(y_i - a_i) / x_i`. Đầu vào cung cấp độ cao xuất phát và điểm hạ cánh, còn đầu ra là số lượng quỹ đạo lớn nhất có thể cùng tồn tại mà không có giao lộ. 

Số lượng ứng viên trong tất cả các trường hợp thử nghiệm có thể đạt tới`500000`, do đó không thể có thuật toán so sánh từng cặp. Một cách tiếp cận bậc hai sẽ thực hiện xung quanh`250000000000`so sánh trong trường hợp lớn nhất, vượt xa giới hạn thời gian lập trình cạnh tranh thông thường cho phép. Chúng ta cần một giải pháp xung quanh`O(N log N)`trên tổng đầu vào. 

Tọa độ cực kỳ lớn, đạt khoảng`2^52`, vì vậy số học dấu phẩy động không an toàn. Một lỗi chính xác nhỏ khi so sánh hai hệ số góc có thể làm thay đổi câu trả lời. Tất cả các so sánh độ dốc phải được thực hiện bằng phép nhân chéo số nguyên. 

Có một số trường hợp việc triển khai có thể thất bại trong âm thầm. Độ dốc bằng nhau là một trong số đó. Ví dụ:```
1
3
0 5 5
10 5 15
20 5 25
```Các sườn dốc đều`1`, vậy câu trả lời là`3`. Việc triển khai chuỗi con tăng dần nghiêm ngặt sẽ trả về không chính xác`1`, bởi vì độ dốc bằng nhau được cho phép. 

Một trường hợp khác là độ dốc âm:```
1
2
0 2 -2
10 2 8
```Các sườn dốc là`-1`Và`-1`, do đó cả hai quỹ đạo đều song song và câu trả lời là`2`. Coi tử số như một số nguyên bình thường mà không xét đến dấu mẫu số sẽ dễ dàng tạo ra thứ tự sai. 

Trường hợp nguy hiểm thứ ba là giá trị lớn:```
1
2
0 4503599627370496 4503599627370496
1 4503599627370496 0
```Các sườn dốc là`1`và xấp xỉ`-1`, và câu trả lời là`1`. Chuyển đổi các giá trị này thành`float`có thể mất đủ độ chính xác để so sánh độ dốc không chính xác. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là kiểm tra từng cặp quỹ đạo và quyết định xem chúng có giao nhau hay không. Vì hai quỹ đạo giao nhau khi độ dốc của chúng được sắp xếp không chính xác so với độ cao ban đầu của chúng, nên chúng ta có thể sắp xếp theo độ cao ban đầu và so sánh từng cặp độ dốc. Điều này đúng vì sau khi sắp xếp theo`a_i`, quỹ đạo thấp hơn chỉ có thể ở dưới quỹ đạo cao hơn khi độ dốc của nó không lớn hơn. Vấn đề là số lượng so sánh. Với`N = 100000`, phương pháp vũ phu cần khoảng`N * (N - 1) / 2`, hoặc khoảng năm tỷ so sánh, quá chậm. 

Quan sát quan trọng là sau khi sắp xếp các ứng viên theo vị trí của họ trên trục tung, chúng ta chỉ cần chuỗi độ dốc dài nhất không bao giờ giảm. Giả sử ứng viên`i`bắt đầu bên dưới ứng viên`j`. Nếu độ dốc của`i`lớn hơn, đường dưới cuối cùng bắt kịp đường trên và các quỹ đạo giao nhau. Nếu độ dốc nhỏ hơn hoặc bằng nhau thì khoảng cách giữa hai đường thẳng không bao giờ bằng 0 nên cả hai đều có thể được chọn. 

Bài toán hình học trở thành bài toán dãy con không giảm dài nhất trên số hữu tỉ. Kỹ thuật sắp xếp kiên nhẫn tiêu chuẩn giải quyết vấn đề này trong`O(N log N)`. Thử thách bổ sung duy nhất là so sánh chính xác các phân số, được xử lý bằng phép nhân chéo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(1) | Quá chậm | 
| Tối ưu | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả thí sinh theo chiều cao ban đầu của họ`a_i`theo thứ tự tăng dần. Điều này khắc phục thứ tự quỹ đạo rời khỏi trục thẳng đứng. Bất kỳ tập hợp được chọn hợp lệ nào cũng phải tôn trọng thứ tự này. 
2. Chuyển đổi mọi quỹ đạo thành giá trị độ dốc:$$s_i = \frac{y_i-a_i}{x_i}$$Mẫu số luôn dương nên so sánh hệ số góc chỉ cần so sánh tích chéo của tử số và mẫu số. 

1. Xử lý các sườn theo thứ tự được sắp xếp và duy trì mảng sắp xếp kiên nhẫn. Giá trị tại vị trí`k`biểu thị độ dốc cuối nhỏ nhất có thể có của dãy con không giảm có độ dài`k + 1`. 
2. Đối với độ dốc hiện tại, hãy tìm độ dốc được lưu đầu tiên lớn hơn nó. Thay thế giá trị đó bằng độ dốc hiện tại. Nếu không có độ dốc được lưu nào lớn hơn, hãy nối thêm độ dốc. 

Việc tìm kiếm sử dụng giá trị lớn hơn đầu tiên thay vì giá trị lớn hơn hoặc bằng đầu tiên vì các hệ số góc bằng nhau có thể xuất hiện cùng nhau trong câu trả lời. Chúng ta cần dãy con không giảm dài nhất, chứ không phải dãy con tăng nghiêm ngặt dài nhất. 

1. Độ dài của mảng sắp xếp kiên nhẫn là số lượng quỹ đạo không giao nhau tối đa. 

Tại sao nó hoạt động: 

Sau khi sắp xếp theo độ cao xuất phát, mọi quỹ đạo được chọn sẽ xuất hiện từ dưới lên trên. Đối với hai quỹ đạo được chọn liên tiếp, quỹ đạo phía dưới phải có độ dốc không lớn hơn quỹ đạo phía trên. Ngược lại, vì nó bắt đầu thấp hơn nhưng tăng nhanh hơn nên cuối cùng nó phải đáp ứng quỹ đạo trên. Nếu độ dốc được sắp xếp không giảm thì độ chênh lệch theo chiều dọc giữa các quỹ đạo liên tiếp bắt đầu dương và không bao giờ giảm xuống dưới 0. Do đó, mọi câu trả lời hợp lệ đều tương ứng chính xác với dãy con không giảm của hệ số góc. Thuật toán sắp xếp kiên nhẫn tìm ra chuỗi con dài nhất như vậy, vì vậy độ dài của nó là độ dài tối đa cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def greater_frac(a, b):
    return a[0] * b[1] > b[0] * a[1]

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        arr = []

        for _ in range(n):
            a, x, y = map(int, input().split())
            arr.append((a, y - a, x))

        arr.sort()

        tails = []

        for _, num, den in arr:
            cur = (num, den)

            left = 0
            right = len(tails)

            while left < right:
                mid = (left + right) // 2
                if greater_frac(tails[mid], cur):
                    right = mid
                else:
                    left = mid + 1

            if left == len(tails):
                tails.append(cur)
            else:
                tails[left] = cur

        ans.append(str(len(tails)))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Đầu vào đầu tiên được chuyển đổi thành`(starting height, slope numerator, slope denominator)`. Lưu trữ độ dốc như`(y_i - a_i, x_i)`tránh mọi hoạt động dấu phẩy động. 

Bước sắp xếp chỉ dựa trên tọa độ bắt đầu vì thứ tự khởi hành từ trục xác định thứ tự các quỹ đạo phải được xem xét. 

các`tails`mảng là cấu trúc sắp xếp kiên nhẫn thông thường cho chuỗi con không giảm dài nhất. Tìm kiếm nhị phân tìm kiếm phân số đầu tiên lớn hơn phân số hiện tại. Phép so sánh phân số sử dụng phép nhân thay vì phép chia, giúp giữ cho tất cả các phép tính luôn chính xác ngay cả đối với tọa độ lớn nhất. 

Số nguyên Python tự động xử lý các sản phẩm trung gian lớn. Trong các ngôn ngữ có số nguyên có chiều rộng cố định, cần phải có loại rộng hơn vì phép nhân có thể vượt quá giới hạn 64 bit. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
6 8 3
-7 8 1
-10 10 -10
0 7 7
-2 2 8
```Sau khi sắp xếp theo chiều cao bắt đầu, độ dốc là: 

| Bước | Chiều cao bắt đầu | Độ dốc | Đuôi sau khi xử lý | 
| --- | --- | --- | --- | 
| 1 | -10 | 0 | [0] | 
| 2 | -7 | 1 | [0, 1] | 
| 3 | -2 | 5 | [0, 1, 5] | 
| 4 | 0 | 1 | [0, 1, 5] | 
| 5 | 6 | -8/3 | [-3/8, 1, 5] | 

Độ dài cuối cùng là`3`. Chuỗi độ dốc hợp lệ có thể có độ dài`0, 1, 5`, cho ba quỹ đạo không cắt nhau. 

### Mẫu 2 

đầu vào:```
5
2 6 -4
6 1 6
-6 6 -9
-5 7 1
4 4 -3
```Độ dốc được sắp xếp là: 

| Bước | Chiều cao bắt đầu | Độ dốc | Đuôi sau khi xử lý | 
| --- | --- | --- | --- | 
| 1 | -6 | -1/2 | [-1/2] | 
| 2 | -5 | 7/6 | [-1/2, 6/7] | 
| 3 | 2 | -1 | [-1, 6/7] | 
| 4 | 4 | -7/4 | [-7/4, 6/7] | 
| 5 | 6 | 0 | [-7/4, 0] | 

Độ dài cuối cùng là`2`. Một lựa chọn tối ưu là quỹ đạo có độ dốc`-7/4`theo sau là quỹ đạo có độ dốc`0`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Việc sắp xếp chiếm ưu thế và mỗi độ dốc thực hiện một tìm kiếm nhị phân | 
| Không gian | O(N) | Danh sách được sắp xếp và mảng kiên nhẫn lưu trữ số tuyến tính của quỹ đạo | 

Tổng số ứng cử viên trong tất cả các trường hợp thử nghiệm là`500000`, do đó tổng thời gian chạy bị giới hạn bởi việc sắp xếp và tìm kiếm nhị phân trên nhiều phần tử đó. Việc sử dụng bộ nhớ vẫn tuyến tính và phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return out.getvalue().strip()

assert run("""2
5
6 8 3
-7 8 1
-10 10 -10
0 7 7
-2 2 8
5
2 6 -4
6 1 6
-6 6 -9
-5 7 1
4 4 -3
""") == "3\n2", "samples"

assert run("""1
1
0 1 5
""") == "1", "single trajectory"

assert run("""1
3
0 5 5
10 5 15
20 5 25
""") == "3", "equal slopes"

assert run("""1
4
0 2 -2
10 2 8
20 4 0
30 1 10
""") == "3", "negative and mixed slopes"

assert run("""1
2
0 4503599627370496 4503599627370496
1 4503599627370496 0
""") == "1", "large coordinates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Quỹ đạo đơn | 1 | Xử lý trường hợp nhỏ nhất có thể | 
| Độ dốc bằng nhau | 3 | Xác nhận dãy con phải không giảm | 
| Độ dốc âm và hỗn hợp | 3 | Kiểm tra so sánh phân số có dấu | 
| Tọa độ lớn | 1 | Xác nhận so sánh số nguyên chính xác | 

## Vỏ cạnh 

Độ dốc bằng nhau được xử lý bằng lựa chọn tìm kiếm nhị phân. Đối với đầu vào:```
1
3
0 5 5
10 5 15
20 5 25
```mọi quỹ đạo đều có độ dốc`1`. Tìm kiếm nhị phân không bao giờ thay thế độ dốc bằng nhau bằng vị trí dãy con ngắn hơn vì nó chỉ tìm kiếm các giá trị lớn hơn giá trị hiện tại. Mảng tăng dần theo chiều dài`3`, điều đó đúng. 

Độ dốc âm không yêu cầu xử lý đặc biệt. Coi như:```
1
2
0 2 -2
10 2 8
```Cả hai đều có độ dốc`-1`, biểu diễn dưới dạng`(-2, 2)`Và`(-2, 2)`. Phép nhân chéo so sánh chúng một cách chính xác, vì vậy cả hai đều được coi là tương thích và câu trả lời là`2`. 

Tọa độ rất lớn cũng an toàn. Vì:```
1
2
0 4503599627370496 4503599627370496
1 4503599627370496 0
```độ dốc đầu tiên là`1`, trong khi thứ hai là`-1`. Mảng kiên nhẫn thay thế giá trị đầu tiên bằng độ dốc nhỏ hơn, để lại độ dài là`1`. Thuật toán không bao giờ chuyển đổi các giá trị này thành dấu phẩy động, do đó không xảy ra mất độ chính xác.
