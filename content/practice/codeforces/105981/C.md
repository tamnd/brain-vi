---
title: "CF 105981C - Tiếng vọng của cổ ngữ"
description: "Chúng ta được cho một mảng các số nguyên và được hỏi một câu hỏi rất cụ thể về thứ tự của nó: liệu nó có thể được chuyển đổi thành một dãy không giảm bằng cách sử dụng nhiều nhất một lần hoán đổi hai phần tử hay không."
date: "2026-06-21T21:44:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105981
codeforces_index: "C"
codeforces_contest_name: "The 2025 Hunan University Programming Contest"
rating: 0
weight: 105981
solve_time_s: 230
verified: true
draft: false
---

[CF 105981C - Tiếng vọng của cổ ngữ](https://codeforces.com/problemset/problem/105981/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên và được hỏi một câu hỏi rất cụ thể về thứ tự của nó: liệu nó có thể được chuyển đổi thành một dãy không giảm bằng cách sử dụng nhiều nhất một lần hoán đổi hai phần tử hay không. Cho phép hoán đổi giữa hai vị trí bất kỳ, không nhất thiết phải liền kề và chúng ta được phép chọn không hoán đổi chút nào nếu mảng đã được sắp xếp. 

Đầu vào đại diện cho một chuỗi đơn và đầu ra là một quyết định nhị phân. Chúng tôi xác nhận rằng mảng có thể được sắp xếp bằng 0 hoặc một lần hoán đổi hoặc chúng tôi từ chối nó. 

Ràng buộc n ≤ 1000 đủ nhỏ để ngay cả lý luận O(n²) cũng có thể vượt qua một cách thoải mái. Điều đó đã gợi ý rằng chúng tôi không cần phải mô phỏng các phép biến đổi phức tạp hoặc thực hiện tối ưu hóa nhiều lớp. Cấu trúc của nhiệm vụ là kiểm tra tính nhất quán toàn cục hơn là xử lý tăng dần. 

Một trường hợp tinh tế xuất hiện khi tồn tại sự trùng lặp. Ví dụ: trong một mảng như`[1, 3, 2, 2, 3]`, có thể tồn tại nhiều giao dịch hoán đổi chính xác, nhưng chỉ có một giao dịch hoán đổi cụ thể là quan trọng. Một cách tiếp cận bất cẩn chỉ kiểm tra “số lần đảo ngược” sẽ không thành công, vì phép đảo ngược không mã hóa trực tiếp liệu một lần hoán đổi duy nhất có thể khắc phục được thứ tự toàn cầu hay không. Tiêu chí chính xác phụ thuộc vào sự không khớp chính xác về vị trí với phiên bản được sắp xếp. 

Một trường hợp thất bại khác là giả định rằng nếu có ít sự đảo ngược thì việc hoán đổi luôn khắc phục được chúng. Ví dụ,`[3, 1, 2]`có nhiều lần đảo ngược, nhưng không có sự hoán đổi đơn lẻ nào khiến nó được sắp xếp. 

Khó khăn thực sự không phải là tính toán mà là việc xác định cấu trúc nào mà “một phép hoán đổi khỏi được sắp xếp” áp đặt lên hoán vị. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi cặp chỉ số i và j có thể có, hoán đổi chúng và kiểm tra xem mảng kết quả có được sắp xếp hay không. Điều này đúng vì nó trực tiếp theo sau phát biểu vấn đề. Tuy nhiên, mỗi lần kiểm tra đều tốn O(n) và có O(n2) giao dịch hoán đổi, tạo ra tổng thời gian là O(n³). Với n = 1000, con số này đạt khoảng 10⁹ thao tác, nằm ở mức giới hạn nhưng vẫn quá chậm trong Python. 

Chúng tôi cải thiện điều này bằng cách tránh mô phỏng lặp đi lặp lại. Quan sát quan trọng là mảng được sắp xếp khác với mảng ban đầu chỉ ở các vị trí mà các phần tử bị đặt sai vị trí so với thứ tự được sắp xếp. Nếu chúng ta sắp xếp mảng và so sánh từng vị trí với mảng ban đầu, tất cả các vị trí đúng đều khớp nhau và những vị trí không chính xác sẽ đánh dấu chính xác vị trí phá vỡ cấu trúc. 

Nếu có nhiều hơn hai vị trí khác nhau, một lần hoán đổi không thể khắc phục được tất cả các điểm không khớp vì một lần hoán đổi chỉ có thể sửa chính xác hai vị trí. Nếu chính xác hai vị trí khác nhau, chúng ta có thể kiểm tra xem việc hoán đổi hai phần tử đó trong mảng ban đầu có tạo ra mảng được sắp xếp hay không. 

Điều này làm giảm vấn đề xuống còn một so sánh duy nhất với phiên bản đã sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng trao đổi vũ lực | O(n³) | O(1) | Quá chậm | 
| So sánh với mảng đã sắp xếp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách so sánh mảng ban đầu với dạng đã sắp xếp của nó và lý giải về cấu trúc không khớp. 

1. Tạo một bản sao đã được sắp xếp của mảng. Điều này thể hiện cấu hình mục tiêu mà chúng tôi muốn tiếp cận. Chi phí sắp xếp O(n log n), đủ nhanh. 
2. So sánh mảng ban đầu và chỉ mục mảng được sắp xếp theo chỉ mục và thu thập tất cả các chỉ mục ở những nơi chúng khác nhau. Các vị trí này thể hiện chính xác nơi thứ tự hiện tại không đồng ý với thứ tự được sắp xếp hợp lệ. 
3. Nếu không có vị trí nào không khớp thì mảng đã được sắp xếp nên không cần hoán đổi. 
4. Nếu có nhiều hơn hai vị trí không khớp thì không thể sửa mảng bằng một lần hoán đổi vì một lần hoán đổi có thể ảnh hưởng đến nhiều nhất hai vị trí trong một hoán vị. 
5. Nếu có chính xác hai vị trí i và j không khớp, hãy thử hoán đổi a[i] và a[j] trong mảng ban đầu và xác minh xem mảng đó có bằng với mảng đã sắp xếp hay không. Nếu đúng như vậy thì câu trả lời là tích cực, nếu không thì không thể khắc phục được bằng một lần hoán đổi. 

Tại sao nó hoạt động: một mảng được sắp xếp là một hoán vị đích cố định. Bất kỳ sự hoán đổi đơn lẻ nào cũng sửa đổi chính xác hai vị trí theo cách làm thay đổi giá trị của chúng. Do đó, tập hợp các giá trị không khớp phải trống hoặc bao gồm chính xác hai vị trí có thể được sửa đồng thời bằng cách hoán đổi hai phần tử đó. Bất kỳ tập hợp không khớp lớn hơn nào cũng không thể được giải quyết bằng một chuyển vị duy nhất, bởi vì một lần hoán đổi không thể sửa đồng thời nhiều hơn hai vị trí không chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    a = list(map(int, input().split()))
    
    b = sorted(a)
    
    diff = []
    for i in range(n):
        if a[i] != b[i]:
            diff.append(i)
    
    if len(diff) == 0:
        print("Sorted")
        return
    
    if len(diff) != 2:
        print("Failed")
        return
    
    i, j = diff
    a[i], a[j] = a[j], a[i]
    
    if a == b:
        print("Sorted")
    else:
        print("Failed")

if __name__ == "__main__":
    solve()
```Giải pháp xây dựng mục tiêu đã sắp xếp và sử dụng nó làm tài liệu tham khảo để so sánh cấu trúc. Danh sách khác biệt cô lập chính xác nơi hoán vị sai lệch. Thử nghiệm hoán đổi cuối cùng là cần thiết vì việc có chính xác hai điểm không khớp không đảm bảo rằng việc hoán đổi chúng sẽ khắc phục được thứ tự trừ khi giá trị của chúng căn chỉnh chính xác. 

## Ví dụ đã hoạt động 

Xem xét đầu vào`[1, 3, 2]`. Mảng được sắp xếp là`[1, 2, 3]`. Sự không khớp xảy ra ở vị trí 1 và 2. Hoán đổi chúng tạo ra`[1, 2, 3]`, vì vậy đầu ra là hợp lệ. 

| Bước | Mảng | Đã sắp xếp | Vị trí khác nhau | 
| --- | --- | --- | --- | 
| Ban đầu | 1 3 2 | 1 2 3 | [1, 2] | 
| Sau khi trao đổi | 1 2 3 | 1 2 3 | [] | 

Điều này xác nhận trường hợp một lần hoán đổi duy nhất sẽ sửa được hoán vị. 

Bây giờ hãy xem xét`[3, 1, 2]`. Mảng được sắp xếp là`[1, 2, 3]`. Tất cả các vị trí đều khác nhau, do đó kích thước khác biệt là 3, điều này ngay lập tức khiến điều đó không thể thực hiện được. 

| Bước | Mảng | Đã sắp xếp | Vị trí khác nhau | 
| --- | --- | --- | --- | 
| Ban đầu | 3 1 2 | 1 2 3 | [0, 1, 2] | 

Thuật toán từ chối mà không thử hoán đổi, phù hợp với giới hạn cấu trúc của một chuyển vị đơn lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; so sánh tuyến tính | 
| Không gian | O(n) | Lưu trữ cho bản sao được sắp xếp và danh sách khác biệt | 

Các ràng buộc n 1000 làm cho điều này trở nên tầm thường để thực thi trong giới hạn. Ngay cả việc sắp xếp lặp đi lặp lại giữa các trường hợp thử nghiệm sẽ vẫn hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# already sorted
assert run("5\n1 2 3 4 5\n") == "Sorted"

# one swap fixes
assert run("3\n1 3 2\n") == "Sorted"

# cannot fix
assert run("3\n3 1 2\n") == "Failed"

# duplicates case
assert run("5\n1 3 2 2 3\n") == "Sorted"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mảng đã được sắp xếp | Đã sắp xếp | trường hợp hoán đổi bằng không | 
| trao đổi chính xác duy nhất | Đã sắp xếp | chính xác là hai điểm không khớp | 
| trật tự hoàn toàn không nhất quán | Thất bại | nhiều hơn hai điểm không khớp | 
| mảng có bản sao | Đã sắp xếp | tính đúng đắn dưới các giá trị bằng nhau | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi mảng đã được sắp xếp. Đối với đầu vào`[1, 2, 3]`, danh sách khác biệt trống và thuật toán ngay lập tức chấp nhận mà không cần cố gắng hoán đổi, điều này tránh được sự đột biến không cần thiết và đảm bảo tính chính xác. 

Một trường hợp quan trọng khác là khi tồn tại chính xác hai điểm không khớp nhưng việc hoán đổi chúng không khắc phục được thứ tự do sai lệch giá trị. Ví dụ`[1, 4, 3, 2]`sắp xếp trở thành`[1, 2, 3, 4]`. Các chỉ số khác biệt là ba vị trí, do đó nó bị từ chối ngay lập tức, tránh nỗ lực sửa lỗi một phần gây hiểu lầm. 

Các bản sao không phá vỡ logic vì so sánh dựa trên giá trị thay vì dựa trên chỉ mục. Ngay cả khi tồn tại nhiều hoán vị được sắp xếp hợp lệ, mảng được sắp xếp vẫn cung cấp một mục tiêu chính tắc và khả năng phát hiện không khớp vẫn ổn định.
