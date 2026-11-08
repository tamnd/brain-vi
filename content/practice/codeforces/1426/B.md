---
title: "CF 1426B - Ma trận đối xứng"
description: "Chúng ta được yêu cầu xác định xem có thể xếp một hình vuông (m nhân m) bằng cách sử dụng (2 nhân 2) viên gạch sao cho hình vuông thu được đối xứng với đường chéo chính của nó hay không. Mỗi ô có các số cố định trong bốn ô của nó và không được phép xoay."
date: "2026-06-11T05:47:31+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1426
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 674 (Div. 3)"
rating: 900
weight: 1426
solve_time_s: 224
verified: true
draft: false
---

[CF 1426B - Ma trận đối xứng](https://codeforces.com/problemset/problem/1426/B) 

**Xếp hạng:** 900 
**Thẻ:** triển khai 
**Thời gian giải:** 3 phút 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định liệu có thể xếp một\(m \times m\)hình vuông sử dụng\(2 \times 2\)xếp sao cho hình vuông thu được đối xứng với đường chéo chính của nó. Mỗi ô có các số cố định trong bốn ô của nó và không được phép xoay. Chúng tôi có vô số loại gạch. Tính đối xứng ở đây có nghĩa là đối với mọi cặp vị trí \((i, j)\) và \((j, i)\), các giá trị phải khớp nhau. 

Đầu vào cho chúng ta\(t\)trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm chỉ định số lượng loại ô\(n\)và kích thước hình vuông\(m\), theo sau là định nghĩa của các ô theo thứ tự. Mỗi ô được đưa ra thành hai dòng, đại diện cho hàng trên cùng và dưới cùng. Đầu ra cho mỗi trường hợp thử nghiệm chỉ đơn giản là "CÓ" nếu hình vuông có thể được xếp đối xứng và "KHÔNG" nếu ngược lại. 

Nhìn vào những hạn chế,\(n\)Và\(m\)cả hai đều có nhiều nhất là 100 và có nhiều nhất là 100 trường hợp thử nghiệm. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào có độ phức tạp về thời gian \(O(n \cdot m)\) cho mỗi trường hợp thử nghiệm đều khả thi. Điểm tinh tế chính là yêu cầu về tính đối xứng kết hợp với việc xếp gạch có nghĩa là không phải tất cả các kết hợp ô đều hợp lệ. Đặc biệt, vì các ô không thể xoay được nên chỉ những ô có các ô chéo bên trái và bên phải có thể căn chỉnh với vị trí phản chiếu của chúng mới hữu ích cho việc xây dựng hình vuông đối xứng. 

Các trường hợp đặc biệt mà cách tiếp cận đơn giản có thể bỏ qua bao gồm khi\(m\)thật kỳ quặc. Nếu như\(m\)thật kỳ lạ, một lát gạch đối xứng với\(2 \times 2\)không thể xếp các ô vì hàng và cột ở giữa sẽ yêu cầu chiều rộng một ô không thể được bao phủ bởi một ô\(2 \times 2\)ngói. Một sự tinh tế khác là chúng ta không cần sử dụng tất cả các ô; chúng ta chỉ cần tìm một ô phù hợp có các ô ngoài đường chéo có thể ghép với chính chúng hoặc một ô khác để duy trì tính đối xứng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng đặt mọi ô ở mọi vị trí trong\(m \times m\)lưới và kiểm tra tính đối xứng. Đối với một\(m \times m\)hình vuông, có khoảng \((m/2)^2\) vị trí của\(2 \times 2\)gạch lát. Với\(n\)xếp, điều này dẫn đến các phép toán \(O(n \cdot (m/2)^2)\), khả thi trong các ràng buộc của bài toán này, nhưng việc xây dựng toàn bộ ma trận và kiểm tra tính đối xứng là không cần thiết. Cái nhìn sâu sắc là chúng ta không cần phải mô phỏng toàn bộ lưới. Điều duy nhất quan trọng là: 

1.\(m\)phải chẵn. 
2. Phải tồn tại ít nhất một ô trong đó giá trị trên cùng bên phải bằng giá trị dưới cùng bên trái. Điều này đảm bảo rằng các viên gạch có thể gặp nhau dọc theo đường chéo và duy trì tính đối xứng. 

Quan sát này làm giảm vấn đề để kiểm tra tính chẵn lẻ của\(m\)và quét các ô để tìm cặp ô liền kề theo đường chéo phù hợp. Nếu như\(m\)là số lẻ, không thể xếp gạch đối xứng được. Nếu như\(m\)là chẵn, chúng ta chỉ cần tìm ít nhất một ô có tính chất đối xứng ngoài đường chéo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Brute Force (mô phỏng ốp lát) | O(n * m2) | O(m2) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu (chẵn lẻ + kiểm tra ô) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case\(t\). Lặp lại qua từng trường hợp thử nghiệm. 
2. Với mỗi test case, hãy đọc số loại ô\(n\)và kích thước hình vuông mong muốn\(m\). 
3. Khởi tạo cờ`found`là sai. Cờ này sẽ cho biết liệu có tồn tại một ô phù hợp hay không. 
4. Với mỗi ô, hãy đọc bốn số nguyên\(a, b, c, d\)lần lượt là các số trên cùng bên trái, trên cùng bên phải, dưới cùng bên trái và dưới cùng bên phải. 
5. Kiểm tra xem\(b = c\). Điều kiện này đảm bảo rằng viên gạch có thể góp phần tạo nên một viên gạch đối xứng dọc theo đường chéo. 
6. Nếu bất kỳ ô nào thỏa mãn điều kiện này, hãy đặt`found`thành Đúng. 
7. Sau khi quét tất cả các ô, nếu`m`là số lẻ, xuất ra "NO" vì a\(2 \times 2\)ốp lát không thể bao phủ một hình vuông có kích thước lẻ một cách đối xứng. 
8. Nếu`m`là số chẵn và`found`là Đúng, xuất ra "CÓ"; nếu không thì xuất ra "NO". 

Tại sao nó hoạt động: tính đối xứng của ma trận giúp giảm bớt vấn đề trong việc đảm bảo rằng các ô không chéo của các ô có thể gặp nhau dọc theo đường chéo và kích thước hình vuông phải chia hết cho 2 để chứa đầy\(2 \times 2\)gạch lát. Bất biến này đảm bảo tính đúng đắn mà không cần xây dựng toàn bộ ma trận. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    found = False
    for _ in range(n):
        a, b = map(int, input().split())
        c, d = map(int, input().split())
        if b == c:
            found = True
    if m % 2 != 0:
        print("NO")
    else:
        print("YES" if found else "NO")
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`. Chúng tôi giữ một lá cờ đơn giản để phát hiện một ô phù hợp. Séc`m % 2 != 0`ngay lập tức xử lý các ô vuông có chiều dài lẻ không thể thực hiện được. Chúng tôi không lưu trữ gạch một cách không cần thiết, giữ cho việc sử dụng không gian ở mức tối thiểu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 4
1 2
5 6
5 7
7 4
8 9
9 8
```Xử lý: 

| Ngói | một | b | c | d | b==c | 
|---|---|---|---|---|---| 
| 1 | 1 | 2 | 5 | 6 | Sai | 
| 2 | 5 | 7 | 7 | 4 | Đúng | 
| 3 | 8 | 9 | 9 | 8 | Đúng |`m = 4`(chẵn), ít nhất một ô có`b == c`, do đó xuất ra "CÓ". 

### Ví dụ về kích thước lẻ 

đầu vào:```
1 3
1 2
2 1
```

`m = 3`(lẻ), xuất ra "NO" ngay lập tức, bất kể nội dung ô xếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(t * n) | Chúng tôi kiểm tra từng ô một lần cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Chỉ cần một cờ boolean duy nhất | 

Giải pháp này dễ dàng phù hợp với giới hạn 1 giây cho tối đa 100 trường hợp thử nghiệm và 100 ô cho mỗi trường hợp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call solution
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        found = False
        for _ in range(n):
            a, b = map(int, input().split())
            c, d = map(int, input().split())
            if b == c:
                found = True
        if m % 2 != 0:
            print("NO")
        else:
            print("YES" if found else "NO")
    return output.getvalue().strip()

# provided samples
assert run("6\n3 4\n1 2\n5 6\n5 7\n7 4\n8 9\n9 8\n2 5\n1 1\n1 1\n2 2\n2 2\n1 100\n10 10\n10 10\n1 2\n4 5\n8 4\n2 2\n1 1\n1 1\n1 2\n3 4\n1 2\n1 1\n1 1") == "YES\nNO\nYES\nNO\nYES\nYES", "sample 1"

# custom cases
assert run("1\n1 2\n1 2\n2 3") == "YES", "single tile matching"
assert run("1\n1 2\n1 2\n3 4") == "NO", "single tile non-matching"
assert run("1\n2 3\n1 2\n2 1\n3 4\n4 3") == "NO", "odd square size"
assert run("1\n2 4\n1 2\n2 1\n3 4\n4 3") == "YES", "even square size, multiple tiles"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| 1 2\n1 2\n2 3 | CÓ | Ngói đơn có`b==c`hoạt động | 
| 1 2\n1 2\n3 4 | KHÔNG | Ngói đơn không có`b==c`thất bại | 
| 2 3\n1 2\n2 1\n3 4\n4 3 | KHÔNG | Số lẻ`m`không thể lát gạch | 
| 2 4\n1 2\n2 1\n3 4\n4 3 | CÓ | Thậm chí`m`với ô phù hợp thành công | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng xảy ra khi kích thước hình vuông\(m\)thật kỳ quặc. Ví dụ: với đầu vào:```
1
1 3
1 2
2 1
```Đầu tiên thuật toán kiểm tra `m % 2 !=
