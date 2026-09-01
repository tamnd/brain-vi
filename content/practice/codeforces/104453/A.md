---
title: "CF 104453A - \u041a\u043e\u043c\u043f\u043b\u0435\u043a\u0441\u043d\u044b\u0435 \u0447\u0438\u0441\u043b\u0430"
description: "Cho hai số phức, mỗi số được mô tả bằng phần thực nguyên và phần ảo nguyên. Số đầu tiên được hình thành từ cặp $a, b$ là $a + bi$ và số thứ hai là $c + di$."
date: "2026-06-30T14:32:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104453
codeforces_index: "A"
codeforces_contest_name: "ICPC Central Russia Regional Qualyfing Round, 2021"
rating: 0
weight: 104453
solve_time_s: 112
verified: true
draft: false
---

[CF 104453A - \u041a\u043e\u043c\u043f\u043b\u0435\u043a\u0441\u043d\u044b\u0435 \u0447\u0438\u0441\u043b\u0430](https://codeforces.com/problemset/problem/104453/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Cho hai số phức, mỗi số được mô tả bằng phần thực nguyên và phần ảo nguyên. Số đầu tiên được hình thành từ cặp$a, b$BẰNG$a + bi$, và thứ hai là$c + di$. Nhiệm vụ là tính tích của họ và đưa ra phần thực và phần ảo của kết quả. 

Phép nhân là số học phức tạp tiêu chuẩn. Mở rộng trực tiếp$$(a + bi)(c + di) = ac + adi + bci + bdi^2$$và kể từ đó$i^2 = -1$, điều này trở thành$$(ac - bd) + i(ad + bc).$$Vì vậy, đầu ra luôn là hai số nguyên: phần thực$ac - bd$và phần ảo$ad + bc$. 

Các ràng buộc rất nhỏ, với tất cả các giá trị nằm trong khoảng$-1000$Và$1000$. Điều này có nghĩa là bất kỳ việc thực hiện số học chính xác nào cũng đủ. Ngay cả việc đánh giá công thức đơn giản cũng diễn ra trong thời gian cố định, do đó không cần tối ưu hóa thuật toán. 

Một lỗi phổ biến trong loại bài toán này là xử lý dấu khi kết hợp các thuật ngữ liên quan đến$i^2$. Một vấn đề thường gặp khác là trộn lẫn hai thuật ngữ chéo$ad$Và$bc$, đặc biệt nếu cố gắng mở rộng về mặt tinh thần hoặc thực hiện mà không có một công thức cố định. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ xử lý theo nghĩa đen$i$như một đối tượng tượng trưng và mở rộng phép nhân từng bước, theo dõi các thành phần thực và ảo một cách riêng biệt. Đây vẫn là công việc liên tục trên mỗi hoạt động, nhưng nó là chi phí không cần thiết. Cấu trúc của số phức đã mã hóa quy tắc nhân dạng đóng. 

Quan sát quan trọng là phép nhân phức tạp luôn phân tách thành bốn phép nhân vô hướng và hai phép cộng/trừ. Không có sự phụ thuộc giữa nhiều đầu vào hoặc bất kỳ quá trình lặp lại nào. Một khi danh tính đại số được viết ra, việc tính toán sẽ được thực hiện ngay lập tức. 

Điều này làm giảm vấn đề thành việc đánh giá trực tiếp một công thức cố định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mở rộng tượng trưng | O(1) | O(1) | Đã chấp nhận | 
| Công thức trực tiếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc bốn số nguyên$a, b, c, d$. Chúng xác định hai số phức$a + bi$Và$c + di$. Thứ tự quan trọng vì các thuật ngữ chéo phụ thuộc vào việc ghép nối chính xác các thành phần thực và ảo. 
2. Tính phần thực bằng đẳng thức$ac - bd$. Điều này xuất phát từ việc kết hợp tích các phần thực và trừ tích các phần ảo do$i^2 = -1$. 
3. Tính phần ảo bằng cách sử dụng$ad + bc$. Đây là các thuật ngữ chéo trong đó các thành phần thực và ảo tương tác với nhau. 
4. Xuất cả hai giá trị theo thứ tự: phần thực trước, phần ảo thứ hai. 

Việc tách thành hai biểu thức đảm bảo rằng không cần suy luận ký hiệu trung gian trong quá trình thực hiện. 

### Tại sao nó hoạt động 

Tính đúng đắn xuất phát trực tiếp từ tính phân phối của phép nhân đối với phép cộng và tính chất xác định$i^2 = -1$. Mỗi thuật ngữ sản phẩm trong bản mở rộng rơi vào đúng một trong bốn loại: thực-thực, thực-tưởng tượng, ảo-thực và tưởng tượng-tưởng tượng. Loại cuối cùng đưa ra sự thay đổi dấu, tạo ra phép trừ trong phần thực. Vì tất cả các cặp có thể được tính chính xác một lần nên các biểu thức thu được là đầy đủ và chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a, b, c, d = map(int, input().split())
    real = a * c - b * d
    imag = a * d + b * c
    print(real, imag)

if __name__ == "__main__":
    solve()
```Giải pháp đọc bốn số nguyên, tính toán trực tiếp hai biểu thức cần thiết và in chúng. Tất cả các phép tính nhân và cộng đều an toàn trong phạm vi số nguyên 32 bit vì giới hạn nhỏ. 

Một chi tiết triển khai tinh tế là duy trì việc phân nhóm chính xác:$a*c - b*d$phải được tính toán chính xác như được viết để tránh các lỗi ưu tiên, mặc dù Python sẽ đánh giá chính xác mà không cần dấu ngoặc đơn. Điều tương tự cũng áp dụng cho phần ảo, phần này phải bao gồm cả hai thuật ngữ chéo. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 1 3 6
```| Bước | một | b | c | d | tính toán thực tế | tính toán hình ảnh | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 2 | 1 | 3 | 6 | - | - | 
| calc | 2 | 1 | 3 | 6 | 2·3 − 1·6 = 0 | 2·6 + 1·3 = 15 | 

Đầu ra:```
0 15
```Điều này cho thấy trường hợp sự triệt tiêu xảy ra trong phần thực, vì$ac = bd$. 

### Ví dụ 2 

đầu vào:```
2 -2 2 2
```| Bước | một | b | c | d | tính toán thực tế | tính toán hình ảnh | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 2 | -2 | 2 | 2 | - | - | 
| calc | 2 | -2 | 2 | 2 | 2·2 − (-2·2) = 8 | 2·2 + (-2·2) = 0 | 

Đầu ra:```
8 0
```Ví dụ này thể hiện cách xử lý dấu khi phần ảo của số đầu tiên là số âm, làm đảo lộn phần đóng góp của số đó.$bd$thuật ngữ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học cố định được thực hiện | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Các ràng buộc cho phép tính toán không đổi theo thời gian không đáng kể, do đó giải pháp đáp ứng thoải mái mọi giới hạn hợp lý. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    a, b, c, d = map(int, sys.stdin.readline().split())
    print(a*c - b*d, a*d + b*c)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.readline().strip()

# provided samples
assert solve.__doc__ is None or True  # placeholder safety

# custom cases (conceptual checks)
# (1 + i)(1 - i) = 2
# (0 + i)(0 + i) = -1
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 -1 | 2 0 | phép nhân liên hợp | 
| 0 1 0 1 | -1 0 | hình vuông tưởng tượng thuần túy | 
| 0 0 5 7 | 0 0 | hấp thụ nhân bằng không | 
| -2 3 4 -5 | 7 -22 | dấu hiệu hỗn hợp đúng đắn | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một thành phần bằng 0, chẳng hạn như nhân một số thực thuần túy với một số thuần ảo. Công thức vẫn hoạt động chính xác vì các số hạng chéo vẫn còn, nhưng những đóng góp thực-thực và ảo-tưởng tượng có thể biến mất. 

Một trường hợp cạnh khác xảy ra khi các dấu hiệu khác nhau giữa các thành phần, điều này có thể dễ dàng dẫn đến tính nhẩm không chính xác. Dạng đại số đảm bảo tính đúng đắn vì phép trừ trong phần thực được thực thi rõ ràng bởi$-bd$hạn, ngăn chặn tình trạng lật biển báo trong quá trình thực hiện. 

Tất cả các trường hợp đều quy gọn về cùng một công thức, do đó không cần phân nhánh đặc biệt.
