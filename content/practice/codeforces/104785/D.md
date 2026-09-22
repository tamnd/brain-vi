---
title: "CF 104785D - Lực lượng giao hàng"
description: "Chúng ta có một công ty có $n$ người chuyển phát, trong đó $n$ được đảm bảo chia hết cho ba. Mỗi người đưa thư có một giá trị sức mạnh và chúng ta phải phân chia tất cả người đưa thư thành các nhóm chính xác $k = n/3$, mỗi nhóm chứa chính xác ba người."
date: "2026-06-28T14:38:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104785
codeforces_index: "D"
codeforces_contest_name: "2023 United Kingdom and Ireland Programming Contest (UKIEPC 2023)"
rating: 0
weight: 104785
solve_time_s: 53
verified: true
draft: false
---

[CF 104785D - Lực lượng giao hàng](https://codeforces.com/problemset/problem/104785/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một công ty với$n$người đưa thư ở đâu$n$được đảm bảo chia hết cho ba. Mỗi chuyển phát có một giá trị sức mạnh và chúng ta phải phân chia tất cả các chuyển phát thành chính xác$k = n/3$nhóm, mỗi nhóm có đúng ba người. 

Đối với mỗi nhóm, hiệu quả của nó được xác định không phải bởi sức mạnh tối đa hoặc tối thiểu bên trong nó, mà bởi sức mạnh trung bình, nghĩa là giá trị lớn thứ hai trong ba nhóm. Mục tiêu là chia những người đưa thư thành bộ ba sao cho tổng của tất cả các trung vị nhóm càng lớn càng tốt. 

Vì vậy, nhiệm vụ không phải là cân bằng các nhóm hay làm cho họ bình đẳng, mà là quyết định cẩn thận những giá trị nào sẽ trở thành yếu tố đóng góp “trung gian” cho mỗi bộ ba. 

Các ràng buộc đi lên đến$n \le 10^6$, điều này ngay lập tức buộc phải có một giải pháp tuyến tính hoặc tuyến tính sau khi sắp xếp. Bất kỳ cách tiếp cận nào cố gắng liệt kê hoặc mô phỏng trực tiếp sự hình thành nhóm sẽ quá chậm vì ngay cả một cấu trúc tổ hợp đơn lẻ trên bộ ba cũng sẽ bùng nổ thành$O(n^3)$hoặc tệ hơn. Sắp xếp tại$O(n \log n)$có thể chấp nhận được và bất cứ điều gì vượt quá một vài lần truyền qua mảng đều phải tuyến tính. 

Một trường hợp thất bại tinh vi phổ biến xuất phát từ việc tham lam nhóm các bộ ba địa phương mà không có trật tự toàn cầu. Ví dụ: với các giá trị như: 

đầu vào:```
6
1 2 3 100 101 102
```Một nhóm ngây thơ có thể hình thành`(100, 101, 102)`Và`(1, 2, 3)`, cho số trung vị`101 + 2 = 103`. Điều đó có vẻ hợp lý, nhưng nói chung nó không phải là logic nhóm tối ưu và các quyết định cục bộ tham lam như vậy sẽ thất bại khi các giá trị được xen kẽ trong các phân phối đối nghịch hơn. 

Khó khăn thực sự là mỗi phần tử tham gia vào đúng một bộ ba, vì vậy việc chọn ai trở thành trung vị trên toàn cầu sẽ tương tác với tất cả các lựa chọn khác. Chúng ta cần một cấu trúc đảm bảo rằng các phần tử “ở giữa” đã chọn càng lớn càng tốt trong khi vẫn là các trung vị hợp lệ bên trong các bộ ba. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử mọi cách có thể để phân chia mảng thành các bộ ba, tính trung vị của mỗi bộ ba và lấy tổng lớn nhất. Về nguyên tắc, điều này đúng vì nó kiểm tra mọi cấu hình hợp lệ. Tuy nhiên, số cách phân vùng$n$các phần tử thành nhóm ba có kích thước lớn về mặt thiên văn, tăng nhanh hơn theo cấp số nhân. Ngay cả đối với$n = 30$, điều này trở nên không khả thi, và tại$n = 10^6$, điều đó là hoàn toàn không thể. 

Quan sát quan trọng là chúng ta thực sự không cần xây dựng bộ ba một cách rõ ràng. Chúng ta chỉ quan tâm đến phần tử ở giữa của mỗi bộ ba. Nếu chúng ta sắp xếp mảng, cấu trúc của nhóm tối ưu sẽ bị hạn chế: các giá trị lớn sẽ đóng vai trò là phần tử "hỗ trợ" cho số trung vị, các giá trị nhỏ sẽ bị hy sinh làm phần cuối của bộ ba và các phần tử còn lại tự nhiên trở thành số trung vị. 

Sau khi sắp xếp, hãy tưởng tượng bạn đang đi vào trong từ cả hai đầu của mảng. Mỗi bộ ba cần một phần tử trợ giúp lớn và một phần tử phụ nhỏ xung quanh điểm trung vị đã chọn. Điều này có nghĩa là với mỗi trung vị mà chúng ta chọn, chúng ta có thể “dành” một phần tử lớn và một phần tử nhỏ để hoàn thành nhóm của nó. Vì chúng tôi muốn số trung vị càng lớn càng tốt nên chúng tôi tránh lãng phí các giá trị lớn làm bạn đồng hành của số trung vị quá sớm. 

Điều này dẫn đến một cấu trúc tham lam rõ ràng: sắp xếp mảng và bỏ qua phần thứ ba nhỏ nhất làm phần bổ sung bắt buộc và phần thứ ba lớn nhất là phần hỗ trợ bắt buộc. Phần ba ở giữa còn lại chứa các ứng cử viên tốt nhất có thể cho số trung vị, nhưng không phải tất cả chúng đều được sử dụng trực tiếp. Thay vào đó, các số trung vị chính xác xuất hiện đều đặn theo thứ tự được sắp xếp. 

Cụ thể, sau khi sắp xếp theo thứ tự tăng dần, giải pháp tối ưu sẽ chọn các phần tử bắt đầu từ chỉ số$k$, sau đó bỏ qua một cái, lấy một cái và lặp lại. Điều này hoạt động vì lần đầu tiên$k$phần tử nhỏ nhất được sử dụng tốt nhất làm “cạnh thấp” của bộ ba, trong khi phần tử lớn nhất$k$các phần tử đóng vai trò là “mặt cao”, để lại chính xác$k$các phần tử ở các vị trí mà chúng có thể được tối đa hóa dưới dạng trung vị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu (sắp xếp + lựa chọn tham lam) |$O(n \log n)$| O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng ta sắp xếp mảng để có thể suy luận về cấu trúc thay vì các bài tập riêng lẻ. Sắp xếp chuyển đổi vấn đề từ nhóm tổ hợp sang lựa chọn vị trí. 

Sau khi sắp xếp theo thứ tự tăng dần, chúng tôi coi mảng là ba phân đoạn khái niệm: phần tử nhỏ nhất, phần tử lớn nhất và vùng ở giữa nơi lấy trung vị. Vì mỗi nhóm sử dụng chính xác một người trợ giúp nhỏ và một người trợ giúp lớn nên chúng tôi dành phần nhỏ nhất$k$các yếu tố như đối tác thấp và lớn nhất$k$các yếu tố như đối tác cao. 

Những gì còn lại ở khu vực giữa là$k$các yếu tố sẽ góp phần vào câu trả lời. Tuy nhiên, chúng tôi không lấy chúng liên tục; thay vào đó, chúng tôi chọn mọi phần tử thứ hai bắt đầu từ chỉ mục$k$, bởi vì giữa các trung vị liên tiếp, chúng ta phải tính đến việc phân bổ các phần tử trợ giúp trong các bộ ba hợp lệ. 

1. Sắp xếp mảng theo thứ tự tăng dần. 
2. Hãy để$k = n/3$. 
3. Khởi tạo một biến`answer = 0`. 
4. Bắt đầu từ chỉ mục$k$, lặp lại mảng và chọn từng phần tử thứ hai cho đến khi$k$các phần tử được chọn. 
5. Thêm từng phần tử đã chọn vào`answer`. 
6. Đầu ra`answer`. 

Lý do bỏ qua mọi phần tử khác là vì mỗi phần tử trung vị được chọn ngầm dành không gian cho một phần tử nhỏ hơn và một phần tử lớn hơn trong đội hình bộ ba của nó và cấu trúc được sắp xếp đảm bảo rằng các vai trò này luôn có thể được lấp đầy mà không ảnh hưởng đến các phần tử trung vị đã chọn khác. 

### Tại sao nó hoạt động 

Sau khi mảng được sắp xếp, bất kỳ nhóm hợp lệ nào cũng có thể được sắp xếp lại để các phần tử nhỏ hơn không bao giờ xuất hiện dưới dạng trung vị trừ khi cần thiết. Mỗi trung vị phải có ít nhất một phần tử nhỏ hơn nó và một phần tử lớn hơn nó. Bằng cách dành chỗ nhỏ nhất$k$các yếu tố như những người bạn đồng hành nhỏ được đảm bảo và những người bạn đồng hành lớn nhất$k$như những người bạn đồng hành lớn được đảm bảo, chúng tôi cô lập chính xác$k$các vị trí mà trung vị có thể được thực hiện. 

Lựa chọn xen kẽ đảm bảo rằng không có hai đường trung tuyến nào cạnh tranh để giành được các phần tử trợ giúp giống nhau, bởi vì giữa mỗi hai đường trung tuyến được chọn đều có sự phân tách cấu trúc tương ứng với các phần tử hỗ trợ được sử dụng. Điều này đảm bảo tính khả thi đồng thời tối đa hóa tổng vì chúng tôi luôn chọn những ứng viên có sẵn lớn nhất từ ​​vùng trung vị cho phép. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    a = list(map(int, input().split()))
    
    a.sort()
    k = n // 3
    
    ans = 0
    
    # pick medians: indices k, k+2, ..., k+2*(k-1)
    for i in range(k):
        idx = k + 2 * i
        ans += a[idx]
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp mảng, điều này rất cần thiết vì tất cả lý luận về cấu trúc đều phụ thuộc vào thứ tự tương đối. Biến$k$đại diện cho số lượng bộ ba chúng ta sẽ tạo thành, vì vậy chúng ta mong đợi chính xác$k$số trung vị để đóng góp vào số tiền cuối cùng. 

Vòng lặp lựa chọn cẩn thận các chỉ số bắt đầu từ$k$và nhảy hai lần mỗi lần. Độ lệch bắt đầu$k$tránh các phần tử nhỏ nhất được dành làm bạn đồng hành cấp thấp bắt buộc trong mỗi bộ ba. Bước hai đảm bảo rằng mỗi trung vị đã chọn được phân tách bằng một phần tử đóng vai trò là khoảng cách cấu trúc để nhóm hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
1 2 3 4 5 6
```Mảng được sắp xếp là`[1, 2, 3, 4, 5, 6]`, Và$k = 2$. 

| Bước | Chỉ số được chọn | Giá trị | Tổng chạy | 
| --- | --- | --- | --- | 
| 1 | 2 | 3 | 3 | 
| 2 | 4 | 5 | 8 | 

Các số trung vị được chọn là 3 và 5, cho tổng số 8. 

Điều này cho thấy hai phần tử nhỏ nhất (1, 2) và hai phần tử lớn nhất (5, 6) được sử dụng hiệu quả như các giá đỡ cấu trúc, để lại các lựa chọn chính xác ở giữa cho các đường trung tuyến. 

### Ví dụ 2 

đầu vào:```
9
9 1 8 2 7 3 6 4 5
```Mảng được sắp xếp là`[1,2,3,4,5,6,7,8,9]`,$k = 3$. 

| Bước | Chỉ số được chọn | Giá trị | Tổng chạy | 
| --- | --- | --- | --- | 
| 1 | 3 | 4 | 4 | 
| 2 | 5 | 6 | 10 | 
| 3 | 7 | 8 | 18 | 

Các số trung vị là 4, 6 và 8. 

Điều này xác nhận rằng ngay cả trong một hoán vị xen kẽ hoàn toàn, việc sắp xếp sẽ khôi phục hoàn toàn cấu trúc và việc lựa chọn chỉ mục tham lam vẫn ổn định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế; quét tuyến tính đơn sau đó | 
| Không gian |$O(n)$| Chi phí lưu trữ và sắp xếp mảng | 

Các ràng buộc cho phép tối đa một triệu phần tử, do đó, một sắp xếp duy nhất theo sau là vượt qua tuyến tính là nằm trong giới hạn. Không có cấu trúc dữ liệu bổ sung được yêu cầu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys as _sys
    backup = _sys.stdout
    _sys.stdout = io.StringIO()
    solve()
    out = _sys.stdout.getvalue()
    _sys.stdout = backup
    return out.strip()

# provided-style small case
assert run("6\n1 2 3 4 5 6\n") == "8"

# all equal
assert run("6\n5 5 5 5 5 5\n") == "10"

# already sorted descending
assert run("6\n6 5 4 3 2 1\n") == "8"

# minimal n = 3
assert run("3\n1 100 50\n") == "50"

# larger structured case
assert run("9\n9 8 7 6 5 4 3 2 1\n") == "18"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | tổng nhất quán | ổn định dưới sự đối xứng | 
| đầu vào giảm dần | xử lý đơn hàng đúng cách | phân loại cần thiết | 
| trường hợp tối thiểu | sự đúng đắn ba lần duy nhất | tính đúng đắn của trường hợp cơ sở | 
| cấu trúc 9 phần tử | tính đúng đắn của nhiều nhóm | giá trị mẫu chung | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các giá trị giống hệt nhau. Đối với đầu vào:```
6
5 5 5 5 5 5
```việc sắp xếp không làm thay đổi mảng và$k = 2$. Thuật toán chọn chỉ số 2 và 4, cả hai đều bằng 5, cho kết quả 10. Bất kỳ nhóm nào cũng cho kết quả như nhau và thuật toán vẫn nhất quán vì mọi phần tử đều có thể hoán đổi cho nhau, do đó mẫu lựa chọn trung vị không quan trọng. 

Một trường hợp cạnh khác là đầu vào hợp lệ nhỏ nhất:```
3
1 2 3
```Đây$k = 1$và sau khi sắp xếp, chúng tôi chọn chỉ mục 1 (dựa trên 0), là giá trị 2. Đây chính xác là giá trị trung bình của bộ ba duy nhất có thể, xác nhận rằng logic lập chỉ mục phù hợp với định nghĩa ngay cả ở kích thước biên.
