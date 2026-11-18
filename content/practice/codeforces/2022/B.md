---
title: "CF 2022B - Nhân Viên Bán Hàng Kar"
description: "Mỗi trường hợp thử nghiệm mô tả một đại lý có nhiều mẫu ô tô, trong đó mẫu i có những chiếc ô tô giống hệt nhau và tất cả đều phải được bán."
date: "2026-06-08T12:36:17+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2022
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 978 (Div. 2)"
rating: 1300
weight: 2022
solve_time_s: 113
verified: true
draft: false
---

[CF 2022B - Nhân viên bán hàng Kar](https://codeforces.com/problemset/problem/2022/B) 

**Đánh giá:** 1300 
**Tags:** tìm kiếm nhị phân, tham lam, toán học 
**Thời gian giải:** 1m 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm mô tả một đại lý với một số mẫu xe, trong đó mẫu`i`có`a_i`những chiếc xe giống hệt nhau phải bán đi. Khách hàng có thể bị thuyết phục mua tới`x`ô tô, nhưng chỉ khi tất cả ô tô được chọn đều có mẫu mã khác nhau, nghĩa là một khách hàng không thể lấy hai ô tô cùng loại. 

Chúng tôi muốn lên lịch cho khách hàng, trong đó mỗi khách hàng có thể chọn tối đa`x`các mô hình khác nhau và loại bỏ một ô tô khỏi mỗi mô hình đã chọn. Mục tiêu là giảm tất cả`a_i`về 0 đồng thời giảm thiểu số lượng khách hàng. 

Ràng buộc`x ≤ 10`tuy nhỏ nhưng số lượng mẫu mã`n`có thể lớn tới`5 · 10^5`, do đó, bất kỳ giải pháp nào cố gắng mô phỏng từng khách hàng một hoặc quét nhiều lần tất cả các mô hình cho mỗi khách hàng sẽ quá chậm. Tổng số tiền của`n`qua các bài kiểm tra cũng đạt`5 · 10^5`, về cơ bản thực thi hành vi tuyến tính hoặc gần tuyến tính cho mỗi lần kiểm tra. 

Một trường hợp phức tạp phát sinh khi một mô hình lấn át tất cả các mô hình khác. Ví dụ, nếu một`a_i`là cực kỳ lớn trong khi số khác lại nhỏ, yếu tố hạn chế là mỗi khách hàng có thể ghép bao nhiêu mẫu xe khác nhau chứ không phải tổng số xe. Một tình huống phức tạp khác là khi số lượng rất cân bằng, trong đó trực giác tham lam luôn chọn số lớn nhất.`x`các nhóm vẫn có thể thất bại trừ khi được chính thức hóa một cách chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận ngây thơ sẽ mô phỏng khách hàng một cách rõ ràng. Mỗi lần, chúng tôi sẽ đón`x`mô hình với những chiếc xe còn lại và giảm chúng. Điều này hoạt động về mặt khái niệm vì nó tuân theo chính xác các quy tắc, nhưng mỗi khách hàng yêu cầu chọn tối đa`x`mô hình từ lên đến`n`, dẫn đến ít nhất`O(n)`làm việc cho mỗi khách hàng. Vì số lượng khách hàng cũng có thể theo thứ tự tối đa`a_i`, phương pháp này trở nên không khả thi. 

Quan sát quan trọng là quá trình này tương đương với việc phân phối`sum(a_i)`đơn vị thành các nhóm có kích thước tối đa`x`, trong đó không nhóm nào có thể lấy nhiều hơn một đơn vị từ mỗi chỉ số trong mỗi vòng. Nút thắt thực sự không phải là tổng số tiền mà là số lần một mô hình phải được phục vụ. Nếu một mô hình có`a_i`ô tô, nó phải xuất hiện ít nhất`a_i`khách hàng khác nhau, nhưng mỗi khách hàng có thể đáp ứng tối đa`x`các mô hình. Điều này tạo ra hạn chế về đóng gói: mỗi khách hàng giảm tối đa`x`tổng số đơn vị còn lại trên các chỉ số khác nhau. 

Một cách hữu ích để xem quá trình này là xem xét cần bao nhiêu “lớp” loại bỏ nếu chúng ta liên tục thực hiện nhiều nhất`x`mô hình hoạt động riêng biệt trên mỗi lớp. Mỗi lớp giảm tối đa`x`tổng số ô tô còn lại, vì vậy giới hạn dưới là`ceil(sum(a_i) / x)`. Tuy nhiên, điều này là chưa đủ, bởi vì một mô hình đơn lẻ có thể tạo ra nhiều lớp hơn mức đóng góp của nó vào tổng thể nếu nó quá tập trung. Ý tưởng đúng là chúng ta luôn cần ít nhất`max(a_i)`các lớp trong đó mỗi lớp có thể chạm vào mô hình đó nhiều nhất một lần và ít nhất`ceil(sum(a_i) / x)`lớp do công suất. 

Do đó, câu trả lời tối ưu là mức tối đa của hai ràng buộc này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng | O(bước · n) | O(1) | Quá chậm | 
| Lý luận ràng buộc tối đa | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số ô tô của tất cả các mẫu xe. Điều này nắm bắt khối lượng công việc tổng thể phải được xử lý. 
2. Tìm giá trị lớn nhất trong số tất cả`a_i`. Điều này nắm bắt được mô hình đơn lẻ có yêu cầu khắt khe nhất vì nó chỉ có thể đóng góp một lần cho mỗi khách hàng. 
3. Tính giới hạn dưới của tổng công suất như sau`(sum(a_i) + x - 1) // x`. Điều này phản ánh rằng mỗi khách hàng có thể loại bỏ tối đa`x`tổng số ô tô, bất kể phân phối. 
4. Lấy mức tối đa của hai giới hạn. Điều này đảm bảo cả năng lực toàn cầu và các ràng buộc trên mỗi mô hình đều được đáp ứng. 
5. Xuất giá trị này cho từng trường hợp thử nghiệm. 

### Tại sao nó hoạt động 

Mỗi khách hàng loại bỏ tối đa một ô tô khỏi bất kỳ mẫu xe nào, vì vậy một mẫu xe có`a_i`ô tô yêu cầu ít nhất`a_i`khách hàng riêng biệt. Điều này đưa ra ràng buộc rằng câu trả lời không thể nhỏ hơn`max(a_i)`. Đồng thời, mỗi khách hàng loại bỏ tối đa`x`tổng số ô tô, vì vậy loại bỏ`S = sum(a_i)`ô tô yêu cầu ít nhất`ceil(S / x)`khách hàng. 

Một lịch trình đạt được giới hạn này tồn tại bởi vì chúng ta luôn có thể phân công ô tô theo từng đợt một cách tham lam: tại mỗi bước hãy lấy tới`x`mô hình với những chiếc xe còn lại và loại bỏ một chiếc khỏi mỗi chiếc. Quá trình này không bao giờ bị kẹt miễn là ít nhất một trong hai ràng buộc chưa được thỏa mãn, vì tổng số ô tô còn lại hoặc chiều cao cột tối đa còn lại vẫn dương. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, x = map(int, input().split())
        a = list(map(int, input().split()))
        
        s = sum(a)
        mx = max(a)
        
        ans = max(mx, (s + x - 1) // x)
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp xử lý từng trường hợp thử nghiệm một cách độc lập. Nó đọc mảng, tính tổng và giá trị lớn nhất trong một lần duyệt và áp dụng công thức dẫn xuất. 

Chi tiết triển khai tinh tế duy nhất là sử dụng phép chia trần số nguyên cho`sum(a_i) / x`, được thực hiện như`(s + x - 1) // x`. Điều này tránh các lỗi dấu phẩy động và đảm bảo tính chính xác cho các giá trị lớn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
3 1 2
```Chúng tôi tính tổng số tiền`6`và tối đa`3`. Giới hạn công suất là`ceil(6 / 2) = 3`. 

| Bước | tổng hợp | tối đa | trần(tổng/x) | trả lời | 
| --- | --- | --- | --- | --- | 
| ban đầu | 6 | 3 | 3 | 3 | 

Kết quả là`3`, nghĩa là ba khách hàng là đủ và cần thiết. 

Điều này thể hiện một trường hợp cân bằng trong đó cả hai ràng buộc đều trùng nhau. 

### Ví dụ 2 

đầu vào:```
5 3
2 2 1 9 2
```Tổng là`16`, tối đa là`9`, giới hạn công suất là`ceil(16 / 3) = 6`. 

| Bước | tổng hợp | tối đa | trần(tổng/x) | trả lời | 
| --- | --- | --- | --- | --- | 
| ban đầu | 16 | 9 | 6 | 9 | 

Ở đây, hạn chế chủ yếu là mô hình lớn duy nhất, buộc phải có chín khách hàng riêng biệt. 

Điều này cho thấy tại sao chỉ lý luận dựa trên tổng hợp là không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | mỗi mảng được quét một lần để tính tổng và max | 
| Không gian | O(1) thêm | chỉ một số biến tích lũy được sử dụng | 

Tổng cộng`n`qua các bài kiểm tra nhiều nhất là`5 · 10^5`, do đó việc quét tuyến tính cho mỗi lần kiểm tra có thể dễ dàng đủ nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, x = map(int, input().split())
        a = list(map(int, input().split()))
        s = sum(a)
        mx = max(a)
        out.append(str(max(mx, (s + x - 1) // x)))
    return "\n".join(out)

# provided samples
assert run("""4
3 2
3 1 2
3 3
2 1 3
5 3
2 2 1 9 2
7 4
2 5 3 3 5 2 5
""") == """3
3
9
7"""

# custom cases
assert run("""1
1 10
5
""") == "5", "single model"

assert run("""1
3 2
100 100 100
""") == "150", "balanced large case"

assert run("""1
4 3
10 1 1 1
""") == "10", "dominant max constraint"

assert run("""1
5 5
1 1 1 1 1
""") == "1", "best packing case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| người mẫu đơn | 5 | sự thống trị tối đa (a_i) | 
| trường hợp lớn cân bằng | 150 | tính đúng đắn của ràng buộc tổng | 
| ràng buộc tối đa chiếm ưu thế | 10 | cọc đơn nặng | 
| trường hợp đóng gói tốt nhất | 1 | nhóm hoàn hảo khi x ≥ n |
