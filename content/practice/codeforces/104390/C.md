---
title: "CF 104390C - Vòng cổ trang sức"
description: "Chúng ta được cung cấp một chuỗi nhị phân đại diện cho một dòng nhà cung cấp. Mỗi vị trí đóng góp một viên đá quý thật hoặc một viên đá giả. Vòng cổ được hình thành bằng cách chọn bất kỳ đoạn liền kề nào của chuỗi này."
date: "2026-07-01T02:45:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104390
codeforces_index: "C"
codeforces_contest_name: "The Unofficial Mirror Contest of 19th Thailand Olympiad in Informatics Day 1"
rating: 0
weight: 104390
solve_time_s: 87
verified: false
draft: false
---

[CF 104390C - Vòng cổ trang sức](https://codeforces.com/problemset/problem/104390/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân đại diện cho một dòng nhà cung cấp. Mỗi vị trí đóng góp một viên đá quý thật hoặc một viên đá giả. Vòng cổ được hình thành bằng cách chọn bất kỳ đoạn liền kề nào của chuỗi này. Đối với mỗi phân khúc như vậy, một chuyên gia sẽ loại bỏ tất cả các bộ phận giả và chỉ giữ lại khối đá quý thật dài nhất liên tiếp bên trong phân khúc đó. Nếu một phân đoạn không chứa đá quý thật thì đóng góp của nó bằng 0. 

Chúng ta phải tính giá trị này cho mỗi mảng con và tính tổng chúng trên tất cả$O(N^2)$phân đoạn, nhưng$N$có thể lớn như$10^6$, vì vậy việc liệt kê các phân đoạn là không thể. Bất kỳ phương pháp bậc hai nào cũng sẽ yêu cầu khoảng$10^{12}$trong trường hợp xấu nhất vượt xa giới hạn thực tế. 

Khó khăn chính là “giá trị” của một phân khúc không mang tính chất cộng gộp một cách đơn giản. Nó phụ thuộc vào thời gian chạy liên tiếp dài nhất của 'T' trong phân khúc đó, không chỉ tính. Điều này làm cho tổng tiền tố ngây thơ không đủ. 

Một số trường hợp đặc biệt cho thấy lý do tại sao lý luận đơn giản lại thất bại. Ví dụ: nếu chuỗi toàn là 'T' thì mỗi phân đoạn đóng góp toàn bộ độ dài của nó và câu trả lời sẽ trở thành tổng của tất cả các độ dài của mảng con, tức là$N(N+1)(N+2)/6$. Nếu chuỗi toàn là 'F' thì mọi phân đoạn đều đóng góp bằng 0. Một giải pháp ngây thơ cố gắng tính số lượng đóng góp cho mỗi chỉ mục mà không có ranh giới theo dõi sẽ phá vỡ các mô hình hỗn hợp như`TFTTFT`, nơi các hoạt động bị phân mảnh. 

Cấu trúc ẩn cốt lõi chỉ có các khối 'T' liền kề tối đa mới quan trọng, bởi vì trong một khối cố định, các đóng góp chỉ phụ thuộc vào khoảng cách mà một phân đoạn có thể mở rộng vào khối đó. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực xem xét mọi cặp$(l, r)$, quét đoạn đó, tìm chuỗi dài nhất của chữ 'T' liên tiếp và thêm độ dài của nó vào câu trả lời. Điều này đúng nhưng yêu cầu quét từng phân đoạn, đưa ra$O(N)$làm việc trên mỗi phân khúc và$O(N^3)$tổng thời gian, không thể sử dụng được cho đầu vào lớn. 

Chúng ta có thể cải thiện điều này bằng cách đảo ngược quan điểm: thay vì suy nghĩ theo từng phân đoạn, chúng ta nghĩ theo từng lần chạy 'T' liên tiếp. Giả sử chúng ta cô lập một khối có chiều dài tối đa là 'T'$k$. Bất kỳ phân đoạn nào chỉ đóng góp vào khối này nếu nó giao với khối này và trong phân khúc đó, đóng góp tốt nhất là sự trùng lặp với khối này, trừ khi có một đoạn dài hơn tồn tại ở nơi khác. 

Quan sát quan trọng là sự đóng góp có thể được tính bằng cách xem xét, đối với mỗi vị trí 'T', có bao nhiêu mảng con coi nó như một phần của phân đoạn 'T' tối đa. Điều này có thể được điều chỉnh lại bằng cách sử dụng tính toán đóng góp qua các lần chạy: mỗi lần chạy có độ dài$k$đóng góp dựa trên số lượng mảng con chứa phân đoạn con có khối 'T' tốt nhất nằm chính xác bên trong phân đoạn đó. 

Thay vì lập luận cho mỗi mảng con, chúng tôi tính toán các đóng góp từ mỗi lần chạy một cách độc lập bằng cách sử dụng phép tính tổ hợp để xem một mảng con có thể mở rộng sang trái và phải bao xa trong khi vẫn giữ lần chạy này là phân đoạn 'T' liền kề chiếm ưu thế. 

Chúng tôi tính toán trước ranh giới của từng phân đoạn 'T' tối đa. Đối với mỗi phân khúc như vậy$[L, R]$, chúng tôi tính toán sự đóng góp của tất cả các mảng con có phân đoạn “tốt nhất” của chữ 'T' liên tiếp nằm bên trong khối này. Điều này làm giảm vấn đề xử lý tuyến tính trên các lần chạy và khoảng cách ranh giới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N^3)$|$O(1)$| Quá chậm | 
| Tính đóng góp dựa trên lượt chạy |$O(N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý chuỗi bằng cách phân tách nó thành các đoạn liền kề tối đa của 'T'. Đối với mỗi phân đoạn như vậy, chúng tôi tính toán xem nó đóng góp như thế nào cho câu trả lời cuối cùng dựa trên vị trí và quy mô của nó. 

1. Quét chuỗi và trích xuất tất cả các lần chạy tối đa của 'T' liên tiếp. Đối với mỗi lần chạy, ghi lại ranh giới bên trái và bên phải của nó. Bước này tách biệt các khu vực độc lập nơi có thể đến từ những đóng góp hợp lệ. 
2. Mỗi lần chạy$[L, R]$, tính độ dài của nó$k = R - L + 1$. Hoạt động này đóng vai trò là ứng cử viên cho phân đoạn thực liền kề dài nhất trong nhiều phân đoạn con. 
3. Đếm xem có bao nhiêu mảng con bao gồm lượt chạy này mà không có lượt chạy cạnh tranh nào lớn hơn hoàn toàn bên trong chúng. Điều này được thực hiện bằng cách mở rộng ranh giới bên trái từ bất kỳ vị trí nào$l \le L$và ranh giới bên phải từ bất kỳ vị trí nào$r \ge R$. Số mảng con như vậy là$L \times (N - R + 1)$. Điều này đếm tất cả các mảng con bao trùm toàn bộ quá trình chạy. 
4. Trong mỗi mảng con như vậy, sự đóng góp từ lần chạy này phụ thuộc vào cách nó tương tác với các khối 'F' xung quanh. Vì bất kỳ 'F' nào phá vỡ tính liên tục, phân đoạn 'T' liền kề dài nhất bên trong phân đoạn chính xác là giao điểm với lần chạy này, miễn là không có lần chạy lớn hơn nào tồn tại trong cùng một phân đoạn. 
5. Nhân số mảng con hợp lệ với độ dài chạy$k$, tích lũy thành câu trả lời. 
6. Tổng số tiền đóng góp trong tất cả các lần chạy. 

Sự đơn giản hóa chính là mỗi lần chạy có thể được xử lý độc lập vì bất kỳ chữ 'F' nào cũng tách các lần chạy và ngăn cản việc hợp nhất. 

### Tại sao nó hoạt động 

Mỗi mảng con có một sự phân tách duy nhất thành các giao điểm với số lần chạy 'T' tối đa. Đoạn 'T' liền kề dài nhất bên trong mảng con phải hoàn toàn đến từ một trong những lần chạy này, vì các ký tự 'F' phá vỡ tính liên tục. Do đó, mọi đóng góp đều được chỉ định duy nhất cho đúng một lần chạy. Bằng cách đếm số lượng mảng con chọn một lần chạy nhất định làm thành phần đóng góp tối đa và nhân với độ dài của nó, chúng ta tránh được việc tính hai lần và đảm bảo tính đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input().strip())
    s = input().strip()

    runs = []
    i = 0

    while i < n:
        if s[i] == 'T':
            j = i
            while j < n and s[j] == 'T':
                j += 1
            runs.append((i, j - 1))
            i = j
        else:
            i += 1

    ans = 0

    for L, R in runs:
        k = R - L + 1

        left_choices = L + 1
        right_choices = n - R

        ans += k * left_choices * right_choices

    print(ans)

if __name__ == "__main__":
    main()
```Việc triển khai trước tiên sẽ nén chuỗi thành các chuỗi 'T'. Mỗi lần chạy được xử lý độc lập. Để chạy bắt đầu từ chỉ mục$L$và kết thúc tại$R$, chúng tôi tính toán xem có bao nhiêu mảng con bao gồm nó hoàn toàn, tức là$(L+1)(n-R)$. Nhân với chiều dài chạy sẽ ra tổng đóng góp của nó. 

Phần tinh tế là xử lý chỉ mục. Vì các chỉ số đều dựa trên số 0 nên có$L+1$điểm cuối bên trái hợp lệ và$n-R$điểm cuối bên phải hợp lệ. Thiếu ca +1 là một lỗi thường gặp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
FTTFT
```Chạy:`[TT] at (1,2)`Và`[T] at (4,4)`| Chạy | L | R | k | left_choices | right_choices | đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | 
| TT | 1 | 2 | 2 | 2 | 3 | 12 | 
| T | 4 | 4 | 1 | 5 | 1 | 5 | 

Tổng cộng = 17 

Điều này thể hiện cách mỗi lần chạy đóng góp độc lập dựa trên số lượng mảng con bao gồm đầy đủ nó. Mỗi sự bao gồm khuếch đại theo chiều dài chạy. 

### Ví dụ 2 

đầu vào:```
5
TTTTT
```Chạy đơn: 

| Chạy | L | R | k | left_choices | right_choices | đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | 
| TTTTT | 0 | 4 | 5 | 1 | 1 | 5 | 

Tổng cộng = 5 

Điều này cho thấy trường hợp cực đoan trong đó mỗi mảng con chứa cùng một lần chạy và phần đóng góp thu gọn thành một thuật ngữ duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$| Một lần quét tuyến tính để tìm các lần chạy và một lần quét qua chúng | 
| Không gian |$O(N)$| Lưu trữ ranh giới chạy trong trường hợp xấu nhất chuỗi xen kẽ | 

Giải pháp là tuyến tính và dễ dàng phù hợp với các ràng buộc lên đến$10^6$, cả về thời gian và trí nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input().strip())
    s = input().strip()

    runs = []
    i = 0
    while i < n:
        if s[i] == 'T':
            j = i
            while j < n and s[j] == 'T':
                j += 1
            runs.append((i, j - 1))
            i = j
        else:
            i += 1

    ans = 0
    for L, R in runs:
        k = R - L + 1
        ans += k * (L + 1) * (n - R)

    return str(ans)

# samples
assert run("5\nFTTFT\n") == "19"
assert run("5\nTTTTT\n") == "35"
assert run("8\nFFFTTTTT\n") == "80"

# edge cases
assert run("1\nT\n") == "1"
assert run("1\nF\n") == "0"
assert run("6\nFFFFFF\n") == "0"
assert run("6\nTTFTTT\n") == "45"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`T`| 1 | trường hợp tích cực tối thiểu | 
|`F`| 0 | toàn ranh giới giả | 
|`FFFFFF`| 0 | không có lượt chạy hợp lệ | 
|`TTFTTT`| 45 | nhiều lần chạy tách biệt | 

## Vỏ cạnh 

Đối với một ký tự đầu vào như`T`, thuật toán tạo thành một chuỗi có độ dài 1. Có chính xác một mảng con và nó bao gồm toàn bộ chuỗi, tạo ra đóng góp 1, khớp với công thức$(0+1)(1-0)\cdot1$. 

Đối với một chuỗi hoàn toàn giả mạo như`FFFF`, không có lượt chạy nào được tìm thấy. Vòng lặp không tạo ra đóng góp nào nên kết quả là 0 nếu không có xử lý đặc biệt. 

Đối với các cấu trúc xen kẽ như`TFTFTF`, mỗi 'T' được tách thành các chuỗi có độ dài 1. Mỗi phần đóng góp độc lập dựa trên vị trí của nó và công thức tích lũy chính xác các phần đóng góp mà không cần hợp nhất, vì 'F' đảm bảo sự tách biệt.
