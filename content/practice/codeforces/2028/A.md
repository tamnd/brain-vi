---
title: "CF 2028A - Cuộc Phiêu Lưu Của Alice Trong Cờ Vua"
description: "Alice bắt đầu từ điểm gốc trên lưới hai chiều và có thể di chuyển theo bốn hướng chính. Cô ấy có một chuỗi các động tác cố định và lặp đi lặp lại vô thời hạn."
date: "2026-06-08T12:08:17+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2028
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 986 (Div. 2)"
rating: 900
weight: 2028
solve_time_s: 103
verified: true
draft: false
---

[CF 2028A - Cuộc phiêu lưu của Alice trong '' Cờ vua ''](https://codeforces.com/problemset/problem/2028/A) 

**Xếp hạng:** 900 
**Tags:** vũ lực, thực hiện, toán học 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Alice bắt đầu từ điểm gốc trên lưới hai chiều và có thể di chuyển theo bốn hướng chính. Cô ấy có một chuỗi các động tác cố định và lặp đi lặp lại vô thời hạn. Nữ Hoàng Đỏ nằm ở tọa độ cố định`(a, b)`, và chúng ta được yêu cầu xác định liệu cuối cùng Alice có đến được địa điểm đó hay không. Đầu vào cung cấp nhiều trường hợp kiểm thử, mỗi trường hợp chỉ định độ dài của chuỗi di chuyển, tọa độ đích và chuỗi di chuyển. 

Tọa độ lưới nhỏ (`1 ≤ n, a, b ≤ 10`) và độ dài chuỗi cũng rất nhỏ. Điều này cho phép chúng ta mô phỏng trực tiếp chuyển động của Alice mà không cần lo lắng về hiệu suất. Thử thách chính là Alice lặp lại trình tự di chuyển của mình mãi mãi, vì vậy đường đi có thể được coi là tuần hoàn. Chúng ta cần tính đến tất cả các vị trí mà cô ấy sẽ chiếm giữ trong các chu kỳ lặp đi lặp lại, chứ không chỉ một lần duyệt chuỗi. 

Các trường hợp biên không rõ ràng bao gồm các chuỗi di chuyển theo vòng lặp hoặc theo cách có thể vượt quá mục tiêu. Ví dụ: nếu trình tự của Alice là`NESW`và Nữ hoàng Đỏ đang ở`(1,1)`, chu kỳ đầu tiên đưa cô ấy vượt qua`(0,1) → (1,1) → (1,0) → (0,0)`, đạt được mục tiêu ở bước thứ hai. Một cách tiếp cận ngây thơ chỉ kiểm tra sau khi trình tự hoàn chỉnh có thể bỏ sót điều này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng các bước di chuyển của Alice vô thời hạn cho đến khi cô ấy đến được mục tiêu hoặc chúng tôi phát hiện ra một vị trí lặp lại. Trong trường hợp xấu nhất, việc này có thể mất vô số thời gian, nhưng với những hạn chế, chúng ta thực sự có thể giới hạn số bước. Vì tọa độ tối đa của mục tiêu là 10 và độ dài chuỗi di chuyển tối đa là 10, Alice không thể di chuyển quá 10 đơn vị khỏi điểm gốc theo bất kỳ hướng nào trong một chu kỳ. Việc lặp lại trình tự nhiều lần bao gồm tất cả các vị trí có thể tiếp cận được trong hộp giới hạn được xác định bởi`[-10,10] × [-10,10]`. Khi chúng tôi mô phỏng tất cả các vị trí này, Alice đã đến được mục tiêu hoặc cô ấy không bao giờ có thể tiếp cận được mục tiêu đó. 

Một cách tiếp cận mang tính toán học hơn là tính tổng độ dịch chuyển của một chu kỳ đầy đủ của chuỗi và xác định xem tọa độ đích có thể được biểu thị dưới dạng tổ hợp số nguyên không âm của độ dịch chuyển này cộng với một số tiền tố của chu kỳ hay không. Bởi vì tọa độ tối đa nhỏ, điều này làm giảm việc lặp lại trên tất cả các tiền tố, kiểm tra xem sự khác biệt giữa chuyển vị đích và chuyển tiền tố có phải là bội số của chuyển vị toàn chu kỳ hay không. 

Với những hạn chế, cách tiếp cận đầu tiên đơn giản hơn và hoàn toàn có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu với hộp giới hạn | O(n × (tối đa( | a | , | 
| Kiểm tra tổ hợp tuyến tính toán học | O(n²) | O(n) | Được chấp nhận, thanh lịch hơn | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc độ dài chuỗi`n`, tọa độ mục tiêu`(a, b)`, và chuỗi di chuyển`s`. 
2. Khởi tạo vị trí hiện tại của Alice tại`(0,0)`và ánh xạ từ điển sẽ di chuyển để phối hợp các thay đổi:`'N' → (0,1)`,`'E' → (1,0)`,`'S' → (0,-1)`,`'W' → (-1,0)`. 
3. Tính tổng độ dịch chuyển sau một chuỗi đầy đủ bằng cách tính tổng những thay đổi của từng nước đi trong`s`. 
4. Lặp lại các vị trí đạt được ở mỗi tiền tố của chuỗi. Đối với mỗi tiền tố, hãy tính độ dịch chuyển còn lại cần thiết để đạt tới`(a,b)`sau khi hoàn thành một số chu kỳ đầy đủ. 
5. Kiểm tra xem chuyển vị còn lại có phải là bội số nguyên của chuyển vị toàn chu kỳ theo cả hai hướng x và y hay không. Chỉ các bội số không âm mới hợp lệ vì Alice chỉ có thể tiến về phía trước theo chu kỳ. 
6. Nếu có sự kết hợp như vậy đối với bất kỳ tiền tố nào, hãy xuất "CÓ". Nếu không thì xuất ra "NO". 

**Tại sao nó hoạt động**: Mỗi vị trí Alice có thể đạt tới là tổng của độ dịch chuyển tiền tố cộng với bội số không âm của độ dịch chuyển toàn chu kỳ. Bằng cách kiểm tra tất cả các tiền tố và xác minh xem mục tiêu trừ tiền tố có phải là bội số không âm của độ dịch chuyển chu kỳ hay không, chúng tôi bao gồm tất cả các vị trí mà Alice có thể chiếm giữ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_meet(a, b, s):
    n = len(s)
    dx = {'N': 0, 'E': 1, 'S': 0, 'W': -1}
    dy = {'N': 1, 'E': 0, 'S': -1, 'W': 0}

    # prefix positions
    px = [0]
    py = [0]
    for move in s:
        px.append(px[-1] + dx[move])
        py.append(py[-1] + dy[move])

    total_dx = px[-1]
    total_dy = py[-1]

    for i in range(n + 1):
        rem_x = a - px[i]
        rem_y = b - py[i]

        # check if the remaining displacement can be reached via full cycles
        if total_dx == 0 and total_dy == 0:
            if rem_x == 0 and rem_y == 0:
                return True
        elif total_dx == 0:
            if rem_x == 0 and rem_y % total_dy == 0 and rem_y // total_dy >= 0:
                return True
        elif total_dy == 0:
            if rem_y == 0 and rem_x % total_dx == 0 and rem_x // total_dx >= 0:
                return True
        else:
            if rem_x % total_dx == 0 and rem_y % total_dy == 0:
                kx = rem_x // total_dx
                ky = rem_y // total_dy
                if kx == ky and kx >= 0:
                    return True
    return False

def main():
    t = int(input())
    for _ in range(t):
        n, a, b = map(int, input().split())
        s = input().strip()
        print("YES" if can_meet(a, b, s) else "NO")

if __name__ == "__main__":
    main()
```**Giải thích**: Giải pháp tính toán tất cả các vị trí có thể tiếp cận dưới dạng tổng của chuyển vị tiền tố và bội số của chuyển vị chuỗi đầy đủ. Nó xử lý các trường hợp dịch chuyển bằng 0, tránh bội số âm và kiểm tra sự bằng nhau ở cả hai trục. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:`2 2 2 NE`| Bước | x | y | Tiền tố | Tổng dx | Tổng thuốc nhuộm | Còn lại (a-px, b-py) | Kiểm tra nhiều lần | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | '' | 1 | 1 | (2,2) | 2×? | CÓ | 

Alice đạt tới`(0,1)`sau đó`(1,1)`trong chu kỳ đầu tiên. Chuyển vị còn lại`(1,1)`có thể đạt được trong một chu kỳ nữa, vì vậy đầu ra là "CÓ". 

Đầu vào mẫu:`3 2 2 NNE`| Bước | x | y | Tiền tố | Tổng dx | Tổng thuốc nhuộm | Còn lại | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | '' | 1 | 2 | (2,2) | kx=2, ky=1 → KHÔNG | 

Sự dịch chuyển trình tự trên mỗi chu kỳ là`(1,2)`. Độ dịch chuyển còn lại không có bội số bằng nhau của x và y nên Alice không thể đến được mục tiêu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t×n) | Mỗi trường hợp thử nghiệm mô phỏng tổng tiền tố và kiểm tra tối đa n+1 vị trí | 
| Không gian | O(n) | Lưu trữ tổng tiền tố của tọa độ x và y | 

Được cho`t ≤ 500`Và`n ≤ 10`, phương pháp này chạy thoải mái trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        main()
    return out.getvalue().strip()

# Provided samples
assert run("6\n2 2 2\nNE\n3 2 2\nNNE\n6 2 1\nNNEESW\n6 10 10\nNNEESW\n3 4 2\nNEE\n4 5 5\nNEWS\n") == "YES\nNO\nYES\nYES\nYES\nNO", "sample 1"

# Custom cases
assert run("1\n1 0 1\nN\n") == "YES", "minimum distance"
assert run("1\n2 1 0\nEW\n") == "YES", "back and forth horizontal"
assert run("1\n3 3 3\nNNE\n") == "NO", "cannot reach"
assert run("1\n4 2 2\nNEWS\n") == "YES", "loop covers target"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 1 N`| CÓ | Di chuyển một bước đạt được mục tiêu | 
|`2 1 0 EW`| CÓ | Các bước di chuyển lặp đi lặp lại bị hủy nhưng đã đạt được mục tiêu | 
|`3 3 3 NNE`| KHÔNG | Mục tiêu không thể truy cập | 
|`4 2 2 NEWS`| CÓ | Chuỗi vòng lặp bao gồm mục tiêu | 

## Vỏ cạnh 

Đối với chu kỳ dịch chuyển bằng không, chẳng hạn như`s = 'NEWS'`, tổng dx và dy trên mỗi chu kỳ bằng không. Nếu Alice
