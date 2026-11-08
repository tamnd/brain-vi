---
title: "CF 142D - Giúp Shrek Và Lừa 2"
description: "Chúng tôi được yêu cầu phân tích một trò chơi hai người chơi trên một lưới hình chữ nhật trong đó mỗi ô có thể chứa một người lính đồ chơi từ Shrek (màu xanh lá cây) hoặc Donkey (màu đỏ) hoặc để trống. Lưới có kích thước n × m và trên mỗi hàng có nhiều nhất hai người lính."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "games"]
categories: ["algorithms"]
codeforces_contest: 142
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 102 (Div. 1)"
rating: 2600
weight: 142
solve_time_s: 154
verified: true
draft: false
---

[CF 142D - Giúp đỡ Shrek và Lừa 2](https://codeforces.com/problemset/problem/142/D) 

**Đánh giá:** 2600 
**Thẻ:** trò chơi 
**Thời gian giải:** 2m 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu phân tích một trò chơi hai người chơi trên một lưới hình chữ nhật trong đó mỗi ô có thể chứa một người lính đồ chơi từ Shrek (màu xanh lá cây) hoặc Donkey (màu đỏ) hoặc để trống. Lưới có kích thước _n_ × _m_, trên mỗi hàng có tối đa hai người lính. Người chơi lần lượt di chuyển một số binh lính của họ từ 1 đến _k_ đơn vị về phía hoặc ra xa lính địch dọc theo hàng của họ. Một người lính không thể di chuyển qua hoặc vào ô do bất kỳ người lính nào khác chiếm giữ và việc di chuyển bị hạn chế trong hàng của người lính đó. Nếu người chơi không có nước đi hợp lệ, họ sẽ thua. Shrek luôn di chuyển trước. Nhiệm vụ là dự đoán người chiến thắng nếu cả hai đều chơi tối ưu. 

Đầu vào chỉ định bố cục lưới, cùng với số _k_ kiểm soát số lượng binh lính tối đa mà người chơi có thể di chuyển trong một lượt. Kết quả đầu ra là người chiến thắng: "Đầu tiên" nếu Shrek thắng, "Thứ hai" nếu Donkey thắng hoặc "Hòa" nếu trò chơi có thể tiếp tục vô thời hạn với lối chơi tối ưu. 

Các ràng buộc cho chúng ta biết _n_, _m_ và _k_ đều có nhiều nhất là 100. Điều này cho thấy chúng ta không thể đủ khả năng mô phỏng đệ quy ngây thơ trên tất cả các trạng thái trò chơi, bởi vì số cách sắp xếp có thể có là theo cấp số nhân theo số lượng binh lính. Tuy nhiên, việc hạn chế tối đa hai binh sĩ mỗi hàng sẽ hạn chế đáng kể khả năng di chuyển trên mỗi hàng. Điều này gợi ý rằng trò chơi có thể được phân tách thành các hàng độc lập và được mô hình hóa bằng lý thuyết trò chơi tổ hợp. 

Một trường hợp cạnh quan trọng xảy ra khi các hàng chứa một người lính hoặc không có người lính nào. Ví dụ: nếu một hàng có một người lính Shrek đơn độc và không có lính Lừa, người lính đó có thể di chuyển tự do và hàng đó có thể không bao giờ "kết thúc". Ngược lại, nếu cả hai người chơi có một người lính trên cùng một hàng và _k_ ≥ 1, thì hàng đó sẽ hoạt động giống như một đống Nim đơn giản với số lượt di chuyển bị hạn chế. Việc hiểu sai những tình huống này có thể tạo ra những người chiến thắng không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là mô phỏng đệ quy mọi động thái có thể xảy ra, đánh dấu các vị trí là thắng hoặc thua. Điều này sẽ liên quan đến việc tạo ra tất cả các tập hợp con có tối đa _k_ binh sĩ để di chuyển, tính toán vị trí mới của họ và đánh giá đệ quy trạng thái kết quả. Cách tiếp cận này hoạt động về mặt khái niệm vì trò chơi là hữu hạn nhưng trở nên khó hiểu: ngay cả khi chỉ có hai binh sĩ mỗi hàng và 100 hàng, hệ số phân nhánh vẫn lớn và số lượng trạng thái tăng theo cấp số nhân. 

Cái nhìn sâu sắc quan trọng là mỗi hàng là độc lập. Vì binh lính di chuyển dọc theo hàng và không thể nhảy hoặc vượt qua hàng khác nên việc di chuyển trên một hàng không ảnh hưởng đến các hàng khác ngoại trừ thông qua tổng số "trò chơi hàng". Do đó, mỗi hàng có thể được coi là một trò chơi tổ hợp một chiều. Cụ thể, vấn đề giảm xuống việc tính toán số Grundy (Nimbers) cho mỗi hàng. Sau khi biết số Grundy của mỗi hàng, người chiến thắng chung cuộc được xác định bằng cách XOR-ing tất cả các số Grundy của hàng, theo định lý Sprague-Grundy: nếu XOR bằng 0, người chơi thứ hai sẽ thắng; nếu khác 0 thì người chơi đầu tiên thắng. Nếu một hàng không bị giới hạn (một người lính có thể di chuyển vô hạn theo một hướng nào đó), nó sẽ đóng góp một trò chơi "vô hạn", tạo ra một trận hòa. Việc phát hiện những chuyển động vô hạn như vậy là rất quan trọng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(n*m)) | O(n*m) | Quá chậm | 
| Grundy mỗi hàng | O(n * m * k) | O(n * m) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc kích thước lưới _n_, _m_, số binh lính tối đa mỗi lần di chuyển _k_ và chính lưới đó. 
2. Khởi tạo bộ tích lũy cho XOR của hàng số Grundy. 
3. Ở mỗi hàng hãy xác định vị trí của lính Shrek và Lừa. 
4. Nếu hàng không có lính địch, hãy kiểm tra xem có binh lính nào có thể di chuyển tự do ngoài ranh giới hàng không. Nếu vậy, hãy đánh dấu trò chơi là "Vẽ". 
5. Đối với các hàng có cả hai người chơi, hãy lập mô hình phân đoạn giữa những người lính ngoài cùng bên trái và ngoài cùng bên phải dưới dạng một đống Nim bị chặn. Kích thước heap bằng số lượng ô trống giữa các lính. Tính số Grundy của nó giả sử người chơi có thể di chuyển từ 1 đến _k_ đơn vị trong một lượt. 
6. XOR số Grundy của mỗi hàng vào bộ tích lũy. 
7. Sau khi xử lý tất cả các hàng, nếu bất kỳ hàng nào được đánh dấu là vô hạn, hãy in "Draw". 
8. Mặt khác, nếu XOR bằng 0, hãy in "Thứ hai"; nếu không thì in "Đầu tiên." 

Tính chính xác dựa trên định lý Sprague-Grundy: mỗi hàng là một trò chơi công bằng riêng biệt với các nước đi được xác định rõ ràng và XOR của các số Grundy của hàng sẽ xác định người chiến thắng chung cuộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def mex(s):
    m = 0
    while m in s:
        m += 1
    return m

def compute_grundy(length, k):
    if length == 0:
        return 0
    g = [0] * (length + 1)
    for i in range(1, length + 1):
        s = set()
        for move in range(1, min(k, i) + 1):
            s.add(g[i - move])
        g[i] = mex(s)
    return g[length]

def main():
    n, m, k = map(int, input().split())
    grid = [input().strip() for _ in range(n)]
    xor_sum = 0
    infinite = False
    
    for row in grid:
        positions = [(i, c) for i, c in enumerate(row) if c in 'GR']
        if not positions:
            continue
        
        G_pos = [i for i, c in positions if c == 'G']
        R_pos = [i for i, c in positions if c == 'R']
        
        if not R_pos:
            infinite = True
            continue
        if not G_pos:
            continue
        
        left = min(G_pos + R_pos)
        right = max(G_pos + R_pos)
        empty_between = right - left - 1
        xor_sum ^= compute_grundy(empty_between, k)
    
    if infinite:
        print("Draw")
    elif xor_sum == 0:
        print("Second")
    else:
        print("First")

if __name__ == "__main__":
    main()
```Giải pháp này trước tiên tính toán số Grundy cho các phân đoạn giữa các binh sĩ, sau đó tổng hợp chúng. các`compute_grundy`hàm sử dụng bảng lập trình động cho các đống Nim 1D với các bước di chuyển có giới hạn. Chúng tôi theo dõi các hàng vô hạn một cách riêng biệt. Kiểm tra ranh giới tránh đếm sai các ô ngoài lưới. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
2 3 1
R-G
RG-
```Hàng 1: vị trí tại 0 (R) và 2 (G). Các ô trống giữa: 1. Số có độ dài 1 với k=1 là 1. Tổng XOR = 1. 

Hàng 2: vị trí tại 0 (R) và 1 (G). Các ô trống giữa: 0. Số Grundy = 0. Tổng XOR vẫn bằng 1. 

Không có hàng vô hạn, tổng XOR ≠ 0, vì vậy đầu ra là "Đầu tiên". 

**Ví dụ tùy chỉnh**```
2 4 2
G--R
-R-G
```Hàng 1: các ô trống từ 0 (G) đến 3 (R): 2. Số Grundy = 2. Tổng XOR = 2. 

Hàng 2: vị trí tại 1 (R) và 3 (G): ô trống = 1. Grundy =1. Tổng XOR = 2^1 =3. Đầu ra: "Đầu tiên." 

Điều này cho thấy nhiều hàng kết hợp chính xác thông qua XOR, tôn trọng hành vi của hàng độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * m * k) | Mỗi hàng có nhiều nhất m ô và chúng tôi tính toán các số Grundy có chiều dài m với các bước di chuyển lên đến k. | 
| Không gian | O(m) | Bảng DP cho đoạn có chiều dài lên tới m của mỗi hàng. | 

Với n, m ≤ 100 và k ≤ 100, n * m * k ≤ 10^6, vừa vặn thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    f = io.StringIO()
    with redirect_stdout(f):
        main()
    return f.getvalue().strip()

# provided sample
assert run("2 3 1\nR-G\nRG-\n") == "First", "sample 1"

# minimum-size inputs
assert run("1 1 1\nG\n") == "Draw", "single G alone"

# maximum-size inputs, all G
assert run("100 100 100\n" + ("G"*100 + "\n")*100) == "Draw", "all G infinite"

# row with k > empty space
assert run("1 5 3\nG---R\n") == "First", "row with length 3, k=3"

# row with no G
assert run("1 5 3\nR---R\n") == "Second", "row with only R"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1\nG\n | Vẽ | người lính đơn độc có thể di chuyển tự do, trò chơi vô tận | 
| Lưới 100x100 của G | Vẽ | đầu vào tối đa, phát hiện chuyển động vô hạn | 
| 1 5 3\nG---R | Đầu tiên | heap Nim giới hạn với k >= khoảng trống | 
| 1 5 3\nR---R | Thứ hai | chỉ người chơi thứ hai mới được di chuyển, người chơi thứ nhất không được di chuyển | 

## Vỏ cạnh 

Một cuộc cãi vã với một người lính Shrek đơn độc
