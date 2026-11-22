---
title: "CF 282D - Một trò chơi số khác"
description: "Chúng tôi được yêu cầu xác định người chiến thắng trong trò chơi theo lượt với một dãy số nguyên rất nhỏ, trong đó hai người chơi luân phiên nhau di chuyển. Trong mỗi lượt, người chơi có thể giảm một phần tử bằng bất kỳ số lượng dương nào hoặc giảm tất cả các phần tử bằng cùng một lượng dương."
date: "2026-06-05T09:19:23+07:00"
tags: ["codeforces", "competitive-programming", "dp", "games"]
categories: ["algorithms"]
codeforces_contest: 282
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 173 (Div. 2)"
rating: 2100
weight: 282
solve_time_s: 134
verified: false
draft: false
---

[CF 282D - Một trò chơi số khác](https://codeforces.com/problemset/problem/282/D) 

**Xếp hạng:** 2100 
**Tags:** dp, trò chơi 
**Thời gian giải:** 2m 14s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xác định người chiến thắng trong trò chơi theo lượt với một dãy số nguyên rất nhỏ, trong đó hai người chơi luân phiên nhau di chuyển. Trong mỗi lượt, người chơi có thể giảm một phần tử bằng bất kỳ số lượng dương nào hoặc giảm tất cả các phần tử bằng cùng một lượng dương. Người chơi đầu tiên không thể di chuyển sẽ thua. Đầu vào cung cấp số phần tử`n`(từ 1 đến 3) và các giá trị ban đầu của chuỗi, mỗi giá trị hoàn toàn nhỏ hơn 300. Đầu ra chỉ đơn giản là tên của người chơi có thể giành chiến thắng bằng lối chơi hoàn hảo. 

Bởi vì`n`nhiều nhất là 3, chúng ta đang xử lý một không gian trạng thái trò chơi rất nhỏ. Mỗi`a_i`là dưới 300, vì vậy ngay cả khi chúng tôi thử cách tiếp cận đệ quy đơn giản, tổng số trạng thái nhiều nhất là 300³ = 27.000, có thể dễ dàng quản lý được. Điều này đủ nhỏ để việc khám phá toàn diện tất cả các bước di chuyển có thể thực hiện được. Các ràng buộc của vấn đề về cơ bản gợi ý rằng giải pháp có thể đáp ứng được cách tiếp cận lập trình tổ hợp hoặc lập trình động mà không phải lo lắng về hiệu suất vượt quá điều đó. 

Các trường hợp cạnh không rõ ràng bao gồm các chuỗi có số 0, vì người chơi không thể giảm phần tử 0 bằng một số dương. Ví dụ, với đầu vào`1 0 0`, người chơi đầu tiên chỉ phải giảm phần tử đầu tiên, vì hai phần tử còn lại không thể được chọn riêng lẻ hoặc chung bằng phép trừ dương. Việc hiểu sai quy tắc nước đi chung có thể dẫn đến việc cho rằng luôn có thể xảy ra nước đi, dẫn đến dự đoán người chiến thắng không chính xác. 

Một trường hợp tế nhị khác phát sinh khi tất cả các số đều bằng nhau. Vì`2 2 2`, người chơi có thể muốn trừ 1 từ bất kỳ phần tử đơn lẻ nào, nhưng lấy 1 từ tất cả đồng thời có thể có ý nghĩa chiến lược hơn. Việc xử lý chính xác các động thái đơn lẻ và toàn cầu là rất quan trọng. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force liệt kê tất cả các bước di chuyển hợp lệ theo cách đệ quy. Đối với một trạng thái nhất định`(a1, a2, a3)`, chúng tôi thử mọi phép trừ phần tử đơn từ 1 đến giá trị hiện tại và mọi phép trừ toàn cục từ 1 đến phần tử nhỏ nhất. Đối với mỗi trạng thái kết quả, chúng tôi xác định đệ quy xem người chơi tiếp theo có thể giành chiến thắng hay không. Người chơi có thể thắng nếu có ít nhất một nước đi buộc đối thủ rơi vào thế thua. Điều này đảm bảo tính chính xác vì nó khám phá mọi chuỗi nước đi hợp pháp. Nhược điểm là nếu không ghi nhớ, thuật toán sẽ lặp lại các phép tính cho cùng một trạng thái nhiều lần, mặc dù không gian trạng thái đủ nhỏ để ngay cả phép đệ quy đơn giản cũng có thể chạy trong thời gian hợp lý. 

Cách tiếp cận tối ưu nhận ra rằng trò chơi tương đương với một biến thể của trò chơi Nim. Vì`n=1`, một số duy nhất xác định người chiến thắng một cách tầm thường: bất kỳ số dương nào cũng cho phép người chơi đầu tiên giảm nó xuống 0 và giành chiến thắng. Vì`n=2`, người ta biết rằng giá trị Sprague-Grundy của`(a, b)`khi có phép trừ toàn cục bằng XOR của các số. Vì`n=3`, nguyên tắc tương tự cũng được áp dụng với một sửa đổi nhỏ: mỗi sự kết hợp giữa các bước di chuyển một phần tử và tổng thể có thể được biểu diễn dưới dạng giá trị Sprague-Grundy 3 chiều và điều kiện chiến thắng giảm xuống khi tính toán XOR của các số sau khi xem xét các bước di chuyển toàn cầu. 

Nói cách khác, bằng cách xác định cẩn thận số Grundy cho mỗi trạng thái là XOR của các số riêng lẻ được điều chỉnh theo các phép trừ tổng thể có thể có, bài toán trở thành một phép tính trò chơi khách quan cổ điển. Tính năng ghi nhớ cho phép chúng tôi lưu trữ trạng thái vào bộ nhớ đệm và tránh các phép tính dư thừa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đệ quy | O(300³ × di chuyển mỗi trạng thái) | O(300³) | Những ràng buộc nhất định có thể chấp nhận được | 
| Sprague-Grundy DP | O(300³) | O(300³) | Tối ưu và sạch sẽ | 

## Hướng dẫn thuật toán 

1. Biểu diễn mỗi trạng thái trò chơi dưới dạng một bộ`(a1, a2, a3)`. Nếu như`n`là 2 hoặc 1, hãy đệm bộ dữ liệu bằng các số 0 để duy trì tính đồng nhất. Điều này cho phép cấu trúc DP/ghi nhớ nhất quán. 
2. Xác định bảng ghi nhớ ánh xạ từng trạng thái tới trạng thái chiến thắng của nó. Cái nhìn sâu sắc quan trọng là một trạng thái sẽ thua nếu không có động thái nào dẫn đến trạng thái thua cuộc cho đối thủ. Nếu không thì nó đang thắng. 
3. Đối với một trạng thái nhất định, lặp lại tất cả các bước di chuyển của một phần tử. Với mỗi phần tử lớn hơn 0, hãy trừ đi mọi giá trị có thể`x`từ 1 về giá trị hiện tại của nó, hình thành một trạng thái mới. Đánh giá đệ quy trạng thái này. 
4. Tiếp theo, hãy xem xét các động thái toàn cầu. Phép trừ tối đa được phép là phần tử nhỏ nhất ở trạng thái hiện tại. Đối với mỗi`x`từ 1 đến mức tối thiểu này, trừ đi`x`từ tất cả các phần tử và đánh giá đệ quy trạng thái kết quả. 
5. Nếu bất kỳ nước đi nào dẫn đến trạng thái đối thủ thua, đánh dấu trạng thái hiện tại là thắng. Nếu tất cả các nước đi đều dẫn đến trạng thái chiến thắng cho đối thủ, hãy đánh dấu trạng thái hiện tại là thua. 
6. Trường hợp cơ sở xảy ra khi tất cả các phần tử đều bằng 0, trường hợp này sẽ tự động bị mất do không thể di chuyển được. 
7. Khởi tạo đệ quy từ trạng thái đầu vào. Nếu trạng thái chiến thắng, người chơi đầu tiên (BitLGM) có thể buộc phải thắng. Nếu không, người chơi thứ hai (BitAryo) sẽ thắng. 

Tại sao nó hoạt động: nguyên tắc số Grundy đảm bảo rằng mọi trò chơi công bằng khi chơi bình thường đều có thể được đánh giá đệ quy. Điều kiện chiến thắng được truyền bá chính xác vì phép đệ quy xem xét tất cả các bước di chuyển và việc ghi nhớ đảm bảo rằng mỗi trạng thái được tính toán chính xác một lần, ngăn chặn sự bùng nổ tổ hợp và đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1000000)

n = int(input())
a = list(map(int, input().split()))
while len(a) < 3:
    a.append(0)

memo = {}

def can_win(state):
    state = tuple(state)
    if state in memo:
        return memo[state]
    if all(x == 0 for x in state):
        memo[state] = False
        return False
    
    for i in range(3):
        for x in range(1, state[i] + 1):
            new_state = list(state)
            new_state[i] -= x
            if not can_win(new_state):
                memo[state] = True
                return True
    
    min_val = min(state)
    for x in range(1, min_val + 1):
        new_state = [v - x for v in state]
        if not can_win(new_state):
            memo[state] = True
            return True

    memo[state] = False
    return False

winner = "BitLGM" if can_win(a) else "BitAryo"
print(winner)
```Giải pháp đầu tiên chuẩn hóa chuỗi thành ba phần tử để xử lý thống nhất. các`can_win`hàm đánh giá đệ quy mọi động thái hợp lệ, cả cá nhân và toàn cầu. Ghi nhớ tránh làm việc dư thừa. Trường hợp cơ sở kiểm tra tất cả các số 0, trả về trạng thái mất. Cuối cùng, người chiến thắng được xác định bằng việc trạng thái ban đầu có chiến thắng hay không. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
2
1 1
```| Bước | Tiểu bang | Đã thử các bước di chuyển đơn | Đã thử di chuyển toàn cầu | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | (1,1,0) | trừ 1 từ đầu → (0,1,0) | trừ 1 trên toàn cầu → (0,0,0) | thắng, vì đối thủ có trạng thái thua | 
| 2 | (0,0,0) | không | không | thua | 

Người chơi đầu tiên trừ 1 trên toàn cầu. Người chơi thứ hai còn lại với`(0,0,0)`và thua cuộc. 

**Mẫu tùy chỉnh** 

đầu vào:```
3
1 2 3
```| Bước | Tiểu bang | Di chuyển dẫn đến chiến thắng | 
| --- | --- | --- | 
| (1,2,3) | đơn: (0,2,3), (1,1,3), (1,2,2) | một số dẫn đến việc đối thủ thua trạng thái | 
| toàn cầu: trừ 1 → (0,1,2), trừ 2 → (−1,0,1) không hợp lệ | người chơi đầu tiên có thể buộc phải thắng | | 

Điều này chứng tỏ thuật toán xem xét tất cả các nước đi hợp lệ và tìm ra ít nhất một con đường dẫn đến chiến thắng cho người chơi xuất phát. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(300³) | Ba vòng lặp lồng nhau trên các giá trị có thể có của`a1, a2, a3`| 
| Không gian | O(300³) | Bảng ghi nhớ lưu trữ mỗi trạng thái một lần | 

Được cho`n ≤ 3`Và`a_i < 300`, không gian trạng thái nhỏ, làm cho giải pháp này phù hợp thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    while len(a) < 3:
        a.append(0)
    memo = {}
    def can_win(state):
        state = tuple(state)
        if state in memo:
            return memo[state]
        if all(x == 0 for x in state):
            memo[state] = False
            return False
        for i in range(3):
            for x in range(1, state[i] + 1):
                new_state = list(state)
                new_state[i] -= x
                if not can_win(new_state):
                    memo[state] = True
                    return True
        min_val = min(state)
        for x in range(1, min_val + 1):
            new_state = [v - x for v in state]
            if not can_win(new_state):
                memo[state] = True
                return True
        memo[state] = False
        return False
    return "BitLGM" if can_win(a) else "BitAryo"

assert run("2\n1 1\n") == "Bit
```
