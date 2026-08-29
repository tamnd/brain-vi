---
title: "CF 104380J - Số 7"
description: "Chúng ta được cấp một số nguyên dương x và chúng ta cần phải di chuyển xuống dưới để tìm số nguyên nhỏ hơn gần nhất mà tránh được giới hạn chữ số cụ thể: không có chữ số thập phân nào của nó có thể là 7."
date: "2026-07-01T17:08:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104380
codeforces_index: "J"
codeforces_contest_name: "The Andover Computing Open (TACO) 2023"
rating: 0
weight: 104380
solve_time_s: 62
verified: true
draft: false
---

[CF 104380J - Số 7](https://codeforces.com/problemset/problem/104380/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên dương duy nhất`x`và chúng ta cần phải di chuyển xuống dưới để tìm số nguyên nhỏ hơn gần nhất mà tránh được ràng buộc về chữ số cụ thể: không có chữ số thập phân nào của nó có thể là`7`. Trong số tất cả các số nguyên nhỏ hơn`x`, chúng ta muốn cái lớn nhất thỏa mãn hạn chế này. 

Về cơ bản, nhiệm vụ này là tìm kiếm các số nguyên nằm dưới một giới hạn nhất định, nhưng với bộ lọc loại trừ bất kỳ số nào chứa chữ số đó.`7`. Đầu ra không chỉ là bất kỳ số hợp lệ nào mà còn là số hợp lệ tối đa theo ràng buộc, điều này khiến đây trở thành vấn đề “tiền thân hợp lệ gần nhất”. 

Ràng buộc`x ≤ 10^4`nghĩa là không gian tìm kiếm rất nhỏ. Ngay cả việc quét tuyến tính từ`x - 1`xuống`1`thực hiện tối đa mười nghìn lần kiểm tra và mỗi lần kiểm tra chỉ kiểm tra một vài chữ số. Điều này ngay lập tức loại trừ sự cần thiết của bất kỳ DP chữ số phức tạp nào hoặc việc tái thiết tham lam; ngay cả sự lặp lại ngây thơ cũng phù hợp một cách thoải mái trong giới hạn thời gian. 

Trường hợp cạnh xuất hiện khi`x`bản thân nó nằm ngay phía trên vùng cấm. Ví dụ, nếu`x = 7000`, thì những con số như`6999`,`6998`, v.v. phải được kiểm tra cẩn thận cho đến khi xuất hiện một giá trị hợp lệ. Một sai lầm bất cẩn là giảm một lần và cho rằng kết quả là hợp lệ, điều này sẽ thất bại ngay lập tức đối với các đầu vào như`x = 70`, Ở đâu`69`là hợp lệ nhưng`69`không phải lúc nào cũng được tìm thấy nếu chỉ thực hiện một bước duy nhất. Một trường hợp tinh tế khác là khi câu trả lời vượt qua ranh giới nhiều chữ số, chẳng hạn như`x = 100`, đáp án ở đâu`99`, không`99`hoặc một cái gì đó liên quan đến logic thay thế chữ số. 

## Phương pháp tiếp cận 

Chiến lược trực tiếp nhất là bắt đầu từ`x - 1`và giảm liên tục cho đến khi tìm được số có biểu diễn thập phân không chứa chữ số`7`. Mỗi ứng cử viên được kiểm tra bằng cách chuyển đổi nó thành một chuỗi và quét các ký tự của nó. Điều này đúng vì chúng tôi khám phá tất cả các số nguyên theo thứ tự giảm dần, đảm bảo rằng số hợp lệ đầu tiên gặp phải là số hợp lệ tối đa bên dưới`x`. 

Mối lo ngại về tính kém hiệu quả chỉ phát sinh nếu chữ số bị cấm gây ra chuỗi dài các số không hợp lệ. Trong trường hợp xấu nhất, chúng ta có thể bỏ qua những con số như`70, 71, 72, ..., 79`hoặc các mẫu lớn hơn như`7000`đi xuống, nhưng ngay cả khi đó tổng số lần kiểm tra vẫn bị giới hạn bởi`10^4`. Mỗi lần kiểm tra tốn O(log x) nên tổng công việc không đáng kể. 

Quan sát quan trọng là không có ràng buộc về cấu trúc liên kết các chữ số ngoại trừ việc loại trừ`7`. Điều đó loại bỏ mọi nhu cầu về lý luận tổ hợp trên các chữ số. Vòng lặp giảm và kiểm tra tham lam nắm bắt hoàn toàn không gian giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (giảm + kiểm tra) | O(x · log x) | O(1) | Đã chấp nhận | 
| Tối ưu (giống như lý luận thô bạo, đơn giản hóa) | O(x · log x) | O(1) | Đã chấp nhận | 

Trong bài toán này, giải pháp “tối ưu” về cơ bản là giải pháp mạnh mẽ được chứng minh bằng các ràng buộc. 

## Hướng dẫn thuật toán 

1. Bắt đầu từ số nguyên ngay bên dưới`x`. Đây là ứng cử viên lớn nhất có thể và đảm bảo chúng tôi tìm kiếm theo đúng thứ tự. 
2. Với mỗi số ứng viên, hãy kiểm tra xem nó có chứa chữ số không`7`bằng cách chuyển đổi nó thành một chuỗi và quét tất cả các ký tự. Bước này thực thi ràng buộc trực tiếp mà không cần trích xuất chữ số số học. 
3. Nếu số đó không chứa`7`, trả lại ngay. Vì chúng ta đang quét xuống nên số hợp lệ đầu tiên được đảm bảo là câu trả lời hợp lệ lớn nhất. 
4. Nếu không, hãy giảm ứng viên đi một và lặp lại việc kiểm tra. 

Vòng lặp tiếp tục cho đến khi tìm thấy số hợp lệ. Vì miền được giới hạn bởi`x ≤ 10^4`, việc chấm dứt được đảm bảo. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến là mọi số lớn hơn ứng cử viên hiện tại và nhỏ hơn`x`đã được xem xét và bác bỏ. Vì chúng ta chỉ di chuyển xuống dưới và không bao giờ bỏ qua các giá trị nên số đầu tiên vượt qua bộ lọc chữ số phải là số hợp lệ lớn nhất trong`x`. Không có khả năng bỏ lỡ câu trả lời tốt hơn vì tất cả các ứng viên lớn hơn đều được kiểm tra sớm hơn trong chuỗi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def has_seven(n: int) -> bool:
    return '7' in str(n)

def solve():
    x = int(input().strip())
    cur = x - 1
    
    while cur > 0:
        if not has_seven(cur):
            print(cur)
            return
        cur -= 1
    
    print(0)

if __name__ == "__main__":
    solve()
```chức năng`has_seven`tách biệt logic kiểm tra chữ số, làm cho vòng lặp chính rõ ràng hơn và ít xảy ra lỗi hơn. Vòng lặp bắt đầu lúc`x - 1`bởi vì vấn đề rõ ràng yêu cầu một số lượng nhỏ hơn. 

Một điểm tinh tế là xử lý trường hợp không có số hợp lệ nào tồn tại trên 0. Từ`1 ≤ x ≤ 10^4`, Và`0`không chứa một`7`, vòng lặp sẽ luôn kết thúc một cách an toàn, nhưng bộ bảo vệ đảm bảo tính đúng đắn ngay cả trong lý luận suy biến. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`8`| cur | chứa '7'? | hành động | 
| --- | --- | --- | 
| 7 | vâng | bỏ qua | 
| 6 | không | đầu ra | 

Thuật toán bỏ qua`7`bởi vì nó vi phạm ràng buộc và ngay lập tức trả về`6`. Điều này xác nhận rằng quá trình quét sẽ bỏ qua các giá trị một chữ số không hợp lệ một cách chính xác. 

### Ví dụ 2 

đầu vào:`777`| cur | chứa '7'? | hành động | 
| --- | --- | --- | 
| 776 | vâng | bỏ qua | 
| 775 | vâng | bỏ qua | 
| ... | ... | tiếp tục | 
| 700 | vâng | bỏ qua | 
| 699 | không | đầu ra | 

Dấu vết này hiển thị một chuỗi dài các ứng cử viên không hợp lệ do chữ số lặp lại`7`. Thuật toán vẫn tìm ra câu trả lời đúng vì nó kiểm tra một cách có hệ thống mọi số từ dưới lên mà không cần giả định về cấu trúc chữ số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(x · log x) | Ở mức giảm tối đa x, mỗi lần kiểm tra chữ số có giá O(log x) do chuyển đổi chuỗi | 
| Không gian | O(1) | Chỉ một vài số nguyên và biểu diễn chuỗi tạm thời | 

Giới hạn hạn chế`x`ở mức 10.000, do đó số lần lặp trong trường hợp xấu nhất là rất nhỏ. Ngay cả một vòng lặp đơn giản cũng có thể thực hiện thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    x = int(sys.stdin.readline().strip())
    cur = x - 1
    
    while cur > 0:
        if '7' not in str(cur):
            print(cur)
            return
        cur -= 1
    
    print(0)

# provided samples
assert run("8\n") == "6"
assert run("777\n") == "699"

# custom cases
assert run("7\n") == "6", "single forbidden number"
assert run("10\n") == "9", "simple boundary"
assert run("100\n") == "99", "two-digit collapse"
assert run("71\n") == "69", "skipping across forbidden digit"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 7 | 6 | ranh giới giá trị cấm duy nhất | 
| 10 | 9 | trường hợp giảm đơn giản | 
| 100 | 99 | chuyển tiếp ranh giới chữ số | 
| 71 | 69 | bỏ qua phạm vi chứa chữ số bị cấm | 

## Vỏ cạnh 

Đối với đầu vào`x = 7`, bộ thuật toán`cur = 6`. Séc`'7' in str(6)`là sai ngay lập tức, vì vậy nó xuất ra`6`. Điều này xác nhận việc xử lý đúng khi chữ số bị cấm nằm chính xác ở ranh giới. 

Đối với đầu vào`x = 70`, thuật toán bắt đầu tại`69`. Từ`69`không chứa`7`, nó được trả về ngay lập tức. Trình tự không bao giờ đánh giá`70`chính nó vì sự bất bình đẳng nghiêm ngặt được thực thi khi khởi tạo. 

Đối với đầu vào`x = 100`, thuật toán đánh giá`99`trực tiếp. Vì không có chữ số nào`7`, đầu ra là`99`, thể hiện việc xử lý chính xác các chuyển đổi chữ số giống như mang mà không cần suy luận số rõ ràng.
