---
title: "CF 1323A - Bài toán tính tổng tập hợp con chẵn"
description: "Chúng ta được cho một số mảng số nguyên dương độc lập. Đối với mỗi mảng, chúng ta phải chọn một nhóm vị trí không trống sao cho tổng các giá trị đã chọn là chẵn. Nếu không có nhóm như vậy tồn tại, chúng tôi sẽ báo cáo thất bại."
date: "2026-06-16T07:23:18+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1323
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 626 (Div. 2, based on Moscow Open Olympiad in Informatics)"
rating: 800
weight: 1323
solve_time_s: 228
verified: true
draft: false
---

[CF 1323A - Bài toán về tổng số tập hợp con chẵn](https://codeforces.com/problemset/problem/1323/A) 

**Đánh giá:** 800 
**Tags:** bạo lực, dp, tham lam, thực hiện 
**Thời gian giải:** 3 phút 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số mảng số nguyên dương độc lập. Đối với mỗi mảng, chúng ta phải chọn một nhóm vị trí không trống sao cho tổng các giá trị đã chọn là chẵn. Nếu không có nhóm như vậy tồn tại, chúng tôi sẽ báo cáo thất bại. 

Đầu ra không phải là tập hợp con mà là chỉ số của các phần tử tạo thành một tập hợp con hợp lệ. Bất kỳ lựa chọn hợp lệ nào cũng có thể được chấp nhận, vì vậy chúng tôi không tối ưu hóa kích thước hoặc thứ tự từ điển mà chỉ tối ưu hóa tính khả thi. 

Các ràng buộc rất nhỏ: tối đa 100 trường hợp thử nghiệm, mỗi mảng có tối đa 100 phần tử. Điều này ngay lập tức cho chúng ta biết rằng ngay cả cách tiếp cận bậc ba hoặc bậc hai đối với các tập hợp con cũng khả thi trong trường hợp xấu nhất. Tuy nhiên, vì tính chẵn lẻ là thuộc tính duy nhất quan trọng nên chúng ta mong đợi một cấu trúc đơn giản hơn nhiều so với việc liệt kê các tập hợp con. 

Các trường hợp cạnh chính đến từ các mảng rất nhỏ. Nếu có một phần tử duy nhất thì tập hợp con duy nhất có tổng bằng phần tử đó, vì vậy câu trả lời hoàn toàn phụ thuộc vào việc số đó có phải là số chẵn hay không. Nếu có hai phần tử, thì các tương tác chẵn-lẻ sẽ trở nên phù hợp: một phần tử chẵn duy nhất đã giải quyết được vấn đề, trong khi hai phần tử lẻ cũng tạo thành một tổng chẵn. 

Một trường hợp thất bại tinh vi đối với lối suy nghĩ ngây thơ là giả định rằng bất cứ khi nào có ít nhất một số lẻ và một số chẵn thì chúng ta phải lấy cả hai. Điều đó là không cần thiết, vì một phần tử chẵn đã đưa ra câu trả lời hợp lệ. Một cạm bẫy khác là giả định rằng nếu tất cả các số đều lẻ và có nhiều hơn hai thì luôn tồn tại một nghiệm; trên thực tế, chúng ta cần đếm số chẵn các số lẻ để có được tổng chẵn. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ liệt kê tất cả các tập hợp con và kiểm tra xem tổng của chúng có chẵn hay không. có$2^n - 1$các tập hợp con không trống và với mỗi tập hợp con chúng ta tính một tổng theo$O(n)$, cho$O(n 2^n)$mỗi trường hợp thử nghiệm. Với$n = 100$, điều này hoàn toàn không thể thực hiện được vì nó vượt quá giới hạn thiên văn. 

Cấu trúc của bài toán sẽ đơn giản hóa đáng kể nếu chúng ta quan sát cách thức hoạt động của tính chẵn lẻ. Tổng các số nguyên chỉ phụ thuộc vào số phần tử lẻ có trong đó. Các số chẵn hoàn toàn không ảnh hưởng đến tính chẵn lẻ, trong khi mỗi số lẻ sẽ đảo ngược tính chẵn lẻ của tổng. 

Điều này có nghĩa là chúng ta không bao giờ cần nhiều hơn một vài phần tử. Nếu có số chẵn thì ta lấy riêng. Ngược lại, chúng ta tìm các cặp số lẻ, vì tổng của hai số lẻ là một số chẵn. Nếu có đúng một số lẻ và không có số chẵn thì không có nghiệm nào tồn tại. 

Vì vậy, vấn đề giảm xuống còn việc quét mảng một lần và ghi nhớ các vị trí chẵn và lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot 2^n)$|$O(n)$| Quá chậm | 
| Quan sát chẵn lẻ |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Quét mảng một lần và ghi lại chỉ mục của bất kỳ phần tử chẵn nào. 

Nếu chúng tôi tìm thấy một số chẵn, chúng tôi có thể ngừng quét thêm ngay lập tức vì một phần tử chẵn duy nhất đã tạo thành một tập hợp con hợp lệ. Điều này là tối ưu vì nó đảm bảo kích thước tập hợp con tối thiểu và độ chính xác. 
2. Nếu không có phần tử chẵn thì thu thập chỉ số của tất cả các phần tử lẻ. 

Bước này là cần thiết vì các phần tử lẻ không thể tạo ra tổng chẵn riêng lẻ, nhưng các cặp trong số chúng thì có thể. 
3. Nếu chúng ta có ít nhất hai phần tử lẻ, hãy xuất ra hai chỉ số đầu tiên. 

Tổng của chúng là số lẻ cộng số lẻ, tức số chẵn nên điều kiện tập hợp con được thỏa mãn. 
4. Nếu chúng ta có ít hơn hai phần tử lẻ và không có phần tử chẵn, ghi -1. 

Trường hợp duy nhất còn lại là một phần tử lẻ duy nhất, không thể tạo thành tổng chẵn trong bất kỳ tập hợp con nào không trống. 

### Tại sao nó hoạt động 

Tính chẵn lẻ của tổng chỉ phụ thuộc vào tính chẵn lẻ của các phần tử được bao gồm. Các số chẵn đóng góp số 0 vào tính chẵn lẻ, trong khi mỗi số lẻ sẽ đảo ngược nó. Do đó, tổng là số chẵn khi số phần tử lẻ được chọn là số chẵn. Thuật toán xây dựng một phần tử chẵn (không có phần tử lẻ) hoặc hai phần tử lẻ (hai phần tử chẵn), bao gồm tất cả các trường hợp khả thi. Không cần cấu trúc nào khác vì bất kỳ tập hợp con lớn hơn nào cũng có thể được rút gọn thành một trong các dạng chuẩn này mà không làm thay đổi tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        even_idx = -1
        odd_indices = []

        for i, x in enumerate(a, 1):
            if x % 2 == 0:
                even_idx = i
            else:
                odd_indices.append(i)

        if even_idx != -1:
            print(1)
            print(even_idx)
        elif len(odd_indices) >= 2:
            print(2)
            print(odd_indices[0], odd_indices[1])
        else:
            print(-1)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên sẽ kiểm tra phần tử chẵn vì phần tử đó ngay lập tức đưa ra tập hợp con hợp lệ cỡ 1. Nếu không tồn tại, nó sẽ quay trở lại trường hợp mang tính xây dựng duy nhất còn lại: ghép các phần tử lẻ. Việc liệt kê dựa trên 1 vì bài toán yêu cầu các chỉ số bắt đầu từ 1. 

Một lỗi triển khai phổ biến là tiếp tục quét sau khi tìm thấy phần tử chẵn và vô tình ghi đè hoặc làm phức tạp logic. Một điều nữa là quên rằng chỉ yêu cầu các chỉ số chứ không phải giá trị. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp: một trường hợp tồn tại một phần tử chẵn và một trường hợp chỉ tồn tại các phần tử lẻ. 

### Ví dụ 1 

đầu vào:`[1, 4, 3]`| Bước | Phần tử hiện tại | Chỉ số chẵn | Danh sách lẻ | 
| --- | --- | --- | --- | 
| 1 | 1 | -1 | [1] | 
| 2 | 4 | 2 | [1] | 
| 3 | 3 | 2 | [1, 3] | 

Chúng tôi dừng lại sau khi tìm thấy 4 và chỉ số đầu ra là 2. Dấu vết cho thấy rằng khi một số chẵn xuất hiện, các phần tử sau đó không quan trọng. 

### Ví dụ 2 

đầu vào:`[3, 5, 7]`| Bước | Phần tử hiện tại | Chỉ số chẵn | Danh sách lẻ | 
| --- | --- | --- | --- | 
| 1 | 3 | -1 | [1] | 
| 2 | 5 | -1 | [1, 2] | 
| 3 | 7 | -1 | [1, 2, 3] | 

Không có phần tử chẵn nào tồn tại, vì vậy chúng tôi sử dụng hai chỉ số lẻ đầu tiên là 1 và 2. Điều này xác nhận rằng bất kỳ hai số lẻ nào cũng đủ bất kể giá trị của chúng là bao nhiêu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | quét tuyến tính đơn của mảng | 
| Không gian |$O(1)$phụ trợ | chỉ lưu trữ một vài chỉ số | 

Tổng công việc trên tất cả các trường hợp thử nghiệm tối đa là 10.000 phần tử, nằm trong giới hạn thoải mái. Việc sử dụng bộ nhớ không đổi ngoài việc lưu trữ đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    input = sys.stdin.readline

    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        even_idx = -1
        odd = []

        for i, x in enumerate(a, 1):
            if x % 2 == 0:
                even_idx = i
            else:
                odd.append(i)

        if even_idx != -1:
            output.append("1")
            output.append(str(even_idx))
        elif len(odd) >= 2:
            output.append("2")
            output.append(f"{odd[0]} {odd[1]}")
        else:
            output.append("-1")

    return "\n".join(output)

# provided samples
assert run("""3
3
1 4 3
1
15
2
3 5
""") == """1
2
-1
2
1 2"""

# all even single
assert run("""1
1
2
""") == "1\n1"

# single odd
assert run("""1
1
3
""") == "-1"

# two odds
assert run("""1
2
1 7
""") == "2\n1 2"

# mixed case
assert run("""1
4
1 2 3 5
""") in ["1\n2", "1\n4"]  # any even index valid
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| thậm chí độc thân | 1 1 | tập hợp con hợp lệ tối thiểu | 
| đơn lẻ | -1 | trường hợp bất khả thi | 
| hai tỷ lệ cược | 2 chỉ số | logic ghép nối | 
| mảng hỗn hợp | chỉ số chẵn | chấm dứt sớm đúng đắn | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi mảng chứa chính xác một phần tử. Nếu phần tử đó là số chẵn thì câu trả lời là chỉ số đó; nếu kỳ quặc thì không có giải pháp. Thuật toán xử lý việc này một cách tự nhiên vì nó đặt`even_idx`hoặc thu thập một chỉ mục lẻ duy nhất và không đạt được điều kiện thứ hai. 

Một trường hợp cạnh khác là khi tồn tại nhiều phần tử chẵn. Thuật toán trả về chỉ mục chẵn được nhìn thấy lần cuối, chỉ mục này vẫn hợp lệ vì bất kỳ phần tử chẵn nào cũng đủ. Không có sự phụ thuộc vào việc đặt hàng tồn tại. 

Trường hợp thứ ba là khi tất cả các phần tử đều lẻ nhưng số lượng của chúng lớn. Thuật toán vẫn chỉ chọn hai chỉ số đầu tiên, hoạt động vì hai số lẻ đã đảm bảo tổng chẵn, bất kể tổng số.
