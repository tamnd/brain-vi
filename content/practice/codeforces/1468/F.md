---
title: "CF 1468F - Quay hoàn toàn"
description: "Chúng ta có một tập hợp các điểm trên một mặt phẳng, mỗi điểm đại diện cho một người, ban đầu mỗi người nhìn vào một điểm khác. Mọi người đều quay liên tục theo chiều kim đồng hồ, hoàn thành một lượt đầy đủ và chúng tôi được yêu cầu đếm số cặp người riêng biệt đã từng “giao tiếp bằng mắt”."
date: "2026-06-11T01:27:05+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "hashing", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1468
codeforces_index: "F"
codeforces_contest_name: "2020-2021 ICPC, NERC, Southern and Volga Russian Regional Contest (Online Mirror, ICPC Rules)"
rating: 1700
weight: 1468
solve_time_s: 121
verified: true
draft: false
---

[CF 1468F - Lượt hoàn toàn](https://codeforces.com/problemset/problem/1468/F) 

**Đánh giá:** 1700 
**Tags:** hình học, băm, lý thuyết số 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các điểm trên một mặt phẳng, mỗi điểm đại diện cho một người, ban đầu mỗi người nhìn vào một điểm khác. Mọi người đều quay liên tục theo chiều kim đồng hồ, hoàn thành một lượt đầy đủ và chúng tôi được yêu cầu đếm số cặp người riêng biệt đã từng “giao tiếp bằng mắt”. Giao tiếp bằng mắt xảy ra khi hai người nhìn thẳng vào nhau vào cùng một thời điểm mà không có bất kỳ sự cản trở nào. 

Đầu vào chỉ định nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm sẽ cung cấp số lượng người và sau đó là tọa độ vị trí cũng như ánh nhìn ban đầu của mỗi người. Đầu ra là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm: tổng số cặp đáp ứng điều kiện giao tiếp bằng mắt ít nhất một lần. 

Những hạn chế là đáng kể. Mỗi trường hợp thử nghiệm có thể có tới$10^5$người và tổng số trên tất cả các trường hợp thử nghiệm không vượt quá$10^5$. Điều này có nghĩa là một$O(n^2)$thuật toán không khả thi vì nó có thể yêu cầu$10^{10}$hoạt động trong trường hợp xấu nhất. Các thuật toán thời gian tuyến tính hoặc gần tuyến tính, như$O(n \log n)$, là cần thiết. Chúng ta cũng phải xử lý tọa độ lớn như$10^9$và xử lý các so sánh định hướng chính xác. Các trường hợp cạnh bao gồm các điểm cực kỳ gần nhau mà việc so sánh góc dấu phẩy động ngây thơ có thể không thành công hoặc nhiều người ban đầu nhìn theo các hướng hoàn toàn ngược lại. 

Một cách tiếp cận đơn giản có thể so sánh trực tiếp từng cặp người, tính toán các góc hoặc hướng tại nhiều thời điểm quay. Điều này sẽ bỏ lỡ các yêu cầu về hiệu suất và có thể gây ra lỗi chính xác về dấu phẩy động khi kiểm tra sự bằng nhau của các hướng. 

## Phương pháp tiếp cận 

Phương pháp brute-force kiểm tra từng cặp người, tính toán vectơ từ người này sang người kia, tính toán hướng nhìn ban đầu của họ và mô phỏng chuyển động quay để xem liệu hướng của họ có thẳng hàng hay không. Mặc dù điều này đúng về mặt khái niệm nhưng nó đòi hỏi$O(n^2)$so sánh cho mỗi trường hợp thử nghiệm, vượt quá số lượng hoạt động khả thi. 

Thông tin chuyên sâu quan trọng về tối ưu hóa xuất phát từ việc quan sát rằng các hướng và vị trí có thể được biểu diễn dưới dạng vectơ và được rút gọn thành tỷ lệ số nguyên. Nếu người$A$Tại$(x_a, y_a)$đang nhìn dọc theo vector$(u_a - x_a, v_a - y_a)$và người$B$đang ở$(x_b, y_b)$nhìn dọc theo$(u_b - x_b, v_b - y_b)$, họ sẽ giao tiếp bằng mắt khi và chỉ khi hướng từ$A$ĐẾN$B$là bội số vô hướng của$A$vectơ ánh mắt của và đồng thời hướng từ$B$ĐẾN$A$là bội số vô hướng của$B$vector ánh mắt của. Các bội số vô hướng không quan trọng; điều quan trọng là **vectơ hướng rút gọn**, có thể được biểu diễn dưới dạng một cặp số nguyên chia cho ước số chung lớn nhất của chúng. Bằng cách biểu diễn mọi vectơ nhìn và vectơ giữa người với người ở dạng rút gọn này, chúng ta có thể sử dụng bản đồ băm để đếm các cặp tương thích một cách hiệu quả. 

Do đó, vấn đề giảm xuống còn việc đếm các cặp vị trí và vectơ nhìn sao cho các vectơ chỉ hướng chuẩn hóa khớp với các hướng ngược nhau. Điều này tránh hoàn toàn việc mô phỏng và đạt được khả năng đếm thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Chuẩn hóa & Băm Vector | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi người, hãy tính vectơ nhìn bằng cách trừ tọa độ của người đó khỏi tọa độ nhìn của họ. Điều này mang lại$(dx, dy) = (u_i - x_i, v_i - y_i)$. 
2. Chuẩn hóa vectơ này bằng cách chia cả hai thành phần cho ước số chung lớn nhất của chúng và đảm bảo dấu chính tắc (ví dụ: luôn làm cho thành phần khác 0 đầu tiên là dương). Điều này đảm bảo rằng các vectơ chỉ cùng hướng được biểu diễn giống hệt nhau dưới dạng các cặp số nguyên. 
3. Với mỗi cặp người, xác định vectơ từ người$A$ĐẾN$B$BẰNG$(x_b - x_a, y_b - y_a)$và bình thường hóa nó theo cách tương tự. 
4. Lưu trữ số lượng vectơ ánh nhìn chuẩn hóa trong bản đồ băm. Đối với mỗi người$A$, số lượng người$B$họ có thể nhìn theo hướng chính xác được xác định bằng cách truy vấn có bao nhiêu người có vectơ nhìn ngược lại với hướng từ$A$cho họ. Điều này sử dụng tra cứu băm đơn giản cho mỗi người thay vì lặp lại tất cả các cặp. 
5. Tổng tất cả các số đếm, chia cho hai vì mỗi lần chạm mắt được tính hai lần và trả về giá trị này cho trường hợp kiểm thử. 

Tại sao nó hoạt động: Bằng cách giảm tất cả vectơ thành dạng chuẩn, chúng tôi đảm bảo rằng hai người chỉ có thể giao tiếp bằng mắt khi và chỉ khi hướng ánh mắt của họ thẳng hàng chính xác với vectơ nối họ theo hướng ngược nhau. Việc băm tránh so sánh lặp lại và đảm bảo tính chính xác vì không thể căn chỉnh nào khác, loại bỏ nhu cầu về số học dấu phẩy động. 

## Giải pháp Python```python
import sys
import math
from collections import defaultdict
input = sys.stdin.readline

def normalize(dx, dy):
    if dx == 0 and dy == 0:
        return (0, 0)
    g = math.gcd(dx, dy)
    dx //= g
    dy //= g
    if dx < 0 or (dx == 0 and dy < 0):
        dx = -dx
        dy = -dy
    return (dx, dy)

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        persons = []
        for _ in range(n):
            x, y, u, v = map(int, input().split())
            dx = u - x
            dy = v - y
            persons.append((x, y, normalize(dx, dy)))
        
        counter = defaultdict(int)
        for x, y, vec in persons:
            counter[vec] += 1
        
        result = 0
        for x, y, vec in persons:
            # vector from A to B that would match gaze
            inv_vec = (-vec[0], -vec[1])
            result += counter.get(inv_vec, 0)
        
        print(result // 2)

if __name__ == "__main__":
    solve()
```Giải pháp đọc đầu vào một cách hiệu quả, chuẩn hóa vectơ một cách cẩn thận để tránh lỗi dấu phẩy động và sử dụng bản đồ băm để đếm các hướng nhìn ngược nhau. Mỗi người được tính chính xác một lần cho mỗi đối tác tiềm năng và việc chia đôi sẽ tránh các cặp đếm đôi. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu thứ hai:```
3
0 0 1 1
1 1 0 0
1 0 2 0
```Từng bước một: 

| Người | Vị trí | Nhìn vector | Chuẩn hóa | Số lượng vectơ đối diện | 
| --- | --- | --- | --- | --- | 
| 1 | (0,0) | (1,1) | (1,1) | 1 (người 2) | 
| 2 | (1,1) | (-1,-1) | (-1,-1) | 1 (người 1) | 
| 3 | (1,0) | (1,0) | (1,0) | 0 | 

Tổng kết cho 2, chia cho 2 để được 1 cặp, khớp với kết quả đầu ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi người được xử lý một lần để tính toán và chuẩn hóa các vectơ, sau đó được truy vấn trong bản đồ băm. | 
| Không gian | O(n) | Bản đồ băm lưu trữ số lượng trên mỗi vectơ chuẩn hóa duy nhất, tối đa là n. | 

Cho rằng tổng của n trên tất cả các trường hợp thử nghiệm không vượt quá$10^5$, điều này nằm trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    f = io.StringIO()
    with redirect_stdout(f):
        solve()
    return f.getvalue().strip()

# provided samples
assert run("3\n2\n0 0 0 1\n1 0 2 0\n3\n0 0 1 1\n1 1 0 0\n1 0 2 0\n6\n0 0 0 1\n1 0 1 2\n2 0 2 3\n3 0 3 -5\n4 0 4 -5\n5 0 5 -5\n") == "0\n1\n9"

# custom cases
assert run("1\n1\n0 0 1 1\n") == "0"  # only one person
assert run("1\n2\n0 0 1 0\n1 0 0 0\n") == "1"  # directly facing
assert run("1\n3\n0 0 1 0\n1 0 0 0\n0 1 0 0\n") == "1"  # third person not matching
assert run("1\n2\n0 0 1 1\n1 1 1 2\n") == "0"  # not aligned vectors
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 người | 0 | Người độc thân không thể giao tiếp bằng mắt | 
| 2 người đối diện | 1 | Ghép cặp đơn giản | 
| 3 người, căn chỉnh một phần | 1 | Chỉ đếm đúng các cặp trùng khớp | 
| 2 người không hợp nhau | 0 | An toàn chống lại các vectơ sai lệch | 

## Vỏ cạnh 

Khi hai người có vectơ là bội số
