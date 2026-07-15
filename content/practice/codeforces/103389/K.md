---
title: "CF 103389K - \u97f3\u4e50\u6e38\u620f"
description: "Nhiệm vụ cơ bản là xử lý một luồng mã thông báo văn bản và trích xuất một thống kê ký tự rất cụ thể từ chúng. Thay vì thực hiện bất kỳ phân tích cú pháp hoặc chuyển đổi cấu trúc nào, chúng tôi đọc tất cả các chuỗi đầu vào và chỉ tập trung vào sự xuất hiện của ký tự gạch nối -."
date: "2026-07-03T12:14:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103389
codeforces_index: "K"
codeforces_contest_name: "2021\u5e74\u4e2d\u56fd\u5927\u5b66\u751f\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\u5973\u751f\u4e13\u573a"
rating: 0
weight: 103389
solve_time_s: 41
verified: true
draft: false
---

[CF 103389K - \u97f3\u4e50\u6e38\u620f](https://codeforces.com/problemset/problem/103389/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ cơ bản là xử lý một luồng mã thông báo văn bản và trích xuất một thống kê ký tự rất cụ thể từ chúng. Thay vì thực hiện bất kỳ phân tích cú pháp hoặc chuyển đổi cấu trúc nào, chúng tôi đọc tất cả các chuỗi đầu vào và chỉ tập trung vào sự xuất hiện của ký tự gạch nối`-`. Mỗi khi ký tự này xuất hiện trong bất kỳ mã thông báo nào, nó sẽ đóng góp một đơn vị cho câu trả lời cuối cùng. 

Bạn có thể coi đầu vào là một chuỗi các chuỗi được phân tách bằng khoảng trắng, có thể kéo dài trên nhiều dòng và đầu ra là một số nguyên duy nhất biểu thị số lượng dấu gạch nối xuất hiện trên tất cả các chuỗi đó cộng lại. Không nhóm, không đặt hàng và không giải thích các chuỗi ngoài việc kiểm tra ký tự. 

Mặc dù mô tả chính thức là tối thiểu nhưng các ràng buộc ngụ ý là tiêu chuẩn cho các tác vụ xử lý chuỗi của Codeforces. Tổng kích thước đầu vào có thể dễ dàng đạt tới hàng trăm nghìn hoặc hàng triệu ký tự. Điều đó ngay lập tức loại trừ mọi chiến lược quét lại hoặc ghép nối không hiệu quả lặp đi lặp lại. Một giải pháp phải xử lý dữ liệu đầu vào trong một lần duy nhất, kiểm tra từng ký tự chính xác một lần. 

Một trường hợp phức tạp phát sinh từ cách kết thúc và cấu trúc đầu vào. Vì việc đọc thường được thực hiện thông qua đầu vào dựa trên mã thông báo hoặc đọc toàn bộ luồng cho đến EOF, nên việc triển khai giả định số lượng chuỗi hoặc dòng cố định có thể bị lỗi âm thầm khi độ dài đầu vào thay đổi. Một trường hợp khác là hoàn toàn không có bất kỳ dấu gạch nối nào, trong đó đầu ra chính xác chỉ đơn giản là 0 và việc triển khai phải đảm bảo chúng vẫn tạo ra đầu ra ngay cả khi không tìm thấy ký tự trùng khớp nào. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản. Chúng tôi đọc tất cả các chuỗi vào bộ nhớ, sau đó với mỗi chuỗi, chúng tôi lặp lại từng ký tự và tăng bộ đếm bất cứ khi nào chúng tôi gặp phải`-`. Điều này đúng vì nó phản ánh trực tiếp định nghĩa của nhiệm vụ. Chi phí của phương pháp này tỷ lệ thuận với tổng số ký tự trên tất cả các chuỗi. Nếu chúng ta biểu thị tổng chiều dài đó là N thì thời gian chạy là O(N), điều này đã tối ưu về độ phức tạp tiệm cận. 

Không có sự tối ưu hóa thuật toán có ý nghĩa nào ngoài điều này, bởi vì mỗi ký tự phải được kiểm tra ít nhất một lần để xác định xem đó có phải là dấu gạch nối hay không. Bất kỳ nỗ lực nào để bỏ qua quá trình quét hoặc xử lý hàng loạt ký tự vẫn sẽ ngầm chạm vào từng ký tự, do đó giới hạn dưới về mặt lý thuyết vẫn là tuyến tính. 

Sự cải tiến thực tế duy nhất là cách đọc dữ liệu đầu vào. Việc sử dụng các phương thức nhập tiêu chuẩn của Python một cách hiệu quả sẽ đảm bảo chúng ta không phải chịu chi phí từ các lệnh gọi hàm lặp lại hoặc nối chuỗi. Do đó, giải pháp tối ưu không phải là giảm độ phức tạp mà là thực hiện quét tuyến tính theo cách trực tiếp và hiệu quả nhất có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét lực lượng vũ phu trên mỗi chuỗi | O(N) | O(1) đến O(N) | Đã chấp nhận | 
| Quét luồng một lượt | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc đầu vào từ đầu vào tiêu chuẩn dưới dạng một luồng liên tục các mã thông báo được phân tách bằng khoảng trắng. Điều này đảm bảo chúng tôi xử lý chính xác mọi kết hợp ngắt dòng và dấu cách mà không đưa ra giả định về định dạng. 
2. Khởi tạo bộ đếm về 0. Bộ đếm này sẽ tích lũy tổng số dấu gạch nối gặp phải trên tất cả các mã thông báo. 
3. Lặp lại từng mã thông báo thu được từ đầu vào. Mỗi mã thông báo được xử lý độc lập nhưng được xử lý giống hệt nhau. 
4. Đối với mỗi mã thông báo, hãy lặp qua từng ký tự và kiểm tra xem nó có bằng không`-`. Nếu có, hãy tăng bộ đếm lên một. Việc kiểm tra ký tự trực tiếp này tránh mọi quá trình xử lý trước hoặc chuyển đổi không cần thiết. 
5. Sau khi tất cả các mã thông báo đã được xử lý, hãy xuất giá trị cuối cùng của bộ đếm. 

Ý tưởng chính là vấn đề giảm hoàn toàn sang việc đếm ký tự trong đầu vào phát trực tuyến, do đó tính chính xác chỉ phụ thuộc vào việc đảm bảo rằng không có ký tự nào bị bỏ qua và không có ký tự nào được tính hai lần. 

### Tại sao nó hoạt động 

Thuật toán duy trì một bất biến đơn giản: sau khi xử lý k mã thông báo, bộ đếm sẽ bằng số lượng dấu gạch nối xuất hiện trong chính xác k mã thông báo đó. Mỗi mã thông báo được quét toàn bộ một lần và mỗi ký tự được đánh giá chính xác một lần. Vì đầu vào được phân chia thành các mã thông báo rời rạc và tất cả các mã thông báo đều được che phủ nên bộ đếm cuối cùng nhất thiết phải bằng tổng số dấu gạch nối trong toàn bộ đầu vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

data = sys.stdin.read().split()

ans = 0
for token in data:
    for ch in token:
        if ch == '-':
            ans += 1

print(ans)
```Giải pháp đọc toàn bộ đầu vào cùng một lúc bằng cách sử dụng`sys.stdin.read()`, giúp tránh chi phí đọc trên mỗi dòng. Việc chia thành các mã thông báo đảm bảo rằng tất cả các chuỗi được phân tách bằng khoảng trắng đều được xử lý thống nhất. 

Cấu trúc vòng lặp lồng nhau là có chủ ý và tối thiểu. Vòng lặp bên ngoài lặp qua các mã thông báo, trong khi vòng lặp bên trong kiểm tra từng ký tự. điều kiện`ch == '-'`là logic duy nhất được yêu cầu và bộ đếm tích lũy kết quả mà không cần bất kỳ bộ lưu trữ phụ trợ nào. 

Một lỗi triển khai phổ biến ở đây là sử dụng lặp đi lặp lại`input()`các cuộc gọi theo vòng lặp, có thể chậm hơn và có nguy cơ thiếu các điều kiện EOF. Một vấn đề nhỏ khác là quên xử lý tất cả các ký tự trong mã thông báo nếu người ta cố gắng tối ưu hóa bằng cách sử dụng các phương thức chuỗi không chính xác. Giải pháp được trình bày sẽ tránh được cả hai cạm bẫy bằng cách lặp lại trực tiếp. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào trong đó các mã thông báo được trộn lẫn trên các dòng: 

đầu vào:```
a-b c--d
e-f
```Chúng tôi xử lý mã thông báo bằng mã thông báo. 

| Mã thông báo | Ký tự được xử lý | Dấu gạch nối được tìm thấy | Quầy | 
| --- | --- | --- | --- | 
| a-b | a, -, b | 1 | 1 | 
| c--d | c, -, -, d | 2 | 3 | 
| e-f | e, -, f | 1 | 4 | 

Đầu ra cuối cùng là 4. Dấu vết này cho thấy rằng ngắt dòng không quan trọng, chỉ có ranh giới mã thông báo và ký tự bên trong chúng. 

Bây giờ hãy xem xét một trường hợp không có dấu gạch nối: 

đầu vào:```
abc def ghi
```| Mã thông báo | Ký tự được xử lý | Dấu gạch nối được tìm thấy | Quầy | 
| --- | --- | --- | --- | 
| abc | a, b, c | 0 | 0 | 
| chắc chắn | đ, đ, f | 0 | 0 | 
| ghi | g, h, tôi | 0 | 0 | 

Kết quả cuối cùng là 0, xác nhận việc xử lý đúng các kết quả trống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi ký tự trong mỗi mã thông báo đều được kiểm tra chính xác một lần | 
| Không gian | O(1) | Chỉ một bộ đếm duy nhất được lưu trữ, đầu vào được truyền trực tuyến | 

Thuật toán dễ dàng phù hợp với các ràng buộc điển hình cho các vấn đề về chuỗi. Ngay cả đối với đầu vào có hàng triệu ký tự, một lần quét tuyến tính trong Python là đủ trong giới hạn thời gian tiêu chuẩn, vì mỗi thao tác là một so sánh ký tự đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().split()
    ans = 0
    for token in data:
        for ch in token:
            if ch == '-':
                ans += 1
    return str(ans)

# constructed cases

# case 1: mixed hyphens
assert run("a-b c--d e-f") == "4", "mixed hyphens"

# case 2: no hyphens
assert run("abc def ghi") == "0", "no hyphens"

# case 3: all hyphens
assert run("--- --") == "5", "all hyphens"

# case 4: single character
assert run("-") == "1", "single hyphen"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| a-b c--d e-f | 4 | phân phối hỗn hợp trên các token | 
| ghi abc def | 0 | sự vắng mặt của nhân vật mục tiêu | 
| --- -- | 5 | dấu gạch ngang dày đặc liên tiếp | 
| - | 1 | trường hợp đầu vào tối thiểu | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi đầu vào không chứa mã thông báo nào cả. Trong hoàn cảnh đó,`sys.stdin.read().split()`trả về một danh sách trống và vòng lặp không bao giờ thực thi. Bộ đếm vẫn giữ nguyên giá trị ban đầu là 0, điều này đúng vì không có ký tự và do đó không có dấu gạch nối. 

Một trường hợp khác là các chuỗi liền kề cực lớn không có khoảng trắng. Vì thuật toán xử lý từng ký tự một cách độc lập nên nó vẫn hoạt động chính xác. Ví dụ: nhập như`a---b---c`được coi là một mã thông báo duy nhất và quá trình quét sẽ đếm trực tiếp tất cả các dấu gạch nối, tạo ra tổng số chính xác. 

Trường hợp cạnh cuối cùng là khoảng trắng không đều, chẳng hạn như nhiều khoảng trắng hoặc dòng mới giữa các mã thông báo. Vì việc phân tách sẽ thu gọn tất cả khoảng trắng một cách thống nhất nên việc nhóm các mã thông báo không liên quan đến tính chính xác của số đếm.
