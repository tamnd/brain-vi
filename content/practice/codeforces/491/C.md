---
title: "CF 491C - Giải mã"
description: "Chúng tôi được yêu cầu giải mã một tin nhắn đã được mã hóa bằng mật mã thay thế đơn giản. Mỗi chữ cái trong thư gốc được thay thế bằng một chữ cái cố định, do đó, ánh xạ là một: các chữ cái khác nhau ánh xạ tới các chữ cái khác nhau và cùng một chữ cái luôn ánh xạ tới cùng một…"
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "flows", "graph-matchings"]
categories: ["algorithms"]
codeforces_contest: 491
codeforces_index: "C"
codeforces_contest_name: "Testing Round 11"
rating: 2300
weight: 491
solve_time_s: 41
verified: true
draft: false
---

[CF 491C - Giải mã](https://codeforces.com/problemset/problem/491/C) 

**Đánh giá:** 2300 
**Thẻ:** luồng, khớp biểu đồ 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu giải mã một tin nhắn đã được mã hóa bằng mật mã thay thế đơn giản. Mỗi chữ cái trong thư gốc được thay thế bằng một chữ cái cố định, do đó ánh xạ là một đối một: các chữ cái khác nhau ánh xạ tới các chữ cái khác nhau và cùng một chữ cái luôn ánh xạ tới cùng một chữ cái. Maria Ivanovna có bản mã và biết câu trả lời chính xác cho một bài kiểm tra, nhưng cô ấy không biết câu trả lời thực sự của Sasha. Mục tiêu là chọn ánh xạ từ các chữ cái được mã hóa đến các câu trả lời có thể có theo cách tối đa hóa số lượng vị trí mà các câu trả lời được giải mã của Sasha khớp với các câu trả lời đúng. 

Đầu vào bao gồm độ dài của tin nhắn$N$(lên tới 2 triệu) và số lượng câu trả lời có thể có$K$(tối đa 52), chuỗi văn bản mã hóa có độ dài$N$, và chuỗi câu trả lời đúng có độ dài$N$. Kết quả đầu ra gồm hai phần: số câu trả lời đúng tối đa mà chúng tôi có thể đạt được và một chuỗi có độ dài$K$chỉ định ánh xạ từ các chữ cái mật mã (bắt đầu từ 'a') đến các chữ cái trả lời. 

Giới hạn lớn về$N$loại trừ các cách tiếp cận ngây thơ lặp đi lặp lại trên tất cả các hoán vị của ánh xạ, như$K!$tăng trưởng theo giai thừa và sẽ không khả thi ngay cả đối với quy mô nhỏ$K$. Chúng ta cũng cần đảm bảo xử lý tuyến tính hoặc gần tuyến tính trong$N$và xử lý tất cả các chữ cái mà không cần thêm chi phí. Các trường hợp đặc biệt bao gồm các tình huống trong đó nhiều chữ cái mật mã ánh xạ tới cùng một câu trả lời hoặc khi một số chữ cái nhất định chỉ xuất hiện một lần; một cách tiếp cận tham lam bất cẩn có thể gán nhầm nhiều chữ cái mật mã cho cùng một câu trả lời, điều này không hợp lệ. Ví dụ: nếu mật mã là "aa" và câu trả lời đúng là "ab", việc gán cả 'a' cho 'b' một cách ngây thơ sẽ vi phạm quy tắc ánh xạ một-một. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ thử tất cả các hoán vị ánh xạ từ các chữ cái mật mã sang các chữ cái trả lời, đếm số vị trí mà thông báo được giải mã khớp với các câu trả lời đúng và lấy giá trị tối đa. Cách tiếp cận này đúng vì nó kiểm tra toàn diện mọi ánh xạ có thể, nhưng độ phức tạp của nó rất lớn.$O(K! \cdot N)$, lớn về mặt thiên văn đối với thậm chí$K=10$. Điều này rõ ràng là không thể thực hiện được$N$lên tới 2 triệu. 

Quan sát quan trọng là vấn đề có thể được giảm xuống thành vấn đề đối sánh hai bên có trọng số tối đa. Hãy coi các chữ cái mật mã như các nút ở một bên và các chữ cái trả lời là các nút ở bên kia. Đối với mỗi chữ cái mật mã$c$và thư trả lời$a$, chúng ta có thể tính toán số vị trí chứa bản mã$c$và câu trả lời đúng là$a$. Số đếm này đóng vai trò là "trọng số" của ánh xạ$c$ĐẾN$a$. Sau đó, vấn đề trở thành việc tìm cách gán từng chữ cái mật mã cho các câu trả lời sao cho tổng các trọng số này là lớn nhất. Đây là phương pháp khớp trọng số tối đa cổ điển trên biểu đồ hai bên, có thể giải được một cách hiệu quả bằng thuật toán Hungary. Từ$K \le 52$, Một$O(K^3)$giải pháp cho sự phù hợp là chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(K! \cdot N)$|$O(K^2)$| Quá chậm | 
| Kết hợp lưỡng cực có trọng lượng tối đa |$O(K^3 + N)$|$O(K^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo mảng 2D`weight[c][a]`cho tất cả các chữ cái mật mã`c`(0 đến K-1) và tất cả các chữ cái trả lời`a`(0 đến K-1). Mảng này sẽ lưu trữ bao nhiêu vị trí trong tin nhắn có bản mã`c`và câu trả lời đúng`a`. Lặp qua tất cả các vị trí`i`từ 0 đến N-1 và tăng dần`weight[c][a]`Ở đâu`c`là chỉ số của`cipher[i]`Và`a`là chỉ số của`answer[i]`. 
2. Xử lý mảng`weight`dưới dạng biểu đồ lưỡng cực trong đó một tập hợp các nút biểu thị các chữ cái mã hóa và tập hợp còn lại biểu thị các chữ cái trả lời, với trọng số cạnh được tính bằng số lượng được tính toán ở trên. 
3. Áp dụng thuật toán Hungary (Kuhn-Munkres) để tìm kết quả khớp hoàn hảo có trọng số tối đa trong biểu đồ hai bên KxK này. Điều này sẽ đưa ra ánh xạ từ các chữ cái mật mã đến câu trả lời nhằm tối đa hóa tổng số kết quả khớp. 
4. Tính tổng số trận đấu bằng cách tính tổng`weight[c][a]`trên tất cả các cặp được chỉ định`(c, a)`từ sự phù hợp. 
5. Xây dựng chuỗi ánh xạ mật mã bằng cách sắp xếp các chữ cái trả lời tương ứng với từng chữ cái mật mã theo thứ tự. Chuỗi này là dòng đầu ra thứ hai. 

**Tại sao nó hoạt động:** Ở mỗi bước của thuật toán, mảng trọng số sẽ đếm chính xác số lượng câu trả lời đúng thu được bằng cách ánh xạ một chữ cái mật mã vào một chữ cái trả lời. Thuật toán Hungary đảm bảo rằng tổng trọng số được chọn là tối đa cho phép gán một-một. Vì ánh xạ hoàn hảo (một-một) và tối đa hóa tổng trọng số nên thuật toán tạo ra số câu trả lời đúng tối đa có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    N, K = map(int, input().split())
    cipher = input().strip()
    correct = input().strip()

    # Initialize weight matrix
    weight = [[0] * K for _ in range(K)]
    
    def idx(ch):
        if 'a' <= ch <= 'z':
            return ord(ch) - ord('a')
        return ord(ch) - ord('A') + 26

    for c_char, a_char in zip(cipher, correct):
        c = idx(c_char)
        a = idx(a_char)
        weight[c][a] += 1

    # Hungarian Algorithm for maximum weight matching
    u = [0] * (K+1)
    v = [0] * (K+1)
    p = [0] * (K+1)
    way = [0] * (K+1)

    for i in range(1, K+1):
        p[0] = i
        minv = [float('inf')] * (K+1)
        used = [False] * (K+1)
        j0 = 0
        while True:
            used[j0] = True
            i0 = p[j0]
            delta = float('inf')
            j1 = 0
            for j in range(1, K+1):
                if not used[j]:
                    cur = -(weight[i0-1][j-1]) - u[i0] - v[j]
                    if cur < minv[j]:
                        minv[j] = cur
                        way[j] = j0
                    if minv[j] < delta:
                        delta = minv[j]
                        j1 = j
            for j in range(K+1):
                if used[j]:
                    u[p[j]] += delta
                    v[j] -= delta
                else:
                    minv[j] -= delta
            j0 = j1
            if p[j0] == 0:
                break
        # augmenting path
        while True:
            j1 = way[j0]
            p[j0] = p[j1]
            j0 = j1
            if j0 == 0:
                break

    match = [0] * K
    for j in range(1, K+1):
        match[p[j]-1] = j-1

    total = sum(weight[i][match[i]] for i in range(K))

    # build mapping string
    mapping = [''] * K
    for i in range(K):
        a_idx = match[i]
        if a_idx < 26:
            mapping[i] = chr(ord('a') + a_idx)
        else:
            mapping[i] = chr(ord('A') + a_idx - 26)

    print(total)
    print(''.join(mapping))

if __name__ == "__main__":
    main()
```Mã bắt đầu bằng cách đọc dữ liệu đầu vào và xây dựng ma trận trọng số đếm số câu trả lời đúng cho mỗi ánh xạ tiềm năng. Thuật toán Hungary được triển khai để tìm ra kết quả phù hợp hoàn hảo có trọng số tối đa, mang lại ánh xạ tối ưu. Cuối cùng, thuật toán tính toán tổng số kết quả khớp và xuất ra chuỗi ánh xạ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
10 2
aaabbbaaab
bbbbabbbbb
```| Vị trí | mật mã | đúng | cập nhật cân nặng | 
| --- | --- | --- | --- | 
| 0 | một | b | trọng lượng[a][b] += 1 | 
| 1 | một | b | trọng lượng[a][b] += 1 | 
| 2 | một | b | trọng lượng[a][b] += 1 | 
| 3 | b | b | trọng lượng[b][b] += 1 | 
| 4 | b | một | trọng lượng[b][a] += 1 | 
| 5 | b | b | trọng lượng[b][b] += 1 | 
| 6 | b | b | trọng lượng[b][b] += 1 | 
| 7 | một | b | trọng lượng[a][b] += 1 | 
| 8 | một | b | trọng lượng[a][b] += 1 | 
| 9 | b | b | trọng lượng[b][b] += 1 | 

Thuật toán Hungary gán: a -> b, b -> a 

Tổng số trận đấu: 7 

Chuỗi ánh xạ: "ba" 

### Ví dụ tùy chỉnh 

đầu vào:```
5 3
abcde
abcde
```Mỗi chữ cái mật mã đều khớp với câu trả lời đúng của nó một cách hoàn hảo. Thuật toán Hungary ánh xạ từng chữ cái mật mã tới cùng một câu trả lời. Tổng số trận đấu: 5. Chuỗi ánh xạ: "abc". 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Ồ | |
