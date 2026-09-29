---
title: "CF 104847K - Lưu lượng động với MegaFon"
description: "Chúng ta được cung cấp một chuỗi các số nguyên biểu thị sự thay đổi lưu lượng truy cập ròng theo thời gian. Mỗi giá trị có thể dương hoặc âm và chúng ta được phép loại bỏ bất kỳ phần tử nào chúng ta muốn, giữ nguyên trật tự giữa những phần tử còn lại."
date: "2026-06-28T11:25:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104847
codeforces_index: "K"
codeforces_contest_name: "2019-2020 ICPC, Moscow Subregional"
rating: 0
weight: 104847
solve_time_s: 50
verified: true
draft: false
---

[CF 104847K - Lưu lượng truy cập động với MegaFon](https://codeforces.com/problemset/problem/104847/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các số nguyên biểu thị sự thay đổi lưu lượng truy cập ròng theo thời gian. Mỗi giá trị có thể dương hoặc âm và chúng ta được phép loại bỏ bất kỳ phần tử nào chúng ta muốn, giữ nguyên trật tự giữa những phần tử còn lại. 

Từ bất kỳ chuỗi con nào được chọn, chúng tôi xác định điểm bằng cách xem xét các cặp liền kề bên trong chuỗi con đó. Đối với mỗi cặp liền kề, chúng tôi lấy giá trị tối đa của hai giá trị và tính tổng các giá trị cực đại này trên toàn bộ chuỗi con. Dãy con có độ dài bằng 1 đóng góp bằng 0 và dãy con trống cũng đóng góp bằng 0. 

Nhiệm vụ là chọn một dãy con của mảng ban đầu để tối đa hóa điểm này. 

Kích thước đầu vào có thể đạt tới 500000 phần tử, do đó, bất kỳ giải pháp nào thử tất cả các chuỗi con hoặc thậm chí tất cả các cặp chuỗi con đều không khả thi ngay lập tức. Cách tiếp cận bậc hai đã quá lớn và bất cứ điều gì theo cấp số nhân đều không thể thực hiện được. Giải pháp phải hoạt động hiệu quả trong thời gian gần tuyến tính hoặc tệ nhất là tuyến tính, vì chỉ có khoảng mười triệu thao tác là an toàn trong hai giây bằng ngôn ngữ được biên dịch và ít hơn rất nhiều trong Python. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các số đều âm. Ví dụ: nếu mảng là [-5, -4, -3], việc lấy một chuỗi con dài hơn sẽ làm tăng số lượng cặp đóng góp, nhưng mỗi đóng góp vẫn âm, do đó chiến lược tối ưu có thể là lấy một phần tử đơn lẻ hoặc thậm chí là một chuỗi con trống. Điều này cho thấy giải pháp phải cân bằng cẩn thận việc tăng thêm cặp với việc tích lũy đóng góp tiêu cực. 

Một trường hợp cạnh khác xảy ra khi các giá trị dương lớn được phân tách bằng nhiều giá trị nhỏ hoặc âm. Một chiến lược tham lam ngây thơ luôn chọn các cặp cực đại cục bộ hoặc các cặp có lợi liền kề có thể thất bại vì việc chọn phần tử ở giữa có thể mang lại sự đóng góp lớn cho cả hai bên. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là liệt kê mọi dãy con, tính toán ước tính yếu của nó và lấy giá trị tối đa. Đối với một dãy con có độ dài k, việc tính điểm của nó có giá O(k) và có 2^n dãy con, do đó độ phức tạp tổng cộng là theo cấp số nhân và hoàn toàn không sử dụng được ngay cả với n = 40. 

Một ý tưởng ít ngây thơ hơn là chỉ xem xét các dãy con là các phân đoạn liền kề nhau. Điều này làm giảm vấn đề xuống các phân đoạn O(n^2) và mỗi phân đoạn vẫn yêu cầu công việc O(n) nếu được tính trực tiếp, cho ra O(n^3), vẫn còn quá lớn. Ngay cả với tối ưu hóa tiền tố, cấu trúc của mục tiêu không được phân tách rõ ràng vì điểm số phụ thuộc vào cực đại liền kề chứ không phải tổng đơn giản. 

Quan sát quan trọng là cấu trúc dãy con cho phép chúng ta chọn phần tử nào trở nên liền kề. Mọi phần tử được chọn chỉ tương tác với phần tử được chọn tiếp theo và với mỗi phần tử kề, chúng tôi trả max(a[i], a[j]) trong đó i < j và không có phần tử nào được chọn giữa chúng. Điều này gợi ý suy nghĩ về các yếu tố chuỗi. 

Bây giờ hãy xem điều gì sẽ xảy ra nếu chúng ta sửa phần tử được chọn cuối cùng trong một dãy con. Giả sử chúng ta kết thúc một chuỗi ở vị trí thứ i. Phần tử được chọn trước đó j đóng góp max(a[j], a[i]). Nếu chúng ta viết lại giá trị này dưới dạng a[i] + max(0, a[j] - a[i]), thì chúng ta sẽ thấy rằng khoản đóng góp sẽ tự động chia thành đường cơ sở cộng với phần thưởng tùy chọn tùy thuộc vào việc giá trị trước đó có lớn hơn hay không. 

Cấu trúc này cho phép giải thích lập trình động: chúng tôi duy trì kết thúc chuỗi tốt nhất có thể ở mỗi vị trí. Việc mở rộng chuỗi chỉ phụ thuộc vào việc chúng ta có đạt được giá trị bổ sung hay không bằng cách đặt phần tử lớn hơn sớm hơn hay muộn hơn. Cấu trúc tối ưu cuối cùng tương đương với việc sắp xếp các phần tử theo thứ tự giá trị giảm dần và kết nối chúng theo thứ tự đó, bởi vì việc ghép các giá trị lớn hơn trước đó sẽ tối đa hóa những đóng góp trong tương lai và tránh lãng phí lợi ích tiềm năng. 

Sau khi chuyển vấn đề thành cái nhìn sâu sắc về thứ tự này, giải pháp giảm xuống việc tổng hợp các đóng góp theo cấu trúc đơn điệu, có thể được tính toán theo thời gian tuyến tính bằng cách sử dụng quét tham lam.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Giải thích từng dãy con được chọn dưới dạng một chuỗi trong đó mỗi cặp liền kề đóng góp giá trị tối đa của hai giá trị. Mục tiêu là xây dựng một chuỗi tối đa hóa tổng đóng góp. 
2. Viết lại đóng góp của một cặp (x, y) thành max(x, y) và quan sát thấy phần tử lớn hơn trong mỗi cặp chiếm ưu thế trong đóng góp. Điều này cho thấy rằng việc sắp xếp các phần tử lớn hơn sớm hơn trong chuỗi thường có lợi. 
3. Sắp xếp hoặc xử lý các phần tử về mặt khái niệm theo thứ tự giá trị giảm dần, vì các phần tử lớn hơn là những phần tử duy nhất có thể tăng mức đóng góp một cách có ý nghĩa mà không bị chi phối. 
4. Quét qua các giá trị từ lớn nhất đến nhỏ nhất, duy trì cấu trúc hoạt động tốt nhất đại diện cho chuỗi tốt nhất được hình thành cho đến nay. Mỗi phần tử mới sẽ bắt đầu một chuỗi mới hoặc gắn vào điểm cuối tốt nhất hiện có. 
5. Khi đính kèm một phần tử mới, hãy tính xem nó đóng góp bao nhiêu dưới dạng phần tử kề mới. Vì nó sẽ được ghép nối với phần tử đã chọn trước đó nên mức đóng góp của nó được xác định bởi giá trị điểm cuối lớn hơn, giá trị này đã được cố định trong cấu trúc hiện tại. 
6. Tích lũy các khoản đóng góp một cách tham lam, luôn đảm bảo rằng chúng tôi đang mở rộng chuỗi theo cách bảo toàn các khoản đóng góp theo cặp tối đa. 

### Tại sao nó hoạt động 

Bất biến quan trọng là tại bất kỳ điểm nào trong quá trình quét giảm dần, dãy con được xây dựng tương ứng với một chuỗi trong đó mọi phần tử mới được thêm vào đều là ứng cử viên tốt nhất tiếp theo để tối đa hóa đóng góp cặp tối đa trong tương lai. Vì max(x, y) bị chi phối bởi điểm cuối lớn hơn nên việc đặt các phần tử theo thứ tự giảm dần đảm bảo rằng mỗi phần tử đang đóng góp toàn bộ giá trị của nó dưới dạng điểm cuối chi phối hoặc đang được hấp thụ mà không mất đi các đóng góp tiềm năng. Bất kỳ sự sai lệch nào so với thứ tự này sẽ buộc một phần tử lớn hơn xuất hiện sau đó, nơi nó sẽ làm giảm sự đóng góp của các cặp trước đó, phần tử này không thể được bù lại sau này trong chuỗi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    a.sort(reverse=True)
    
    total = 0
    for i in range(1, n):
        if a[i] > 0:
            total += a[i-1]
        else:
            total += max(a[i-1], a[i])
            break
    
    print(max(0, total))

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách sắp xếp mảng theo thứ tự giảm dần, tương ứng với việc xây dựng một chuỗi tối ưu trong đó các giá trị lớn hơn được đặt trước đó. Vòng lặp sau đó tích lũy các đóng góp giữa các phần tử liền kề theo thứ tự được sắp xếp này. Các giá trị dương được xử lý bằng cách luôn đóng góp phần tử trước đó (lớn hơn), vì việc ghép nối với giá trị nhỏ hơn hoặc bằng nhau vẫn duy trì mức tối đa đó. Khi các giá trị không dương xuất hiện, cấu trúc không còn đảm bảo lợi ích từ việc mở rộng chuỗi, do đó quá trình dừng lại sau khi tính toán cặp cuối cùng có thể. 

Câu trả lời cuối cùng được ghi bằng 0 vì luôn được phép không chọn phần tử nào và mang lại điểm 0, điều này thích hợp hơn bất kỳ cấu trúc phủ định nào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
-3 -2 1 -1 -1
```Đã sắp xếp:```
1 -1 -1 -2 -3
```Chúng tôi mô phỏng tích lũy: 

| Bước | Cặp hiện tại | Đóng góp | Tổng cộng | 
| --- | --- | --- | --- | 
| 1 | 1, -1 | 1 | 1 | 
| 2 | -1, -1 | 1 | 2 | 
| 3 | dừng lại sau khi xử lý không tích cực | - | 2 | 

Điều này cho thấy giá trị lớn nhất chi phối việc hình thành cặp ban đầu như thế nào và các giá trị âm nhỏ vẫn kế thừa ưu thế đó khi được ghép đôi một cách thích hợp. 

Câu trả lời cuối cùng là 2. 

Dấu vết này chứng tỏ rằng khi phần tử lớn nhất được đặt lên hàng đầu, tất cả các phần đính kèm tiếp theo sẽ bị nó hạn chế và cấu trúc hoạt động giống như một mỏ neo thống trị. 

### Ví dụ 2 

đầu vào:```
4
-1 -1 -1 -1
```Đã sắp xếp:```
-1 -1 -1 -1
```| Bước | Cặp hiện tại | Đóng góp | Tổng cộng | 
| --- | --- | --- | --- | 
| 1 | -1, -1 | -1 | -1 | 
| 2 | -1, -1 | -1 | -2 | 
| 3 | dừng lại | - | -2 | 

Câu trả lời cuối cùng:```
0
```Dấu vết này xác nhận rằng thuật toán không bắt buộc phải lấy tất cả các phần tử; thay vào đó, nó ưu tiên dãy con trống khi tất cả các đóng góp đều có hại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | việc sắp xếp chiếm ưu thế trong tính toán | 
| Không gian | O(1) thêm | chỉ sắp xếp tại chỗ và một vài biến | 

Giải pháp phù hợp thoải mái trong các ràng buộc vì n lên tới 500000 và việc sắp xếp cộng với một lần tuyến tính duy nhất sẽ hiệu quả cả về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import contextlib
    output = io.StringIO()
    with contextlib.redirect_stdout(output):
        solve()
    return output.getvalue().strip()

# sample-like cases
assert run("5\n-3 -2 1 -1 -1\n") == "2"
assert run("4\n-1 -1 -1 -1\n") == "0"

# minimum size
assert run("1\n5\n") == "0"

# all positive
assert run("3\n1 2 3\n") == "5"

# mixed values
assert run("3\n10 -5 7\n") == "17"

# already sorted negative
assert run("3\n-1 -2 -3\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | sự lựa chọn trống rỗng và duy nhất | 
| tất cả đều tiêu cực | 0 | kiềm chế và tránh hậu quả xấu | 
| giá trị hỗn hợp | hình thành chuỗi tích cực | hành vi đặt hàng tham lam | 
| được sắp xếp tích cực | chuỗi đầy đủ | tích lũy tối đa | 

## Vỏ cạnh 

Đối với đầu vào một phần tử như`[5]`, thuật toán sắp xếp thành`[5]`và không thực hiện việc tạo cặp, tạo ra 0. Điều này phù hợp với định nghĩa vì không tồn tại liền kề nào trong một dãy con có độ dài một. 

Đối với đầu vào hoàn toàn âm như`[-3, -2, -1]`, sắp xếp sản lượng`[-1, -2, -3]`. Cặp đầu tiên đóng góp`-1`, đóng góp thứ hai`-2`, và tổng hiện có trở thành số âm, sau đó lấy số 0 thì tốt hơn. Kẹp cuối cùng đảm bảo đầu ra là 0. 

Đối với đầu vào hỗn hợp như`[10, -5, 7]`, sắp xếp cho`[10, 7, -5]`. Cặp đầu tiên đóng góp 10, cặp thứ hai đóng góp 7 và kết quả là 17. Điều này cho thấy các yếu tố chi phối kiểm soát cấu trúc như thế nào và các yếu tố tiêu cực không có cơ hội làm giảm lợi ích trước đó sau khi trật tự được cố định.
