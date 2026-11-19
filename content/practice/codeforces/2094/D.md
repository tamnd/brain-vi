---
title: "CF 2094D - Tung Tung Sahur"
description: "Chúng ta được cung cấp một chuỗi các lần đánh trống dự định được ghi lại, trong đó mỗi ký tự là đánh trái hoặc đánh phải. Mỗi cú đánh dự định không tạo ra độ dài âm thanh cố định."
date: "2026-06-08T05:34:19+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "strings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2094
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1017 (Div. 4)"
rating: 1100
weight: 2094
solve_time_s: 66
verified: true
draft: false
---

[CF 2094D - Tung Tung Sahur](https://codeforces.com/problemset/problem/2094/D) 

**Đánh giá:** 1100 
**Tags:** tham lam, chuỗi, hai con trỏ 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các lần đánh trống dự định được ghi lại, trong đó mỗi ký tự là đánh trái hoặc đánh phải. Mỗi cú đánh dự định không tạo ra độ dài âm thanh cố định. Thay vào đó, một cú đánh có thể được nghe dưới dạng một ký tự hoặc hai ký tự giống hệt nhau trong âm thanh được ghi cuối cùng. 

Vì vậy, mỗi ký tự trong chuỗi ban đầu sẽ mở rộng độc lập thành một khối ngắn: cú đánh bên trái trở thành một chữ “L” hoặc hai chữ “L”, và cú đánh bên phải trở thành một chữ “R” hoặc hai chữ “R”. Sau tất cả các lần mở rộng, chuỗi kết quả được nối vào chuỗi âm thanh được quan sát. 

Nhiệm vụ là quyết định xem một chuỗi quan sát nhất định có thể được tạo ra từ chuỗi dự định theo các quy tắc này hay không. 

Các ràng buộc đủ lớn để bất kỳ phương pháp bậc hai hoặc quay lui nào trên tất cả các bản mở rộng đều không khả thi. Tổng độ dài trên tất cả các trường hợp thử nghiệm lên tới 200000, do đó, giải pháp phải xử lý từng ký tự theo thời gian tuyến tính tổng thể. 

Một cạm bẫy ngây thơ xuất hiện khi cố gắng ghép từng nhân vật mà không tôn trọng việc phân nhóm. Ví dụ: nếu p = "LR" và s = ​​"LLRL", việc kiểm tra tham lam trên mỗi ký tự có thể chấp nhận kết quả khớp một phần quá sớm và không phát hiện được rằng nhóm thứ hai không thể căn chỉnh rõ ràng. 

Một trường hợp thất bại phổ biến khác là sử dụng quá nhiều ký tự trong s cho một ký tự trong p. Ví dụ: nếu p có một chữ 'L' duy nhất, thì việc sử dụng ba chữ 'L' liên tiếp trong s là không thể, nhưng việc triển khai con trỏ bất cẩn có thể không phát hiện được ranh giới giữa các nhóm. 

Khó khăn chính là chúng ta phải tôn trọng việc nhóm: mỗi ký tự trong p tương ứng với một khối liền kề trong s có độ dài 1 hoặc 2 và các khối không thể hợp nhất giữa các ký tự khác nhau. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là xử lý mỗi ký tự trong p và nhánh thành hai lựa chọn: mở rộng nó thành một hoặc hai ký tự, sau đó mô phỏng chuỗi kết quả và so sánh với s. Điều này tạo ra số lượng khả năng theo cấp số nhân, lên tới 2^{|p|}, điều này là không thể ngay cả đối với kích thước đầu vào vừa phải. 

Cấu trúc của bài toán gợi ý một chiến lược so khớp tham lam thay vì liệt kê. Vì mỗi ký tự trong p tạo ra một khối liền kề gồm các ký tự giống hệt nhau trong s, nên chúng ta có thể quét cả hai chuỗi từ trái sang phải và khớp các nhóm. 

Quan sát quan trọng là cả p và s đều có thể được nén thành các chuỗi ký tự bằng nhau. Mỗi lần chạy trong p phải tương ứng với một lần chạy trong s có cùng một ký tự và độ dài lần chạy tính bằng s phải nằm giữa độ dài lần chạy tính bằng p và gấp đôi độ dài lần chạy đó, vì mỗi ký tự mở rộng độc lập đến độ dài 1 hoặc 2. 

Vì vậy, vấn đề giảm xuống còn việc so sánh độ dài chạy theo thứ tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Kết hợp tham lam trong thời gian dài | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nén cả hai chuỗi p và s thành các chuỗi lần chạy, trong đó mỗi lần chạy là một cặp (ký tự, độ dài). Điều này là cần thiết vì việc mở rộng bảo toàn tính liền kề, vì vậy chỉ có ranh giới chạy mới quan trọng. 
2. Nếu số lần chạy trong p và s khác nhau, trả về ngay NO. Mỗi lần truy cập tạo ra chính xác một lần chạy, do đó, sự không khớp về cấu trúc không thể được khắc phục bằng cách mở rộng. 
3. Lặp lại các lần chạy tương ứng theo thứ tự p và s. 
4. Đối với mỗi cặp lần chạy, hãy kiểm tra xem các ký tự có khớp nhau không. Nếu chúng khác nhau, hãy trả về NO ngay lập tức vì việc mở rộng không bao giờ thay đổi ký tự. 
5. Gọi độ dài bước chạy trong p là x và trong s là y. Vì mỗi ký tự trong p trở thành 1 hoặc 2 ký tự, nên tổng số mở rộng của một chuỗi x lần truy cập giống hệt nhau phải thỏa mãn x ≤ y 2x. 
6. Nếu bất kỳ lần chạy nào vi phạm bất đẳng thức này, hãy trả về NO. 
7. Nếu tất cả các lần chạy đều thỏa mãn điều kiện, trả về CÓ. 

### Tại sao nó hoạt động

Mỗi ký tự trong p mở rộng độc lập nhưng vẫn giữ nguyên trật tự, do đó chuỗi cuối cùng chính xác là sự nối các khối tương ứng một-một với các lần chạy của p. Trong mỗi lần chạy, mỗi ký tự gốc đóng góp 1 hoặc 2 ký tự giống hệt nhau, nghĩa là độ dài lần chạy chỉ có thể thay đổi trong một khoảng thời gian cố định mà không ảnh hưởng đến các lần chạy lân cận. Vì các lần chạy không thể hợp nhất hoặc phân chia thành các ký tự khác nhau nên việc xác thực từng lần chạy một cách độc lập vừa cần thiết vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def compress(s):
    runs = []
    i = 0
    n = len(s)
    while i < n:
        j = i
        while j < n and s[j] == s[i]:
            j += 1
        runs.append((s[i], j - i))
        i = j
    return runs

t = int(input())
for _ in range(t):
    p = input().strip()
    s = input().strip()

    rp = compress(p)
    rs = compress(s)

    if len(rp) != len(rs):
        print("NO")
        continue

    ok = True
    for (cp, lp), (cs, ls) in zip(rp, rs):
        if cp != cs:
            ok = False
            break
        if ls < lp or ls > 2 * lp:
            ok = False
            break

    print("YES" if ok else "NO")
```Bước nén đảm bảo chúng ta chỉ so sánh cấu trúc có ý nghĩa thay vì các ký tự riêng lẻ. Mỗi lần chạy được lưu trữ dưới dạng một ký tự và tần số của nó. 

Vòng lặp chính sau đó thực thi các ràng buộc về cấu trúc: cùng một kiểu chạy và các giới hạn mở rộng hợp lệ. Séc`ls < lp or ls > 2 * lp`trực tiếp mã hóa hành vi được phép của mỗi lần truy cập mở rộng độc lập. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

p = "LR", s = "LLRR" 

| Bước | p chạy | s chạy | Kiểm tra | 
| --- | --- | --- | --- | 
| 1 | L:1 | L:2 | 1 2 2 2 | 
| 2 | R:1 | R:2 | 1 2 2 2 | 

Tất cả các lần chạy đều khớp, vì vậy đầu ra là CÓ. 

Điều này chứng tỏ trường hợp mỗi lần truy cập đều mở rộng tối đa. 

### Ví dụ 2 

đầu vào: 

p = "LRLR", s = "LRLR" 

| Bước | p chạy | s chạy | Kiểm tra | 
| --- | --- | --- | --- | 
| 1 | L:1 | L:1 | | 
| 2 | R:1 | R:1 | | 
| 3 | L:1 | L:1 | | 
| 4 | R:1 | R:1 | | 

Tất cả các lần chạy đều thỏa mãn các ràng buộc, vì vậy CÓ. 

Điều này cho thấy khả năng mở rộng tối thiểu trong đó mỗi lần truy cập đều có độ dài đơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được truy cập một lần trong quá trình nén và một lần trong quá trình so sánh | 
| Không gian | O(n) | Chạy bộ lưu trữ trong trường hợp xấu nhất khi không thể nén được | 

Tổng kích thước đầu vào được giới hạn bởi 2×10^5 trong tất cả các trường hợp thử nghiệm, do đó, giải pháp tuyến tính sẽ phù hợp một cách thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def compress(s):
        runs = []
        i = 0
        n = len(s)
        while i < n:
            j = i
            while j < n and s[j] == s[i]:
                j += 1
            runs.append((s[i], j - i))
            i = j
        return runs

    t = int(input())
    out = []
    for _ in range(t):
        p = input().strip()
        s = input().strip()

        rp = compress(p)
        rs = compress(s)

        if len(rp) != len(rs):
            out.append("YES" if False else "NO")
            continue

        ok = True
        for (cp, lp), (cs, ls) in zip(rp, rs):
            if cp != cs or ls < lp or ls > 2 * lp:
                ok = False
                break

        out.append("YES" if ok else "NO")

    return "\n".join(out)

# provided samples
assert run("""5
R
RR
LRLR
LRLR
LR
LLLR
LLLLLRL
LLLLRRLL
LLRLRLRRL
LLLRLRRLLRRRL
""") == """YES
YES
NO
NO
YES"""

# custom cases
assert run("""3
L
LL
R
RRR
LR
LRRR
""") == """YES
NO
NO"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| L vs LL | CÓ | mở rộng tối đa một lần chạy | 
| R vs RRR | KHÔNG | mở rộng không hợp lệ vượt quá 2× | 
| LR vs LRRR | KHÔNG | cấu trúc chạy không khớp | 

## Vỏ cạnh 

Trường hợp một cạnh là khi s có thêm sự lặp lại bên trong một lần chạy trông vẫn hợp lệ cục bộ nhưng lại phá vỡ sự liên kết trên toàn cầu. Ví dụ: p = "LR" và s = ​​"LLLR". Việc nén mang lại p chạy [(L,1),(R,1)] và s chạy [(L,3),(R,1)]. Lần chạy đầu tiên không đạt được ràng buộc vì 3 > 2 × 1, vì vậy câu trả lời là KHÔNG. Thuật toán nắm bắt được điều này ngay lập tức trong quá trình so sánh lần chạy. 

Một trường hợp khác là khi s có ít lần chạy hơn p do việc hợp nhất xuyên ranh giới là không thể theo quy tắc. Ví dụ: p = "LRL" và s = ​​"LLRR". Việc nén cho p là ba lần chạy nhưng s là hai lần chạy, do đó thuật toán sẽ loại bỏ ngay lập tức. Điều này đảm bảo chúng tôi không bao giờ cho phép hợp nhất ký tự chéo một cách không chính xác.
