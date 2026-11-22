---
title: "CF 276C - Cô bé và tổng tối đa"
description: "Chúng ta được cung cấp một mảng số và một tập hợp các truy vấn khoảng trên các vị trí trong mảng đó. Mỗi truy vấn yêu cầu tổng các phần tử trong một phân đoạn liền kề. Trước khi trả lời bất kỳ truy vấn nào, chúng ta được phép hoán vị mảng một cách tự do."
date: "2026-06-05T02:22:31+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 276
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 169 (Div. 2)"
rating: 1500
weight: 276
solve_time_s: 64
verified: true
draft: false
---

[CF 276C - Cô bé và số tiền tối đa](https://codeforces.com/problemset/problem/276/C) 

**Đánh giá:** 1500 
**Tags:** cấu trúc dữ liệu, tham lam, triển khai, sắp xếp 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng số và một tập hợp các truy vấn khoảng trên các vị trí trong mảng đó. Mỗi truy vấn yêu cầu tổng các phần tử trong một phân đoạn liền kề. Trước khi trả lời bất kỳ truy vấn nào, chúng ta được phép hoán vị mảng một cách tự do. Sau khi sắp xếp lại, mọi truy vấn đều được đánh giá theo cách sắp xếp mới này và chúng tôi muốn tối đa hóa tổng tổng của tất cả các kết quả truy vấn được cộng lại. 

Một cách hữu ích để diễn giải lại nhiệm vụ là quên đi các truy vấn dưới dạng các thao tác riêng biệt và thay vào đó hãy nghĩ xem mỗi vị trí trong mảng được “sử dụng” bao nhiêu lần trên tất cả các truy vấn. Nếu một vị trí xuất hiện trong nhiều phạm vi truy vấn thì bất kỳ giá trị nào chúng ta đặt ở đó sẽ được cộng nhiều lần vào tổng giá trị cuối cùng. Nếu một vị thế hiếm khi được đảm bảo thì giá trị của nó sẽ đóng góp rất ít. 

Các ràng buộc lên tới 200.000 phần tử và 200.000 truy vấn, ngay lập tức loại trừ bất kỳ giải pháp nào tính toán lại tổng cho mỗi hoán vị hoặc trên mỗi mô phỏng truy vấn. Bất kỳ bậc hai hoặc thậm chí$O(n \log n)$mỗi cách tiếp cận hoán vị là quá chậm. Chúng ta cần một giải pháp chủ yếu bằng cách sắp xếp và quét tuyến tính, lý tưởng nhất là$O(n \log n)$. 

Một ý tưởng ngây thơ nhưng hấp dẫn là thử tất cả các hoán vị hoặc gán các giá trị một cách tham lam mà không tính toán tần số vị trí một cách cẩn thận. Điều đó không thành công vì giá trị của một phần tử phụ thuộc hoàn toàn vào số lượng khoảng truy vấn bao gồm vị trí cuối cùng của nó chứ không phụ thuộc vào chính phần tử đó. 

Chế độ lỗi cụ thể đang xử lý từng truy vấn một cách độc lập. Ví dụ, với mảng`[1,2,3]`và truy vấn`[1,2]`Và`[2,3]`, việc gán số lượng lớn cho "giữa" hoặc "cuối" mà không đếm chồng chéo sẽ dẫn đến vị trí dưới mức tối ưu. Chiến lược đúng phụ thuộc vào mức độ bao phủ tích lũy chứ không phải lý luận theo truy vấn. 

Một cạm bẫy tinh vi khác là cho rằng điểm cuối đều quan trọng như nhau. Trên thực tế, vị trí 2 trong ví dụ trên có giá trị hơn vì nó xuất hiện trong cả hai truy vấn, trong khi vị trí 1 và 3 chỉ xuất hiện một lần. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi hoán vị của mảng và tính tổng truy vấn kết quả cho mỗi cách sắp xếp. Đối với mỗi hoán vị, đánh giá tất cả chi phí truy vấn$O(q)$, và có$n!$hoán vị. Ngay cả việc hạn chế một số thứ như hoán đổi các phần tử cục bộ cũng không cải thiện được trường hợp xấu nhất một cách có ý nghĩa; không gian sắp xếp quá lớn. 

Quan sát quan trọng là sự đóng góp của từng vị trí mảng sẽ độc lập sau khi chúng tôi khắc phục được số lượng truy vấn bao gồm vị trí đó. Nếu vị trí$i$được bao gồm trong$c_i$truy vấn, sau đó đặt giá trị$x$có đóng góp$x \cdot c_i$đến số tiền cuối cùng. Điều này biến vấn đề thành một nhiệm vụ phù hợp: chúng tôi muốn gán giá trị lớn cho các vị trí có số lượng bao phủ lớn. 

Vì vậy, vấn đề giảm xuống còn tính toán tần suất mỗi chỉ mục được bao phủ bởi các truy vấn, sau đó ghép các giá trị mảng lớn nhất với số lượng bao phủ lớn nhất. Đây là một ứng dụng trực tiếp của phép gán tối ưu tham lam: sắp xếp cả hai chuỗi theo cùng một thứ tự sẽ tối đa hóa tích số chấm. 

Để tính toán phạm vi bao phủ một cách hiệu quả, chúng tôi sử dụng một mảng khác biệt. Đối với mỗi truy vấn$[l, r]$, chúng tôi tăng dần tại$l$và giảm dần tại$r+1$, sau đó tổng tiền tố để khôi phục mức độ phù hợp cho mỗi chỉ mục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n! \cdot q)$|$O(n)$| Quá chậm | 
| Tối ưu (tần suất + sắp xếp) |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`freq`kích thước`n + 2`với số không. Điều này sẽ theo dõi có bao nhiêu truy vấn bao gồm từng vị trí một cách gián tiếp thông qua cập nhật phạm vi. 
2. Đối với mỗi truy vấn$[l, r]$, tăng`freq[l]`bằng 1 và giảm dần`freq[r+1]`bằng 1. Điều này mã hóa ý tưởng rằng mức độ phù hợp bắt đầu từ`l`và kết thúc sau`r`. 
3. Tính tổng tiền tố`freq`từ trái sang phải. Sau bước này,`freq[i]`đại diện cho số lượng truy vấn bao gồm chỉ mục`i`. 
4. Sắp xếp mảng ban đầu`a`theo thứ tự giảm dần. Theo trực giác, các giá trị lớn hơn sẽ được gán cho các vị trí được sử dụng thường xuyên hơn. 
5. Sắp xếp`freq`mảng theo thứ tự giảm dần (bỏ qua phần đệm). Bây giờ cả hai mảng đều được căn chỉnh sao cho giá trị lớn nhất được ghép với vị trí được sử dụng thường xuyên nhất, giá trị lớn thứ hai với vị trí thường xuyên thứ hai, v.v. 
6. Nhân các phần tử tương ứng rồi cộng lại để ra đáp án cuối cùng. 

### Tại sao nó hoạt động 

Sau khi tính toán phạm vi bảo hiểm, mỗi vị trí đóng góp độc lập vào tổng cuối cùng. Nếu chúng ta ấn định việc gán giá trị cho các vị trí thì tổng điểm sẽ trở thành tổng của các sản phẩm$a[i] \cdot c[i]$. Đây chính xác là một bài toán tích số chấm cực đại khi hoán vị và bất đẳng thức sắp xếp lại đảm bảo rằng việc sắp xếp cả hai chuỗi theo cùng một thứ tự sẽ mang lại tổng tối đa có thể. Không có hoán vị nào khác có thể cải thiện tổng số vì bất kỳ hoán đổi nào làm lệch giá trị lớn hơn với phạm vi bao phủ nhỏ hơn sẽ làm giảm tích số chấm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, q = map(int, input().split())
a = list(map(int, input().split()))

freq = [0] * (n + 2)

for _ in range(q):
    l, r = map(int, input().split())
    freq[l] += 1
    freq[r + 1] -= 1

for i in range(1, n + 1):
    freq[i] += freq[i - 1]

freq = freq[1:n + 1]

a.sort(reverse=True)
freq.sort(reverse=True)

ans = 0
for x, c in zip(a, freq):
    ans += x * c

print(ans)
```Giải pháp trước tiên xây dựng cấu hình tần suất cho từng chỉ mục bằng cách sử dụng một mảng khác biệt, đảm bảo mỗi truy vấn đóng góp trong thời gian không đổi. Bước tích lũy tiền tố sẽ chuyển đổi số đó thành số lượng thực tế trên mỗi vị trí. 

Sắp xếp cả hai mảng là bước quan trọng khi phép gán tham lam xảy ra. Ghép nối các giá trị lớn nhất với tần số lớn nhất đảm bảo tổng đóng góp tối đa. 

Vòng lặp cuối cùng chỉ đơn giản tính tích số chấm của các chuỗi căn chỉnh này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n=3, q=3
a = [5, 3, 2]
queries: [1,2], [2,3], [1,3]
```Phạm vi tính toán đầu tiên: 

| tôi | tần số trước | sau tiền tố | 
| --- | --- | --- | 
| 1 | +1 (Q1,Q3) | 2 | 
| 2 | +1 (Q1,Q2,Q3) | 3 | 
| 3 | -1 khởi động điều chỉnh + Q2,Q3 | 2 | 

Vì vậy tần số cuối cùng là`[2,3,2]`. 

Bây giờ hãy sắp xếp: 

| Mảng | Đã sắp xếp | 
| --- | --- | 
| một | [5,3,2] | 
| tần số | [3,2,2] | 

Sản phẩm chấm: 

| Cặp | Đóng góp | 
| --- | --- | 
| 5*3 | 15 | 
| 3*2 | 6 | 
| 2*2 | 4 | 

Tổng cộng = 25. 

Dấu vết này cho thấy vị trí ở giữa trở nên có giá trị nhất như thế nào vì nó được bao phủ bởi tất cả các truy vấn. 

### Ví dụ 2 

đầu vào:```
n=4, q=2
a = [1, 10, 100, 1000]
queries: [1,2], [3,4]
```Bảo hiểm: 

| tôi | tần số | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 1 | 

Cả hai truy vấn đều rời rạc nên tất cả các vị trí đều bằng nhau. 

Mảng được sắp xếp: 

| một | tần số | 
| --- | --- | 
| [1000, 100, 10, 1] | [1,1,1,1] | 

Sản phẩm chấm: 

1000 + 100 + 10 + 1 = 1111 

Điều này cho thấy rằng khi tất cả các tần số bằng nhau thì hoán vị không thành vấn đề và câu trả lời sẽ giảm về tổng của tất cả các phần tử. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n + q)$| Việc sắp xếp chiếm ưu thế, tiền tố và xử lý truy vấn là tuyến tính | 
| Không gian |$O(n)$| Mảng tần số và lưu trữ đầu vào | 

Các ràng buộc cho phép lên tới 200.000 phần tử và truy vấn, do đó cần có các giải pháp tuyến tính hoặc log-tuyến tính. Thuật toán phù hợp thoải mái trong giới hạn vì nó chỉ thực hiện một lần truy vấn và hai loại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    freq = [0] * (n + 2)

    for _ in range(q):
        l, r = map(int, input().split())
        freq[l] += 1
        freq[r + 1] -= 1

    for i in range(1, n + 1):
        freq[i] += freq[i - 1]

    freq = freq[1:n + 1]

    a.sort(reverse=True)
    freq.sort(reverse=True)

    ans = sum(x * c for x, c in zip(a, freq))
    return str(ans)

# provided sample
assert run("""3 3
5 3 2
1 2
2 3
1 3
""") == "25"

# all equal values
assert run("""3 2
7 7 7
1 2
2 3
""") == "21"

# minimum size
assert run("""1 1
5
1 1
""") == "5"

# disjoint queries
assert run("""4 2
1 10 100 1000
1 2
3 4
""") == "1111"

# heavy overlap
assert run("""5 3
1 2 3 4 5
1 5
2 4
3 3
""") == str(5*3 + 4*2 + 3*3 + 2*2 + 1*1)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | 25 | tính chính xác trên các phạm vi chồng chéo | 
| tất cả đều bình đẳng | 21 | hoán vị không liên quan | 
| n=1 | 5 | ranh giới tối thiểu | 
| rời rạc | 1111 | tần số thống nhất | 
| chồng chéo nặng nề | tính toán | nhiều đóng góp chồng chéo | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các truy vấn giống hệt nhau, chẳng hạn như mọi truy vấn đều$[1, n]$. Trong tình huống này, mọi vị trí đều có tần số giống nhau và bất kỳ hoán vị nào cũng sẽ tạo ra kết quả như nhau. Thuật toán xử lý việc này một cách chính xác vì mảng tần số trở thành không đổi và việc sắp xếp duy trì tính đồng nhất. 

Một trường hợp khác là khi các truy vấn có độ lệch cao về một vùng, ví dụ như nhiều khoảng chồng chéo tập trung quanh một chỉ mục. Việc tính toán tần số tiền tố một cách chính xác sẽ tạo ra đỉnh cao nhất tại chỉ mục đó và việc sắp xếp đảm bảo giá trị mảng lớn nhất được chỉ định ở đó. 

Cuối cùng, khi$n = 1$, chỉ có một khả năng sắp xếp và một khả năng đóng góp. Mảng chênh lệch vẫn hoạt động rõ ràng, tạo ra một giá trị tần số duy nhất bằng số lượng truy vấn bao gồm chỉ số 1 và tích số chấm giảm xuống thành một phép nhân đơn giản.
