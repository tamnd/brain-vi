---
title: "CF 104683A - Banis và Thẻ"
description: "Chúng ta được cho một bộ thẻ được đánh số từ 1 đến n. Đối với mỗi truy vấn, ai đó chọn một giá trị m và hỏi tổng của tất cả các số thẻ chia hết cho m. Nói cách khác, chúng ta đang tính tổng mọi bội số của m xuất hiện trong phạm vi từ 1 đến n."
date: "2026-06-29T08:54:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104683
codeforces_index: "A"
codeforces_contest_name: "TheForces Round #24 (DIV3-Forces)"
rating: 0
weight: 104683
solve_time_s: 75
verified: true
draft: false
---

[CF 104683A - Banis và Thẻ](https://codeforces.com/problemset/problem/104683/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một bộ thẻ được đánh số từ 1 đến n. Đối với mỗi truy vấn, ai đó chọn một giá trị m và hỏi tổng của tất cả các số thẻ chia hết cho m. Nói cách khác, chúng ta đang tính tổng mọi bội số của m xuất hiện trong phạm vi từ 1 đến n. 

Vì vậy, đối với một truy vấn cố định, nhiệm vụ là tính m + 2m + 3m + … miễn là số hạng không vượt quá n. Đây chỉ đơn giản là tổng của cấp số cộng được hình thành bởi bội số của m trong phạm vi. 

Đầu vào chứa tối đa 100.000 truy vấn độc lập và mỗi truy vấn n có thể lớn bằng 1e9. Sự kết hợp này ngay lập tức loại trừ bất kỳ cách tiếp cận nào lặp lại trên tất cả các số lên tới n cho mỗi truy vấn. Ngay cả việc lặp lại từng bội số một cũng không thể thực hiện được khi n lớn và t cũng lớn, vì trường hợp xấu nhất sẽ liên quan đến thứ tự các phép toán 1e14. 

Trường hợp cạnh chính xuất phát từ việc hiểu sai phạm vi của bội số. Việc triển khai đơn giản có thể cố gắng lặp từ 1 đến n và kiểm tra tính chia hết, điều này rõ ràng là không khả thi. Một vấn đề tế nhị khác là tràn hoặc độ chính xác nếu ai đó cố gắng tích lũy mà không nhận ra dạng đóng tồn tại, mặc dù Python tránh tràn nhưng nó vẫn sẽ TLE. 

Một ví dụ nhỏ cho thấy cấu trúc rõ ràng. Nếu n = 12 và m = 2 thì các quân bài hợp lệ là 2, 4, 6, 8, 10, 12 và tổng của chúng là 42. Nếu n = 1 và m = 1 thì đáp án là 1 vì chỉ có một số. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi truy vấn, chúng tôi quét tất cả các số nguyên từ 1 đến n và cộng các số chia hết cho m. Điều này đúng vì nó trực tiếp tuân theo định nghĩa. Tuy nhiên, nó thực hiện n lần lặp cho mỗi truy vấn, dẫn đến 1e14 thao tác trong trường hợp xấu nhất khi cả t và n đều lớn, vượt xa giới hạn. 

Quan sát quan trọng là các số đóng góp vào tổng chính xác là bội số của m: m, 2m, 3 m, v.v. cho đến k lớn nhất sao cho km ≤ n. Số số hạng như vậy là sàn(n/m). Khi chúng tôi xác định được điều này, vấn đề sẽ giảm xuống việc tính tổng k số tự nhiên đầu tiên được chia tỷ lệ theo m. 

Vậy tổng sẽ trở thành: 

m (1 + 2 + … + k), trong đó k = sàn(n/m). 

Tổng của k số nguyên đầu tiên là k(k + 1)/2 nên kết quả cuối cùng là: 

m * k * (k + 1) / 2. 

Điều này làm giảm mỗi truy vấn thành số học thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) mỗi truy vấn | O(1) | Quá chậm | 
| Tối ưu | O(1) mỗi truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc n và m cho một truy vấn. Chúng xác định phạm vi thẻ và kích thước bước của bội số hợp lệ. 
2. Tính k = n // m. Điều này cho biết số bội của m nằm trong [1, n], vì mọi số hạng hợp lệ đều có dạng i·m. 
3. Tính tổng các số nguyên từ 1 đến k bằng cách sử dụng k * (k + 1) // 2. Điều này thể hiện vị trí chỉ số của bội số. 
4. Nhân kết quả đó với m để chuyển tỷ lệ từ chỉ số sang giá trị thẻ thực tế. 
5. Xuất kết quả cho truy vấn. 

### Tại sao nó hoạt động 

Mọi số hợp lệ trong tổng có thể được viết duy nhất là i·m trong đó i nằm trong khoảng từ 1 đến k = sàn(n / m). Ánh xạ này là một-một, vì vậy việc tính tổng các giá trị thẻ tương đương với việc tính tổng một chuỗi các số nguyên liên tiếp được chia theo tỷ lệ. Bởi vì chuỗi là số học với sai phân không đổi nên việc thay thế nó bằng dạng đóng sẽ duy trì tính chính xác cho tất cả các dữ liệu đầu vào hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        k = n // m
        ans = m * k * (k + 1) // 2
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp hoàn toàn dựa vào việc chuyển đổi bài toán thành phép đếm bội số và áp dụng công thức chuỗi số học. Phép chia số nguyên n // m rất quan trọng vì nó xác định có bao nhiêu bước đầy đủ có kích thước m phù hợp với phạm vi. Phép nhân với m phải xảy ra sau khi tính toán số tam giác để tránh sự tăng trưởng không cần thiết trong các bước trung gian, mặc dù Python vẫn xử lý các số nguyên lớn. 

Một lỗi phổ biến là đảo ngược thứ tự hoặc cố gắng tính m * i lặp đi lặp lại trong một vòng lặp. Một vấn đề tế nhị khác là quên rằng k bằng 0 khi m > n, nhưng phép chia số nguyên xử lý trường hợp này một cách tự nhiên một cách chính xác. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi tính toán cho hai trường hợp. 

### Ví dụ 1: n = 12, m = 2 

| Bước | n | m | k = n//m | k(k+1)/2 | Cuối cùng | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 12 | 2 | - | - | - | 
| Tính k | 12 | 2 | 6 | - | - | 
| Tổng tam giác | 12 | 2 | 6 | 21 | - | 
| Nhân với m | 12 | 2 | 6 | 21 | 42 | 

Kết quả khớp với phép liệt kê trực tiếp các số chẵn lên đến 12. Điều này xác nhận rằng việc ánh xạ bội số tới chỉ số hoạt động chính xác. 

### Ví dụ 2: n = 1, m = 1 

| Bước | n | m | k = n//m | k(k+1)/2 | Cuối cùng | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 1 | 1 | - | - | - | 
| Tính k | 1 | 1 | 1 | - | - | 
| Tổng tam giác | 1 | 1 | 1 | 1 | - | 
| Nhân với m | 1 | 1 | 1 | 1 | 1 | 

Điều này xác nhận tính chính xác của đầu vào không tầm thường nhỏ nhất, trong đó chỉ tồn tại một phần tử. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi truy vấn được xử lý bằng các phép toán số học không đổi | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp dễ dàng phù hợp với giới hạn vì thậm chí 100.000 truy vấn chỉ yêu cầu số học số nguyên đơn giản, không đáng kể so với các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(sys.stdin.readline())
    for _ in range(t):
        n, m = map(int, sys.stdin.readline().split())
        k = n // m
        output.append(str(m * k * (k + 1) // 2))
    return "\n".join(output)

# provided samples
assert run("""3
12 2
1 1
1010 10
""") == """42
1
5050""", "sample 1"

# custom cases
assert run("""1
10 3
""") == "18", "basic multiple count"

assert run("""1
5 10
""") == "0", "m greater than n"

assert run("""1
1000000000 1
""") == "500000000500000000", "maximum n, m=1"

assert run("""1
6 6
""") == "6", "single multiple"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10 3 | 18 | mẫu chia số không tầm thường | 
| 5 10 | 0 | trường hợp không tồn tại bội số | 
| 1e9 1 | số tiền lớn | độ ổn định số học giới hạn tối đa | 
| 6 6 | 6 | cấp số cộng một phần tử | 

## Vỏ cạnh 

Khi m > n, k trở thành 0 do phép chia số nguyên. Ví dụ: đầu vào n = 5, m = 10 tạo ra k = 0 và công thức ước tính là 0 * 1/2 * m = 0. Điều này khớp với thực tế là không có số thẻ nào chia hết cho m trong phạm vi đó. 

Khi m bằng n, k trở thành 1 và kết quả giảm xuống m * 1 = m, nắm bắt chính xác phần tử hợp lệ duy nhất. 

Khi m = 1 thì mọi số từ 1 đến n đều được đưa vào. Công thức trở thành n(n + 1)/2, khớp với tổng tiêu chuẩn của n số nguyên đầu tiên, xác nhận rằng công thức tổng quát suy biến chính xác thành một trường hợp phổ biến.
