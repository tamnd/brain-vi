---
title: "CF 105723I - Nghệ thuật sắp xếp lại"
description: "Chúng ta được cho một số trường hợp thử nghiệm, mỗi trường hợp chứa một mảng các số nguyên. Với mỗi mảng, chúng ta được phép sắp xếp lại các phần tử của nó một cách tùy ý. Sau khi sắp xếp lại, chúng tôi tính toán giá trị dẫn xuất ở mọi vị trí, được xác định bằng giá trị được đặt ở đó trừ đi chỉ số vị trí của nó."
date: "2026-06-22T04:45:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105723
codeforces_index: "I"
codeforces_contest_name: "MTB Presents AUST Inter University Programming Contest 2025"
rating: 0
weight: 105723
solve_time_s: 50
verified: true
draft: false
---

[CF 105723I - Nghệ thuật sắp xếp lại](https://codeforces.com/problemset/problem/105723/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số trường hợp thử nghiệm, mỗi trường hợp chứa một mảng các số nguyên. Với mỗi mảng, chúng ta được phép sắp xếp lại các phần tử của nó một cách tùy ý. Sau khi sắp xếp lại, chúng tôi tính toán giá trị dẫn xuất ở mọi vị trí, được xác định bằng giá trị được đặt ở đó trừ đi chỉ số vị trí của nó. 

Yêu cầu là tất cả các giá trị dẫn xuất này phải khác nhau. Nói cách khác, nếu chúng ta xem xét mọi vị trí và tính toán khoảng cách giữa giá trị và chỉ số của nó thì không có hai vị trí nào được phép tạo ra cùng một khoảng cách. Nếu sự sắp xếp lại như vậy tồn tại, chúng ta phải đưa ra một sự sắp xếp lại; nếu không chúng ta sẽ xuất ra âm một. 

Các ràng buộc rất lớn, lên tới 100.000 trường hợp thử nghiệm và tổng số lên tới 500.000 phần tử. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các hoán vị hoặc thực hiện kiểm tra bậc hai cho mỗi trường hợp thử nghiệm. Chúng ta cần một phương pháp xử lý từng trường hợp thử nghiệm theo thời gian tuyến tính hoặc tuyến tính, lý tưởng nhất là sắp xếp một lần rồi thực hiện một lần. 

Một vấn đề khó phát hiện trong những vấn đề như thế này là các bản sao trong mảng có thể tạo ra xung đột ẩn trong các biểu thức dẫn xuất. Ví dụ: nếu nhiều giá trị bằng nhau được đặt một cách bất cẩn trên các chỉ mục, thì chỉ số trừ giá trị của chúng có thể vô tình lặp lại. Một kẻ tham lam ngây thơ đặt các phần tử tùy ý không có cấu trúc có thể thất bại ngay cả trên các ví dụ nhỏ như một mảng như`[5, 5, 5, 5]`, nơi mà việc bố trí bất cẩn có thể dễ dàng tạo ra những khác biệt lặp đi lặp lại như`5 − 1`Và`5 − 2`cả hai đều sụp đổ thành cùng một giá trị sau khi lựa chọn sắp xếp lại. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ sẽ thử tất cả các hoán vị của mảng và kiểm tra xem các giá trị dẫn xuất có khác nhau đối với mỗi hoán vị hay không. Đối với một mảng có độ dài n, có n hoán vị giai thừa và tính hợp lệ của tính toán cho mỗi hoán vị cần có thời gian tuyến tính, dẫn đến tổng số khoảng n giai thừa nhân với n phép toán. Ngay cả khi n bằng 10, điều này hoàn toàn không khả thi và sự bùng nổ còn tồi tệ hơn nhiều đối với các ràng buộc lớn hơn. 

Quan sát quan trọng là chúng ta thực sự không cần phải khám phá các sắp xếp tổ hợp. Chúng ta chỉ cần kiểm soát thứ tự sao cho chuỗi giá trị trừ chỉ số không bao giờ lặp lại. Nếu chúng ta áp đặt một cấu trúc đơn điệu mạnh lên dãy, chúng ta có thể đảm bảo rằng các giá trị dẫn xuất này cũng được sắp xếp nghiêm ngặt, điều này tự động hàm ý tính khác biệt. 

Sắp xếp mảng theo thứ tự không tăng là đủ để áp đặt cấu trúc đó. Sau khi các giá trị được sắp xếp từ lớn nhất đến nhỏ nhất, mỗi bước bên phải sẽ giảm giá trị của a tối đa bằng 0 trong khi tăng chỉ số lên đúng một. Điều này đảm bảo rằng biểu thức a[i] trừ i giảm đi rất nhiều khi chúng ta di chuyển dọc theo mảng. Chuỗi giảm nghiêm ngặt không thể chứa các bản sao, do đó điều kiện được tự động thỏa mãn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n!) cho mỗi trường hợp thử nghiệm | O(n) | Quá chậm | 
| Sắp xếp giảm dần | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập và xây dựng một thứ tự đảm bảo tính đơn điệu nghiêm ngặt của chuỗi dẫn xuất. 

1. Đọc mảng cho test case hiện tại. Bản thân các giá trị chưa được sửa đổi, chỉ được thu thập. 
2. Sắp xếp mảng theo thứ tự giảm dần để các giá trị lớn hơn luôn xuất hiện sớm hơn trong cách sắp xếp cuối cùng. Điều này tạo ra một cấu trúc giảm dần có kiểm soát trong các giá trị. 
3. Đặt lại các giá trị đã sắp xếp vào các vị trí từ trái sang phải. Phần tử đầu tiên của mảng được sắp xếp sẽ ở vị trí 1, phần tử thứ hai ở vị trí 2, v.v. 
4. Tính các giá trị dẫn xuất về mặt khái niệm dưới dạng a[i] trừ i. Vì mảng được sắp xếp theo thứ tự giảm dần nên mỗi bước về bên phải sẽ làm giảm biểu thức ít nhất một khi so sánh với vị trí trước đó. 
5. Xuất trực tiếp mảng đã xây dựng. 

### Tại sao nó hoạt động 

Đặt mảng được sắp xếp lại là b, được sắp xếp sao cho b1 ≥ b2 ≥ ... ≥ bn. Xét dãy dẫn xuất di = bi − i. Khi di chuyển từ vị trí i đến i + 1, sự thay đổi là di − di+1 = (bi − i) − (bi+1 − (i + 1)) = (bi − bi+1) + 1. Vì bi ≥ bi+1, đại lượng này luôn ít nhất là 1. Do đó di giảm nghiêm ngặt trên toàn mảng. Dãy số giảm nghiêm ngặt không thể lặp lại các giá trị, do đó tất cả di đều phân biệt theo từng cặp, thỏa mãn yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort(reverse=True)
        out.append(" ".join(map(str, a)))
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp hoàn toàn dựa vào việc sắp xếp từng mảng theo thứ tự giảm dần và in trực tiếp. Chi tiết triển khai chính là sử dụng cách sắp xếp ngược thay vì cố gắng theo dõi rõ ràng sự khác biệt. Không cần phải tính a[i] trừ i trong quá trình thực hiện, vì bằng chứng đảm bảo tính chính xác chỉ từ thứ tự. 

Một sai lầm phổ biến là cố gắng xây dựng hoán vị một cách tham lam trong khi theo dõi các khác biệt đã sử dụng. Cách tiếp cận đó dẫn đến việc ghi sổ kế toán không cần thiết và có nguy cơ xung đột nếu thực hiện không chính xác. Thứ tự được sắp xếp bỏ qua tất cả những điều đó bằng cách thực thi cấu trúc toàn cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng đầu vào:`[3, 1, 4, 2]`Sau khi sắp xếp theo thứ tự giảm dần, ta được`[4, 3, 2, 1]`. 

Chúng tôi tính toán các giá trị dẫn xuất từng bước. 

| tôi | b[i] | b[i] − i | 
| --- | --- | --- | 
| 1 | 4 | 3 | 
| 2 | 3 | 1 | 
| 3 | 2 | -1 | 
| 4 | 1 | -3 | 

Mỗi giá trị hoàn toàn nhỏ hơn giá trị trước đó, do đó không xảy ra sự lặp lại. 

Dấu vết này xác nhận rằng ngay cả khi mảng ban đầu không có thứ tự, sự sắp xếp giảm dần vẫn đảm bảo tính đơn điệu nghiêm ngặt của biểu thức dẫn xuất. 

### Ví dụ 2 

Mảng đầu vào:`[5, 5, 5, 5]`Sau khi sắp xếp, nó vẫn còn`[5, 5, 5, 5]`. 

Chúng tôi tính toán: 

| tôi | b[i] | b[i] − i | 
| --- | --- | --- | 
| 1 | 5 | 4 | 
| 2 | 5 | 3 | 
| 3 | 5 | 2 | 
| 4 | 5 | 1 | 

Mặc dù tất cả các giá trị đều giống hệt nhau, phép trừ theo chỉ số sẽ tạo ra một chuỗi giảm dần, do đó tránh được xung đột. 

Trường hợp này cho thấy tại sao việc trùng lặp trong đầu vào không thành vấn đề miễn là thứ tự nhất quán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi trường hợp thử nghiệm được sắp xếp một lần và tổng kích thước của các thử nghiệm bị giới hạn | 
| Không gian | O(n) | Lưu trữ cho mảng và bộ đệm đầu ra | 

Tổng công việc trên tất cả các trường hợp thử nghiệm vẫn nằm trong giới hạn vì tổng của tất cả n tối đa là 500.000 và việc sắp xếp chiếm ưu thế trong thời gian chạy. Điều này thoải mái phù hợp với các ràng buộc một giây điển hình trong môi trường lập trình cạnh tranh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    res = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort(reverse=True)
        res.append(" ".join(map(str, a)))
    return "\n".join(res)

# provided sample style case
assert run("1\n4\n3 1 4 2\n") == "4 3 2 1"

# minimum size
assert run("1\n2\n1 2\n") == "2 1"

# all equal
assert run("1\n5\n7 7 7 7 7\n") == "7 7 7 7 7"

# already sorted increasing
assert run("1\n4\n1 2 3 4\n") == "4 3 2 1"

# mixed
assert run("1\n6\n10 1 10 1 10 1\n") == "10 10 10 1 1 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 4 / 3 1 4 2`|`4 3 2 1`| tính đúng đắn chung | 
|`1 2 / 1 2`|`2 1`| kích thước tối thiểu | 
|`1 5 / 7 7 7 7 7`|`7 7 7 7 7`| trùng lặp | 
|`1 4 / 1 2 3 4`|`4 3 2 1`| đã đặt hàng đầu vào | 
|`1 6 / 10 1 10 1 10 1`|`10 10 10 1 1 1`| trùng lặp hỗn hợp | 

## Vỏ cạnh 

Đối với các mảng có tất cả các giá trị giống hệt nhau, một cách tiếp cận đơn giản có thể cố gắng trải rộng các giá trị bằng nhau một cách cẩn thận để tránh xung đột. Trong thực tế, việc sắp xếp đã sửa chữa được cấu trúc. Đối với một đầu vào như`[9, 9, 9, 9]`, thuật toán giữ cho nó không thay đổi và các giá trị dẫn xuất trở thành`[8, 7, 6, 5]`, tất cả đều khác biệt. 

Đối với các mảng tăng dần đã được sắp xếp, một nỗ lực tham lam có thể khiến mảng không thay đổi và giả định nhầm tính hợp lệ mà không kiểm tra cấu trúc thứ tự. Vì`[1, 2, 3, 4]`, thay vào đó, thuật toán sẽ đảo ngược nó thành`[4, 3, 2, 1]`, tạo ra một chuỗi dẫn xuất giảm nghiêm ngặt và tránh các giá trị lặp lại.
