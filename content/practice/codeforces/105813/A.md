---
title: "CF 105813A - Thomas"
description: "Chúng ta được yêu cầu xây dựng một tập hợp lớn các chuỗi nhị phân có độ dài cố định $n$, với một hạn chế duy nhất về việc hai chuỗi được chọn có thể khác nhau như thế nào. Mỗi chuỗi được tạo thành từ số không và số một."
date: "2026-06-25T23:42:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105813
codeforces_index: "A"
codeforces_contest_name: "Rutgers University Programming Contest Spring 2025"
rating: 0
weight: 105813
solve_time_s: 43
verified: true
draft: false
---

[CF 105813A – Thomas](https://codeforces.com/problemset/problem/105813/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một tập hợp lớn các chuỗi nhị phân có độ dài cố định$n$, với một hạn chế duy nhất về việc hai chuỗi được chọn có thể khác nhau như thế nào. 

Mỗi chuỗi được tạo thành từ số không và số một. Tình huống bị cấm là khi hai chuỗi khác nhau khác nhau ở đúng một vị trí, nghĩa là bạn có thể biến chuỗi này thành chuỗi kia bằng cách lật một bit và không làm gì khác. Tương tự, khoảng cách Hamming của chúng không bao giờ chính xác bằng 1. 

Nhiệm vụ không phải là tối ưu hóa một giá trị hoặc trả lời một truy vấn mà là xuất ra một cách rõ ràng bất kỳ tập hợp lớn nhất có thể nào thỏa mãn ràng buộc này. 

Ràng buộc$n \le 15$gợi ý rằng toàn bộ không gian của$2^n$các chuỗi đủ nhỏ để suy luận về mặt cấu trúc thay vì mô phỏng việc kiểm tra theo cặp. Một cách tiếp cận đơn giản để so sánh tất cả các cặp chuỗi sẽ liên quan đến khoảng$2^n$ứng viên và$O(2^{2n})$kiểm tra theo cặp, vốn đã là ranh giới cho$n=15$bởi vì nó có nghĩa là khoảng một tỷ so sánh. 

Trường hợp cạnh tinh tế xuất hiện khi$n=1$. Chỉ có hai chuỗi: "0" và "1" và chúng khác nhau ở đúng một vị trí. Điều này có nghĩa là chúng ta không thể lấy cả hai và mọi giải pháp đúng đều chỉ đưa ra một trong số chúng. Việc xây dựng bất cẩn giả định các mẫu đối xứng hoặc chẵn lẻ mà không kiểm tra trường hợp này có thể xuất ra cả hai chuỗi không chính xác. 

Đối với lớn hơn$n$, ví dụ nhỏ làm rõ cấu trúc. Vì$n=2$, đáp án tối ưu là$\{00, 11\}$. Cặp đôi$\{01, 10\}$có giá trị như nhau, nhưng việc trộn lẫn bất kỳ cặp nào trong số này sẽ vi phạm quy tắc vì 00 khác với 01 trong một bit. 

Điều này gợi ý rằng tính kề cận trong biểu đồ siêu lập phương của các chuỗi nhị phân là hạn chế thực sự: chúng ta đang chọn một tập hợp các đỉnh lớn nhất không có cạnh nào bên trong tập hợp đó. 

## Phương pháp tiếp cận 

Quan điểm brute-force là coi mỗi chuỗi nhị phân như một nút trong một$n$đồ thị siêu khối chiều, trong đó các cạnh kết nối các chuỗi khác nhau chính xác một bit. Bài toán trở thành việc chọn tập con lớn nhất của các đỉnh không có cạnh trong, đây là bài toán tập hợp độc lập cực đại cổ điển trên biểu đồ này. 

Một lực lượng vũ phu trực tiếp sẽ liệt kê tất cả các tập hợp con của các đỉnh và kiểm tra tính hợp lệ. Với$2^n$đỉnh thì số tập con là$2^{2^n}$, điều này trở nên hoàn toàn không khả thi ngay cả đối với$n=5$. Ngay cả việc hạn chế kiểm tra xung đột theo cặp trong một tập hợp con đã chọn cũng dẫn đến sự bùng nổ theo cấp số nhân. 

Quan sát cấu trúc quan trọng là siêu lập phương có tính chất lưỡng cực. Bất kỳ chuỗi nhị phân nào cũng có thể được phân loại theo tính chẵn lẻ của số bit của nó. Việc lật một bit luôn làm thay đổi tính chẵn lẻ, vì vậy mỗi cạnh đều kết nối chuỗi chẵn lẻ với chuỗi chẵn lẻ lẻ. Điều này chia toàn bộ biểu đồ thành hai phân vùng độc lập trong đó không có cạnh nào tồn tại bên trong phân vùng. 

Một khi điều này được nhận ra, tập hợp độc lập tối đa chỉ đơn giản là phân vùng lớn hơn trong hai phân vùng mà trong siêu khối có kích thước bằng nhau. Do đó, tất cả các chuỗi chẵn lẻ hoặc tất cả các chuỗi chẵn lẻ lẻ đều tạo thành một giải pháp tối ưu hợp lệ và cả hai đều có kích thước$2^{n-1}$. 

Điều này làm giảm nhiệm vụ từ tìm kiếm tổ hợp trên các tập hợp con sang xây dựng xác định dựa trên tính chẵn lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con |$O(2^{2^n} \cdot n)$|$O(2^n)$| Quá chậm | 
| Xây dựng dựa trên sự ngang bằng |$O(n 2^n)$|$O(2^n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Cấu trúc tối ưu dựa vào việc nhóm tất cả các chuỗi nhị phân theo số chuỗi chẵn hay số lẻ, sau đó chọn toàn bộ một nhóm. 

1. Lặp lại tất cả các số nguyên từ 0 đến$2^n - 1$. Mỗi số nguyên đại diện cho một chuỗi nhị phân có độ dài$n$. Mã hóa này cho phép thao tác bit trực tiếp mà không cần thao tác chuỗi. 
2. Đối với mỗi số, hãy đếm xem có bao nhiêu bit được đặt thành 1. Số lượng này xác định xem chuỗi thuộc lớp chẵn lẻ chẵn hay lẻ. 
3. Chọn một lớp chẵn lẻ làm tập hợp câu trả lời. Lựa chọn tự nhiên là tất cả các chuỗi có tính chẵn lẻ chẵn, vì nó bao gồm chuỗi hoàn toàn bằng 0 và giúp việc triển khai trở nên đơn giản. 
4. Đối với mọi số có tính chẵn lẻ khớp với lớp đã chọn, hãy chuyển đổi nó thành một chuỗi nhị phân có độ dài$n$và lưu trữ nó. 
5. Xuất ra kích thước của tập hợp này theo sau là tất cả các chuỗi. 

Phần không rõ ràng là tại sao tính chẵn lẻ là đủ. Lý do là việc lật một bit luôn chuyển đổi tính chẵn lẻ, do đó, bất kỳ hai chuỗi nào trong cùng một lớp chẵn lẻ phải khác nhau ở ít nhất hai vị trí. Điều này đảm bảo ràng buộc được thỏa mãn một cách tự động mà không cần kiểm tra rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())

    res = []
    for mask in range(1 << n):
        if bin(mask).count("1") % 2 == 0:
            res.append(mask)

    print(len(res))
    for mask in res:
        s = format(mask, f"0{n}b")
        print(s)

if __name__ == "__main__":
    solve()
```Giải pháp lặp lại trên tất cả các mặt nạ bit và lọc chúng theo tính chẵn lẻ. Bước chuyển đổi sử dụng định dạng nhị phân có chiều rộng cố định để giữ nguyên các số 0 đứng đầu, điều này cần thiết vì các chuỗi ngắn hơn sẽ được hiểu không chính xác. 

Một chi tiết triển khai tinh tế là việc đếm số bit thông qua`bin(mask).count("1")`là đủ cho$n \le 15$, nhưng trong các ràng buộc cao hơn, nội dung nhanh hơn như`bit_count()`sẽ được ưu tiên hơn. Ở đây nó không quan trọng về hiệu suất. 

Một chi tiết quan trọng khác là bảo toàn chính xác độ dài chuỗi$n$, vì việc bỏ qua các số 0 đứng đầu sẽ thu gọn các đỉnh khác biệt thành các biểu diễn mơ hồ. 

## Ví dụ đã hoạt động 

cho$n = 2$, chúng tôi liệt kê các mặt nạ từ 0 đến 3. 

| mặt nạ | nhị phân | số lượng | đã chọn | 
| --- | --- | --- | --- | 
| 0 | 00 | 0 | vâng | 
| 1 | 01 | 1 | không | 
| 2 | 10 | 1 | không | 
| 3 | 11 | 2 | vâng | 

Tập đầu ra là {00, 11}. Điều này phù hợp với giải pháp tối ưu đã biết và không có cặp nào khác nhau ở một vị trí chính xác. 

Dấu vết này cho thấy thuật toán không xem xét tính kề cận một cách rõ ràng. Thay vào đó, nó dựa hoàn toàn vào sự phân tách chẵn lẻ. 

Vì$n = 3$, mặt nạ từ 0 đến 7 tạo ra: 

| mặt nạ | nhị phân | số lượng | đã chọn | 
| --- | --- | --- | --- | 
| 0 | 000 | 0 | vâng | 
| 1 | 001 | 1 | không | 
| 2 | 010 | 1 | không | 
| 3 | 011 | 2 | vâng | 
| 4 | 100 | 1 | không | 
| 5 | 101 | 2 | vâng | 
| 6 | 110 | 2 | vâng | 
| 7 | 111 | 3 | không | 

Tập kết quả chứa 4 chuỗi, bằng$2^{3-1}$, xác nhận rằng chúng ta đang chọn chính xác một nửa siêu khối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n 2^n)$| Chúng tôi lặp lại tất cả$2^n$mặt nạ và tính toán số bit cũng như chuyển đổi | 
| Không gian |$O(2^n)$| Chúng tôi lưu trữ một nửa số chuỗi nhị phân trong tập đầu ra | 

Sự ràng buộc$n \le 15$làm cho$2^n = 32768$, do đó việc xây dựng là đơn giản để thực hiện trong giới hạn. Việc sử dụng bộ nhớ cũng nhỏ vì tối đa 16384 chuỗi được lưu trữ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import sys as _sys

    n = int(input().strip())
    res = []
    for mask in range(1 << n):
        if bin(mask).count("1") % 2 == 0:
            res.append(format(mask, f"0{n}b"))
    out = str(len(res)) + "\n" + "\n".join(res) + "\n"
    return out

# provided samples
assert run("1\n") == "1\n0\n"
assert run("2\n") == "2\n00\n11\n"

# custom cases
assert run("3\n").splitlines()[0] == "4"
assert run("4\n").splitlines()[0] == "8"
assert run("1\n") in ["1\n0\n", "1\n1\n"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n = 1 | chuỗi đơn | trường hợp cạnh nhỏ nhất | 
| n = 2 | 2 dây | tính đúng đắn của cấu trúc cơ bản | 
| n = 3 | 4 dây | cân bằng tỷ lệ và chẵn lẻ | 
| n = 4 | 8 dây | mô hình tăng trưởng theo cấp số nhân | 

## Vỏ cạnh 

cho$n = 1$, chỉ tồn tại hai chuỗi: "0" và "1". Cấu trúc chẵn lẻ chỉ chọn một trong số chúng vì "1" có chẵn lẻ lẻ và "0" có chẵn lẻ. Thuật toán đưa ra chính xác một chuỗi, khớp với ràng buộc rằng không có cặp nào có thể khác nhau ở đúng một vị trí. 

Vì$n = 2$, toàn bộ được chia thành hai phân vùng. Thuật toán chọn chính xác {00, 11}. Nếu một cách triển khai ngây thơ cố gắng bao gồm các chuỗi một cách tham lam mà không kiểm tra cấu trúc toàn cục, thì nó có thể vô tình bao gồm 01 và 00 cùng nhau, vi phạm quy tắc vì chúng khác nhau một bit. 

Vì$n = 3$, vùng lân cận trở nên dày đặc hơn, nhưng tính chẵn lẻ vẫn đảm bảo sự tách biệt. Mỗi chuỗi trong tập hợp đã chọn khác với bất kỳ chuỗi nào khác ở ít nhất hai vị trí bit, có thể được xác minh bằng cách so sánh trực tiếp các cặp đại diện như 000 và 011. 

Việc xây dựng tránh được tất cả những cạm bẫy như vậy bởi vì nó không bao giờ suy luận cục bộ về các cặp, mà chỉ về cấu trúc chẵn lẻ toàn cục của siêu khối.
