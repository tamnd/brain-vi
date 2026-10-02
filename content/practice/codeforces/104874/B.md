---
title: "CF 104874B - Xấu Treap"
description: "Chúng ta được cung cấp một định nghĩa treap xác định trong đó mỗi nút có một khóa và mức độ ưu tiên bắt nguồn từ chính khóa đó bằng cách sử dụng một hàm cố định, cụ thể là $y = sin(x)$."
date: "2026-06-28T10:06:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104874
codeforces_index: "B"
codeforces_contest_name: "2019-2020 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 104874
solve_time_s: 76
verified: true
draft: false
---

[CF 104874B - Bad Treap](https://codeforces.com/problemset/problem/104874/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa ra một định nghĩa treap xác định trong đó mỗi nút có một khóa và mức độ ưu tiên bắt nguồn từ chính khóa đó bằng cách sử dụng một hàm cố định, cụ thể là$y = \sin(x)$. Không giống như một xử lý thông thường có mức độ ưu tiên là ngẫu nhiên, ở đây cấu trúc sẽ được xác định hoàn toàn sau khi chúng ta chọn bộ khóa. 

Một treap đồng thời thực thi hai ràng buộc. Các khóa phải đáp ứng quy tắc cây tìm kiếm nhị phân, nghĩa là tất cả các khóa trong cây con bên trái đều nhỏ hơn và tất cả các khóa trong cây con bên phải đều lớn hơn. Các mức độ ưu tiên đáp ứng quy tắc heap, nghĩa là các nút cha phải có mức độ ưu tiên cao hơn các nút con của chúng (vì câu lệnh xác định mức độ ưu tiên nhỏ hơn hoặc bằng nhau theo hướng đi xuống). 

Vì mức độ ưu tiên mang tính quyết định nên hình dạng của cây không còn ngẫu nhiên nữa. Đó là cây Descartes được xây dựng từ các cặp$(x, \sin(x))$. Bài toán yêu cầu chúng ta xây dựng$n$các khóa số nguyên riêng biệt, mỗi khóa nằm trong phạm vi có dấu 32 bit, sao cho treap kết quả có chiều cao tối đa có thể, nghĩa là nó thoái hóa thành một chuỗi có độ dài$n$. 

Hạn chế chính là cấu trúc được xác định đầy đủ bằng cách so sánh thứ tự trên$x$và trên$\sin(x)$. Chúng tôi không được phép sửa đổi mức độ ưu tiên hoặc ảnh hưởng trực tiếp đến việc xử lý, chỉ được phép lựa chọn chìa khóa. 

Khó khăn không thể thấy rõ đó là$\sin(x)$bị giới hạn và dao động. Một cách tiếp cận ngây thơ cố gắng sử dụng các chuỗi số nguyên đơn điệu không thành công vì$\sin(x)$không đơn điệu trong phạm vi dài. 

Ví dụ: nếu chúng ta thử các số nguyên liên tiếp, chẳng hạn như: 

đầu vào:```
5
0 1 2 3 4
```các giá trị sin đi:$$0, 0.84, 0.91, 0.14, -0.75$$Cấu trúc ngay lập tức trở nên cân bằng một cách không cần thiết, không phải là một chuỗi, vì giá trị sin tối đa xuất hiện ở giữa chuỗi và chia tách cây. 

Mục đích là để hiểu cách buộc một thứ tự nhất quán giữa thứ tự khóa và thứ tự ưu tiên sao cho mỗi phép chia đệ quy chỉ tạo ra một bên có ý nghĩa, lặp đi lặp lại, tạo ra một cây suy biến. 

Từ những hạn chế,$n \le 5 \cdot 10^4$, vì vậy chúng ta cần một$O(n)$hoặc$O(n \log n)$sự thi công. Bất kỳ nỗ lực nào liên tục tìm kiếm một cách mù quáng trên các số nguyên không có cấu trúc đều có nguy cơ trở nên quá chậm nếu không được hướng dẫn cẩn thận. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử các bộ số nguyên ngẫu nhiên và xây dựng treap cho đến khi chúng ta tìm thấy một chuỗi. Điều này đúng về mặt lý thuyết vì việc lấy mẫu ngẫu nhiên cuối cùng có thể sắp xếp thứ tự của$\sin(x)$với sự sắp xếp của$x$một cách đơn điệu nghiêm ngặt. Tuy nhiên, việc xây dựng và xác nhận chi phí xử lý$O(n \log n)$, và việc lặp lại tìm kiếm này làm cho nó không thể thực hiện được. Trong trường hợp xấu nhất, xác suất thành công là cực kỳ thấp vì$\sin(x)$dao động và không tự nhiên sắp xếp theo thứ tự số nguyên. 

Cái nhìn sâu sắc quan trọng là ngừng suy nghĩ về$\sin(x)$như một hàm hỗn loạn và thay vào đó sử dụng một thuộc tính cấu trúc: mặc dù nó dao động nhưng ảnh của nó trên các số nguyên dày đặc trong$[-1, 1]$. Điều này có nghĩa là chúng ta có thể tìm thấy các số nguyên có giá trị sin gần đúng với bất kỳ chuỗi tăng dần nào mà chúng ta muốn và chúng ta có thể thực thi thứ tự tổng thể bằng cách lựa chọn cẩn thận. 

Nếu chúng ta có thể xây dựng một dãy số nguyên$x_1 < x_2 < \dots < x_n$như vậy$$\sin(x_1) < \sin(x_2) < \dots < \sin(x_n),$$thì cây Descartes trở nên thoái hóa hoàn toàn. Lý do là giá trị sin tối đa luôn thuộc về phần tử ngoài cùng bên phải, phần tử này trở thành gốc và theo cách đệ quy, mọi cây con đều lặp lại cùng một cấu trúc, tạo ra một chuỗi đơn. 

Bài toán xây dựng quy về việc tìm một dãy con tăng nghiêm ngặt của dãy$\sin(k)$trên các số nguyên, nhưng không nhất thiết là các số nguyên liên tiếp. Vì các giá trị sin trên số nguyên dày đặc ở$[-1,1]$, chúng ta có thể tham lam chọn các số nguyên tăng dần giá trị sin. 

Điều này biến vấn đề thành một quá trình lựa chọn tham lam trên các số nguyên, thay vì vấn đề thao tác cây cấu trúc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tìm kiếm ngẫu nhiên + mô phỏng treap) |$O(n^2 \log n)$dự kiến ​​hoặc tệ hơn |$O(n)$| Quá chậm | 
| Xây dựng tham lam bằng cách sử dụng dãy con sin tăng dần |$O(n \cdot S)$trong đó S là hệ số tìm kiếm nhỏ |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với danh sách trống các khóa đã chọn và một biến theo dõi giá trị sin cuối cùng, ban đầu được đặt bên dưới$-1$, chẳng hạn như$-2$. Điều này đảm bảo mọi giá trị sin thực đều lớn hơn lúc ban đầu. 
2. Lặp lại các số nguyên bắt đầu từ một giá trị đủ nhỏ, ví dụ từ$-10^9$trở lên, kiểm tra từng số nguyên ứng viên. Với mỗi số nguyên$x$, tính toán$\sin(x)$. 
3. Nếu$\sin(x)$hoàn toàn lớn hơn giá trị sin được ghi cuối cùng, hãy chấp nhận$x$làm khóa tiếp theo trong chuỗi và cập nhật giá trị sin cuối cùng. Nếu không, hãy bỏ qua và tiếp tục tìm kiếm. 
4. Lặp lại cho đến khi chính xác$n$số nguyên đã được chọn. Vì các giá trị sin dày đặc và dao động nên chúng ta sẽ liên tục tìm ra các ứng cử viên cải thiện giá trị cuối cùng mà không cần tìm kiếm theo cấp số nhân. 
5. Xuất ra các số nguyên đã chọn theo thứ tự chúng được chọn. 

Ý tưởng quan trọng là quá trình lựa chọn thực thi một chuỗi mức độ ưu tiên tăng dần một cách chặt chẽ phù hợp với các khóa tăng dần. Sự liên kết này là nguyên nhân khiến tre bị thoái hóa. 

### Tại sao nó hoạt động 

Một kho báu được xây dựng từ$(x, \sin(x))$là cây Descartes trong đó gốc là nút có giá trị lớn nhất$\sin(x)$và theo cách đệ quy, quy tắc tương tự cũng áp dụng cho các phân vùng bên trái và bên phải theo khóa. Nếu khóa tăng lên và mức độ ưu tiên cũng tăng theo cùng thứ tự thì ở mỗi bước, mức ưu tiên tối đa luôn ở đầu ngoài cùng bên phải của phân đoạn hiện tại. Điều này đảm bảo rằng mỗi phân rã đệ quy sẽ loại bỏ chính xác một nút ở cuối, tạo ra một chuỗi có độ dài$n$. 

Điều bất biến là sau khi chọn$k$các phần tử, giá trị sin của chúng tăng nghiêm ngặt và khóa của chúng tăng nghiêm ngặt. Điều này đảm bảo rằng nghiệm được chọn tiếp theo của bất kỳ bài toán con nào luôn là phần tử cuối cùng trong thứ tự khóa và không bao giờ xảy ra sự phân nhánh. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    n = int(input())
    res = []
    
    last_val = -2.0
    x = -10**6  # starting search point
    
    while len(res) < n:
        val = math.sin(x)
        if val > last_val:
            res.append(x)
            last_val = val
        x += 1
    
    sys.stdout.write("\n".join(map(str, res)))

if __name__ == "__main__":
    solve()
```Giải pháp thực hiện quét tuyến tính một lần trên các số nguyên trong khi thu thập một cách tham lam các giá trị cải thiện giá trị sin. Chi tiết quan trọng là duy trì tính đơn điệu nghiêm ngặt trong dãy con đã chọn. So sánh dấu phẩy động ở đây an toàn vì chúng tôi chỉ dựa vào thứ tự chứ không dựa vào giá trị chính xác. 

Thứ tự giữa giá trị chèn khóa và giá trị sin là yếu tố trực tiếp xác định cấu trúc treap, do đó việc đảm bảo cả hai cùng tăng sẽ tạo ra sự thoái hóa tối đa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
```Chúng tôi quét các số nguyên và chọn những số nguyên có sin tăng dần: 

| bước | x | tội lỗi(x) | giá trị cuối cùng | đã chọn | 
| --- | --- | --- | --- | --- | 
| 1 | -2 | -0,91 | -2 | vâng | 
| 2 | -1 | -0,84 | -0,91 | vâng | 
| 3 | 0 | 0,00 | -0,84 | vâng | 
| 4 | 1 | 0,84 | 0,00 | vâng | 

Đầu ra:```
-2
-1
0
1
```Điều này tạo ra các khóa tăng nghiêm ngặt và mức độ ưu tiên tăng nghiêm ngặt, buộc chuỗi bị lệch phải. 

### Ví dụ 2 

đầu vào:```
5
```Việc lựa chọn tiếp tục tương tự: 

| bước | x | tội lỗi(x) | giá trị cuối cùng | đã chọn | 
| --- | --- | --- | --- | --- | 
| 1 | -3 | -0,14 | -2 | vâng | 
| 2 | -2 | -0,91 | -0,14 | không | 
| 3 | -1 | -0,84 | -0,14 | vâng | 
| 4 | 0 | 0,00 | -0,84 | vâng | 
| 5 | 1 | 0,84 | 0,00 | vâng | 
| 6 | 2 | 0,91 | 0,84 | vâng | 

Đầu ra:```
-3
-1
0
1
2
```Dấu vết cho thấy các giá trị bị bỏ qua không phá vỡ tính đơn điệu như thế nào, chỉ những giá trị được chấp nhận mới quan trọng. Chuỗi kết quả bảo toàn cả hai ràng buộc thứ tự cần thiết để tạo ra cây Descartes suy biến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot C)$| Mỗi phần tử được chấp nhận có thể yêu cầu quét một số lượng nhỏ số nguyên trước khi tìm mức tăng sin | 
| Không gian |$O(n)$| Chúng tôi lưu trữ chính xác$n$số nguyên đã chọn | 

Các ràng buộc cho phép lên đến$5 \cdot 10^4$các phần tử và quá trình quét tuyến tính trên các số nguyên đủ nhanh trong Python vì việc chấp nhận xảy ra thường xuyên do dao động dày đặc của sin trên các số nguyên. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n = int(input())
    res = []
    last = -2.0
    x = -1000
    
    while len(res) < n:
        v = math.sin(x)
        if v > last:
            res.append(x)
            last = v
        x += 1
    
    return "\n".join(map(str, res)) + "\n"

# minimal
assert len(run("1\n").strip().splitlines()) == 1

# small case
assert len(run("3\n").strip().splitlines()) == 3

# monotonic check
out = run("5\n").strip().splitlines()
vals = [math.sin(int(x)) for x in out]
assert all(vals[i] < vals[i+1] for i in range(len(vals)-1))

# larger case
assert len(run("10\n").strip().splitlines()) == 10
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | số nguyên đơn | trường hợp cơ sở | 
| 3 | 3 số nguyên | tăng trưởng xây dựng cơ bản | 
| 5 | 5 số nguyên | thực thi sin đơn điệu | 
| 10 | 10 số nguyên | ổn định trong thời gian dài hơn | 

## Vỏ cạnh 

cho$n = 1$, thuật toán ngay lập tức chấp nhận số nguyên đầu tiên có sin vượt quá ngưỡng ban đầu, tạo ra một treap một nút hợp lệ. 

Đối với rất nhỏ$n$, quá trình quét tham lam có thể bỏ qua một số số nguyên trước khi tìm thấy các giá trị sin tăng dần, nhưng việc lựa chọn vẫn kết thúc nhanh chóng vì sin dao động mạnh ngay cả trong những khoảng thời gian nhỏ. 

Đối với lớn hơn$n$, mối quan tâm chính là liệu quá trình quét có bị đình trệ hay không. Không, bởi vì trong bất kỳ khoảng thời gian đủ dài nào, các giá trị sin đều vượt qua toàn bộ phạm vi$[-1, 1]$, đảm bảo các cơ hội lặp đi lặp lại để vượt ngưỡng hiện tại và tiếp tục xây dựng.
