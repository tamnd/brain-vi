---
title: "CF 104380I - Điểm kém"
description: "Chúng tôi được cung cấp một chuỗi các điểm thi cho một học sinh, mỗi điểm là một số nguyên từ 0 đến 100. Nhiệm vụ là tạo ra một phiên bản rõ ràng của chuỗi này trong đó mọi điểm dưới 60 đều bị loại bỏ, trong khi vẫn giữ thứ tự tương đối của các điểm còn lại giống hệt như trong…"
date: "2026-07-01T17:07:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104380
codeforces_index: "I"
codeforces_contest_name: "The Andover Computing Open (TACO) 2023"
rating: 0
weight: 104380
solve_time_s: 53
verified: true
draft: false
---

[CF 104380I - Điểm kém](https://codeforces.com/problemset/problem/104380/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các điểm thi cho một học sinh, mỗi điểm là một số nguyên từ 0 đến 100. Nhiệm vụ là tạo ra một phiên bản rõ ràng của chuỗi này trong đó mọi điểm dưới 60 đều bị loại bỏ, trong khi vẫn giữ thứ tự tương đối của các điểm còn lại giống hệt như trong danh sách ban đầu. 

Đầu vào về cơ bản là một vấn đề chuyển đổi danh sách: chúng tôi quét qua danh sách một lần và quyết định, đối với từng phần tử, liệu nó có tồn tại ở đầu ra hay bị loại bỏ. Không có sự sắp xếp lại, không tổng hợp và không có sự tương tác giữa các phần tử ngoài việc lọc dựa trên ngưỡng. 

Giới hạn về số lượng điểm, lên tới 100.000, ngay lập tức loại trừ bất kỳ phương pháp nào liên tục dịch chuyển các phần tử bên trong danh sách hoặc thực hiện quét lồng nhau. Một cách triển khai đơn giản loại bỏ các phần tử khỏi danh sách Python trong khi lặp lại nó có thể chuyển sang hành vi bậc hai, vì mỗi lần xóa sẽ khiến các phần tử bị dịch chuyển. Với 100.000 phần tử, loại hành vi đó sẽ quá chậm dưới giới hạn 1 giây. Hướng an toàn duy nhất là xây dựng một lần duy nhất của đầu ra. 

Các trường hợp cạnh chủ yếu là cấu trúc hơn là số. Nếu tất cả các điểm đều dưới 60, đầu ra trống và không có gì được in. Nếu tất cả các điểm đều từ 60 trở lên thì đầu ra khớp chính xác với đầu vào. Một lỗi nhỏ thường xuất hiện khi in một kết quả trống: một số triển khai vô tình in thêm một dòng mới hoặc không tạo ra kết quả nào cả. Một vấn đề khác phát sinh nếu người ta cố gắng lọc tại chỗ bằng cách xóa chỉ mục, việc này có thể bỏ qua các phần tử do dịch chuyển chỉ mục. Ví dụ, trong`[59, 58, 61]`, loại bỏ 59 ca 58 vào vị trí của nó và vòng lặp dựa trên chỉ mục có thể bỏ qua việc kiểm tra nó tùy thuộc vào cách cấu trúc lặp lại. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để nghĩ về vấn đề này là mô phỏng quy trình theo đúng nghĩa đen: quét danh sách và xóa bất kỳ phần tử nào dưới 60. Theo nghĩa trừu tượng, điều này đúng vì nó khớp chính xác với đặc điểm kỹ thuật. Tuy nhiên, việc thực hiện xóa bên trong danh sách Python rất tốn kém. Mỗi lần xóa sẽ dịch chuyển tất cả các phần tử tiếp theo sang trái một vị trí, tốn O(n) cho mỗi lần xóa trong trường hợp xấu nhất. Nếu hầu hết các phần tử đều nhỏ, chúng tôi có thể xóa gần như tất cả các mục, dẫn đến hành vi O(n²). 

Hiểu biết sâu sắc về cấu trúc là việc xóa là không cần thiết nếu chúng ta không bao giờ thay đổi danh sách tại chỗ. Thay vào đó, chúng ta có thể tạo danh sách mới và chỉ nối thêm các phần tử hợp lệ. Điều này chuyển đổi chi phí xử lý của từng phần tử thành O(1), vì việc thêm vào danh sách được khấu hao theo thời gian không đổi. Vấn đề giảm xuống còn một lần quét tuyến tính trong đó chúng tôi kiểm tra một điều kiện và quyết định có sao chép phần tử về phía trước hay không. 

Lý do điều này hoạt động hiệu quả là vì thứ tự đầu ra giống với thứ tự đầu vào cho các phần tử được giữ lại. Không có sự phụ thuộc giữa các quyết định nên mỗi cấp độ có thể được đánh giá độc lập. Khi chúng tôi ngừng cố gắng bảo tồn vùng chứa ban đầu và thay vào đó xây dựng một vùng chứa được lọc, độ phức tạp sẽ chuyển từ phương trình bậc hai sang tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Loại bỏ tại chỗ trong quá trình lặp lại | O(n²) | O(1) | Quá chậm | 
| Xây dựng danh sách lọc | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lớp`n`. Điều này xác định có bao nhiêu giá trị chúng tôi sẽ kiểm tra chính xác một lần. 
2. Khởi tạo danh sách trống`result`sẽ lưu trữ tất cả các lớp vượt qua kiểm tra ngưỡng. 
3. Lặp lại từng bước`n`điểm khi chúng được đọc từ đầu vào. 
4. Đối với mỗi lớp, so sánh với 60. 
5. Nếu điểm ít nhất là 60, hãy thêm nó vào`result`. Nếu không, hãy bỏ qua nó hoàn toàn. 
6. Sau khi xử lý tất cả các lớp, hãy in từng giá trị vào`result`trên dòng riêng của nó theo đúng thứ tự chúng được thêm vào. 

Lựa chọn thiết kế quan trọng là chúng tôi không bao giờ sửa đổi trình tự đầu vào. Mỗi quyết định đều mang tính cục bộ đối với một phần tử duy nhất, vì vậy, chúng tôi tránh mọi tác dụng phụ có thể ảnh hưởng đến các lần lặp lại trong tương lai. 

### Tại sao nó hoạt động 

Tại mỗi bước quét,`result`chứa chính xác trình tự của tất cả các lớp được thấy cho đến nay có ít nhất là 60, theo cùng thứ tự chúng xuất hiện trong đầu vào. Khi xử lý một điểm mới, chúng tôi sẽ loại bỏ nó nếu nó dưới 60 hoặc thêm nó vào cuối nếu nó hợp lệ. Điều này bảo đảm cả tính chính xác của việc lọc và tính ổn định của thứ tự. Vì mỗi phần tử được kiểm tra chính xác một lần và việc đưa nó vào chỉ phụ thuộc vào giá trị của chính nó nên không thao tác nào sau đó có thể làm mất hiệu lực của quyết định trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    result = []

    for _ in range(n):
        x = int(input())
        if x >= 60:
            result.append(x)

    sys.stdout.write("\n".join(map(str, result)))

if __name__ == "__main__":
    main()
```Giải pháp sử dụng đầu vào nhanh thông qua`sys.stdin.readline`bởi vì đọc tới 100.000 dòng với tiêu chuẩn`input()`có thể giới thiệu chi phí không cần thiết. Mỗi điểm được xử lý ngay sau khi đọc và chỉ những điểm hợp lệ mới được lưu trữ. 

Một chi tiết triển khai tinh tế là việc xây dựng đầu ra cuối cùng. Thay vì in từng dòng, chúng ta nối tất cả các số nguyên hợp lệ thành một chuỗi. Điều này tránh các lệnh gọi I/O lặp lại, có thể bị chậm trong Python trong các giới hạn chặt chẽ. 

Một điểm khác là xử lý trường hợp`result`trống rỗng. Thao tác nối tự nhiên tạo ra một chuỗi trống, phù hợp với định dạng đầu ra được yêu cầu mà không có dòng bổ sung. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
100
90
59
65
40
```Chúng tôi xử lý từng lớp một cách tuần tự: 

| Bước | Lớp | Tình trạng (>=60) | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 100 | Đúng | [100] | 
| 2 | 90 | Đúng | [100, 90] | 
| 3 | 59 | Sai | [100, 90] | 
| 4 | 65 | Đúng | [100, 90, 65] | 
| 5 | 40 | Sai | [100, 90, 65] | 

Đầu ra:```
100
90
65
```Dấu vết này cho thấy rằng quá trình lọc không bao giờ thay đổi thứ tự tương đối và chỉ loại bỏ các phần tử cục bộ dựa trên giá trị. 

### Ví dụ 2 

đầu vào:```
4
59
60
58
61
```| Bước | Lớp | Tình trạng (>=60) | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 59 | Sai | [] | 
| 2 | 60 | Đúng | [60] | 
| 3 | 58 | Sai | [60] | 
| 4 | 61 | Đúng | [60, 61] | 

Đầu ra:```
60
61
```Ví dụ này nhấn mạnh rằng các phần tử hợp lệ vẫn được sắp xếp chính xác ngay cả khi được bao quanh bởi các phần tử không hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi lớp được đọc một lần và kiểm tra một lần | 
| Không gian | O(k) | Chỉ lưu trữ loại ≥ 60, trong đó k ≤ n | 

Thuật toán chia tỷ lệ tuyến tính theo số điểm, đây là mức tối ưu vì mọi yếu tố đầu vào phải được kiểm tra ít nhất một lần. Với n lên tới 100.000, điều này thoải mái phù hợp với cả hạn chế về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import main
    main()
    return sys.stdout.getvalue().strip()

# sample
assert run("5\n100\n90\n59\n65\n40\n") == "100\n90\n65"

# all removed
assert run("3\n10\n20\n30\n") == ""

# all kept
assert run("3\n60\n70\n100\n") == "60\n70\n100"

# boundary values
assert run("4\n59\n60\n61\n0\n") == "60\n61"

# single element kept
assert run("1\n100\n") == "100"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả < 60 | trống | xử lý đúng việc không có đầu ra | 
| tất cả ≥ 60 | cùng danh sách | ổn định và duy trì đầy đủ | 
| ranh giới hỗn hợp | danh sách đã lọc | hành vi ngưỡng đúng | 
| phần tử đơn | giống hoặc trống | độ chính xác đầu vào tối thiểu | 

## Vỏ cạnh 

Khi tất cả các lớp đều dưới 60, chẳng hạn như`10, 20, 30`, thuật toán vẫn lặp qua từng giá trị và đơn giản là không bao giờ thêm bất cứ thứ gì. Kết quả vẫn là một danh sách trống và thao tác nối tạo ra một chuỗi trống, khớp chính xác với định dạng đầu ra được yêu cầu mà không cần thêm dòng. 

Khi tất cả các điểm đều hợp lệ, chẳng hạn như`60, 80, 100`, mọi phần tử đều thỏa mãn điều kiện và được nối theo thứ tự. Thuật toán thực sự trở thành bản sao trực tiếp của đầu vào, xác nhận rằng không có biến đổi ngoài ý muốn nào xảy ra. 

Khi các giá trị luân phiên quanh ngưỡng, chẳng hạn như`59, 60, 58, 61`, mỗi quyết định là độc lập. Quá trình quét cho thấy các giá trị bị từ chối không ảnh hưởng đến việc chấp nhận sau này và thứ tự được giữ nguyên chính xác theo yêu cầu.
