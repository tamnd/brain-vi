---
title: "CF 105930L - Stella"
description: "Mỗi trường hợp kiểm tra đầu vào đưa ra hai phân loại sao được viết theo định dạng cố định: một chữ cái viết hoa theo sau là một chữ số. Chữ cái đại diện cho một loại nhiệt độ thô được sắp xếp từ nóng nhất đến lạnh nhất là O, B, A, F, G, K, M."
date: "2026-06-21T15:49:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105930
codeforces_index: "L"
codeforces_contest_name: "The 15th Shandong CCPC Provincial Collegiate Programming Contest"
rating: 0
weight: 105930
solve_time_s: 43
verified: true
draft: false
---

[CF 105930L - Stella](https://codeforces.com/problemset/problem/105930/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp kiểm tra đầu vào đưa ra hai phân loại sao được viết theo định dạng cố định: một chữ cái viết hoa theo sau là một chữ số. Chữ cái biểu thị một lớp nhiệt độ thô được sắp xếp từ nóng nhất đến lạnh nhất là O, B, A, F, G, K, M. Bên trong mỗi lớp, chữ số từ 0 đến 9 tinh chỉnh nhiệt độ, trong đó 0 nóng hơn và 9 mát hơn. 

Vì vậy, mọi phân loại thực sự là một điểm trên thang nhiệt độ một chiều với 70 giá trị riêng biệt có thể có. Nhiệm vụ là so sánh hai điểm như vậy và quyết định xem ngôi sao thứ nhất nóng hơn, mát hơn hay có cùng nhiệt độ với ngôi sao thứ hai. 

Các ràng buộc rất nhỏ: lên tới 1000 phép so sánh, mỗi phép so sánh liên quan đến việc xử lý thời gian không đổi. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào phân tích từng cặp và so sánh trong O(1) là đủ, trong khi mọi giải pháp liên quan đến sắp xếp hoặc mô phỏng phức tạp đều không cần thiết. 

Một trường hợp thất bại tinh tế xuất phát từ việc so sánh từ điển dưới dạng chuỗi. Ví dụ: việc so sánh "A9" và "B0" dưới dạng chuỗi hoạt động vô tình ở đây vì thứ tự chữ cái khớp với thứ tự ASCII, nhưng việc dựa vào điều này rất dễ bị hỏng và có thể bị hỏng nếu mã hóa hoặc biểu diễn thay đổi. Một sai lầm tiềm ẩn khác là bỏ qua việc so sánh chữ số bị đảo ngược về ý nghĩa: chữ số nhỏ hơn có nghĩa là nóng hơn chứ không phải lạnh hơn. 

Các trường hợp cạnh chủ yếu là về các chữ cái và chữ số ranh giới. Chẳng hạn, so sánh O0 với O0 sẽ cho kết quả như nhau. So sánh M9 với O0 trước tiên sẽ mang lại kết quả mát hơn, vì M9 là lạnh nhất có thể và O0 là nóng nhất. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để giải thích vấn đề là ánh xạ rõ ràng mọi phân loại thành một giá trị nhiệt độ bằng số trong phạm vi từ 0 đến 69. Chúng tôi gán O0 là 0, O1 là 1, ..., O9 là 9, sau đó là B0 là 10, v.v. cho đến khi M9 là 69. Sau khi cả hai đầu vào được chuyển đổi, phép so sánh sẽ giảm xuống so sánh số nguyên. 

Điều này hiệu quả vì hệ thống phân loại được sắp xếp nghiêm ngặt trước tiên theo lớp chữ cái, sau đó theo chữ số trong lớp. Ánh xạ lực lượng vũ phu đã có kích thước không đổi và do đó hiệu quả, nhưng nó hơi gián tiếp. 

Chúng ta có thể đơn giản hóa hơn nữa bằng cách quan sát rằng thứ tự là từ điển với bảng chữ cái tùy chỉnh cho các chữ cái và thứ tự đảo ngược cho các chữ số bên trong mỗi chữ cái. Thay vì xây dựng một bảng ánh xạ đầy đủ, chúng ta có thể tính chỉ số của chữ cái và kết hợp nó với chữ số. 

Đặt chữ cái xếp hạng O, B, A, F, G, K, M tương ứng từ 0 đến 6. Khi đó một ngôi sao được biểu diễn dưới dạng`rank(letter) * 10 + digit`. Điều này trực tiếp mã hóa tổng thứ tự thành một số nguyên duy nhất, đảm bảo tính chính xác. 

Phương pháp brute-force sử dụng tra cứu từ điển cho mỗi chữ cái, trong khi phương pháp tối ưu thay thế nó bằng mã hóa số học trực tiếp. Cả hai đều là O(1) cho mỗi trường hợp thử nghiệm, nhưng trường hợp sau sạch hơn và tránh được các cấu trúc dữ liệu bổ sung. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng ánh xạ Brute | O(T) | O(1) | Đã chấp nhận | 
| Mã hóa số học | O(T) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi muốn biểu diễn bằng số trực tiếp cho từng phân loại để việc so sánh trở thành so sánh số nguyên đơn giản. 

1. Đọc hai chuỗi phân loại cho từng trường hợp kiểm thử. Mỗi chuỗi có độ dài chính xác là 2, do đó việc phân tích cú pháp là thời gian không đổi. 
2. Chuyển chữ cái thành thứ hạng từ 0 đến 6 theo vị trí của nó trong dãy nhiệt độ O, B, A, F, G, K, M. Thứ tự này được cố định và xác định mức nhiệt độ thô. 
3. Chuyển đổi ký tự chữ số thành số nguyên từ 0 đến 9. Chữ số này tinh chỉnh nhiệt độ trong lớp chữ cái của nó. 
4. Tính một giá trị số nguyên dưới dạng`rank * 10 + digit`. Điều này hiệu quả vì mỗi lớp chữ cái bao gồm chính xác 10 giá trị và thứ tự chữ số nhất quán trong mỗi lớp. 
5. Lặp lại cho cả hai ngôi sao, tạo ra hai số nguyên. 
6. So sánh hai số nguyên. Nếu giá trị đầu tiên nhỏ hơn thì nó tương ứng với ngôi sao nóng hơn, vì O0 là giá trị tối thiểu. Nếu lớn hơn thì mát hơn. Nếu bằng nhau thì cả hai cách phân loại đều giống nhau. 

Lý do chính khiến phép chuyển đổi này hợp lệ là vì thứ tự ban đầu đã được cấu trúc như một sản phẩm được sắp xếp theo từ điển: khóa chính là lớp chữ cái, khóa phụ là chữ số theo thứ tự độ nóng ngược. 

### Tại sao nó hoạt động 

Việc ánh xạ duy trì thứ tự vì các lớp chữ cái có nhiệt độ đơn điệu nghiêm ngặt và mỗi lớp chứa chính xác 10 phân mục có thứ tự thống nhất. Không có sự chồng chéo giữa các lớp vì việc nhân với 10 sẽ tách chúng thành các khoảng rời rạc. Trong một lớp, so sánh chữ số nhất quán với hướng nhiệt độ, do đó số nguyên kết hợp sẽ mã hóa đầy đủ thứ tự ban đầu mà không có sự mơ hồ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

order = {
    'O': 0,
    'B': 1,
    'A': 2,
    'F': 3,
    'G': 4,
    'K': 5,
    'M': 6
}

def encode(s):
    return order[s[0]] * 10 + (ord(s[1]) - ord('0'))

t = int(input())
out = []

for _ in range(t):
    a, b = input().split()
    va = encode(a)
    vb = encode(b)

    if va < vb:
        out.append("hotter")
    elif va > vb:
        out.append("cooler")
    else:
        out.append("same")

print("\n".join(out))
```Giải pháp bắt đầu bằng việc xác định thứ tự cố định của các lớp quang phổ. Điều này tránh logic có điều kiện lặp đi lặp lại trong quá trình so sánh. Mỗi chuỗi phân loại được chuyển đổi thành một số nguyên nhỏ gọn bằng cách sử dụng chức năng mã hóa, tách riêng phần đóng góp chữ cái và phần đóng góp chữ số. 

Một lỗi phổ biến là so sánh trực tiếp các chuỗi. Mặc dù nó có thể chuyển một số tập dữ liệu nhưng nó dựa vào thứ tự ASCII và không mã hóa rõ ràng thứ tự khoa học. Một vấn đề tế nhị khác là quên rằng các giá trị được mã hóa nhỏ hơn tương ứng với các ngôi sao nóng hơn, do đó hướng so sánh phải được căn chỉnh cẩn thận với ý nghĩa của ánh xạ. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
O2 O7
```| Bước | Trái | Đúng | Mã Hóa Trái | Được Mã Hóa Đúng | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| Phân tích | O2 | O7 | - | - | - | 
| Mã hóa chữ cái | O→0 | O→0 | 0*10+2=2 | 0*10+7=7 | - | 
| So sánh | 2 | 7 | 2 < 7 | nóng hơn | nóng hơn | 

Điều này cho thấy rằng trong cùng một lớp, chữ số thấp hơn có nghĩa là nóng hơn. 

Bây giờ hãy xem xét:```
A0 B9
```| Bước | Trái | Đúng | Mã Hóa Trái | Được Mã Hóa Đúng | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| Phân tích | A0 | B9 | - | - | - | 
| Mã hóa chữ cái | A→2 | B→1 | 2*10+0=20 | 1*10+9=19 | - | 
| So sánh | 20 | 19 | 20 > 19 | mát hơn | mát hơn | 

Điều này thể hiện thứ tự giữa các lớp, trong đó bất kỳ ngôi sao lớp B nào cũng nóng hơn bất kỳ ngôi sao lớp A nào, bất kể chữ số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp thử nghiệm thực hiện phân tích cú pháp và so sánh theo thời gian không đổi | 
| Không gian | O(1) | Chỉ sử dụng ánh xạ cố định và lưu trữ đầu ra | 

Giải pháp này phù hợp một cách thoải mái với các ràng buộc vì ngay cả 1000 thao tác cũng không đáng kể trong giới hạn thời gian nghiêm ngặt. Mỗi thao tác là số học đơn giản và lập chỉ mục chuỗi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    order = {'O':0,'B':1,'A':2,'F':3,'G':4,'K':5,'M':6}

    def encode(s):
        return order[s[0]] * 10 + (ord(s[1]) - ord('0'))

    t = int(input())
    out = []
    for _ in range(t):
        a, b = input().split()
        va = encode(a)
        vb = encode(b)
        if va < vb:
            out.append("hotter")
        elif va > vb:
            out.append("cooler")
        else:
            out.append("same")

    return "\n".join(out)

# provided samples
assert run("5\nO2 O7\nM9 M2\nG2 G2\nA0 B9\nF8 K1") == "hotter\ncooler\nsame\ncooler\nhotter"

# custom cases
assert run("1\nO0 O0") == "same"
assert run("1\nO0 M9") == "hotter"
assert run("1\nM9 O0") == "cooler"
assert run("1\nG9 G0") == "cooler"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| O0 O0 | giống nhau | ranh giới cực đoan giống hệt nhau | 
| O0 M9 | nóng hơn | so sánh toàn dải | 
| M9 O0 | mát hơn | đảo ngược toàn dải | 
| G9 G0 | mát hơn | đảo ngược thứ tự chữ số trong lớp | 

## Vỏ cạnh 

Trường hợp một cạnh là so sánh các ngôi sao giống hệt nhau như`M9 M9`. Mã hóa cho cả hai kết quả là 6*10+9=69 và thuật toán đưa ra kết quả chính xác giống nhau vì đẳng thức được bảo toàn chính xác. 

Một trường hợp cạnh khác là ranh giới giữa các lớp, chẳng hạn như`B0 A9`. Mã hóa mang lại B0 = 1_10+0=10 và A9 = 2_10+9=29, do đó B0 được xác định chính xác là nóng hơn mặc dù có lợi thế về phạm vi chữ số nhỏ hơn trong lớp khác. 

Trường hợp cuối cùng là đảo ngược nội bộ lớp như`F8 F1`. Mã hóa cho 38 và 31 tương ứng, do đó 38 > 31 và đầu ra mát hơn đối với ngôi sao đầu tiên, phù hợp với thực tế là chữ số cao hơn có nghĩa là lạnh hơn trong một lớp.
