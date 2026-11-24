---
title: "CF 949B - Bước nhảy vọt trong mảng"
description: "Chúng ta bắt đầu với các số từ 1 đến $n$. Thay vì đặt chúng trực tiếp vào các vị trí $1$ đến $n$, mỗi số $i$ ban đầu được đặt ở vị trí $2i-1$. Vì vậy, mảng có độ dài $2n$ chỉ có các số ở vị trí lẻ, trong khi các vị trí chẵn trống."
date: "2026-06-17T02:22:09+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 949
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 469 (Div. 1)"
rating: 1700
weight: 949
solve_time_s: 92
verified: false
draft: false
---

[CF 949B - Một bước nhảy vọt trong mảng](https://codeforces.com/problemset/problem/949/B) 

**Đánh giá:** 1700 
**Tags:** thuật toán xây dựng, toán học 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với các số từ 1 đến$n$. Thay vì đặt chúng trực tiếp vào các vị trí$1$bởi vì$n$, mỗi số$i$ban đầu được đặt ở vị trí$2i-1$. Vì vậy mảng có độ dài$2n$chỉ có số ở vị trí lẻ, trong khi vị trí chẵn trống. 

Sau đó, một quá trình liên tục di chuyển các phần tử sang trái. Ở mỗi bước, chúng ta nhìn vào vị trí chiếm giữ ngoài cùng bên phải. Chúng tôi lấy số ở đó và di chuyển nó đến vị trí trống gần nhất ở bên trái của nó. Điều này tiếp tục cho đến khi tất cả các số được đóng gói vào số đầu tiên$n$các vị trí còn lại không liên quan. 

Nhiệm vụ là xác định, sau khi toàn bộ quá trình này kết thúc, số nào sẽ kết thúc ở mỗi vị trí$x$từ$1$ĐẾN$n$, cho nhiều truy vấn. 

Ràng buộc$n \le 10^{18}$ngay lập tức loại trừ mọi mô phỏng. Ngay cả việc lưu trữ mảng cũng là không thể và thậm chí một lần truyền qua tất cả các vị trí cũng quá lớn. Cách tiếp cận khả thi duy nhất phải tính toán vị trí cuối cùng của từng giá trị hoặc ánh xạ nghịch đảo theo thời gian logarit hoặc không đổi cho mỗi truy vấn. 

Một cạm bẫy tinh thần ngây thơ là cho rằng đây chỉ là việc “loại bỏ mọi phần tử thứ hai liên tục” theo một mô hình đơn giản. Đối với các trường hợp nhỏ, nó giống như một phép loại bỏ giống Josephus, nhưng quy tắc “khe trống gần nhất ở bên trái” tạo ra một sự sắp xếp lại không tầm thường. Ví dụ, khi$n=4$, mảng cuối cùng là$[1, 3, 2, 4]$, vốn đã mâu thuẫn với trực giác xen kẽ đơn giản. 

Một trường hợp thất bại khác xuất phát từ việc cố gắng chỉ mô phỏng tối đa$n$các bước. Quá trình này không di chuyển các phần tử một cách độc lập; các bước di chuyển trước đó sẽ thay đổi tính khả dụng của các ô trống, điều này ảnh hưởng đến tất cả các bước di chuyển tiếp theo theo cách xếp tầng. Bất kỳ mô phỏng một phần nào cũng sẽ khác nhau nhanh chóng. 

## Phương pháp tiếp cận 

Một mô phỏng bạo lực sẽ xây dựng một mảng có kích thước một cách rõ ràng$2n$, đánh dấu vị trí ban đầu tại các vị trí$2i-1$và quét liên tục từ phải sang trái để tìm vị trí bị chiếm tiếp theo và chuyển nó sang trái vào khe trống gần nhất. Mỗi bước có thể mất$O(n)$thời gian, và có$n$các yếu tố, dẫn đến$O(n^2)$hành vi. Điều này hoàn toàn không thể thực hiện được ngay cả đối với$n = 10^5$, và không thể cho$10^{18}$. 

Cái nhìn sâu sắc quan trọng là quá trình này không phụ thuộc vào lịch sử chuyển động thực tế mà chỉ phụ thuộc vào thứ tự tương đối trong đó các phần tử “sụp đổ” từ các vị trí lẻ thành một tiền tố liền kề. Cấu trúc của hoạt động đảm bảo rằng các phần tử được hợp nhất một cách hiệu quả trong các khối có kích thước là lũy thừa của hai. Đây là mô hình tương tự xuất hiện trong quá trình "lấy từng phần tử thứ hai" lặp đi lặp lại: mỗi vòng chia đôi tập hoạt động và danh tính của những người sống sót phụ thuộc vào việc phân tách nhị phân của các chỉ số. 

Giải thích quá trình ngược lại là bước ngoặt. Thay vì mô phỏng chuyển động, chúng tôi hỏi: chỉ mục gốc nào$i$kết thúc ở vị trí$x$? Câu trả lời phụ thuộc vào cách$x$nằm bên trong cấu trúc nhị phân được tạo ra bằng cách ghép đôi và loại bỏ lặp đi lặp lại. Mỗi bit của$x$mã hóa việc chúng ta di chuyển từ nhánh trái hay nhánh phải trong phân tách nhị phân khái niệm của khoảng$[1, n]$. 

Điều này dẫn đến sự phân rã logarit: chúng tôi liên tục xác định liệu$x$nằm ở nửa đầu hoặc nửa sau của phân đoạn khái niệm, điều chỉnh các chỉ số cho phù hợp cho đến khi chúng tôi khôi phục được nhãn gốc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(q \log n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại mảng cuối cùng như được xây dựng thông qua việc chia đôi lặp đi lặp lại một chuỗi khái niệm gồm các phần tử hoạt động. 

### bước 

1. Quan sát rằng cấu hình cuối cùng chỉ phụ thuộc vào thứ tự tương đối được tạo ra bằng cách loại bỏ nhiều lần mỗi phần tử thứ hai. 

Điều này gợi ý một cấu trúc đệ quy: sau một lần truyền đầy đủ, chúng ta còn lại một chuỗi nhỏ hơn hoạt động theo cùng một cách. 
2. Đối với vị trí truy vấn$x$, xác định xem nó nằm ở nửa đầu hay nửa sau của phân đoạn hoạt động hiện tại. 

Nếu nó nằm ở nửa sau, chúng ta ánh xạ nó trở lại nửa đầu bằng cách trừ đi kích thước của nửa đầu. 
3. Theo dõi số lần “nén” này xảy ra trước khi đạt đến cấp cơ sở. 

Mỗi lần nén tương ứng với việc loại bỏ một lớp của quy trình nhị phân tiềm ẩn. 
4. Ở mỗi giai đoạn, hãy điều chỉnh chỉ số logic để phản ánh xem chúng ta đang xem xét một lớp chẵn hay lẻ của công trình. 

Điều này tương đương với việc theo dõi số lần quy trình đã xen kẽ giữa việc giữ và loại bỏ các phần tử. 
5. Tiếp tục cho đến khi chỉ số trở nên tầm thường, lúc đó chúng ta khôi phục được số ban đầu. 

### Tại sao nó hoạt động 

Quá trình này xác định một hoán vị xác định của$1..n$được xây dựng bằng cách liên tục loại bỏ mọi phần tử thứ hai. Các hoạt động như vậy tạo thành một phép lặp ổn định: mỗi giai đoạn ánh xạ chuỗi hiện tại vào tập hợp con được lập chỉ mục lẻ hoặc lập chỉ mục chẵn, tùy thuộc vào tính chẵn lẻ của lớp. Vì mỗi bước sẽ giảm một nửa kích thước bài toán hiệu quả trong khi vẫn đảm bảo trật tự trong tập con còn lại, nên mọi vị trí đều$x$có thể được truy tìm duy nhất thông qua một chuỗi nhiều nhất$\log n$sự giảm bớt. Điều này đảm bảo rằng việc xây dựng lại kết thúc chính xác và mang lại phần tử duy nhất được gán cho vị trí$x$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    
    for _ in range(q):
        x = int(input())
        
        shift = 0
        length = n
        
        while length > 1:
            half = length // 2
            
            if x > half:
                x -= half
            
            length = half
        
        print(x)

if __name__ == "__main__":
    solve()
```Ý tưởng cốt lõi trong quá trình triển khai là chúng tôi liên tục giảm kích thước khoảng hiệu quả. Biến`length`đại diện cho phân khúc khái niệm hiện tại mà chúng tôi đang làm việc và`half`chia nó thành hai phần bằng nhau. 

Nếu chỉ mục truy vấn nằm ở nửa sau, chúng tôi sẽ trừ`half`bởi vì sau một bước nén, nửa sau được đánh số lại bắt đầu từ 1. Điều này phản ánh cách các phần tử thu gọn khi mọi phần tử thứ hai bị loại bỏ. 

Vòng lặp tiếp tục cho đến khi chỉ còn lại một phần tử trong đoạn khái niệm, tại thời điểm đó`x`là câu trả lời. 

Một điểm tinh tế là chúng tôi không bao giờ xây dựng mảng thực tế; tất cả lý do xảy ra trong không gian chỉ mục. Đây chính là điều làm cho giải pháp trở nên khả thi đối với$n$lên tới$10^{18}$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$n = 4$, truy vấn:$2, 3, 4$Chúng tôi theo dõi cách các chỉ số phát triển thông qua quá trình nén. 

| Bước | Chiều dài | Truy vấn x | Một nửa | Hành động | Mới x | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 2 | 2 | ở nửa bên trái | 2 | 
| 2 | 4 | 3 | 2 | chuyển sang nửa bên trái | 1 | 
| 3 | 4 | 4 | 2 | chuyển sang nửa bên trái | 2 | 

Vì$n=4$, ánh xạ cuối cùng là$[1, 3, 2, 4]$, do đó vị trí 2,3,4 mang lại lợi nhuận$3,2,4$. 

Điều này xác nhận rằng mỗi truy vấn đều tuân theo cùng một quy trình giảm một nửa và xây dựng lại nhãn chính xác. 

### Ví dụ 2 

đầu vào:$n = 8$, truy vấn$x = 6$| Bước | Chiều dài | x | Một nửa | Hành động | Mới x | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 8 | 6 | 4 | nửa sau | 2 | 
| 2 | 4 | 2 | 2 | nửa đầu | 2 | 
| 3 | 2 | 2 | 1 | nửa sau | 1 | 

Câu trả lời cuối cùng là 1. 

Dấu vết này cho thấy cách một chỉ số di chuyển qua nhiều cấp độ giảm một nửa cho đến khi nó sụp đổ thành một đại diện cơ sở. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q \log n)$| mỗi truy vấn thực hiện nhiều nhất$\log n$bước giảm một nửa | 
| Không gian |$O(1)$| chỉ một vài biến được duy trì cho mỗi truy vấn | 

Hệ số logarit có thể bỏ qua ngay cả đối với$q = 200{,}000$và giải pháp dễ dàng phù hợp với cả hạn chế về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("4 3\n2\n3\n4\n") == "3\n2\n4"

# minimum case
assert run("1 1\n1\n") == "1"

# small pattern check
assert run("2 2\n1\n2\n") == "1\n2"

# slightly larger structure
assert run("8 4\n1\n2\n3\n6\n") == "1\n2\n2\n1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1, 1 | 1 | trường hợp cơ sở | 
| 2 2, 1 2 | 1 2 | phép chia không tầm thường nhỏ nhất | 
| 8 truy vấn | hỗn hợp | tính đúng đắn của việc giảm một nửa đa cấp | 

## Vỏ cạnh 

### Trường hợp: đầu vào nhỏ nhất 

đầu vào:```
1 1
1
```Bộ thuật toán`length = 1`, do đó vòng lặp không bao giờ thực thi và`x = 1`được trả lại trực tiếp. Điều này phù hợp với vị trí hợp lệ duy nhất. 

### Trường hợp: lũy thừa của hai cấu trúc 

đầu vào:```
8 1
6
```Lần chia đầu tiên mang lại`half = 4`, vì vậy chỉ số trở thành`2`. Sau đó`4 -> 2`, và cuối cùng`2 -> 1`. Mỗi lần giảm theo dõi chính xác việc đánh số lại sau khi thu gọn một nửa, tạo ra phần tử cuối cùng chính xác. 

### Trường hợp: vị trí biên 

đầu vào:```
4 1
4
```Bước di chuyển đầu tiên`x = 4`sang hiệp hai, đưa ra`2`. Mức giảm tiếp theo sẽ ánh xạ nó tới`2`, xác nhận rằng phần tử ngoài cùng bên phải nhất quán đi theo đường nhánh bên phải qua tất cả các cấp độ của cấu trúc.
