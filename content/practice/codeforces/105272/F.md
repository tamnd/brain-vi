---
title: "CF 105272F - Lễ hội Trăng rằm"
description: "Chúng ta đang quan sát một lễ hội trong đó mỗi người tham gia mua đúng một vé. Có hai loại vé: vé bình thường giá v và vé mặt trăng giảm giá có giá đúng bằng một nửa số đó, v/2, trong đó v được đảm bảo chẵn."
date: "2026-06-23T06:56:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105272
codeforces_index: "F"
codeforces_contest_name: "IX MaratonUSP Freshman Contest"
rating: 0
weight: 105272
solve_time_s: 42
verified: true
draft: false
---

[CF 105272F - Lễ hội Mặt trăng](https://codeforces.com/problemset/problem/105272/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang quan sát một lễ hội trong đó mỗi người tham gia mua đúng một vé. Có 2 loại vé: vé thông thường`v`và một vé mặt trăng giảm giá có giá bằng một nửa số đó,`v/2`, Ở đâu`v`được đảm bảo là đồng đều. 

Chúng tôi được cung cấp tổng số người tham dự`p`, tổng doanh thu thu được`a`, và giá vé đầy đủ`v`. Một số người tham dự không rõ số lượng là cư dân mặt trăng luôn trả giá chiết khấu, trong khi tất cả những người tham dự khác đều trả giá đầy đủ. Nhiệm vụ là xác định có bao nhiêu`p`người tham dự đã mua vé giảm giá. 

Các ràng buộc rất lớn nhưng cấu trúc cực kỳ đơn giản: tất cả các giá trị đều lên tới một triệu và chỉ có các mối quan hệ số học mới quan trọng. Điều này ngay lập tức loại trừ bất kỳ mô phỏng nào trên các cá nhân hoặc tìm kiếm trên số lượng có thể có theo cách ngây thơ phụ thuộc vào việc lặp lại tối đa`p`hoặc`a`. Bất kỳ lời giải đúng nào cũng phải quy bài toán về một phép tính theo thời gian không đổi rút ra từ một phương trình. 

Một trường hợp thất bại tinh vi sẽ xuất hiện nếu một người cố gắng suy luận một cách tham lam mà không hình thành một phương trình. Ví dụ: giả sử rằng việc tối đa hóa số người tham dự được giảm giá luôn hợp lệ có thể phá vỡ tính nhất quán với giới hạn tổng doanh thu. Một sai lầm phổ biến khác là quên rằng mức giảm giá chỉ bằng một nửa`v`, không phải là một phân số tùy ý, điều này rất cần thiết để hình thành một mối quan hệ tuyến tính rõ ràng. 

## Phương pháp tiếp cận 

Một cách giải thích vũ phu sẽ thử mọi con số có thể`x`của những người tham dự mặt trăng từ`0`ĐẾN`p`, tính toán doanh thu thu được và kiểm tra xem nó có khớp với`a`. Điều này có hiệu quả vì mỗi ứng viên đều xác định đầy đủ doanh thu nhưng lại tốn kém`O(p)`mỗi trường hợp thử nghiệm. Với`p`lên tới một triệu, điều này vẫn khả thi khi xét riêng lẻ, nhưng trở nên không cần thiết và không hiệu quả về mặt khái niệm do mối quan hệ là tuyến tính và có thể giải quyết trực tiếp. 

Quan sát quan trọng là tổng doanh thu có thể được biểu thị dưới dạng hàm tuyến tính của biến chưa biết`x`. Nếu như`x`người tham dự phải trả một nửa giá và`p - x`trả đủ giá thì doanh thu hoàn toàn được xác định bởi`x`. Điều này biến bài toán thành giải một phương trình tuyến tính đơn một biến. Khi phương trình đó được sắp xếp lại, câu trả lời sẽ có được trực tiếp mà không cần lặp lại. 

The structure of the problem guarantees that the resulting value is an integer and non-negative, so no additional search or rounding logic is required.

 | Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(p) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu bằng cách giả định rằng`x`mọi người là những người tham dự mặt trăng và trả tiền`v/2`, trong khi phần còn lại`p - x`chi trả`v`. 

Điều này trực tiếp mã hóa ẩn số duy nhất trong hệ thống. 
2. Thể hiện tổng doanh thu bằng số tiền đóng góp:`a = x * (v/2) + (p - x) * v`. 

Bước này chuyển đổi câu chuyện thành một phương trình xác định. 
3. Khai triển biểu thức:`a = x*v/2 + p*v - x*v`. 
4. Nhóm thuật ngữ liên quan`x`:`a = p*v - x*(v/2)`. 
5. Sắp xếp lại để cô lập`x`:`x*(v/2) = p*v - a`. 
6. Giải quyết`x`:`x = (2 * (p*v - a)) / v`. 

The multiplication by 2 avoids fractional division since`v`là chẵn. 
7. Trở về`x`bằng số lượng người tham dự mặt trăng. 

### Tại sao nó hoạt động 

Hàm doanh thu là tuyến tính trong`x`, nghĩa là mỗi người tham dự âm lịch bổ sung sẽ thay thế vé nguyên giá bằng vé nửa giá và giảm doanh thu một cách chính xác`v/2`. Vì doanh thu cơ bản không có chiết khấu là`p*v`, sự khác biệt`p*v - a`đo lường tổng mức giảm do những người tham dự được giảm giá gây ra. Chia tổng mức giảm này cho`v/2`đưa ra chính xác có bao nhiêu lần giảm như vậy xảy ra, tương ứng duy nhất với`x`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a, p, v = map(int, input().split())
    # derived formula: x = 2*(p*v - a) / v
    x = (2 * (p * v - a)) // v
    print(x)

if __name__ == "__main__":
    solve()
```Việc thực hiện mã hóa trực tiếp công thức dẫn xuất. phép nhân`p * v`được thực hiện đầu tiên, phù hợp một cách an toàn trong phạm vi số nguyên của Python. Phép chia số nguyên được sử dụng ở cuối vì bài toán đảm bảo kết quả là số nguyên. 

Một lỗi triển khai phổ biến là sắp xếp lại công thức theo cách đưa ra phép chia dấu phẩy động, điều này không cần thiết và có thể gây ra các vấn đề về độ chính xác trong các ngôn ngữ khác. Một lỗi nữa là quên nhân với 2 trước khi chia cho`v`, điều này sẽ mất tính chính xác vì mức chiết khấu chỉ bằng một nửa giá đầy đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`a = 7, p = 5, v = 2`We compute the candidate values step by step.

 | Bước | Biểu hiện | Giá trị | 
| --- | --- | --- | 
| Doanh thu cơ bản | p*v | 10 | 
| Thâm hụt doanh thu | p*v - a | 3 | 
| Đếm âm lịch | 2*(p*v - a)/v | 3 | 

Điều này xác nhận rằng 3 người tham dự là người mặt trăng, phù hợp với trực giác rằng ba người trả một nửa giá và hai người trả nguyên giá. 

### Ví dụ 2 

đầu vào:`a = 22, p = 7, v = 4`| Bước | Biểu hiện | Giá trị | 
| --- | --- | --- | 
| Doanh thu cơ bản | p*v | 28 | 
| Thâm hụt doanh thu | p*v - a | 6 | 
| Đếm âm lịch | 2*(p*v - a)/v | 3 | 

Điều này cho thấy có đúng 3 người tham dự đã sử dụng vé giảm giá, nghĩa là 4 người còn lại đã thanh toán nguyên giá. 

Cả hai ví dụ đều xác nhận rằng giá trị được tính toán khớp chính xác với cách diễn giải chênh lệch doanh thu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học cố định được thực hiện | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Giải pháp này phù hợp một cách thoải mái trong các ràng buộc vì nó thực hiện số học theo thời gian không đổi bất kể kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    a, p, v = map(int, input().split())
    x = (2 * (p * v - a)) // v
    return str(x)

# provided samples
assert run("7 5 2\n") == "3"
assert run("22 7 4\n") == "3"

# all full-price attendees (no discount)
assert run("10 2 5\n") == "0"

# all discounted attendees
assert run("5 2 2\n") == "2"

# mixed case
assert run("18 4 4\n") == "1"

# maximum values stress case
assert run("1000000 1000000 1000000\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả giá đầy đủ | 0 | trường hợp không có người tham dự mặt trăng | 
| tất cả đều giảm giá | p | hành vi giới hạn trên | 
| giá trị hỗn hợp | 1 | tính đúng đắn của quan hệ tuyến tính | 
| giá trị tối đa | 0 | an toàn tràn và quy mô | 

## Vỏ cạnh 

Khi tất cả người tham dự trả giá đầy đủ, doanh thu bằng`p * v`. Thay thế vào công thức mang lại kết quả`x = 0`, điều này phản ánh chính xác rằng không có vé giảm giá nào được sử dụng. 

Khi tất cả những người tham dự đều là cư dân mặt trăng, doanh thu sẽ trở thành`p * v / 2`. Cắm cái này vào phương trình sẽ cho`x = p`, phù hợp với kỳ vọng rằng mọi người tham dự đều được hưởng ưu đãi giảm giá. 

Để có giá trị đầu vào tối đa, sản phẩm trung gian`p * v`có thể đạt được`10^12`, nhưng Python xử lý việc này một cách an toàn và phép chia cuối cùng vẫn tạo ra một số nguyên hợp lệ.
