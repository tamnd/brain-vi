---
title: "CF 105925A - Mèo Schr\u00f6dinger mơ hồ"
description: "Chúng ta được cung cấp một hệ thống có hai thông tin nhị phân: hộp mở hay đóng và trạng thái thực tế bên trong của con mèo bên trong hộp. Giá trị đầu tiên, C, mô tả khả năng quan sát được. Nếu hộp đóng lại thì bên ngoài không thể quan sát được trạng thái của con mèo."
date: "2026-06-21T11:58:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105925
codeforces_index: "A"
codeforces_contest_name: "SBC Brazilian Phase Zero 2025"
rating: 0
weight: 105925
solve_time_s: 47
verified: true
draft: false
---

[CF 105925A - Con mèo Schr\u00f6dinger mơ hồ](https://codeforces.com/problemset/problem/105925/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hệ thống có hai thông tin nhị phân: hộp mở hay đóng và trạng thái thực tế bên trong của con mèo bên trong hộp. 

Giá trị đầu tiên, C, mô tả khả năng quan sát được. Nếu hộp đóng lại thì bên ngoài không thể quan sát được trạng thái của con mèo. Nếu hộp được mở, trạng thái sẽ hoàn toàn có thể quan sát được. 

Giá trị thứ hai, G, mô tả trạng thái bên trong thực sự của con mèo, còn sống hay đã chết. Tuy nhiên, giá trị này chỉ có ý nghĩa về mặt đầu ra khi hộp được mở, vì chỉ khi đó mới có thể xác nhận trạng thái trực tiếp. 

Nhiệm vụ là quyết định xem có thể nói gì về con mèo từ quan điểm của người quan sát bên ngoài. Nếu hộp bị đóng, cả hai khả năng vẫn hợp lệ bất kể giá trị ẩn là bao nhiêu, do đó hệ thống vẫn còn mơ hồ. Nếu hộp mở, người quan sát có thể trực tiếp xác định con mèo còn sống hay đã chết từ G. 

Kích thước đầu vào là không đổi, chỉ bao gồm hai bit thông tin, do đó không có ràng buộc thuật toán nào yêu cầu tối ưu hóa ngoài việc đánh giá thời gian không đổi. Bất kỳ cách tiếp cận nào ngoài phân tích trường hợp trực tiếp sẽ là chi phí không cần thiết. 

Một lỗi phổ biến là luôn tin tưởng G bất kể C. Ví dụ: nếu đầu vào là C = 1 và G = 0, người ta có thể đưa ra kết quả không chính xác rằng con mèo đã chết. Điều này sai vì khi đóng hộp, người quan sát không có quyền truy cập vào G nên cả hai kết quả vẫn không thể phân biệt được. 

Một sai lầm khác là bỏ qua G hoàn toàn và luôn tạo ra sự mơ hồ. Ví dụ, C = 0 và G = 1 phải đưa ra một câu trả lời chắc chắn, vì việc mở hộp sẽ làm mất đi sự không chắc chắn. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực là mô phỏng trực tiếp ý tưởng về khả năng quan sát: xem xét cả hai cách giải thích có thể có của hệ thống khi hộp được đóng và chỉ một cách giải thích khi nó mở. Khi C = 1, chúng ta sẽ coi con mèo có khả năng còn sống và có khả năng đã chết cùng một lúc vì không thể quan sát được. Khi C = 0, chúng ta chỉ cần đọc G và đưa ra trạng thái tương ứng. 

Lý luận thô bạo này có hiệu quả nhưng dư thừa về mặt khái niệm. Việc phân nhánh trên “các thực tế có thể xảy ra” khi hộp bị đóng là không cần thiết vì cả hai nhánh luôn hợp lệ bất kể G. Việc quan sát thấy C hoàn toàn kiểm soát liệu G có thể đọc được hay không sẽ rút gọn vấn đề thành một quy tắc có điều kiện đơn giản. 

Một khi chúng ta nhận ra rằng C đóng vai trò như một cổng kiểm tra xem G có quan trọng hay không thì toàn bộ vấn đề sẽ chuyển thành quyết định có hai trường hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) | O(1) | Cấu trúc quá chậm, không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai số nguyên C và G. Chúng biểu thị khả năng quan sát và trạng thái ẩn tương ứng. 
2. Kiểm tra xem C có bằng 1 hay không, nghĩa là hộp đã đóng. Trong trường hợp này, người quan sát không thể phân biệt giữa sống và chết bất kể G, do đó đầu ra phải phản ánh sự không chắc chắn. 
3. Nếu C bằng 0 thì hộp đang mở nên giá trị ẩn G hoàn toàn có thể quan sát được. Trong trường hợp này, ánh xạ G trực tiếp đến câu trả lời cuối cùng: 1 tương ứng với còn sống và 0 tương ứng với đã chết. 
4. In chuỗi trạng thái kết quả. 

### Tại sao nó hoạt động 

Tính đúng đắn xuất phát từ việc C phân chia vấn đề thành hai chế độ với khả năng truy cập thông tin cơ bản khác nhau. Khi C = 1, không có quan sát nào có thể làm giảm độ không đảm bảo, do đó cả hai trạng thái vẫn đồng thời duy trì các diễn giải có giá trị. Khi C = 0, hệ thống hiển thị G hoàn toàn, làm cho đầu ra mang tính xác định. Không có sự tương tác giữa nhiều đầu vào ngoài hành vi định tuyến này, do đó không có trạng thái trung gian nào có thể phát sinh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    C, G = map(int, input().split())
    
    if C == 1:
        print("vivo e morto")
    else:
        if G == 1:
            print("vivo")
        else:
            print("morto")

if __name__ == "__main__":
    solve()
```Việc triển khai mã hóa trực tiếp hai chế độ được mô tả trong thuật toán. Nhánh đầu tiên xử lý trường hợp hộp đóng, trong đó đầu ra được cố định bất kể G. Nhánh thứ hai xử lý trường hợp hộp mở, trong đó G được sử dụng làm tra cứu trực tiếp cho trạng thái cuối cùng. Không có trường hợp đặc biệt nào ngoài hai điều kiện này và không cần chuẩn hóa hoặc tiền xử lý bổ sung. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 0
```| Bước | C | G | Tình trạng | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | C = 1 | vivo e morto | 

Khi hộp được đóng lại, giá trị của G không liên quan. Mặc dù G bằng 0 nhưng người quan sát không thể truy cập nó, vì vậy cả người sống và người chết vẫn có thể diễn giải được. 

### Ví dụ 2 

đầu vào:```
0 1
```| Bước | C | G | Tình trạng | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | C = 0 | vivo | 

Ở đây hộp được mở nên giá trị ẩn được bộc lộ đầy đủ. Vì G = 1 nên con mèo chắc chắn còn sống. 

Dấu vết này cho thấy việc mở hộp loại bỏ hoàn toàn sự mơ hồ và giảm vấn đề xuống việc đọc trực tiếp G. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số lần kiểm tra có điều kiện không đổi và một lần đọc đầu vào | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Giải pháp này dễ dàng phù hợp với mọi ràng buộc vì nó thực hiện một số thao tác cố định không phụ thuộc vào kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue()

# To properly test, we redefine run with capture
def run(inp: str) -> str:
    old_in = sys.stdin
    old_out = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_in
        sys.stdout = old_out

# provided samples
assert run("1 0\n") == "vivo e morto", "sample 1"
assert run("0 1\n") == "vivo", "sample 2"

# custom cases
assert run("0 0\n") == "morto", "open box, dead cat"
assert run("1 1\n") == "vivo e morto", "closed box ignores G"
assert run("0 1\n") == "vivo", "open box, alive cat"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 | vivo e morto | hộp kín buộc sự mơ hồ | 
| 0 0 | tử thi | mở hộp đọc đúng trạng thái chết | 
| 1 1 | vivo e morto | xác nhận G bị bỏ qua khi đóng | 
| 0 1 | vivo | khẳng định quan sát trực tiếp khi mở | 

## Vỏ cạnh 

Sự khác biệt duy nhất có ý nghĩa về cạnh là liệu C có lật giữa 0 và 1 hay không. 

Đối với đầu vào:```
1 1
```thuật toán đi vào nhánh hộp đóng và ngay lập tức đưa ra sự mơ hồ, bỏ qua hoàn toàn G. Điều này xác nhận rằng ngay cả trạng thái bên trong rõ ràng “còn sống” cũng không ảnh hưởng đến khả năng quan sát khi hệ thống được niêm phong. 

Đối với đầu vào:```
0 0
```thuật toán đi vào nhánh hộp mở và đọc G trực tiếp. Vì G bằng 0 nên nó tạo ra “morto”, chứng tỏ rằng không còn sự mơ hồ khi C cho phép quan sát. 

Cả hai trường hợp đều xác nhận rằng thuật toán tách chính xác điều kiện nhận thức (C) khỏi trạng thái vật lý (G), đây là cấu trúc cốt lõi của vấn đề.
