---
title: "CF 104339A - Ba vị vua"
description: "Mỗi vị vua chỉ huy một đội quân được chia thành các trung đoàn giống hệt nhau. Barley có trung đoàn $a$, mỗi trung đoàn chứa $x$ binh lính, vì vậy tổng quy mô quân đội của anh ta là $a cdot x$. Hoa bia và mạch nha được mô tả theo cùng một cách, sử dụng $b cdot y$ và $c cdot z$."
date: "2026-07-01T18:37:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104339
codeforces_index: "A"
codeforces_contest_name: "FAMCS Olympiad for scholars, Qualification (copy)"
rating: 0
weight: 104339
solve_time_s: 57
verified: true
draft: false
---

[CF 104339A - Ba vị vua](https://codeforces.com/problemset/problem/104339/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi vị vua chỉ huy một đội quân được chia thành các trung đoàn giống hệt nhau. lúa mạch có$a$trung đoàn, mỗi trung đoàn bao gồm$x$binh lính, vậy tổng quy mô quân đội của anh ta là$a \cdot x$. Hoa bia và mạch nha được mô tả theo cùng một cách, sử dụng$b \cdot y$Và$c \cdot z$. 

Nhiệm vụ không phải là xếp hạng cả ba đội quân mà là tìm ra tổng quy mô quân đội tối đa trong số đó và sau đó liệt kê mọi vị vua có quân đội phù hợp với số lượng tối đa đó. Đầu ra phải chứa tên của tất cả các vị vua như vậy và những tên đó phải được in theo thứ tự từ điển. 

Các ràng buộc đủ nhỏ để số học trực tiếp là đủ. Mỗi giá trị tối đa là$10^3$, vậy tổng quy mô quân đội của mỗi quân tối đa là$10^6$. Ngay cả khi chúng tôi tính toán lại mọi thứ nhiều lần thì chi phí cũng không đáng kể. Điều này đặt giải pháp chắc chắn trong thời gian không đổi cho mỗi trường hợp thử nghiệm. 

Sự tinh tế duy nhất đến từ các trường hợp bình đẳng. Rất dễ in sai một quân vua nếu một quân vua lớn hơn rất nhiều, nhưng vấn đề rõ ràng đòi hỏi phải thu thập tất cả các quân vua bị ràng buộc ở mức tối đa. Một sai lầm khác có thể xảy ra là quên thứ tự từ điển của tên, điều này chỉ quan trọng khi có nhiều vị vua đủ điều kiện. 

Trường hợp cạnh cụ thể là khi cả ba đội quân đều bằng nhau, ví dụ: 

đầu vào:```
2 2 2 3 3 3
```Tất cả các tổng số là$6$, vì vậy kết quả đúng là:```
Barley Hops Malt
```Cách tiếp cận ngây thơ “chọn mức tối đa và in một tên” sẽ thất bại ở đây. 

Một trường hợp khác là khi hai quân vua hòa nhau ở mức tối đa: 

đầu vào:```
2 3 3 6 3 4
```Tổng số là lúa mạch$12$, Hoa bia$9$, mạch nha$12$, vì vậy cả Barley và Malt đều phải được in. 

## Phương pháp tiếp cận 

Giải thích bạo lực sẽ tính toán từng quy mô quân đội một cách độc lập và sau đó liên tục quét qua các kết quả để xác định kết quả nào là tối đa. Vì chỉ có ba giá trị, ngay cả một cách tiếp cận quá phức tạp vẫn có độ phức tạp tầm thường, nhưng người ta có thể tưởng tượng việc mở rộng mô hình này cho nhiều vị vua nơi việc quét lặp lại trở nên tốn kém. 

Trong bài toán này, cấu trúc đủ đơn giản nên không cần phải tinh chỉnh lặp lại. Chúng ta tính ba tích một lần, so sánh chúng một lần và sau đó chọn tất cả các mục bằng giá trị lớn nhất. Quan sát quan trọng là “tập quyết định” cực kỳ nhỏ và có kích thước cố định, do đó việc sắp xếp hoặc thực hiện nhiều bước là không cần thiết. 

Ý tưởng ép buộc có hiệu quả vì nó đánh giá trực tiếp tất cả các ứng viên. Nó trở nên không cần thiết không phải do kém hiệu quả mà vì không có cấu trúc tổ hợp để khai thác nên tập dữ liệu có kích thước cố định và hoàn toàn độc lập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đánh giá trực tiếp bằng chức năng quét | O(1) | O(1) | Đã chấp nhận | 
| So sánh trực tiếp tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số quân của mỗi vị vua như sau:$A = a \cdot x$,$B = b \cdot y$, Và$C = c \cdot z$. Điều này chuyển đổi đầu vào từ cấu trúc trung đoàn thành các giá trị vô hướng có thể so sánh được. 
2. Xác định giá trị lớn nhất giữa$A$,$B$, Và$C$. Điều này thể hiện quy mô quân đội mạnh nhất. 
3. Khởi tạo danh sách người chiến thắng trống. Đối với mỗi vị vua, hãy kiểm tra xem tổng số của họ có bằng mức tối đa hay không. Nếu vậy, hãy bao gồm tên của họ. Điều này đảm bảo mối quan hệ được bảo tồn. 
4. Xuất tên đã chọn theo thứ tự từ điển. Vì tên là các chuỗi cố định nên điều này làm giảm thứ tự giữa "Lúa mạch", "Hoa bia" và "Mạch nha". 

### Tại sao nó hoạt động 

Sức mạnh của mỗi vị vua được nắm bắt hoàn toàn bằng một phép nhân vô hướng duy nhất. Việc so sánh các đội quân rút gọn thành việc so sánh những đại lượng vô hướng này, và sự bình đẳng với mức tối đa sẽ xác định chính xác tất cả các đội quân mạnh nhất. Bởi vì mỗi quân vua đều được kiểm tra độc lập dựa trên cùng một giá trị tham chiếu nên không có người chiến thắng nào có thể bị bỏ qua hoặc thêm sai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a, b, c, x, y, z = map(int, input().split())

    barley = a * x
    hops = b * y
    malt = c * z

    mx = max(barley, hops, malt)

    ans = []
    if barley == mx:
        ans.append("Barley")
    if hops == mx:
        ans.append("Hops")
    if malt == mx:
        ans.append("Malt")

    print(" ".join(ans))

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên chuyển đổi trung đoàn của mỗi vị vua thành tổng số. Sau đó nó tìm giá trị lớn nhất trong số ba giá trị được tính toán. Mỗi tên vua được thêm vào có điều kiện khi và chỉ khi tổng số tính toán của chúng khớp với mức tối đa này. Thứ tự kiểm tra đã chính xác về mặt từ điển vì "Barley" đứng trước "Hops" và "Malt", do đó không cần sắp xếp bổ sung. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 4 3 6 3 4
```Tổng số: 

Lúa mạch = 12, Hoa bia = 12, Mạch nha = 12 

| Bước | lúa mạch | Hoa bia | Mạch nha | Tối đa | Người chiến thắng hiện tại | 
| --- | --- | --- | --- | --- | --- | 
| Tính tổng | 12 | 12 | 12 | - | - | 
| Tìm tối đa | 12 | 12 | 12 | 12 | - | 
| Kiểm tra lúa mạch | 12 | 12 | 12 | 12 | lúa mạch | 
| Kiểm tra bước nhảy | 12 | 12 | 12 | 12 | Hoa bia lúa mạch | 
| Kiểm tra mạch nha | 12 | 12 | 12 | 12 | Mạch nha hoa bia lúa mạch | 

Điều này thể hiện khả năng xử lý hòa hoàn toàn trong đó mọi vị vua đều phù hợp với mức tối đa. 

### Mẫu 2 

đầu vào:```
2 3 3 6 3 4
```Tổng số: 

Lúa mạch = 12, Hoa bia = 9, Mạch nha = 12 

| Bước | lúa mạch | Hoa bia | Mạch nha | Tối đa | Người chiến thắng hiện tại | 
| --- | --- | --- | --- | --- | --- | 
| Tính tổng | 12 | 9 | 12 | - | - | 
| Tìm tối đa | 12 | 9 | 12 | 12 | - | 
| Kiểm tra lúa mạch | 12 | 9 | 12 | 12 | lúa mạch | 
| Kiểm tra bước nhảy | 12 | 9 | 12 | 12 | lúa mạch | 
| Kiểm tra mạch nha | 12 | 9 | 12 | 12 | Mạch nha lúa mạch | 

Điều này cho thấy chỉ bao gồm có chọn lọc những kết quả phù hợp với mức tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học và phép so sánh cố định được thực hiện | 
| Không gian | O(1) | Chỉ sử dụng một số lượng biến không đổi | 

Quá trình tính toán diễn ra theo thời gian không đổi bất kể cường độ đầu vào, dễ dàng nằm trong giới hạn giới hạn 2 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return io.StringIO().getvalue() if False else (lambda: (solve(), ""))[1]()

# provided samples
assert run("2 4 3 6 3 4\n") == "Barley Hops Malt", "sample 1"
assert run("2 3 3 6 3 4\n") == "Barley Malt", "sample 2"

# custom cases
assert run("1 1 1 1 2 3\n") == "Hops Malt", "tie on max 3"
assert run("5 1 1 2 2 2\n") == "Barley", "single max"
assert run("3 3 3 3 3 3\n") == "Barley Hops Malt", "all equal"
assert run("10 1 10 1 5 1\n") == "Barley Malt", "boundary tie"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 1 2 3 | Hoa bia mạch nha | nhiều người chiến thắng, đặt hàng không tầm thường | 
| 5 1 1 2 2 2 | lúa mạch | tối đa rõ ràng duy nhất | 
| 3 3 3 3 3 3 | Mạch nha hoa bia lúa mạch | hộp cà vạt đầy đủ | 
| 10 1 10 1 5 1 | Mạch nha lúa mạch | buộc với cường độ khác nhau | 

## Vỏ cạnh 

Khi cả ba đội quân đều bằng nhau, thuật toán vẫn bao gồm mọi quân vua một cách chính xác vì mỗi quân so sánh đều kiểm tra sự bằng nhau với cùng một giá trị tối đa. Đối với đầu vào:```
3 3 3 3 3 3
```tổng số được tính toán đều là 9, vì vậy tối đa là 9 và cả ba lần kiểm tra có điều kiện đều thành công, tạo ra tất cả các tên theo đúng thứ tự. 

Khi có chính xác hai đội quân hòa nhau ở mức tối đa, chẳng hạn như:```
2 3 3 6 3 4
```Lúa mạch và mạch nha đều có giá trị là 12 trong khi Hoa bia nhỏ hơn. Tối đa là 12, vì vậy chỉ có hai cái đó được thêm vào. Thuật toán không dựa vào thứ tự nghiêm ngặt giữa các vị vua, chỉ dựa vào sự bình đẳng so với mức tối đa toàn cầu, đảm bảo tính chính xác bất kể các mối quan hệ được phân phối như thế nào.
