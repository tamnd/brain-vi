---
title: "CF 106457B - Bespin"
description: "Nhiệm vụ cơ bản là chọn một tập hợp con tàu không trống, trong đó mỗi con tàu là khác nhau và đếm xem có bao nhiêu tập hợp con đó có kích thước lẻ. Bạn được cấp một số nguyên $n$, biểu thị số lượng tàu riêng biệt có sẵn."
date: "2026-06-25T09:13:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106457
codeforces_index: "B"
codeforces_contest_name: "UTPC Spring 2026 Open Contest"
rating: 0
weight: 106457
solve_time_s: 38
verified: true
draft: false
---

[CF 106457B - Bespin](https://codeforces.com/problemset/problem/106457/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ cơ bản là chọn một tập hợp con tàu không trống, trong đó mỗi con tàu là khác nhau và đếm xem có bao nhiêu tập hợp con đó có kích thước lẻ. 

Bạn được cho một số nguyên duy nhất$n$, đại diện cho số lượng tàu riêng biệt có sẵn. Mỗi tập hợp con tàu tương ứng với một đội tàu có thể có. Tuy nhiên, chỉ những tập con có kích thước lẻ mới được coi là hợp lệ. Tập hợp con trống sẽ tự động bị loại trừ vì một hạm đội phải chứa ít nhất một tàu. 

Vì vậy, vấn đề giảm xuống việc đếm xem có bao nhiêu tập hợp con của một tập hợp kích thước$n$có số lượng lẻ. 

Nếu chúng ta nghĩ về mặt liệt kê thô, mỗi tàu có thể được bao gồm hoặc bị loại trừ, do đó có$2^n$tổng số tập hợp con. Trong số này, chúng tôi muốn chính xác những phần tử có số phần tử được chọn là số lẻ. 

Một ví dụ nhỏ làm rõ cấu trúc. Nếu như$n = 3$, các tập con là:$\{\}, \{1\}, \{2\}, \{3\}, \{1,2\}, \{1,3\}, \{2,3\}, \{1,2,3\}$. 

Các tập con có kích thước lẻ là tập đơn và tập đầy đủ, cho$3 + 1 = 4$. 

Những ràng buộc cho phép$n \le 60$, điều này ngay lập tức loại trừ mọi cách tiếp cận liệt kê rõ ràng các tập hợp con. Thậm chí$2^{60}$vượt xa tầm với của tính toán. Bất cứ điều gì theo cấp số nhân trong$n$quá chậm trừ khi nó rút gọn về công thức thời gian không đổi. 

Trường hợp cạnh tinh tế xuất hiện khi$n = 1$. Tập hợp con duy nhất là$\{1\}$, hợp lệ vì kích thước của nó là 1. Câu trả lời đúng là 1. Việc triển khai ngây thơ bao gồm nhầm tập hợp trống hoặc tính chẵn lẻ khi đếm sai sẽ thất bại ở đây. 

Một trường hợp khác là$n = 2$. Các tập hợp con là$\{1\}, \{2\}$, cả hai đều hợp lệ, vì vậy câu trả lời là 2. Điều này đã gợi ý sự đối xứng giữa số lượng tập hợp con lẻ và chẵn, điều này trở thành thông tin chi tiết về cấu trúc quan trọng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ tạo ra tất cả$2^n$tập hợp con và đếm những tập hợp có kích thước lẻ. Về mặt khái niệm thì điều này rất đơn giản: lặp qua mọi bitmask từ 0 đến$2^n - 1$, đếm các bit đã đặt và tăng câu trả lời nếu tính chẵn lẻ là số lẻ. Điều này đúng vì mỗi tập hợp con được biểu diễn chính xác một lần và tính chẵn lẻ được xác định rõ ràng. 

Vấn đề là thời gian chạy. Ngay cả đối với$n = 40$,$2^{40}$đã về rồi$10^{12}$hoạt động vượt quá giới hạn cho phép. Kích thước vấn đề lên tới 60 khiến điều này hoàn toàn không thể sử dụng được. 

Quan sát quan trọng là các tập hợp con của một tập hợp hữu hạn phân bố đồng đều trên các lớp chẵn lẻ. Mỗi tập hợp con có thể được ghép nối với một tập hợp con khác chỉ khác bằng cách chuyển đổi một phần tử cố định. Việc ghép nối này làm đảo lộn tính chẵn lẻ, tạo ra sự tương ứng một-một giữa các tập hợp con có kích thước chẵn và có kích thước lẻ. 

Cụ thể hơn, hãy xem xét tất cả các tập con của$n$các phần tử. Nếu chúng ta lấy bất kỳ tập hợp con nào$S$, việc chuyển đổi sự hiện diện của phần tử đầu tiên sẽ ánh xạ nó tới một tập hợp con riêng biệt$S'$. Một trong số này có kích thước chẵn, cái còn lại có kích thước lẻ. Điều này phân chia toàn bộ tập hợp sức mạnh thành từng cặp, nghĩa là chính xác một nửa số tập hợp con có kích thước lẻ. 

Vì có$2^n$chính xác là tổng số tập hợp con$2^{n-1}$trong số họ có số lượng lẻ. 

Tập trống không ảnh hưởng đến đối số này vì nó được ghép nối với tập đơn lẻ chứa phần tử đã chọn, do đó nó đã được tính trong cấu trúc ghép nối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n 2^n)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$n$, đại diện cho số lượng tàu riêng biệt. 
2. Nhận biết rằng mỗi tập hợp con tương ứng với một lựa chọn nhị phân cho mỗi tàu, đưa ra$2^n$tổng số tập hợp con. Mục đích không phải là liệt kê chúng mà là tính xem có bao nhiêu có kích thước lẻ. 
3. Sử dụng thực tế cấu trúc rằng các tập hợp con có thể được ghép nối bằng cách lật thành viên của một phần tử cố định, điều này đảm bảo sự phân chia bằng nhau giữa các kích thước chẵn và lẻ. 
4. Kết luận số đội tàu hợp lệ là$2^{n-1}$. 
5. Tính trực tiếp giá trị này bằng cách sử dụng phép lũy thừa nhanh hoặc dịch chuyển bit. 

Công việc tính toán duy nhất là đánh giá lũy thừa của hai, có thể được thực hiện trong thời gian không đổi hoặc logarit tùy thuộc vào việc thực hiện. 

### Tại sao nó hoạt động 

Điều bất biến là việc chuyển đổi một phần tử cố định xác định sự phân đôi giữa các tập hợp con có kích thước chẵn và các tập hợp con có kích thước lẻ. Mọi tập hợp con đều chứa phần tử đã chọn hoặc không chứa phần tử đó và việc lật thành viên đó sẽ thay đổi tính chẵn lẻ mà không tạo ra xung đột. Vì ánh xạ này có thể đảo ngược và bao trùm toàn bộ tập hợp lũy thừa nên hai lớp chẵn lẻ phải có kích thước bằng nhau, buộc mỗi lớp phải chứa chính xác một nửa tổng số tập hợp con. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
print(1 << (n - 1))
```Giải pháp dựa vào việc dịch chuyển bit thay vì`pow(2, n-1)`bởi vì nó vừa đơn giản hơn vừa tránh được các vấn đề chuyển đổi dấu phẩy động, mặc dù các số nguyên của Python sẽ xử lý cả hai một cách an toàn. Sự thay đổi trực tiếp xây dựng$2^{n-1}$. 

Sự tinh tế duy nhất là trường hợp$n = 1$, nơi sự thay đổi trở thành$1 << 0 = 1$, phù hợp với câu trả lời đúng. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 3 

Chúng tôi theo dõi cách phân chia các tập hợp con theo kích thước tương đương. 

| Kích thước tập hợp con | Đếm | 
| --- | --- | 
| 0 | 1 | 
| 1 | 3 | 
| 2 | 3 | 
| 3 | 1 | 

Các tập con có kích thước lẻ là các tập con có kích thước 1 và 3. 

Điều này xác nhận trực giác ghép nối: mỗi tập hợp con chẵn có một tập hợp con lẻ tương ứng thu được bằng cách chuyển đổi một phần tử cố định. 

Đầu ra là$2^{2} = 4$. 

### Ví dụ 2: n = 4 

| Kích thước tập hợp con | Đếm | 
| --- | --- | 
| 0 | 1 | 
| 1 | 4 | 
| 2 | 6 | 
| 3 | 4 | 
| 4 | 1 | 

Tổng số tập hợp con lẻ$4 + 4 = 8$, phù hợp$2^{3}$. 

Ví dụ này củng cố rằng sự phân bố là đối xứng bất kể$n$, không phụ thuộc vào trường hợp nhỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Tính lũy thừa của hai thông qua dịch chuyển bit | 
| Không gian |$O(1)$| Chỉ có một số nguyên được lưu trữ | 

Những ràng buộc cho phép$n \le 60$, do đó, kết quả dễ dàng phù hợp trong phạm vi số nguyên 64 bit và Python xử lý nó một cách tự nhiên mà không gặp sự cố tràn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    n = int(input().strip())
    return str(1 << (n - 1))

# provided samples
assert run("3\n") == "4"

# custom cases
assert run("1\n") == "1", "minimum size"
assert run("2\n") == "2", "small even split"
assert run("4\n") == "8", "basic binomial symmetry"
assert run("60\n") == str(1 << 59), "maximum boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | trường hợp hợp lệ nhỏ nhất | 
| 2 | 2 | phép chia đúng cho n chẵn nhỏ | 
| 4 | 8 | đối xứng tổng quát | 
| 60 | 2^59 | xử lý giới hạn trên | 

## Vỏ cạnh 

cho$n = 1$, chỉ có một tập hợp con: con tàu duy nhất. Thuật toán tính toán$2^{0} = 1$, được tính chính xác là hợp lệ vì kích thước của nó là số lẻ. 

Đối với các giá trị lớn hơn như$n = 60$, kết quả sẽ trở thành một số nguyên rất lớn, nhưng Python xử lý các số nguyên có độ chính xác tùy ý, do đó không xảy ra sự cố tràn hoặc độ chính xác. Việc tính toán vẫn là một sự dịch chuyển một bit, do đó việc thực thi có thời gian không đổi và ổn định ngay cả ở biên.
