---
title: "CF 1110A - Tính chẵn lẻ"
description: "Bài toán đưa ra một số $n$ không phải ở dạng thập phân mà ở một số cơ số $b$ tùy ý. Các chữ số của $n$ được liệt kê từ số quan trọng nhất đến ít quan trọng nhất và nhiệm vụ là quyết định xem $n$ là số chẵn hay số lẻ trong hệ thập phân."
date: "2026-06-12T05:07:30+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1110
codeforces_index: "A"
codeforces_contest_name: "Codeforces Global Round 1"
rating: 900
weight: 1110
solve_time_s: 309
verified: false
draft: false
---

[CF 1110A - Tính chẵn lẻ](https://codeforces.com/problemset/problem/1110/A) 

**Xếp hạng:** 900 
**Thẻ:** toán 
**Thời gian giải:** 5 phút 9 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Bài toán đưa ra một số$n$không phải ở dạng thập phân, mà ở một số cơ số tùy ý$b$. Các chữ số của$n$được liệt kê từ quan trọng nhất đến ít quan trọng nhất và nhiệm vụ là quyết định xem liệu$n$là chẵn hoặc lẻ ở dạng thập phân. Ví dụ: nếu cơ số là 13 và các chữ số là [3, 2, 7] thì giá trị thập phân của$n$là$3\cdot13^2 + 2\cdot13 + 7 = 540$, tức là chẵn. 

Các ràng buộc cho phép lên đến$10^5$chữ số và mỗi chữ số có thể lớn tới 99 (vì$b \le 100$). Việc tính toán số thập phân đầy đủ là không thể vì giá trị của$n$có thể lớn về mặt thiên văn, vượt xa giới hạn số nguyên tiêu chuẩn. Điều này loại trừ bất kỳ cách tiếp cận nào cố gắng tính toán trực tiếp$n$và sau đó kiểm tra tính chẵn lẻ của nó. 

Trường hợp cạnh phát sinh khi chỉ có một chữ số hoặc khi cơ số chẵn. Đối với số có một chữ số, tính chẵn lẻ chỉ được xác định bởi chữ số đó. Đối với các số lớn hơn, sự tương tác giữa số chẵn lẻ của cơ số và các chữ số rất quan trọng, bởi vì nhân một chữ số lẻ hoặc chẵn với lũy thừa của cơ số có thể ảnh hưởng đến số chẵn lẻ cuối cùng theo những cách có thể dự đoán được. 

Một cách tiếp cận bất cẩn sẽ cố gắng tính toán$n$trực tiếp hoặc giảm modulo 2 không đúng điểm. Ví dụ: tổng hợp các sản phẩm$a_i \cdot b^{k-i}$mà không xem xét số học mô-đun sẽ tràn. Một trường hợp tinh tế khác là khi cơ số chẵn: thì tất cả các số hạng ngoại trừ số cuối cùng đều được đảm bảo là số chẵn, vì vậy chỉ có chữ số cuối cùng mới quan trọng. Quan sát này là rất quan trọng. 

## Phương pháp tiếp cận 

Phương pháp vũ phu sẽ xây dựng$n$bằng cách tính tổng$a_1 \cdot b^{k-1} + a_2 \cdot b^{k-2} + \dots + a_k$. Điều này làm việc cho nhỏ$k$Và$b$, nhưng với$k$lên tới$10^5$, cách tiếp cận này sẽ yêu cầu nhân các số rất lớn nhiều lần. Ngay cả với số học mô-đun để giảm mô-đun 2, nó vẫn yêu cầu lặp lại tất cả các chữ số và khả năng tính toán của$b$, điều đó là không cần thiết. 

Cách tiếp cận tối ưu tận dụng số học mô-đun và tính chất chẵn lẻ. Một số là số chẵn nếu nó chia hết cho 2, có thể được xác định theo modulo 2. Quan sát rằng mọi lũy thừa của cơ số chẵn đều là số chẵn, vậy khi$b$là số chẵn, tất cả các chữ số ngoại trừ số cuối cùng được nhân với một số chẵn, đóng góp 0 modulo 2. Chỉ có chữ số cuối cùng$a_k$ảnh hưởng đến sự ngang bằng. Khi$b$thật kỳ lạ, mọi sức mạnh của$b$thật kỳ quặc. Vì vậy, mỗi thuật ngữ đều góp phần$a_i \cdot 1 \mod 2 = a_i \mod 2$, do đó tính chẵn lẻ của$n$là tổng của tất cả các chữ số modulo 2. 

Thông tin chuyên sâu này giúp giảm vấn đề trong việc kiểm tra chữ số cuối cùng (cơ số chẵn) hoặc tính tổng tất cả các chữ số modulo 2 (cơ số lẻ). Dù sao thì cách tiếp cận bạo lực vẫn lặp lại trên các chữ số, nhưng cách tiếp cận tối ưu chỉ thực hiện một phép toán modulo đơn giản cho mỗi chữ số, tránh hoàn toàn các phép toán số nguyên lớn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k log n) | O(1) | Quá chậm do số lượng lớn | 
| Tối ưu | O(k) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc căn cứ`b`và số chữ số`k`. Chúng xác định cấu trúc của số trong cơ số đã cho của nó. 
2. Đọc`k`chữ số vào một danh sách`a`. Chúng được sắp xếp từ quan trọng nhất đến ít quan trọng nhất. 
3. Kiểm tra xem`b`là chẵn. Nếu đúng như vậy thì mọi sức mạnh của`b`lớn hơn 0 là số chẵn. Do đó, tất cả các đóng góp ngoại trừ chữ số cuối cùng đều không liên quan đến tính chẵn lẻ. 
4. Nếu`b`là số chẵn, in "chẵn" nếu chữ số cuối cùng`a[-1]`là số chẵn, ngược lại in ra "lẻ". 
5. Nếu`b`là số lẻ, lặp lại tất cả các chữ số và tính tổng theo modulo 2. Nếu tổng chia hết cho 2, hãy in "chẵn"; nếu không thì in "lẻ". 

Tại sao nó hoạt động: Khi giảm modulo 2, phép nhân với số chẵn sẽ dẫn đến 0, do đó, chỉ các chữ số nhân với lũy thừa lẻ của cơ số lẻ mới đóng góp. Thuật toán duy trì tính bất biến là tổng modulo-2 hiện tại phản ánh chính xác tính chẵn lẻ của toàn bộ số ở mỗi bước. Điều này đảm bảo tính chính xác mà không cần tính toán giá trị đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

b, k = map(int, input().split())
a = list(map(int, input().split()))

if b % 2 == 0:
    print("even" if a[-1] % 2 == 0 else "odd")
else:
    total = sum(a) % 2
    print("even" if total == 0 else "odd")
```Giải pháp đầu tiên đọc đầu vào một cách hiệu quả bằng cách sử dụng`sys.stdin.readline`. Nó kiểm tra tính chẵn lẻ cơ sở ngay lập tức. Đối với các cơ số chẵn, chỉ chữ số cuối cùng được kiểm tra, điều này tránh được việc tính toán không cần thiết. Đối với các cơ số lẻ, nó tính tổng tất cả các chữ số theo modulo 2, đảm bảo kết quả khớp với tính chẵn lẻ của số mà không bao giờ tạo ra số nguyên đầy đủ. Có thể tránh các lỗi riêng lẻ bằng cách lập chỉ mục chính xác cho chữ số cuối cùng là`a[-1]`. 

## Ví dụ đã hoạt động 

Mẫu 1:`13 3`với chữ số`[3, 2, 7]`. 

| Bước | Mô tả | Giá trị | 
| --- | --- | --- | 
| Đọc b, k | cơ số 13, 3 chữ số | b=13, k=3 | 
| Đọc chữ số | danh sách chữ số | a=[3,2,7] | 
| Kiểm tra b chẵn lẻ | 13 là số lẻ | tổng(a) % 2 = (3+2+7)%2 = 12%2 = 0 | 
| Đầu ra | thậm chí | "thậm chí" | 

Điều này chứng tỏ rằng khi cơ số lẻ, tổng tất cả các chữ số theo modulo 2 sẽ xác định chính xác tính chẵn lẻ. 

Mẫu 2:`2 5`với chữ số`[1, 0, 0, 1, 1]`. 

| Bước | Mô tả | Giá trị | 
| --- | --- | --- | 
| Đọc b, k | cơ số 2, 5 chữ số | b=2, k=5 | 
| Đọc chữ số | danh sách chữ số | a=[1,0,0,1,1] | 
| Kiểm tra b chẵn lẻ | 2 là chẵn | chữ số cuối cùng a[-1] = 1 -> lẻ | 
| Đầu ra | lẻ | "kỳ quặc" | 

Điều này cho thấy rằng đối với các cơ số chẵn, chỉ có chữ số cuối cùng là quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k) | Một lần chuyển qua danh sách các chữ số để tính tổng modulo 2 (hoặc lấy phần tử cuối cùng) | 
| Không gian | O(k) | Lưu trữ danh sách chữ số | 

Được cho`k <= 10^5`, Hoạt động O(k) nằm trong giới hạn 1 giây và mức sử dụng bộ nhớ cho 100.000 số nguyên vừa vặn thoải mái trong phạm vi 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    b, k = map(int, input().split())
    a = list(map(int, input().split()))
    if b % 2 == 0:
        return "even" if a[-1] % 2 == 0 else "odd"
    else:
        return "even" if sum(a) % 2 == 0 else "odd"

# Provided samples
assert run("13 3\n3 2 7\n") == "even", "sample 1"
assert run("10 9\n1 2 3 4 5 6 7 8 9\n") == "odd", "sample 2"
assert run("99 5\n32 92 85 74 4\n") == "odd", "sample 3"
assert run("2 1\n2\n") == "even", "sample 4"

# Custom cases
assert run("2 5\n0 0 0 0 0\n") == "even", "all zeros even base"
assert run("2 5\n1 1 1 1 1\n") == "odd", "all ones even base"
assert run("3 4\n1 2 0 2\n") == "odd", "odd base multiple digits"
assert run("100 2\n99 50\n") == "even", "even base, last digit even"
assert run("5 3\n2 4 1\n") == "odd", "odd base, sum odd"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 5, [0,0,0,0,0] | thậm chí | cơ số chẵn, chữ số cuối cùng bằng 0 | 
| 2 5, [1,1,1,1,1] | lẻ | cơ số chẵn, chữ số cuối cùng lẻ | 
| 3 4, [1,2,0,2] | lẻ | cơ số lẻ, tổng modulo 2 | 
| 100 2, [99,50] | thậm chí | cơ số chẵn, chữ số cuối cùng chẵn | 
| 5 3, [2,4,1] | lẻ | cơ số lẻ, tổng modulo 2 | 

## Vỏ cạnh 

Đối với một số có một chữ số trong cơ số bất kỳ, thuật toán sẽ rút gọn chính xác về việc kiểm tra chữ số đó theo modulo 2. Đối với`b`thậm chí, nó còn sử dụng`a[-1] % 2`. Vì`b`số lẻ,`sum(a) % 2`giống hệt với`a[0] % 2`. Đối với đầu vào có kích thước tối đa với 100
