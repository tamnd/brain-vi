---
title: "CF 20B - Phương trình"
description: "Chúng ta được yêu cầu giải một phương trình bậc hai có dạng , trong đó , , và là các số nguyên trong khoảng . Mục tiêu là tìm tất cả các nghiệm thực của phương trình, đếm chúng và in chúng theo thứ tự tăng dần với độ chính xác cao."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 20
codeforces_index: "B"
codeforces_contest_name: "Codeforces Alpha Round 20 (Codeforces format)"
rating: 2000
weight: 20
solve_time_s: 65
verified: true
draft: false
---
[CF 20B - Phương trình](https://codeforces.com/problemset/problem/20/B) 

**Xếp hạng:** 2000 
**Thẻ:** toán 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu giải phương trình bậc hai có dạng$Ax^2 + Bx + C = 0$, Ở đâu$A$,$B$, Và$C$là các số nguyên trong khoảng$[-10^5, 10^5]$. Mục tiêu là tìm tất cả các nghiệm thực của phương trình, đếm chúng và in chúng theo thứ tự tăng dần với độ chính xác cao. Nếu phương trình có vô số nghiệm thì ta sẽ xuất ra$-1$. Nếu không có giải pháp, chúng tôi in$0$. 

Các hệ số đầu vào có thể bao gồm số 0, điều này tạo ra sự tinh tế. Ví dụ, nếu$A = 0$, phương trình giảm về tuyến tính ($Bx + C = 0$) và nếu cả hai$A = 0$Và$B = 0$, phương trình vô nghiệm (nếu$C \neq 0$) hoặc vô số nghiệm (nếu$C = 0$). 

Các ràng buộc đủ nhỏ để chúng ta không cần phải lo lắng về hiệu quả thuật toán ngoài số học đơn giản. Việc tính căn bậc hai và thực hiện kiểm tra có điều kiện đều diễn ra trong thời gian cho phép. Thách thức hoàn toàn nằm ở việc xử lý chính xác mọi trường hợp và duy trì độ chính xác về mặt số học. 

Các trường hợp khó khăn không rõ ràng bao gồm các tình huống sau. Nếu như$A = 0$Và$B = 0$, thì lời giải phụ thuộc hoàn toàn vào$C$. Ví dụ,$0x^2 + 0x + 5 = 0$không có rễ, trong khi$0x^2 + 0x + 0 = 0$có vô số rễ. Một sự tinh tế khác nảy sinh khi giá trị phân biệt bằng 0, tạo ra chính xác một gốc, gốc này không được trùng lặp ở đầu ra. Những phân biệt đối xử tiêu cực không tạo ra nguồn gốc thực sự. Việc xử lý độ chính xác của dấu phẩy động một cách chính xác cũng cần thiết để đảm bảo các gốc được in có ít nhất năm chữ số sau số thập phân. 

## Phương pháp tiếp cận 

Một cách tiếp cận ngây thơ là thử tất cả các giá trị có thể có của$x$trong một phạm vi, kiểm tra xem chúng có thỏa mãn không$Ax^2 + Bx + C = 0$đến độ chính xác nào đó. Phương pháp vũ phu này là không cần thiết, vì công thức bậc hai đưa ra nghiệm giải tích chính xác cho bất kỳ phương trình bậc hai nào, miễn là chúng ta xử lý các trường hợp suy biến một cách chính xác. Đếm$x$trong một khoảng thời gian lớn sẽ yêu cầu số lượng thao tác không giới hạn và không bao giờ có thể xác định được gốc một cách đáng tin cậy với độ chính xác cao. 

Cách tiếp cận tối ưu dựa vào đại số tiêu chuẩn. Vì$A \neq 0$, chúng tôi tính toán phân biệt$D = B^2 - 4AC$. Nếu như$D > 0$, có hai nghiệm phân biệt được cho bởi$(-B - \sqrt{D}) / (2A)$Và$(-B + \sqrt{D}) / (2A)$. Nếu như$D = 0$, có một gốc$-B / (2A)$. Nếu như$D < 0$, không có rễ thực sự. Đối với trường hợp tuyến tính ($A = 0$Và$B \neq 0$), có đúng một gốc$-C / B$. Nếu cả hai$A$Và$B$bằng 0, nghiệm có thể là nghiệm bằng 0 hoặc vô số nghiệm tùy thuộc vào việc$C$là số không. 

Cái nhìn sâu sắc quan trọng là xử lý các trường hợp tuyến tính và bậc hai một cách riêng biệt và sử dụng phân biệt đối xử cho phương trình bậc hai. Việc sắp xếp cẩn thận các điều kiện đảm bảo rằng chúng ta không cố gắng chia cho 0 hoặc lấy căn bậc hai của một số âm. Khi các gốc được tìm thấy, việc sắp xếp chúng đảm bảo thứ tự tăng dần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(Phạm vi của x) | O(1) | Quá chậm và thiếu chính xác | 
| Phân tích (phân biệt đối xử) | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$A$,$B$, Và$C$. Các hệ số này xác định phương trình$Ax^2 + Bx + C = 0$. 
2. Kiểm tra xem$A = 0$. Nếu đúng như vậy thì phương trình không phải là phương trình bậc hai mà là tuyến tính. 
3. Nếu$A = 0$Và$B = 0$, kiểm tra$C$. Nếu như$C = 0$, in$-1$cho vô số giải pháp. Ngược lại, in$0$không có giải pháp. 
4. Nếu$A = 0$Nhưng$B \neq 0$, tính nghiệm đơn$-C / B$, in$1$, sau đó là gốc có ít nhất năm chữ số sau số thập phân. 
5. Nếu$A \neq 0$, tính phân biệt$D = B^2 - 4AC$. 
6. Nếu$D < 0$, không có gốc thực sự, vì vậy hãy in$0$. 
7. Nếu$D = 0$, tính nghiệm đơn$-B / (2A)$và in$1$tiếp theo là gốc. 
8. Nếu$D > 0$, tính hai gốc$(-B - \sqrt{D}) / (2A)$Và$(-B + \sqrt{D}) / (2A)$. In$2$tiếp theo là các gốc theo thứ tự tăng dần. 

Tại sao nó hoạt động: Mỗi trường hợp đều loại trừ lẫn nhau và đầy đủ. Trường hợp tuyến tính được xử lý riêng để tránh chia cho 0. Công thức bậc hai với phân tích phân biệt đảm bảo việc đếm và tính toán chính xác các nghiệm, đồng thời việc sắp xếp hai nghiệm đảm bảo thứ tự tăng dần. Không có trường hợp cạnh nào bị thiếu vì mọi sự kết hợp của các hệ số 0/khác 0 đều được xem xét. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

A, B, C = map(int, input().split())

if A == 0:
    if B == 0:
        if C == 0:
            print(-1)
        else:
            print(0)
    else:
        x = -C / B
        print(1)
        print(f"{x:.10f}")
else:
    D = B * B - 4 * A * C
    if D < 0:
        print(0)
    elif D == 0:
        x = -B / (2 * A)
        print(1)
        print(f"{x:.10f}")
    else:
        sqrt_D = math.sqrt(D)
        x1 = (-B - sqrt_D) / (2 * A)
        x2 = (-B + sqrt_D) / (2 * A)
        x1, x2 = sorted([x1, x2])
        print(2)
        print(f"{x1:.10f}")
        print(f"{x2:.10f}")
```Mã này phân tách các trường hợp tuyến tính và bậc hai để tránh phép chia bằng 0 và xử lý chính xác các nghiệm vô hạn. Chúng tôi sử dụng`math.sqrt`chỉ dành cho các phân biệt tích cực và sắp xếp các gốc khi có hai để đảm bảo thứ tự tăng dần. In bằng`.10f`cung cấp đủ độ chính xác cho yêu cầu của bài toán. 

## Ví dụ đã hoạt động 

**Mẫu 1:**`1 -5 6`| Bước | A | B | C | D | Rễ | Đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | 
| Đầu vào | 1 | -5 | 6 | 25-24=1 | 2, 3 | 2, 2.0, 3.0 | 

Điều này cho thấy một phương trình bậc hai tiêu chuẩn có hai nghiệm phân biệt. Thuật toán tính toán chính xác phân biệt và sắp xếp các nghiệm. 

**Mẫu tùy chỉnh 2:**`0 0 0`| Bước | A | B | C | Rễ | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| Đầu vào | 0 | 0 | 0 | Vô hạn | -1 | 

Điều này thể hiện trường hợp nghiệm vô hạn. Kiểm tra tuyến tính bắt cả hai hệ số bằng 0 và trả về`-1`. 

**Mẫu tùy chỉnh 3:**`0 4 -8`| Bước | A | B | C | Rễ | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| Đầu vào | 0 | 4 | -8 | 2 | 1, 2.0 | 

Trường hợp phương trình tuyến tính xác định chính xác một nghiệm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học, căn bậc hai và kiểm tra có điều kiện được thực hiện. | 
| Không gian | O(1) | Chỉ một số biến được sử dụng cho các hệ số và nghiệm. | 

Thuật toán này nằm trong giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        # call the solution
        A, B, C = map(int, input().split())
        if A == 0:
            if B == 0:
                if C == 0:
                    print(-1)
                else:
                    print(0)
            else:
                x = -C / B
                print(1)
                print(f"{x:.10f}")
        else:
            D = B * B - 4 * A * C
            if D < 0:
                print(0)
            elif D == 0:
                x = -B / (2 * A)
                print(1)
                print(f"{x:.10f}")
            else:
                sqrt_D = math.sqrt(D)
                x1 = (-B - sqrt_D) / (2 * A)
                x2 = (-
```
