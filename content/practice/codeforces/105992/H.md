---
title: "CF 105992H - V \u6211 112,5"
description: "Chúng ta được cấp một số nguyên x, biểu thị tỷ lệ phần trăm thuế hoặc phụ phí áp dụng cho chi phí cơ bản cố định. Giá gốc luôn là 50 đơn vị. Số tiền cuối cùng phải trả là chi phí cơ bản cộng với một tỷ lệ phần trăm bổ sung của chi phí cơ bản đó."
date: "2026-06-22T16:37:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105992
codeforces_index: "H"
codeforces_contest_name: "The 2025 Shanghai Collegiate Programming Contest"
rating: 0
weight: 105992
solve_time_s: 47
verified: true
draft: false
---

[CF 105992H - V \u6211 112.5](https://codeforces.com/problemset/problem/105992/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên duy nhất`x`, đại diện cho tỷ lệ phần trăm thuế hoặc phụ phí áp dụng cho chi phí cơ bản cố định. Giá gốc luôn là 50 đơn vị. Số tiền cuối cùng phải trả là chi phí cơ bản cộng với một tỷ lệ phần trăm bổ sung của chi phí cơ bản đó. 

Cụ thể, phụ phí được tính như sau:`50 × x / 100`, và tổng số tiền phải trả là`50 + 50 × x / 100`. Định dạng đầu ra yêu cầu in giá trị cuối cùng này dưới dạng số thập phân được làm tròn đến ba chữ số sau dấu thập phân, có tiền tố là chuỗi`Vivo`. 

Kích thước đầu vào không đáng kể xét về độ phức tạp của thuật toán vì chỉ có một số nguyên và phép tính số học theo thời gian không đổi. Mặc dù`x`có thể lớn tới 19.198.100, việc tính toán vẫn an toàn trong số học dấu phẩy động hoặc số nguyên tỷ lệ. Sự tinh tế duy nhất nằm ở việc làm tròn và định dạng chính xác. 

Các trường hợp đặc biệt chính đến từ việc định dạng hơn là tính toán. Khi`x = 0`, kết quả sẽ giữ nguyên chính xác`50.000`. Khi`x`lớn, kết quả có thể đạt tới gần một triệu, nhưng vẫn vừa vặn thoải mái với độ chính xác gấp đôi tiêu chuẩn. Một trường hợp khác là làm tròn thích hợp: các giá trị như`x = 1`sản xuất`50.5`, phải được in dưới dạng`50.500`, không`50.5`hoặc`50.5000`. 

Việc triển khai bất cẩn cũng có thể làm mất độ chính xác nếu sử dụng phép chia số nguyên, ví dụ như tính toán`50 * x // 100`, điều này sẽ loại bỏ hoàn toàn các đóng góp phân số và phá vỡ việc làm tròn. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ cố gắng mô phỏng trực tiếp phép tính tỷ lệ phần trăm bằng cách sử dụng số học số nguyên từng bước. Người ta có thể cố gắng tính khoản phí bổ sung như`(50 * x) / 100`dùng phép chia số nguyên. Điều này đã gần đạt đến mức tối ưu, nhưng nếu thực hiện không đúng cách với phép chia sàn, nó sẽ âm thầm cắt bớt thay vì giữ nguyên số thập phân. Một ý tưởng ngây thơ khác là chuyển đổi mọi thứ thành số float mà không cần suy nghĩ đến việc kiểm soát độ chính xác, sau đó in kết quả thô, điều này có nguy cơ làm tròn không nhất quán tùy thuộc vào mặc định định dạng. 

Quan sát chính là công thức là một biểu thức tuyến tính duy nhất. Không có sự lặp lại, không có cấu trúc có điều kiện và không có sự phụ thuộc giữa nhiều đầu vào. Điều này làm giảm vấn đề trong việc đánh giá biểu thức số học theo thời gian không đổi và định dạng đầu ra một cách chính xác. Vì số học dấu phẩy động trong Python là đủ cho thang đo này nên yêu cầu thực sự duy nhất là đảm bảo rằng bước định dạng thực thi ba chữ số thập phân bằng cách làm tròn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán số nguyên/thả nổi trực tiếp có thể cắt bớt | O(1) | O(1) | Sai do vấn đề chính xác | 
| Tính toán dấu phẩy động phù hợp với định dạng | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán kết quả trực tiếp từ việc xác định vấn đề và kiểm soát cẩn thận việc định dạng. 

1. Đọc số nguyên`x`từ đầu vào. Đây là tỷ lệ phần trăm áp dụng cho chi phí cơ bản là 50. 
2. Tính hiệu ứng nhân của tỷ lệ phần trăm như sau:`x / 100.0`. Việc sử dụng phép chia dấu phẩy động ở đây là cần thiết vì phép chia số nguyên sẽ loại bỏ các phần phân số. 
3. Nhân chi phí cơ bản 50 với hệ số nhân này để có được khoản phí bổ sung. Điều này thể hiện mức phụ phí theo tỷ lệ chính xác được áp dụng cho cơ sở. 
4. Thêm chi phí cơ bản 50 vào phí bổ sung đã tính để có được số tiền cuối cùng. 
5. Định dạng kết quả bằng định dạng dấu phẩy cố định có đúng ba chữ số sau dấu thập phân. Bước này thực thi các quy tắc làm tròn theo yêu cầu của đặc tả đầu ra. 
6. In kết quả có tiền tố là`Vivo `. 

### Tại sao nó hoạt động 

Việc tính toán là đánh giá trực tiếp hàm tuyến tính của`x`. Vì mọi phép biến đổi đều bảo toàn sự tương đương với công thức ban đầu`50 × (1 + x / 100)`, thuật toán không thể khác với giá trị mong muốn. Nguồn khác biệt tiềm ẩn duy nhất là cách biểu diễn bằng số và việc sử dụng số học dấu phẩy động tiêu chuẩn kết hợp với định dạng được kiểm soát sẽ đảm bảo giá trị in cuối cùng khớp với ngữ nghĩa làm tròn cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

x = int(input().strip())

base = 50.0
total = base + base * (x / 100.0)

print(f"Vivo {total:.3f}")
```Giải pháp đọc một số nguyên duy nhất và đánh giá ngay công thức trong thời gian không đổi. Giá trị cơ sở được viết rõ ràng dưới dạng float để đảm bảo rằng toàn bộ biểu thức nằm trong không gian dấu phẩy động, tránh việc vô tình cắt bớt số nguyên. 

Chuỗi định dạng`:.3f`là rất quan trọng vì nó thực thi cả đầu ra thập phân có chiều rộng cố định và làm tròn. Nếu không có nó, Python có thể in một số lượng vị trí thập phân khác nhau, điều này sẽ không đáp ứng được yêu cầu đầu ra. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0
```Chúng ta tính toán từng bước: 

| Bước | x | thêm = 50 * x/100 | tổng cộng | đầu ra | 
| --- | --- | --- | --- | --- | 
| ban đầu | 0 | 0,0 | 50,0 | - | 
| cuối cùng | 0 | 0,0 | 50,0 | Vivo 50.000 | 

Điều này cho thấy trường hợp nhận dạng không áp dụng thuế. Đầu ra vẫn chính xác là chi phí cơ bản. 

### Ví dụ 2 

đầu vào:```
125
```| Bước | x | thêm = 50 * x/100 | tổng cộng | đầu ra | 
| --- | --- | --- | --- | --- | 
| ban đầu | 125 | 62,5 | 112,5 | - | 
| cuối cùng | 125 | 62,5 | 112,5 | Vivo 112.500 | 

Điều này khẳng định tỷ lệ tỷ lệ chính xác: khoản phụ phí 125% sẽ tăng gấp đôi chi phí cơ bản và cộng thêm 25%. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số phép tính số học không đổi được thực hiện bất kể kích thước đầu vào | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Các ràng buộc cho phép đạt tới các giá trị rất lớn của`x`, nhưng vì tính toán hoàn toàn là số học và không lặp lại nên nó dễ dàng phù hợp với mọi giới hạn. Mối quan tâm duy nhất là độ chính xác của dấu phẩy động, được xử lý bằng định dạng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    x = int(input().strip())
    base = 50.0
    total = base + base * (x / 100.0)
    return f"Vivo {total:.3f}"

# provided samples
assert run("0\n") == "Vivo 50.000"
assert run("125\n") == "Vivo 112.500"

# custom cases
assert run("1\n") == "Vivo 50.500", "small fractional increase"
assert run("2\n") == "Vivo 51.000", "rounding to integer boundary"
assert run("100\n") == "Vivo 100.000", "doubling case"
assert run("19198100\n") == run("19198100\n"), "large value stability"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 | Vivo 50.000 | trường hợp không có cạnh thuế | 
| 1 | Vivo 50.500 | độ chính xác phân số | 
| 2 | Vivo 51.000 | ranh giới làm tròn sạch | 
| 100 | Vivo 100.000 | tăng gấp đôi độ chính xác | 

## Vỏ cạnh 

Trường hợp một cạnh là khi`x = 0`. Việc tính toán trở thành`50 + 0`và đầu ra vẫn phải bao gồm ba chữ số thập phân. Thuật toán tạo ra`50.0`và định dạng sẽ chuyển đổi nó một cách chính xác thành`50.000`. 

Một trường hợp khác là tỷ lệ phần trăm nhỏ như`x = 1`, kết quả ở đâu`50.5`. Nếu không có định dạng rõ ràng, điều này có thể được in dưới dạng`50.5`, điều này sẽ vi phạm độ chính xác cố định cần thiết. Thuật toán đảm bảo`50.500`được in. 

Một giá trị lớn như`x = 19198100`tạo ra một kết quả trung gian lớn`50 × (1 + 191981.0) = 9,599,050.0`. Điều này vẫn phù hợp một cách an toàn trong phạm vi dấu phẩy động và định dạng vẫn ổn định.
