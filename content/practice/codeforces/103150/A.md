---
title: "CF 103150A - Truy vấn phạm vi bổ sung"
description: "Chúng ta được cung cấp một mảng các số nguyên và chúng ta liên tục áp dụng một phép biến đổi rất cụ thể cho nó. Trong một bước chuyển đổi, mọi vị trí đều được cập nhật cùng lúc sao cho mỗi phần tử trở thành tổng của tất cả các phần tử khác trong mảng, ngoại trừ chính nó."
date: "2026-07-03T19:53:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103150
codeforces_index: "A"
codeforces_contest_name: "EZ Programming Contest #1"
rating: 0
weight: 103150
solve_time_s: 52
verified: true
draft: false
---

[CF 103150A - Truy vấn phạm vi bổ sung](https://codeforces.com/problemset/problem/103150/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và chúng ta liên tục áp dụng một phép biến đổi rất cụ thể cho nó. Trong một bước chuyển đổi, mọi vị trí đều được cập nhật cùng lúc sao cho mỗi phần tử trở thành tổng của tất cả các phần tử khác trong mảng, ngoại trừ chính nó. 

Sau khi thực hiện chính xác phép biến đổi này$k$Đôi khi, chúng tôi được yêu cầu tính toán mức độ phân tán của các giá trị mảng cuối cùng, được đo bằng chênh lệch tuyệt đối tối đa giữa hai phần tử bất kỳ. 

Một cách quan trọng để diễn giải lại thao tác là tập trung vào tổng của mảng. Gọi tổng hiện tại là$S$. Sau một thao tác, mỗi phần tử$a_i$trở thành$S - a_i$. Điều này có nghĩa là mọi giá trị đang được phản ánh xung quanh tổng số hiện tại theo một cách rất có cấu trúc, điều này cho thấy rằng sự phát triển của mảng rất đều đặn thay vì hỗn loạn. 

Những hạn chế làm cho quan sát này trở nên quan trọng. Số lượng hoạt động$k$có thể rất lớn, lên tới$10^9$, vì vậy việc mô phỏng quy trình từng bước là không thể. Ngay cả một lần vượt qua cho mỗi thao tác cũng sẽ vượt quá giới hạn thời gian khi tính tổng trên tất cả các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào phụ thuộc vào việc áp dụng phép biến đổi một cách rõ ràng nhiều lần. 

Một hạn chế quan trọng khác là tổng số phần tử trong tất cả các trường hợp thử nghiệm lên tới$2 \cdot 10^5$. Điều này cho thấy chúng ta nên hướng tới một$O(n)$hoặc$O(n \log n)$giải pháp cho mỗi trường hợp thử nghiệm, lý tưởng nhất là xử lý thời gian không đổi cho mỗi thử nghiệm sau khi tiền xử lý. 

Trường hợp cạnh tinh tế xuất hiện khi$n = 1$. Trong trường hợp đó, thao tác thay thế phần tử duy nhất bằng tổng của tất cả các phần tử khác bằng 0. Nhưng vì không có "phần tử nào khác" nên tổng bằng 0 và mãi mãi bằng 0. Điều này có nghĩa là câu trả lời luôn bằng 0 bất kể$k$. Bất kỳ giải pháp nào cũng phải xử lý rõ ràng trường hợp này để tránh tính toán không cần thiết hoặc chuyển đổi không chính xác. 

Trường hợp cạnh quan trọng thứ hai là khi tất cả các phần tử đều bằng nhau. Sau một thao tác, mỗi phần tử sẽ trở thành$(n-1)x$, do đó mảng vẫn đồng nhất. Sự khác biệt giữa mức tối đa và tối thiểu vẫn bằng 0 và điều này vẫn đúng cho tất cả các hoạt động tiếp theo. 

Hành vi thú vị chỉ xuất hiện khi các giá trị khác nhau và thách thức chính là hiểu được sự khác biệt giữa các phần tử phát triển như thế nào khi biến đổi lặp đi lặp lại. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu trực tiếp áp dụng phép biến đổi$k$lần. Mỗi thao tác sẽ tính lại tổng số và xây dựng lại mảng theo$O(n)$, dẫn đến sự phức tạp tổng thể của$O(nk)$. Với$k$lên đến$10^9$, điều này hoàn toàn không thể thực hiện được. 

Thông tin chi tiết quan trọng là theo dõi cách các phần tử riêng lẻ liên quan với nhau thay vì giá trị tuyệt đối của chúng. Nếu chúng ta mở rộng một thao tác, mỗi phần tử sẽ trở thành$S - a_i$, tương đương với$(n-1)\cdot \text{mean of array} - (a_i - \text{mean})$đến các phép biến đổi tuyến tính. Quan trọng hơn, điều quan trọng đối với câu trả lời cuối cùng không phải là giá trị tuyệt đối mà là sự khác biệt giữa hai vị trí. 

Chúng ta hãy xem xét hai yếu tố$a_i$Và$a_j$. Sau một thao tác, sự khác biệt của chúng trở thành:$$(S - a_i) - (S - a_j) = a_j - a_i$$Vậy hiệu đổi dấu nhưng giữ nguyên độ lớn. 

Sau hai thao tác, hiệu lại đảo ngược và trở về dấu ban đầu. Điều này có nghĩa là cấu trúc của sự khác biệt theo cặp là tuần hoàn với chu kỳ 2. Toàn bộ mảng xen kẽ giữa hai trạng thái: mảng ban đầu và phiên bản được chuyển đổi của nó trong đó mỗi giá trị là$S - a_i$. 

Vì vậy, vấn đề giảm xuống còn việc đánh giá nhiều nhất là hai trạng thái tùy thuộc vào việc liệu$k$là chẵn hoặc lẻ. 

Chúng tôi tính toán: 

- Nếu$k$chẵn thì cấu trúc mảng tương đương với bản gốc. 
- Nếu như$k$là số lẻ, mỗi phần tử trở thành$S - a_i$, Ở đâu$S$là tổng ban đầu 

Từ đó, việc tính toán chênh lệch tối đa rất đơn giản: chúng ta chỉ lấy chênh lệch giữa tối đa và tối thiểu ở trạng thái tương ứng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(nk)$|$O(n)$| Quá chậm | 
| Quan sát chẵn lẻ (hệ thống 2 trạng thái) |$O(n)$|$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và tính tổng của nó$S$, cũng như các giá trị tối thiểu và tối đa của nó. Ba giá trị này xác định đầy đủ cả hai trạng thái có thể có của hệ thống, vì phép biến đổi chỉ phụ thuộc vào$S$và các yếu tố riêng lẻ. 
2. Nếu$n = 1$, ngay lập tức xuất ra 0. Với một phần tử, phép biến đổi luôn tạo ra 0 và không có biến thể. 
3. Nếu$k$là chẵn, chúng ta biết mảng vẫn giữ nguyên cấu hình ban đầu một cách hiệu quả. Sự khác biệt tối đa chỉ đơn giản là$\max(a) - \min(a)$, bởi vì không có thay đổi cấu trúc nào ảnh hưởng đến thứ tự tương đối. 
4. Nếu$k$là số lẻ, mỗi phần tử trở thành$S - a_i$. Trong mảng được chuyển đổi này, phần tử lớn nhất tương ứng với$S - \min(a)$và mức tối thiểu tương ứng với$S - \max(a)$. Sự đảo ngược này xảy ra vì phép trừ sẽ đảo ngược thứ tự. 
5. Tính chênh lệch ở trạng thái chuyển đổi:$$(S - \min(a)) - (S - \max(a)) = \max(a) - \min(a)$$vì vậy sự khác biệt vẫn không thay đổi về mặt số lượng. 
6. Xuất ra sự khác biệt đã tính toán này. 

Tại sao nó hoạt động được bắt nguồn từ thực tế là mỗi thao tác áp dụng một phép biến đổi affine cho các giá trị mảng. Trong khi bản thân các giá trị thay đổi đáng kể thì tất cả các khác biệt theo cặp chỉ thay đổi dấu và không bao giờ thay đổi độ lớn. Vì bài toán chỉ yêu cầu chênh lệch tuyệt đối lớn nhất nên hệ thống sẽ chuyển sang hai trạng thái xen kẽ nhau, khiến cho việc lặp lại tiếp theo không còn phù hợp nữa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        arr = list(map(int, input().split()))

        if n == 1:
            print(0)
            continue

        total = sum(arr)
        mn = min(arr)
        mx = max(arr)

        if k % 2 == 0:
            print(mx - mn)
        else:
            # transformed array would be S - a[i]
            # min becomes S - mx, max becomes S - mn
            print((total - mn) - (total - mx))

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo quan sát rằng chúng tôi không bao giờ xây dựng các mảng trung gian một cách rõ ràng. Chúng tôi chỉ theo dõi số liệu thống kê tổng hợp. Điểm tinh tế duy nhất là xử lý tính chẵn lẻ của$k$, vì nó xác định liệu chúng ta đánh giá mảng ban đầu hay phiên bản đã chuyển đổi của nó. 

biểu thức`(total - mn) - (total - mx)`được sắp xếp cẩn thận để phản ánh đúng giá trị tối đa được chuyển đổi trừ đi giá trị tối thiểu được chuyển đổi. Mở rộng nó theo đại số cho thấy nó đơn giản hóa trở lại`mx - mn`, nhưng việc giữ nó ở dạng này làm cho logic trở nên rõ ràng và tránh được những sai lầm khi điều chỉnh ý tưởng cho phù hợp với các biến thể của vấn đề. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 1
1 3 5 4
```Chúng tôi tính toán: 

- tổng cộng = 13 
- phút = 1 
- tối đa = 5 

| Bước | Tiểu bang | 
| --- | --- | 
| Mảng ban đầu | [1, 3, 5, 4] | 
| k = 1 (lẻ) | mảng được chuyển đổi sử dụng S - a[i] | 
| S | 13 | 
| Mảng chuyển đổi | [12, 10, 8, 9] | 

Tối đa là 12 và tối thiểu là 8. 

Vậy đáp án là 4. 

Điều này xác nhận rằng đối với số lẻ$k$, phép biến đổi sẽ lật các giá trị xung quanh tổng nhưng vẫn giữ nguyên độ rộng phạm vi. 

### Ví dụ 2 

đầu vào:```
5 2
2 2 2 2 2
```| Bước | Tiểu bang | 
| --- | --- | 
| Mảng ban đầu | [2, 2, 2, 2, 2] | 
| tối thiểu/tối đa | 2/2 | 
| k thậm chí | trạng thái ban đầu | 

Mảng không đổi khi chuyển đổi, vì vậy mỗi bước đều bảo đảm sự bình đẳng giữa các phần tử. Sự khác biệt tối đa vẫn là 0. 

Điều này cho thấy các mảng thống nhất là các điểm cố định của hoạt động và không bao giờ phát triển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Chúng tôi tính tổng, tối thiểu, tối đa một lần cho mỗi mảng | 
| Không gian |$O(1)$thêm | Chỉ tổng hợp vô hướng được lưu trữ | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$tổng số phần tử, do đó chỉ cần quét tuyến tính một lần cho mỗi trường hợp thử nghiệm là đủ. Không phụ thuộc vào$k$xuất hiện trong thuật toán cuối cùng, điều này rất cần thiết vì$k$có thể lớn như$10^9$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = []

    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        if n == 1:
            out.append("0")
            continue

        s = sum(a)
        mn, mx = min(a), max(a)

        if k % 2 == 0:
            out.append(str(mx - mn))
        else:
            out.append(str((s - mn) - (s - mx)))

    return "\n".join(out)

# provided sample
assert run("""3
4 1
1 3 5 4
1 1000
2020
10 100000
1 1 1 1 1 1 1 1 1 1
""") == """4
0
0"""

# all equal
assert run("""1
5 123
7 7 7 7 7
""") == "0"

# single element
assert run("""1
1 999
42
""") == "0"

# even k preserves range
assert run("""1
4 2
1 10 5 7
""") == "9"

# odd k still same range width
assert run("""1
4 1
1 10 5 7
""") == "9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | xử lý trường hợp cơ bản | 
| tất cả đều bình đẳng | 0 | hành vi điểm cố định | 
| thậm chí k | mx-mn | tính đúng đắn chẵn lẻ | 
| lẻ k | cùng phạm vi | biến đổi bất biến | 

## Vỏ cạnh 

cho$n = 1$, thuật toán lập tức cho kết quả bằng 0 vì không có cặp phần tử nào tạo thành sự khác biệt. Phép biến đổi suy biến thành một chuỗi 0 không đổi, do đó không cần suy luận gì thêm. 

Đối với các mảng trong đó tất cả các phần tử đều bằng nhau, mỗi phép biến đổi sẽ tạo ra một mảng thống nhất khác có tỷ lệ bằng$n-1$, bảo toàn mức chênh lệch bằng không. Giá trị tối đa được tính toán vẫn bằng 0, khớp với câu trả lời đúng cho cả số chẵn và số lẻ$k$. 

Đối với lớn$k$, giải pháp không bao giờ mô phỏng các hoạt động. Thay vào đó, nó hoàn toàn dựa vào tính chẵn lẻ, vì vậy các giá trị như$10^9$không ảnh hưởng đến thời gian chạy hoặc tính chính xác.
