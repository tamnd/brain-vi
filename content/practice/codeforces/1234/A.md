---
title: "CF 1234A - Bình đẳng giá trở lại"
description: "Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi cửa hàng đều có bảng giá các mặt hàng. Mục tiêu là thay thế tất cả các mức giá khác nhau này bằng một mức giá thống nhất duy nhất để việc bán tất cả các mặt hàng ở mức giá duy nhất này không làm giảm tổng doanh thu so với ban đầu…"
date: "2026-06-13T19:09:19+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1234
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 590 (Div. 3)"
rating: 800
weight: 1234
solve_time_s: 246
verified: true
draft: false
---

[CF 1234A - Bình đẳng lại giá](https://codeforces.com/problemset/problem/1234/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 4 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi cửa hàng đều có bảng giá các mặt hàng. Mục tiêu là thay thế tất cả các mức giá khác nhau này bằng một mức giá thống nhất duy nhất để việc bán tất cả các mặt hàng ở mức giá duy nhất này không làm giảm tổng doanh thu so với mức giá ban đầu. 

Nói cách khác, nếu tổng doanh thu ban đầu là tổng của tất cả các mức giá nhất định thì chúng ta muốn chọn một số`x`bán như vậy`n`các mặt hàng có giá`x`mỗi cái đều mang lại tổng doanh thu`n * x`, và đây ít nhất phải là số tiền ban đầu. Trong số tất cả các giá trị hợp lệ như vậy của`x`, chúng tôi muốn cái nhỏ nhất có thể. 

Mỗi truy vấn là độc lập, vì vậy chúng tôi lặp lại tính toán này cho mọi trường hợp thử nghiệm. 

Các hạn chế là nhỏ:`n ≤ 100`Và`q ≤ 100`. Điều này có nghĩa là ngay cả các giải pháp có hành vi bậc ba hoặc tệ hơn đối với một truy vấn vẫn có khả năng vượt qua. Tuy nhiên, cấu trúc của bài toán gợi ý rằng chúng ta nên tránh mô phỏng không cần thiết và thay vào đó hãy tìm một điều kiện toán học trực tiếp. 

Một trường hợp cạnh ngây thơ nhưng quan trọng phát sinh khi tất cả các giá trị giống hệt nhau. Ví dụ: nếu tất cả giá đều`1`, thì tổng số là`n`, và mức giá thống nhất tối thiểu cũng là`1`. Bất kỳ lý do nào cố gắng "so khớp tối đa" hoặc "làm tròn theo phần tử" không chính xác đều có thể thất bại ở đây nếu nó bỏ qua ràng buộc toàn cục. 

Một trường hợp thất bại tinh vi khác xuất hiện khi các giá trị rất khác nhau. Ví dụ,`[1, 100, 100]`có tổng`201`, và giá thống nhất đúng là`ceil(201/3) = 67`. Bất kỳ cách tiếp cận nào sử dụng nhầm giá trị tối đa hoặc trung vị sẽ đưa ra câu trả lời sai vì ràng buộc là về tổng chứ không phải các giá trị riêng lẻ. 

## Phương pháp tiếp cận 

Cách mạnh mẽ nhất để suy nghĩ về vấn đề là thử mọi mức giá thống nhất có thể của ứng viên`x`. Đối với mỗi`x`, chúng tôi kiểm tra xem`n * x ≥ sum(a_i)`. Nhỏ nhất như vậy`x`là câu trả lời. Từ`x`có thể dao động lên tới tổng của tất cả các phần tử, điều này có thể yêu cầu lặp lại tối đa khoảng`10^9`trong trường hợp xấu nhất, điều này là không cần thiết mặc dù các ràng buộc là nhỏ. 

Quan sát quan trọng là điều kiện chỉ phụ thuộc vào tổng của mảng. Doanh thu ban đầu được cố định khi chúng tôi tính toán`S = sum(a_i)`. Doanh thu mới là`n * x`. Chúng ta cần số nguyên nhỏ nhất`x`như vậy:`n * x ≥ S`Đây là một bất đẳng thức đơn giản. Số nguyên nhỏ nhất thỏa mãn điều đó là trần của`S / n`. 

Điều này chuyển đổi vấn đề từ việc tìm kiếm các ứng viên sang tính toán một biểu thức số học cho mỗi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(S / n) mỗi truy vấn | O(1) | Về mặt khái niệm quá chậm | 
| Tối ưu | O(n) mỗi truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết từng truy vấn một cách độc lập bằng cách sử dụng cùng một lý do. 

1. Đọc số lượng hàng hóa`n`cho truy vấn hiện tại. Điều này xác định số lần mức giá thống nhất mới sẽ được áp dụng trong tính toán tổng doanh thu. 
2. Đọc dãy giá và tính tổng của chúng`S`. Đây là tổng doanh thu ban đầu mà chúng tôi phải đạt hoặc vượt quá. 
3. Tính số nguyên nhỏ nhất`x`như vậy`n * x ≥ S`. Điều này tương đương với việc thực hiện phép chia trần số nguyên của`S`qua`n`. 
4. Xuất giá trị này cho truy vấn. 

Bước tinh tế duy nhất là chia trần số nguyên. Thay vì sử dụng phép chia dấu phẩy động, chúng tôi tính toán nó một cách an toàn như`(S + n - 1) // n`, đảm bảo tính chính xác mà không gặp vấn đề về độ chính xác. 

### Tại sao nó hoạt động 

Ràng buộc`n * x ≥ S`đặc trưng đầy đủ tính hợp lệ vì doanh thu sau khi thống nhất chỉ phụ thuộc vào`x`và số lượng mặt hàng. Bất kỳ giải pháp hợp lệ nào cũng phải thỏa mãn bất đẳng thức này và mọi giá trị nhỏ hơn sẽ vi phạm nó. Kể từ khi chức năng`n * x`tăng đơn điệu trong`x`, số nguyên hợp lệ nhỏ nhất chính xác là mức trần của`S / n`, làm cho giải pháp vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())
    for _ in range(q):
        n = int(input())
        a = list(map(int, input().split()))
        s = sum(a)
        ans = (s + n - 1) // n
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp được cấu trúc xung quanh việc xử lý theo truy vấn. Tính tổng là bước tổng hợp có ý nghĩa duy nhất và mọi thứ khác đều giảm xuống thành một phép biến đổi số học duy nhất. Công thức chia số nguyên tránh hoàn toàn việc tính toán dấu phẩy động, đảm bảo tính chính xác ngay cả đối với số tiền lớn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
a = [1, 2, 3, 4, 5]
```Chúng tôi tính tổng`S = 15`. 

| Bước | n | tổng S | tính toán | kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 15 | (15 + 4) // 5 | 3 | 

Đầu ra là`3`. Điều này khẳng định tổng doanh thu`5 * 3 = 15`chính xác phù hợp với bản gốc. 

Dấu vết này cho thấy trường hợp điều kiện bình đẳng chặt chẽ, nghĩa là mức giá tối ưu sẽ bảo toàn chính xác doanh thu mà không vượt quá mức đó. 

### Ví dụ 2 

đầu vào:```
n = 3
a = [1, 2, 2]
```Chúng tôi tính toán`S = 5`. 

| Bước | n | tổng S | tính toán | kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 5 | (5 + 2) // 3 | 2 | 

Đầu ra là`2`. Ở đây tổng ban đầu là 5, trong khi`3 * 1 = 3`quá nhỏ và`3 * 2 = 6`là đủ. 

Điều này chứng tỏ tại sao việc làm tròn là cần thiết: việc tính sàn sẽ đánh giá thấp doanh thu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi truy vấn | tổng hợp mảng chi phối công việc | 
| Không gian | O(1) thêm | chỉ một vài biến được lưu trữ | 

Cho rằng`n ≤ 100`Và`q ≤ 100`, tổng các phép toán là cực kỳ nhỏ, nằm trong giới hạn. Ngay cả việc thực hiện đơn giản cũng hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    q = int(sys.stdin.readline())
    for _ in range(q):
        n = int(sys.stdin.readline())
        a = list(map(int, sys.stdin.readline().split()))
        s = sum(a)
        ans = (s + n - 1) // n
        output.append(str(ans))
    return "\n".join(output)

# provided samples
assert run("""3
5
1 2 3 4 5
3
1 2 2
4
1 1 1 1
""") == "3\n2\n1"

# custom cases
assert run("""1
1
100
""") == "100"

assert run("""1
3
10 10 10
""") == "10"

assert run("""1
4
1 1 1 100
""") == "26"

assert run("""1
2
1 100
""") == "51"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | cùng giá trị | n = vỏ 1 cạnh | 
| tất cả đều bình đẳng | giá trị không đổi | trường hợp ổn định | 
| phân phối lệch | trần đúng | giá trị không đồng đều | 
| hai giá trị cực trị | hành vi làm tròn | tính đúng đắn của logic trần | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi`n = 1`. Thuật toán tính toán`(a1 + 0) // 1 = a1`, điều này đúng vì mức giá thống nhất duy nhất có thể có là chính giá trị đó. 

Một trường hợp khác là khi tất cả các giá trị giống hệt nhau. Nếu như`a = [k, k, ..., k]`, sau đó`S = n * k`, Và`(S + n - 1) // n = k`, bảo toàn chính xác giá trị ban đầu. 

Một trường hợp thú vị hơn là khi các giá trị thay đổi đáng kể, chẳng hạn như`a = [1, 100, 100]`. Đây`S = 201`, Và`n = 3`, do đó việc tính toán cho`67`. Kiểm tra thủ công,`3 * 66 = 198`là quá nhỏ, trong khi`3 * 67 = 201`đáp ứng yêu cầu, xác nhận tính đúng đắn của hành vi trần.
