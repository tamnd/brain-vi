---
title: "CF 104745C - Lợi nhuận tối đa"
description: "Chúng ta được cung cấp một chuỗi séc ngân hàng, mỗi séc có giá trị tiền tệ cố định. Từ trình tự này, chúng tôi được phép chọn tối đa k séc trong một ngày và mục tiêu của chúng tôi là tối đa hóa tổng giá trị của các séc đã chọn."
date: "2026-06-28T23:01:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104745
codeforces_index: "C"
codeforces_contest_name: "CAMA 2023"
rating: 0
weight: 104745
solve_time_s: 49
verified: true
draft: false
---

[CF 104745C - Lợi nhuận tối đa](https://codeforces.com/problemset/problem/104745/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi séc ngân hàng, mỗi séc có giá trị tiền tệ cố định. Từ dãy này ta được phép chọn nhiều nhất`k`séc trong một ngày và mục tiêu của chúng tôi là tối đa hóa tổng giá trị của các séc đã chọn. Không có hạn chế nào về việc chúng tôi chọn séc nào ngoài giới hạn về số lượng chúng tôi có thể thực hiện. 

Đầu vào mô tả hai điều: số lượng séc có sẵn`n`, và số lượng tối đa`k`điều đó có thể được chuộc lại. Sau đó theo sau một loạt`n`số nguyên đại diện cho giá trị của mỗi lần kiểm tra. Đầu ra là một số duy nhất, tổng lớn nhất có thể mà chúng ta có thể thu được bằng cách chọn không quá`k`các phần tử từ mảng. 

Ràng buộc`n ≤ 10^4`có nghĩa là ngay cả các giải pháp bậc hai như kiểm tra tất cả các tập hợp con hoặc mô phỏng các kết hợp cũng sẽ quá chậm. MỘT`O(n^2)`Cách tiếp cận này có rủi ro xung quanh 10^8 thao tác, nằm ở ranh giới trong Python với giới hạn nghiêm ngặt là 1 giây. Bất cứ điều gì theo cấp số nhân ngay lập tức là không thể. 

Cấu trúc của vấn đề không che giấu yêu cầu đặt hàng. Chúng tôi không bắt buộc phải chọn các séc liền kề hoặc giữ nguyên thứ tự ngăn xếp. Bất kỳ tập hợp con nào có kích thước lên tới`k`là hợp lệ, giúp đơn giản hóa vấn đề thành một nhiệm vụ lựa chọn thuần túy. 

Các trường hợp lợi ích chính xuất phát từ việc hiểu cụm từ “tối đa k séc”. Ví dụ, nếu`k = n`, chúng ta phải lấy tất cả các phần tử. Nếu như`k = 1`, chúng ta chỉ cần chọn giá trị đơn lớn nhất. 

Hãy xem xét các tình huống sau: 

đầu vào:```
5 1
3 10 2 8 7
```Đầu ra:```
10
```Một cách tiếp cận đơn giản có thể thử kết hợp, nhưng câu trả lời đúng chỉ là phần tử tối đa. 

đầu vào:```
4 4
1 2 3 4
```Đầu ra:```
10
```Ở đây, chúng ta phải lấy mọi thứ vì giới hạn cho phép tất cả các yếu tố. 

Bất kỳ cách tiếp cận không chính xác nào thường xuất phát từ sự hiểu lầm liệu việc lựa chọn có bị hạn chế bởi vị trí hoặc thứ tự hay không. Trong bài toán này, nó hoàn toàn mang tính tổ hợp. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ thử tối đa mọi tập hợp con của`k`các phần tử, tính tổng của nó và theo dõi kết quả tốt nhất. Điều này đúng vì nó khớp trực tiếp với định nghĩa của nhiệm vụ, nhưng số lượng tập hợp con là rất lớn. Thậm chí hạn chế chính xác`k`yếu tố mang lại$\binom{n}{k}$, phát triển quá nhanh để liệt kê một lần`n`thậm chí lên tới vài chục. 

Quan sát quan trọng là thứ tự của các phần tử là không liên quan. Tổng của bất kỳ tập hợp con nào được chọn chỉ phụ thuộc vào giá trị nào được đưa vào chứ không phụ thuộc vào vị trí của chúng. Điều này làm giảm nhiệm vụ lựa chọn lớn nhất`k`các giá trị từ mảng. Bất kỳ giải pháp tối ưu nào cũng phải bao gồm các phần tử lớn nhất vì việc thay thế phần tử được chọn nhỏ hơn bằng phần tử lớn hơn không được sử dụng luôn cải thiện hoặc bảo toàn tổng số. 

Điều này biến vấn đề thành một vấn đề lựa chọn cổ điển: sắp xếp mảng và lấy phần trên cùng`k`giá trị hoặc duy trì cấu trúc đang chạy để theo dõi`k`phần tử lớn nhất. 

Sắp xếp là cách tiếp cận đơn giản và trực tiếp nhất. Sau khi sắp xếp theo thứ tự giảm dần thì đáp án chỉ là tổng của đáp số đầu tiên`k`các phần tử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Sắp xếp Top k | O(n log n) | O(1) hoặc O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách tập trung vào thực tế là chỉ những giá trị lớn nhất mới góp phần tạo ra lựa chọn tối ưu. 

1. Đọc`n`Và`k`, sau đó đọc danh sách các giá trị kiểm tra. Điều này xác định toàn bộ nhóm ứng viên mà chúng tôi có thể chọn. 
2. Sắp xếp danh sách theo thứ tự giảm dần sao cho giá trị lớn nhất xuất hiện trước. Bước này đảm bảo rằng những ứng viên tốt nhất được xếp ở vị trí đầu tiên, khiến việc lựa chọn trở nên đơn giản. 
3. Lấy cái đầu tiên`k`các phần tử trong danh sách đã sắp xếp. Những giá trị này tương ứng với các giá trị cao nhất có thể có. 
4. Tính tổng của chúng`k`phần tử và xuất nó dưới dạng câu trả lời. 

Mỗi bước làm giảm độ phức tạp của vấn đề: thay vì lý luận về các tập hợp con, chúng tôi giảm nó thành thứ tự và cắt lát, điều này mang tính quyết định sau khi sắp xếp. 

### Tại sao nó hoạt động 

Tính đúng đắn đến từ một đối số trao đổi đơn giản. Giả sử một lựa chọn tối ưu bao gồm một số phần tử`a`trong khi loại trừ một phần tử lớn hơn`b`không có trong lựa chọn. Thay thế`a`với`b`tăng tổng số tiền mà không vi phạm ràng buộc chọn nhiều nhất`k`các phần tử. Việc lặp lại quá trình thay thế này đảm bảo rằng bất kỳ giải pháp tối ưu nào cũng có thể được chuyển đổi thành một giải pháp bao gồm chính xác`k`những giá trị lớn nhất. Vì vậy việc lựa chọn đầu`k`sau khi sắp xếp luôn là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    arr = list(map(int, input().split()))
    arr.sort(reverse=True)
    print(sum(arr[:k]))

if __name__ == "__main__":
    solve()
```Giải pháp đọc đầu vào bằng I/O nhanh vì đầu vào mặc định của Python có thể chậm đối với các mảng lớn. Bước sắp xếp đặt các giá trị lớn hơn lên đầu tiên. lát cắt`arr[:k]`trích xuất chính xác số lần kiểm tra được phép và tính tổng chúng sẽ tạo ra tổng số tối ưu. 

Một chi tiết tinh tế là đảm bảo rằng chúng tôi không vô tình đưa vào nhiều hơn`k`các phần tử. Vì việc cắt lát không bao gồm giới hạn và an toàn ngay cả khi`k = n`, điều này tránh hoàn toàn việc xử lý trường hợp cạnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
11 5 10
```Mảng được sắp xếp:`[11, 10, 5]`| Bước | Trạng thái mảng | Yếu tố được chọn | Tổng hợp | 
| --- | --- | --- | --- | 
| Sau khi sắp xếp | 11 10 5 | - | 0 | 
| Lấy k=2 đầu tiên | 11 10 | 11 10 | 21 | 

Điều này chứng tỏ cách sắp xếp trực tiếp hiển thị tập hợp con tối ưu mà không cần bất kỳ lý luận tổ hợp nào. 

### Ví dụ 2 

đầu vào:```
5 3
4 1 7 3 9
```Mảng được sắp xếp:`[9, 7, 4, 3, 1]`| Bước | Trạng thái mảng | Yếu tố được chọn | Tổng hợp | 
| --- | --- | --- | --- | 
| Sau khi sắp xếp | 9 7 4 3 1 | - | 0 | 
| Lấy đầu tiên k=3 | 9 7 4 | 9 7 4 | 20 | 

Điều này xác nhận rằng ngay cả khi các giá trị lớn nằm rải rác trong mảng ban đầu, việc sắp xếp sẽ bình thường hóa cấu trúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, tổng là tuyến tính | 
| Không gian | O(1) | Sắp xếp được thực hiện ngoài việc lưu trữ đầu vào | 

Những hạn chế`n ≤ 10^4`làm một`n log n`tiếp cận dễ dàng đủ nhanh, vì tối đa là khoảng 10^5 thao tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    arr = list(map(int, input().split()))
    arr.sort(reverse=True)
    return str(sum(arr[:k]))

# provided sample
assert run("3 2\n11 5 10\n") == "21"

# minimum n
assert run("1 1\n7\n") == "7"

# k = n
assert run("4 4\n1 2 3 4\n") == "10"

# all equal
assert run("5 3\n5 5 5 5 5\n") == "15"

# mixed values
assert run("6 2\n-1 10 3 7 2 8\n") == "18"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1/7 | 7 | trường hợp kích thước tối thiểu | 
| 4 4 / 1 2 3 4 | 10 | chọn tất cả các phần tử | 
| 5 3 / cả 5s | 15 | giá trị thống nhất | 
| mảng hỗn hợp | 18 | lựa chọn tham lam đúng đắn | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi`k`bằng`n`. Thuật toán sắp xếp mảng và lấy tất cả các phần tử. Vì việc cắt không cắt vượt quá giới hạn nên tổng đương nhiên bao gồm mọi giá trị. 

Đối với đầu vào:```
4 4
3 1 4 2
```Sau khi sắp xếp:`[4, 3, 2, 1]`Chúng tôi lấy tất cả 4 yếu tố: 

tổng = 10 

Điều này phù hợp với yêu cầu vì chọn nhiều nhất`k`cho phép sử dụng toàn bộ bộ. 

Một trường hợp cạnh khác là khi`k = 1`. Thuật toán giảm xuống việc chọn phần tử tối đa. Sắp xếp đảm bảo nó ở vị trí 0. 

Đối với đầu vào:```
5 1
2 9 1 8 3
```Đã sắp xếp:`[9, 8, 3, 2, 1]`Chúng tôi chỉ lấy phần tử đầu tiên, tạo ra 9, theo định nghĩa là tối ưu vì bất kỳ lựa chọn nào khác sẽ nhỏ hơn.
