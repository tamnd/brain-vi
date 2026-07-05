---
title: "CF 102899B - KK \u5b66\u51e0\u4f55"
description: "Chúng ta được cung cấp một chuỗi các hình hình học, mỗi hình được mô tả bằng một mã định danh loại, theo sau là kích thước của nó. Mỗi hình đóng góp một diện tích và nhiệm vụ là tích lũy tổng diện tích trên tất cả các hình và đưa ra tổng cuối cùng được làm tròn đến một chữ số thập phân."
date: "2026-07-04T08:19:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102899
codeforces_index: "B"
codeforces_contest_name: "The 2nd Hangzhou Normal University Freshman Programming Contest"
rating: 0
weight: 102899
solve_time_s: 47
verified: true
draft: false
---

[CF 102899B - KK \u5b66\u51e0\u4f55](https://codeforces.com/problemset/problem/102899/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các hình hình học, mỗi hình được mô tả bằng một mã định danh loại, theo sau là kích thước của nó. Mỗi hình đóng góp một diện tích và nhiệm vụ là tích lũy tổng diện tích trên tất cả các hình và đưa ra tổng cuối cùng được làm tròn đến một chữ số thập phân. Hình học liên quan rất đơn giản: hình tròn sử dụng giá trị cố định π bằng 3, hình tam giác sử dụng công thức tiêu chuẩn nửa đáy nhân chiều cao và hình chữ nhật sử dụng tích độ dài các cạnh. 

Kích thước đầu vào nhỏ, với tối đa 1000 hình dạng và tất cả các kích thước được giới hạn bởi 100. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ giải pháp nào chạy theo thời gian tuyến tính trên đầu vào đều đủ và ngay cả số học dấu phẩy động đơn giản cũng an toàn về hiệu suất và độ chính xác. Không cần cấu trúc dữ liệu không gian hoặc thủ thuật tối ưu hóa, vì mỗi hình dạng độc lập với các hình dạng khác và đóng góp bổ sung cho câu trả lời cuối cùng. 

Một trường hợp tinh tế đến từ độ chính xác và định dạng. Mặc dù tất cả đầu vào đều là số nguyên, diện tích tam giác liên quan đến việc chia cho 2 và đầu ra cuối cùng phải được in chính xác một chữ số sau dấu thập phân. Việc triển khai bất cẩn sử dụng phép chia số nguyên sẽ âm thầm phá vỡ một nửa số trường hợp. Ví dụ: một tam giác có đáy 3 và chiều cao 1 sẽ đóng góp 1,5, nhưng phép chia số nguyên sẽ tạo ra 1. 

Một trường hợp cạnh khác là giá trị π cố định. Nhiều lập trình viên sử dụng math.pi theo bản năng, nhưng ở đây bài toán xác định rõ ràng π = 3, do đó, việc sử dụng một giá trị gần đúng thực như 3,14159 sẽ tạo ra các câu trả lời luôn sai. 

## Phương pháp tiếp cận 

Cấu trúc của bài toán có tính chất cộng: mỗi số đóng góp độc lập vào tổng số hiện có. Điều này có nghĩa là cách tiếp cận trực tiếp nhất cũng là cách tiếp cận đúng. Chúng ta đọc từng hình, tính diện tích của nó ngay lập tức và thêm nó vào bộ tích lũy. 

Cách giải thích bạo lực vẫn sẽ thực hiện chính xác điều tương tự, vì không có sự tương tác giữa các hình dạng. Sự khác biệt duy nhất giữa một giải pháp đơn giản và tối ưu là độ sạch sẽ của quá trình triển khai hơn là hiệu quả của thuật toán. Ngay cả khi chúng tôi tính toán lại mọi thứ nhiều lần hoặc lưu trữ tất cả các hình dạng và xử lý chúng sau đó, chi phí sẽ vẫn tuyến tính theo số lượng hình dạng, điều này không đáng kể trong các ràng buộc. 

Quan sát chính là vấn đề không yêu cầu bất kỳ phép biến đổi, sắp xếp hoặc mối quan hệ hình học nào giữa các hình. Mỗi dòng là độc lập nên việc tính toán giảm xuống thành ánh xạ trực tiếp từ bản ghi đầu vào sang phần đóng góp bằng số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (lưu trữ và tính toán lại) | O(n) | O(n) | Đã chấp nhận | 
| Tối ưu (tích lũy luồng) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng con số một và duy trì tổng số hiện có.

1. Đọc số hình n, số này xác định chúng ta sẽ thực hiện bao nhiêu phép tính độc lập. Điều này đặt số lần lặp cho vòng lặp chính. 
2. Khởi tạo biến tổng thành 0. Biến này tích lũy tổng của tất cả các diện tích được tính toán. Nó phải là dấu phẩy động để lưu trữ chính xác các nửa tam giác. 
3. Đối với mỗi dòng trong số n dòng, hãy đọc mã định danh loại t và các tham số liên quan. Bước phân tích cú pháp hoàn toàn mang tính cấu trúc và xác định công thức nào sẽ được áp dụng. 
4. Nếu loại là 1, hãy hiểu các tham số là bán kính r của đường tròn. Tính diện tích là 3 * r * r bằng cách sử dụng giá trị π cố định của bài toán. Thêm phần này vào tổng số. 
5. Nếu loại là 2, hãy hiểu các tham số là đáy L và chiều cao h của một tam giác. Tính diện tích theo dạng (L*h)/2.0 để đảm bảo sử dụng phép chia dấu phẩy động. Thêm phần này vào tổng số. 
6. Nếu loại là 3, hãy hiểu các tham số là kích thước hình chữ nhật L và W. Tính diện tích là L * W và cộng nó vào tổng. 
7. Sau khi xử lý tất cả các hình dạng, tổng kết quả đầu ra được định dạng chính xác bằng một chữ số sau dấu thập phân. 

Tính đúng đắn của quy trình này phụ thuộc vào thực tế là mỗi hình đóng góp một cách độc lập và tuyến tính vào tổng cuối cùng, do đó không có thứ tự hoặc cấu trúc trung gian nào ảnh hưởng đến kết quả. 

### Tại sao nó hoạt động 

Thuật toán duy trì bất biến rằng sau khi xử lý i hình, tổng bằng tổng diện tích của chính xác i hình đó. Mỗi lần lặp lại thêm chính xác một giá trị vùng độc lập mới được lấy từ dòng đầu vào hiện tại. Vì không có bước nào sửa đổi các đóng góp trước đó và không có hình dạng nào phụ thuộc vào bước khác, nên bất biến giữ nguyên quy nạp cho đến phần tử cuối cùng, tại điểm đó tổng bằng tổng của tất cả các diện tích. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    total = 0.0

    for _ in range(n):
        parts = input().split()
        t = int(parts[0])

        if t == 1:
            r = int(parts[1])
            total += 3 * r * r
        elif t == 2:
            L = int(parts[1])
            h = int(parts[2])
            total += (L * h) / 2.0
        else:
            L = int(parts[1])
            W = int(parts[2])
            total += L * W

    print(f"{total:.1f}")

if __name__ == "__main__":
    solve()
```Việc triển khai giữ mọi thứ trong một lần chuyển qua đầu vào. Việc sử dụng số học dấu phẩy động chỉ cần thiết cho các hình tam giác, nhưng bộ tích lũy được giữ ở dạng float xuyên suốt để tránh chuyển đổi kiểu lặp lại. Bước định dạng ở cuối đảm bảo hành vi làm tròn chính xác đến một chữ số thập phân. 

Một lỗi thường gặp là viết`(L * h) // 2`, buộc phải chia số nguyên và loại bỏ các nửa phân số. Một sai lầm khác là quên sử dụng 3 làm số π, điều này được yêu cầu rõ ràng trong bài toán và ghi đè các hằng số toán học. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào nhỏ có hình tròn và hình chữ nhật. 

đầu vào:```
2
1 2
3 3 4
```| Bước | Loại | Tính toán | Tổng cộng | 
| --- | --- | --- | --- | 
| 1 | Vòng tròn r=2 | 3 × 2 × 2 = 12 | 12 | 
| 2 | Hình chữ nhật 3×4 | 12 | 24 | 

Đầu ra cuối cùng là 24.0. Dấu vết này cho thấy sự tích lũy hoàn toàn mang tính cộng và không phụ thuộc vào trật tự. 

Bây giờ hãy xem xét một trường hợp liên quan đến một hình tam giác. 

đầu vào:```
3
2 3 1
1 1
2 4 2
```| Bước | Loại | Tính toán | Tổng cộng | 
| --- | --- | --- | --- | 
| 1 | Tam giác 3×1 | 1,5 | 1,5 | 
| 2 | Vòng tròn r=1 | 3 | 4,5 | 
| 3 | Tam giác 4×2 | 4 | 8,5 | 

Ví dụ này chứng minh tại sao phép chia dấu phẩy động là cần thiết. Tam giác đầu tiên đóng góp một giá trị phân số và việc bảo toàn nó là điều cần thiết để đảm bảo tính chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi trong số n hình được xử lý chính xác một lần với công việc không đổi trên mỗi hình | 
| Không gian | O(1) | Chỉ sử dụng tổng hiện có và một số biến tạm thời | 

Các ràng buộc cho phép tối đa 1000 hình dạng, do đó, một lần truyền với tính toán thời gian không đổi cho mỗi hình dạng sẽ nhanh chóng trong giới hạn. Việc sử dụng bộ nhớ là tối thiểu do không cần lưu trữ toàn bộ đầu vào ngoài dòng hiện tại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    output = io.StringIO()
    sys_stdout = sys.stdout
    sys.stdout = output

    solve()

    sys.stdout = sys_stdout
    return output.getvalue().strip()

# provided sample-like case
assert run("2\n1 2\n3 3 4\n") == "24.0"

# triangle precision case
assert run("1\n2 3 1\n") == "1.5"

# all circles
assert run("3\n1 1\n1 2\n1 3\n") == "42.0"

# mixed minimal case
assert run("3\n1 1\n2 2 2\n3 1 1\n") == "9.0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 vòng tròn | 3.0 | công thức đường tròn cơ bản và π = 3 | 
| chỉ hình tam giác | 1,5 | xử lý phân số đúng | 
| nhiều vòng tròn | 42.0 | chính xác tích lũy lặp đi lặp lại | 
| hình dạng hỗn hợp | 9,0 | tích hợp cả ba công thức | 

## Vỏ cạnh 

Một trường hợp quan trọng là phép tính tam giác trong đó phép chia số nguyên sẽ âm thầm phá vỡ tính chính xác. Đối với một đầu vào như`2 3 1`, kết quả đúng là 1,5. Thuật toán sử dụng rõ ràng phép chia dấu phẩy động`(L * h) / 2.0`, đảm bảo rằng thành phần phân số được bảo toàn. 

Một trường hợp khác là định dạng chính xác. Ngay cả khi kết quả là một số nguyên, chẳng hạn như một hình chữ nhật`3 2`, đầu ra vẫn phải được in dưới dạng`6.0`. Đầu ra được định dạng`f"{total:.1f}"`thực thi điều này một cách nhất quán. 

Trường hợp cạnh cuối cùng là việc sử dụng sai số π. Nếu một bộ giải sử dụng hằng số thư viện chuẩn như`math.pi`, đóng góp của vòng kết nối sẽ lớn hơn một chút so với dự kiến ​​và lỗi sẽ tích lũy trên nhiều vòng kết nối. Việc sử dụng hằng số cố định 3 sẽ tránh được hoàn toàn sự trôi dạt hệ thống này.
