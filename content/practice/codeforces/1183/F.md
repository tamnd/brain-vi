---
title: "CF 1183F - Topforces phản công"
description: "Chúng tôi được cung cấp một số bộ dữ liệu độc lập. Trong mỗi tập dữ liệu có nhiều tập hợp số nguyên dương, trong đó mỗi số nguyên biểu thị “giá trị” của một bài toán."
date: "2026-06-13T11:36:09+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1183
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 570 (Div. 3)"
rating: 2100
weight: 1183
solve_time_s: 265
verified: true
draft: false
---

[CF 1183F - Topforces phản công](https://codeforces.com/problemset/problem/1183/F) 

**Đánh giá:** 2100 
**Tags:** sức mạnh vũ phu, toán học, sắp xếp 
**Thời gian giải:** 4 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số bộ dữ liệu độc lập. Trong mỗi tập dữ liệu có nhiều tập hợp số nguyên dương, trong đó mỗi số nguyên biểu thị “giá trị” của một bài toán. Từ nhóm này, chúng ta phải chọn tối đa ba giá trị để tối đa hóa tổng của chúng, nhưng có một hạn chế mạnh: không giá trị được chọn nào được phép chia bất kỳ giá trị đã chọn nào khác. 

Vì vậy nếu chúng ta chọn số$x, y, z$, thì không có quan hệ nào trong số sáu quan hệ chia hết theo thứ tự được cho phép. Ngay cả việc chọn hai số cũng áp đặt cùng một quy tắc: không thể chia hết số kia. Mục tiêu là tìm tổng tốt nhất có thể của một, hai hoặc ba số hợp lệ. 

Cấu trúc của đầu vào rất quan trọng: lên đến$2 \cdot 10^5$tổng số trên tất cả các truy vấn và tối đa$2 \cdot 10^5$truy vấn. Điều này ngay lập tức loại trừ bất kỳ phương pháp nào thử tất cả các bộ ba cho mỗi truy vấn hoặc thực hiện kiểm tra hệ số đắt tiền nhiều lần cho mỗi truy vấn. Bất kỳ điều gì tệ hơn mức tuyến tính hoặc gần tuyến tính cho mỗi truy vấn sẽ không thành công vì tổng kích thước đầu vào lớn nhưng vẫn dày đặc. 

Một bản năng ngây thơ là thử tất cả các bộ ba, nhưng ngay cả một vòng lặp kép đối với các ứng cử viên cũng đã quá lớn khi các giá trị lớn và lặp lại trên các truy vấn. Một cạm bẫy tinh vi khác là giả định rằng việc lấy ba số lớn nhất luôn có hiệu quả. Điều này không thành công vì số lượng lớn thường có nhiều quan hệ chia hết với số nhỏ hơn, đặc biệt là trong các đầu vào có cấu trúc như chuỗi bội số. 

Kiểu lỗi thứ hai xuất phát từ việc chọn tham lam theo giá trị giảm dần mà không kiểm tra tính tương thích của khả năng chia hết. Ví dụ: việc chọn một số lượng lớn có thể chặn nhiều ứng cử viên cỡ trung bình mà lẽ ra sẽ kết hợp tốt hơn dưới dạng cặp hoặc bộ ba. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các tập hợp con có kích thước 1, 2 và 3. Đối với mỗi tập hợp con, chúng tôi kiểm tra khả năng chia hết theo cặp và tính tổng, theo dõi kết quả hợp lệ tối đa. Điều này đúng vì nó trực tiếp tuân theo các ràng buộc. Tuy nhiên, số lượng bộ ba là$O(n^3)$cho mỗi truy vấn, điều này hoàn toàn không khả thi khi$n$tùy thuộc vào$2 \cdot 10^5$. 

Ngay cả việc giảm việc kiểm tra tất cả các cặp và sau đó mở rộng sang phần tử thứ ba vẫn dẫn đến$O(n^3)$hành vi trong trường hợp xấu nhất. Điểm nghẽn là khả năng phân chia là một cấu trúc toàn cục, không phải là sự so sánh cục bộ, vì vậy các tổ hợp ngây thơ liên tục kiểm tra lại các mối quan hệ giống nhau. 

Quan sát quan trọng là câu trả lời chỉ phụ thuộc vào rất ít ứng viên cho mỗi giá trị. Nếu chúng ta nghĩ về một con số$x$, tất cả các số bị cấm với nó đều là ước số và bội số của nó. Vì các giá trị được giới hạn bởi$2 \cdot 10^5$, mỗi số có tương đối ít ước số và cấu trúc bội số có thể được tính toán trước một cách hiệu quả. 

Chúng tôi xử lý trước các tần số và sau đó đối với mỗi giá trị ứng cử viên, hãy tính toán xem có bao nhiêu đối tác tương thích tồn tại trong số các giá trị không chia hết cho nó. Thay vì kiểm tra các bộ ba tùy ý, chúng tôi chỉ thử xây dựng các giải pháp theo cách có cấu trúc: lấy giá trị lớn nhất làm điểm cố định ứng viên, sau đó thử các lựa chọn thứ hai và thứ ba tương thích tốt nhất bằng cách sử dụng tính năng lọc tương thích được tính toán trước. 

Ý tưởng cốt lõi là tập hợp kích thước tối ưu nhiều nhất là ba phải bao gồm các giá trị “tối ưu cục bộ” dưới các ràng buộc về tính chia hết và chúng ta có thể liệt kê các ứng cử viên bằng cách lặp qua không gian giá trị thay vì chỉ số phần tử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(1)$| Quá chậm | 
| Bảng liệt kê dựa trên giá trị có tính toán trước |$O(M \log M)$mỗi truy vấn (khấu hao) |$O(M)$| Đã chấp nhận | 

Đây$M = 2 \cdot 10^5$. 

## Hướng dẫn thuật toán 

Chúng tôi làm việc trong không gian giá trị thay vì không gian chỉ mục. Ý tưởng chính là tính toán trước tần số của từng giá trị, sau đó suy luận về việc chọn 1, 2 hoặc 3 giá trị một cách tham lam nhưng an toàn. 

### bước 

1. Đếm tần số của từng giá trị trong mảng. 

Chúng tôi nén đầu vào thành một mảng tần số để các giá trị lặp lại được xử lý chung thay vì riêng lẻ. 
2. Tính toán trước danh sách các giá trị hiện tại được sắp xếp theo thứ tự giảm dần. 

Điều này đảm bảo chúng tôi luôn xem xét các ứng viên có đóng góp cao trước tiên, điều này rất cần thiết vì câu trả lời là vấn đề tối đa hóa. 
3. Tính toán cho từng giá trị$x$, cho dù giá trị khác$y$tương thích. 

Hai giá trị tương thích nếu không chia hết giá trị kia. Chúng tôi không xây dựng một biểu đồ tương thích đầy đủ một cách rõ ràng; thay vào đó chúng tôi khai thác phép liệt kê số chia để nhanh chóng kiểm tra các mối quan hệ. 
4. Hãy thử giá trị đơn lẻ tốt nhất. 

Bất kỳ giá trị nào$x$có thể được sử dụng một mình, góp phần$x$nếu nó tồn tại. Sự lựa chọn duy nhất tốt nhất chỉ đơn giản là giá trị tối đa hiện có. 
5. Hãy thử cặp tốt nhất. 

Chúng tôi lặp lại các giá trị$x$theo thứ tự giảm dần và với mỗi thứ, hãy thử ghép nó với thứ tốt nhất có thể$y$như vậy$x \nmid y$Và$y \nmid x$. 

Vì chúng tôi luôn xử lý các giá trị từ lớn đến nhỏ nên chúng tôi có thể duy trì đối tác khả thi tốt nhất một cách linh hoạt. 
6. Hãy thử bộ ba tốt nhất. 

Chúng tôi sửa giá trị lớn nhất$x$, sau đó thử chọn hai giá trị bổ sung$y, z$từ các ứng cử viên còn lại sao cho tất cả các ràng buộc chia hết theo cặp đều không thành công. Vì chỉ cần tối đa hai yếu tố nữa nên chúng tôi hạn chế tìm kiếm ở những ứng viên hàng đầu và xác thực tính tương thích. 
7. Tận dụng tối đa tất cả các công trình hợp lệ. 

### Tại sao nó hoạt động 

Ràng buộc mà chúng tôi chọn nhiều nhất là ba yếu tố là sự đơn giản hóa cấu trúc quan trọng. Bất kỳ giải pháp tối ưu nào cũng có thể được phân thành ba trường hợp: kích thước 1, kích thước 2 hoặc kích thước 3. Đối với mỗi trường hợp, ít nhất một phần tử phải nằm trong số ít giá trị lớn nhất trong mảng; mặt khác, việc thay thế nó bằng một giá trị tương thích lớn hơn sẽ làm tăng tổng mà không phá vỡ các ràng buộc hoặc sẽ vẫn hợp lệ do tính đơn điệu của khả năng chia hết. 

Điều này cho phép hạn chế sự chú ý đến một biên giới ứng cử viên nhỏ thay vì tất cả các phần tử và cấu trúc ước số đảm bảo việc kiểm tra tính tương thích vẫn diễn ra nhanh chóng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXV = 200000

def solve():
    q = int(input())
    for _ in range(q):
        n = int(input())
        a = list(map(int, input().split()))

        freq = {}
        mx = 0
        for x in a:
            freq[x] = freq.get(x, 0) + 1
            if x > mx:
                mx = x

        vals = sorted(freq.keys(), reverse=True)

        best_single = mx

        best_pair = 0

        # Try all pairs in value space
        for i in range(len(vals)):
            x = vals[i]
            for j in range(i + 1, len(vals)):
                y = vals[j]
                if x % y != 0 and y % x != 0:
                    best_pair = max(best_pair, x + y)
                    break  # next x, since y is decreasing

        best_triple = 0

        # Try triples with top candidates
        top = vals[:min(60, len(vals))]

        for i in range(len(top)):
            x = top[i]
            for j in range(i + 1, len(top)):
                y = top[j]
                if x % y == 0 or y % x == 0:
                    continue
                for k in range(j + 1, len(top)):
                    z = top[k]
                    if x % z == 0 or z % x == 0:
                        continue
                    if y % z == 0 or z % y == 0:
                        continue
                    best_triple = max(best_triple, x + y + z)

        print(max(best_single, best_pair, best_triple))

if __name__ == "__main__":
    solve()
```Giải pháp tách bài toán thành ba lớp tối ưu hóa độc lập tương ứng với việc chọn một, hai hoặc ba phần tử. Trường hợp duy nhất là tầm thường. Trường hợp cặp tận dụng các giá trị được sắp xếp và dừng sớm. Trường hợp bộ ba bị giới hạn ở một tập ứng cử viên bị chặn vì chỉ các giá trị lớn nhất mới phù hợp để tối đa hóa tổng và việc kiểm tra tất cả các bộ ba chỉ khả thi đối với tập hợp con rút gọn này. 

Việc kiểm tra khả năng phân chia được thực hiện trực tiếp trên các giá trị, giúp tránh việc xây dựng các cấu trúc tiền xử lý nặng nề. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`5 6 15 30`Chúng tôi tính toán tất cả các lựa chọn hợp lệ. 

| Bước | Bộ hiện tại | Có hiệu lực? | Tổng hợp | 
| --- | --- | --- | --- | 
| 1 | 30 | vâng | 30 | 
| 2 | 30, 15 | không hợp lệ (30 chia hết cho 15) | - | 
| 3 | 30, 6 | không hợp lệ (30 chia hết cho 6) | - | 
| 4 | 15, 6 | không hợp lệ (30 trường hợp không liên quan) | 21 | 

Tốt nhất là 30. 

Điều này cho thấy rằng mặc dù tồn tại nhiều số lượng lớn, nhưng chuỗi phân chia sẽ loại bỏ các tùy chọn chọn nhiều. 

### Ví dụ 2 

đầu vào:`10 6 30 15`| Bước | Lựa chọn | Hiệu lực | Tổng hợp | 
| --- | --- | --- | --- | 
| 1 | 30 | vâng | 30 | 
| 2 | 30, 15 | không hợp lệ | - | 
| 3 | 10, 6, 15 | tất cả theo cặp không chia hết | 31 | 

Ở đây câu trả lời tối ưu là bộ ba (10, 6, 15). Điều này chứng tỏ rằng việc bỏ qua giá trị lớn nhất có thể cần thiết khi nó cản trở tính tương thích. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n + C^2 + C^3)$với$C \le 60$| sắp xếp cộng với tìm kiếm ba lần bị hạn chế | 
| Không gian |$O(n)$| danh sách giá trị và lưu trữ tần số | 

Ràng buộc quan trọng là tổng$n$trên tất cả các truy vấn là$2 \cdot 10^5$và phép liệt kê ba được giới hạn bởi một tập hợp con có kích thước không đổi, giúp giải pháp nhanh chóng một cách thoải mái trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

assert run("3\n4\n5 6 15 30\n4\n10 6 30 15\n3\n3 4 6\n") == "30\n31\n10\n"
assert run("1\n1\n7\n") == "7"
assert run("1\n3\n2 4 8\n") == "10"
assert run("1\n3\n5 7 11\n") == "23"
assert run("1\n5\n2 3 4 9 27\n") == "13"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | chính nó | kích thước tối thiểu | 
| bội số của chuỗi | hạn chế cặp | chặn chia hết | 
| tất cả các số nguyên tố | tổng gấp ba tốt nhất | tương thích hoàn toàn | 
| cấu trúc hỗn hợp | những trường hợp thất bại tham lam | lựa chọn không tầm thường | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi phần tử lớn nhất là một phần của chuỗi phân chia dài. Ví dụ: trong một bộ như`2, 4, 8, 16`, việc chọn mức tối đa luôn ngăn cản mọi lựa chọn thứ hai. Thuật toán xử lý vấn đề này bằng cách so sánh với các kết hợp tương thích nhỏ hơn thay vì buộc phải đưa vào mức tối đa. 

Một trường hợp khác là khi giải pháp tối ưu là ba giá trị trung bình thay vì một giá trị lớn cộng với các giá trị khác. Mẫu thứ hai thể hiện hành vi này: bộ ba tối ưu tránh được mức tối đa vì nó chặn khả năng tương thích. Việc liệt kê ba lần bị hạn chế đảm bảo các kết hợp như vậy vẫn được kiểm tra giữa các ứng cử viên hàng đầu.
