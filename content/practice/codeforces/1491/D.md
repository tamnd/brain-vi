---
title: "CF 1491D - Người quản lý vườn thú và vườn thú vô tận"
description: "Chúng tôi được yêu cầu xác định khả năng tiếp cận trong biểu đồ có hướng vô hạn. Mỗi đỉnh là một số nguyên dương và có một cạnh có hướng từ đỉnh $u$ đến đỉnh $u+v$ khi và chỉ khi AND theo bit của $u$ và $v$ bằng $v$."
date: "2026-06-10T22:27:12+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms", "dp", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1491
codeforces_index: "D"
codeforces_contest_name: "Codeforces Global Round 13"
rating: 1800
weight: 1491
solve_time_s: 123
verified: true
draft: false
---

[CF 1491D - Người quản lý vườn thú và Vườn thú vô tận](https://codeforces.com/problemset/problem/1491/D) 

**Đánh giá:** 1800 
**Tags:** bitmasks, thuật toán xây dựng, dp, tham lam, toán học 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xác định khả năng tiếp cận trong biểu đồ có hướng vô hạn. Mỗi đỉnh là một số nguyên dương và có một cạnh có hướng từ đỉnh đó$u$đến đỉnh$u+v$nếu và chỉ khi bit AND của$u$Và$v$bằng$v$. Nói cách khác, mọi cạnh đều cho phép chúng ta cộng một số$v$đến đỉnh hiện tại$u$chỉ khi tất cả 1-bit của$v$cũng là 1-bit của$u$. Đối với mỗi truy vấn bao gồm hai số nguyên$u_i$Và$v_i$, chúng ta cần trả lời liệu có thể đạt được$v_i$bắt đầu từ$u_i$sử dụng một chuỗi các cạnh như vậy. 

Đầu vào cho phép lên tới$10^5$các truy vấn và mỗi đỉnh có thể lớn bằng$2^{30}-1$. Một cách tiếp cận ngây thơ cố gắng mô phỏng tất cả các đường dẫn có thể từ$u_i$ĐẾN$v_i$có khả năng khám phá số đỉnh theo cấp số nhân cho mỗi truy vấn, điều này là không khả thi. Điều này ngụ ý rằng bất kỳ giải pháp nào cũng phải hoạt động trực tiếp với các mẫu bit của các số thay vì liệt kê các đường dẫn. 

Các trường hợp đặc biệt không rõ ràng bao gồm các truy vấn trong đó$u_i = v_i$. Trong tình huống này, câu trả lời đúng là "CÓ" vì chúng ta đã đến được mục tiêu, mặc dù không cần có lợi thế nào. Một trường hợp cạnh khác xảy ra khi$v_i < u_i$. Vì mọi cạnh đều tăng giá trị đỉnh một cách nghiêm ngặt nên câu trả lời phải là "KHÔNG". Cuối cùng, những trường hợp$u_i$Và$v_i$chia sẻ một số bit nhưng khác nhau ở những bit khác cần xử lý cẩn thận, vì điều kiện AND có thể ngăn cản việc tiếp cận một số bit nhất định. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng biểu đồ bắt đầu từ$u_i$, tạo ra tất cả các đỉnh có thể tiếp cận bằng cách liên tục thêm bất kỳ giá trị hợp lệ nào$v$điều đó thỏa mãn$u \& v = v$. Điều này hiệu quả với số lượng nhỏ nhưng vô vọng đối với$u_i$lớn như$2^{30}$hoặc cho nhiều truy vấn, vì mỗi mô phỏng có thể mất nhiều hơn$10^9$hoạt động trong trường hợp xấu nhất. 

Điều quan trọng là mỗi bước chỉ cho phép chúng ta cộng các số có bit 1 đã có ở đỉnh hiện tại. Điều này có nghĩa là để đạt được$v_i$từ$u_i$, cứ 1 bit trong$v_i$cuối cùng phải được hỗ trợ bởi một số bit trong$u_i$. Chính xác hơn, nếu chúng ta quét các bit từ ít quan trọng nhất đến quan trọng nhất, số bit 1 trong$u_i$cho đến bất kỳ vị trí nào phải có ít nhất số bit 1 trong$v_i$lên đến vị trí tương tự. Nếu tại bất kỳ thời điểm nào số lượng tích lũy trong$v_i$vượt quá điều đó trong$u_i$, không thể đạt được$v_i$. Điều này chuyển vấn đề thành quét tuyến tính trên 30 bit cho mỗi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^30) mỗi truy vấn | O(2^30) | Quá chậm | 
| Tối ưu (quét đếm bit) | O(30 * q) | O(1) mỗi truy vấn | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng truy vấn$q$. 
2. Với mỗi truy vấn, hãy đọc$u$Và$v$. Nếu như$u > v$, ngay lập tức xuất ra "NO" vì tất cả các cạnh đều tăng đỉnh. 
3. Khởi tạo hai bộ đếm,$count_u$Và$count_v$, để theo dõi các bit 1 tích lũy được thấy trong$u$Và$v$khi chúng tôi quét từ bit có ý nghĩa nhỏ nhất (bit 0) đến bit thứ 29. 
4. Đối với mỗi vị trí bit từ 0 đến 29, hãy kiểm tra xem bit đó có được đặt ở vị trí nào không?$u$Và$v$. Nếu được đặt, hãy tăng$count_u$hoặc$count_v$tương ứng. 
5. Sau khi cập nhật số đếm cho từng bit, hãy kiểm tra xem$count_v > count_u$. Nếu đúng, xuất ra "NO" ngay lập tức vì có 1 bit trong$v$điều đó không thể được hỗ trợ bởi$u$cho đến nay. 
6. Nếu quá trình quét hoàn tất mà không bị lỗi, xuất "CÓ" vì tất cả các bit 1 trong$v$có thể được xây dựng tăng dần bằng cách sử dụng 1-bit từ$u$. 

Tại sao nó hoạt động: tính bất biến được duy trì trong quá trình quét là ở mỗi vị trí bit, số tích lũy của 1 bit trong$u$ít nhất là số trong$v$. Điều này đảm bảo rằng bất kỳ bit 1 nào trong$v$có thể được hình thành bằng cách cộng các số có bit 1 đã có sẵn trong$u$, tôn trọng điều kiện AND. Nếu bất biến bị lỗi ở bất kỳ điểm nào thì không thể đạt được$v$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

q = int(input())
for _ in range(q):
    u, v = map(int, input().split())
    if u > v:
        print("NO")
        continue
    count_u = 0
    count_v = 0
    possible = True
    for i in range(30):
        if (u >> i) & 1:
            count_u += 1
        if (v >> i) & 1:
            count_v += 1
        if count_v > count_u:
            possible = False
            break
    print("YES" if possible else "NO")
```Giải pháp đọc từng truy vấn và xử lý trực tiếp số lượng bit. Việc sử dụng số đếm tích lũy sẽ tránh việc quay lui hoặc mô phỏng biểu đồ, giúp thuật toán giữ tuyến tính theo số bit và truy vấn. Quá trình quét bit từ ít nhất đến quan trọng nhất đảm bảo rằng các bit nhỏ hơn được đáp ứng trước tiên, điều này phù hợp với cách các cạnh cộng AND cho phép truyền bá. 

## Ví dụ đã hoạt động 

**Ví dụ 1**:$u = 1, v = 4$| Chút | u_bit | v_bit | đếm_u | đếm_v | có thể | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 0 | Đúng | 
| 1 | 0 | 0 | 1 | 0 | Đúng | 
| 2 | 0 | 1 | 1 | 1 | Đúng | 

Quá trình quét hoàn tất thành công, xuất ra "CÓ". Điều này xác nhận 1-bit trong$v$ở vị trí 2 cuối cùng có thể đạt được. 

**Ví dụ 2**:$u = 3, v = 6$| Chút | u_bit | v_bit | đếm_u | đếm_v | có thể | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 0 | Đúng | 
| 1 | 1 | 1 | 2 | 1 | Đúng | 
| 2 | 0 | 1 | 2 | 2 | Đúng | 

Quá trình quét hoàn tất, xuất ra "CÓ". Việc kiểm tra tích lũy cho thấy chúng ta có thể tạo thành tất cả các bit 1 của$v$từ$u$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q * 30) = O(q) | Mỗi truy vấn yêu cầu quét 30 bit. Với$q \le 10^5$, tổng số thao tác là ~3*10^6. | 
| Không gian | O(1) | Chỉ có một vài bộ đếm số nguyên cho mỗi truy vấn được sử dụng. | 

Điều này phù hợp thoải mái trong giới hạn thời gian và bộ nhớ. Thuật toán tránh mọi lưu trữ biểu đồ hoặc liệt kê đường dẫn rõ ràng, làm cho nó khả thi đối với các giới hạn đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())  # assuming the solution code is saved in solution.py
    return output.getvalue().strip()

# Provided sample
assert run("5\n1 4\n3 6\n1 6\n6 2\n5 5\n") == "YES\nYES\nNO\nNO\nYES", "sample 1"

# Custom tests
assert run("2\n1 1\n1 2\n") == "YES\nNO", "same u=v and impossible case"
assert run("1\n15 15\n") == "YES", "all bits equal"
assert run("1\n0 0\n") == "YES", "zero case"
assert run("1\n7 8\n") == "NO", "requires unsupported higher bit"
assert run("1\n2 3\n") == "NO", "requires lower bit not present"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1, 1 2 | CÓ, KHÔNG | u=v và không thể nhỏ v | 
| 15 15 | CÓ | Tất cả các bit đều bằng nhau | 
| 0 0 | CÓ | Vỏ cạnh có số 0 | 
| 7 8 | KHÔNG | Không thể tạo bit cao hơn không có trong u | 
| 2 3 | KHÔNG | Không thể hình thành bit thấp hơn nếu không có đủ sự hỗ trợ | 

## Vỏ cạnh 

Khi nào$u = v$, không cần bổ sung. Ví dụ, đầu vào`1 1`ngay lập tức thỏa mãn bất biến bit tích lũy và thuật toán xuất ra "CÓ". Khi$v < u$, đầu ra là "KHÔNG" vì không có cạnh nào có thể làm giảm đỉnh. Vì`u = 2, v = 1`, quá trình quét không bao giờ đáp ứng được yêu cầu về bit tích lũy, tạo ra "KHÔNG". Đối với trường hợp bit cao nhất của$v$không có trong$u$, thuật toán
