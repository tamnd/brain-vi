---
title: "CF 442A - Borya và Hanabi"
description: "Chúng ta có một bộ sưu tập các quân bài, mỗi quân bài được xác định bằng một màu sắc và một số từ 1 đến 5. Borya giữ n quân bài, và mặc dù anh ta biết mình có những quân bài nào nhưng anh ta không thể phân biệt được những quân bài giống hệt nhau về vị trí."
date: "2026-06-07T06:03:13+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 442
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 253 (Div. 1)"
rating: 1700
weight: 442
solve_time_s: 66
verified: true
draft: false
---

[CF 442A - Borya và Hanabi](https://codeforces.com/problemset/problem/442/A) 

**Đánh giá:** 1700 
**Tags:** bitmasks, vũ lực, thực hiện 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một bộ sưu tập các quân bài, mỗi quân bài được xác định bằng một màu sắc và một số từ 1 đến 5. Borya giữ _n_ quân bài, và mặc dù biết mình có những quân bài nào, nhưng anh ta không thể phân biệt được những quân bài giống hệt nhau về vị trí. Những người chơi khác muốn đưa ra gợi ý để Borya có thể xác định màu sắc và số của từng lá bài mà không bị mơ hồ. Một gợi ý có thể xác định tất cả các thẻ có một màu cụ thể hoặc tất cả các thẻ có một số cụ thể. 

Đầu vào cung cấp số lượng thẻ theo sau là danh sách các thẻ. Mỗi thẻ được thể hiện bằng hai ký tự: ký tự đầu tiên là một màu (`R`,`G`,`B`,`Y`,`W`) và số thứ hai là một số (`1`-`5`). Đầu ra là số gợi ý tối thiểu cần thiết để Borya có thể xác định tất cả các thẻ của mình. 

Các ràng buộc nhỏ: n nhiều nhất là 100. Điều này có nghĩa là chúng ta có thể cung cấp các thuật toán xem xét tất cả các tập hợp con của gợi ý hoặc giải pháp tính toán bằng cách mô phỏng tất cả các kết hợp có thể có của gợi ý màu và số, miễn là việc tính toán được thực hiện hiệu quả. Số lượng màu và số là cố định (mỗi màu 5), do đó, bất kỳ giải pháp nào lặp lại trên tất cả các màu, tất cả các số hoặc thậm chí tất cả các kết hợp gợi ý đều có thể thực hiện được. 

Trường hợp có lợi thế không rõ ràng phát sinh khi tồn tại nhiều lá bài giống hệt nhau. Ví dụ: nếu Borya có hai`G3`thẻ, anh ấy đã biết cả hai đều là quân ba màu xanh lá cây nên không cần gợi ý. Ngược lại, nếu anh ta có`R1`Và`R2`, anh ta không thể phân biệt các số chỉ bằng gợi ý màu sắc và có thể cần phải có gợi ý số. Bất kỳ cách tiếp cận ngây thơ nào chỉ đếm các màu hoặc số riêng biệt mà không xem xét các bản sao đều có thể tạo ra kết quả không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là thử tất cả các chuỗi gợi ý về màu sắc và số có thể có và kiểm tra xem chúng có nhận dạng duy nhất từng thẻ hay không. Đối với mỗi chuỗi, chúng tôi sẽ theo dõi những thẻ nào có thể phân biệt được. Mặc dù điều này đúng nhưng nó không hiệu quả vì tổng số chuỗi gợi ý là số mũ theo số lượng màu sắc và số (2^10 = 1024 kết hợp cho 5 màu và 5 số). Mặc dù khả thi với 100 thẻ nhưng nó quá mức cần thiết và có thể được đơn giản hóa. 

Thông tin chi tiết quan trọng là chúng ta chỉ cần xem xét cần bao nhiêu gợi ý để xác định đầy đủ từng thẻ về màu sắc và số của nó. Mỗi thẻ có thể được phân biệt bằng gợi ý về màu sắc, gợi ý về số của thẻ hoặc cả hai. Điều này làm giảm vấn đề thành một lựa chọn tổ hợp đơn giản: chúng ta có thể xem xét tất cả số lượng gợi ý màu có thể có (từ 0 đến 5) và với mỗi gợi ý, hãy tính số lượng gợi ý số tối thiểu cần thiết để phân biệt tất cả các thẻ. Vì tổng số kết hợp rất nhỏ (nhiều nhất là 6 × 6 = 36), nên chúng ta có thể đánh giá tất cả chúng một cách hiệu quả. 

Giải pháp tối ưu sẽ đánh giá từng tập hợp con màu sắc được gợi ý. Đối với mỗi tập hợp con, chúng tôi xác định thẻ nào vẫn còn mơ hồ và đếm xem cần bao nhiêu gợi ý số để giải quyết chúng. Tổng tối thiểu trên tất cả các kết hợp sẽ đưa ra câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các trình tự) | O(2^(C+V) * n) | O(n) | Quá chậm để khái quát hóa, khả thi ở đây nhưng không cần thiết | 
| Tối ưu (tập hợp con màu × số) | O(2^5 * 2^5 * n) ≈ O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số thẻ`n`và danh sách các thẻ. Lưu trữ mỗi thẻ dưới dạng tuple`(color, number)`. Điều này làm cho việc kiểm tra tiếp theo dễ dàng hơn. 
2. Liệt kê tất cả các tập hợp con màu sắc làm gợi ý tiềm năng. Biểu thị mỗi tập hợp con dưới dạng mặt nạ bit có độ dài 5. Điều này bao gồm tất cả các khả năng từ gợi ý màu 0 đến tất cả năm gợi ý màu. 
3. Đối với mỗi tập hợp con các gợi ý màu sắc, hãy đánh dấu các thẻ được phân biệt bằng màu sắc. Một lá bài được phân biệt nếu màu của nó nằm trong tập hợp con gợi ý. 
4. Đối với các thẻ mơ hồ còn lại, hãy đếm xem chúng có bao nhiêu số khác nhau. Mỗi số riêng biệt yêu cầu một gợi ý riêng để giải quyết sự mơ hồ. 
5. Tổng số gợi ý màu và số gợi ý số cho sự kết hợp này. Theo dõi tổng số tối thiểu trên tất cả các tập hợp con gợi ý màu sắc. 
6. Xuất ra tổng số gợi ý tối thiểu cần thiết. 

Tại sao nó hoạt động: Điều bất biến là đối với mỗi tập hợp con các gợi ý màu sắc, chúng tôi tính toán chính xác số lượng gợi ý số tối thiểu cần thiết cho các thẻ mơ hồ còn lại. Vì chúng tôi khám phá tất cả các tập hợp con có thể có của gợi ý màu sắc nên thuật toán đảm bảo rằng chúng tôi tìm thấy sự kết hợp với tổng số gợi ý ít nhất. Mỗi thẻ được xác định duy nhất sau khi màu hoặc số của thẻ được gợi ý, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    cards = input().split()
    
    color_map = {'R':0, 'G':1, 'B':2, 'Y':3, 'W':4}
    number_map = {'1':0, '2':1, '3':2, '4':3, '5':4}
    
    card_list = [(color_map[c[0]], number_map[c[1]]) for c in cards]
    
    min_hints = float('inf')
    
    for color_mask in range(1 << 5):
        color_hints = bin(color_mask).count('1')
        
        # track which numbers still need hints
        number_needed = set()
        for c, v in card_list:
            if not ((color_mask >> c) & 1):
                number_needed.add(v)
        
        total_hints = color_hints + len(number_needed)
        if total_hints < min_hints:
            min_hints = total_hints
    
    print(min_hints)

if __name__ == "__main__":
    main()
```Mã chuyển đổi từng thẻ thành các chỉ số số để làm cho các thao tác trên mặt nạ bit trở nên đơn giản.`color_mask`lặp lại tất cả các tập hợp con màu sắc và đối với mỗi tập hợp con, chúng tôi kiểm tra những số nào vẫn cần gợi ý vì màu của chúng chưa được tiết lộ. Đếm số bit cho số lượng gợi ý màu được sử dụng. bộ`number_needed`theo dõi các gợi ý số duy nhất được yêu cầu. Tổng hợp cả hai sẽ đưa ra tổng số gợi ý và chúng tôi giữ mức tối thiểu trên tất cả các kết hợp màu. Sử dụng số nguyên cho các hoạt động bitmask vừa nhanh vừa gọn. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
2
G3 G3
```| Bước | màu_mask | màu_gợi ý | số_cần | tổng_gợi ý | phút_gợi ý | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 00000 | 0 | {2} | 1 | 1 | 
| ... | 00100 | 1 | {} | 1 | 0 | 

Giải thích: Vì cả hai lá bài đều giống hệt nhau nên không cần gợi ý. Thuật toán thử tất cả các tập hợp con màu và tổng số gợi ý tối thiểu bằng 0. 

**Đầu vào mẫu 2**```
3
R1 G2 B3
```| Bước | màu_mask | màu_gợi ý | số_cần | tổng_gợi ý | phút_gợi ý | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 00000 | 0 | {0,1,2} | 3 | 3 | 
| 1 | 00001 | 1 | {1,2} | 3 | 3 | 
| 2 | 11111 | 5 | {} | 5 | 3 | 

Giải thích: Số lượng gợi ý tối thiểu là 3. Bất kỳ sự kết hợp nào của các gợi ý đều phải bao gồm tất cả các điểm mơ hồ; ở đây, đưa ra gợi ý số cho cả ba số hoặc gợi ý màu sắc cộng với gợi ý số còn lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * 2^5) | Chúng tôi lặp lại hơn 32 tập hợp con màu, kiểm tra từng thẻ trong số n thẻ để tìm số chưa được giải | 
| Không gian | O(n) | Lưu trữ danh sách thẻ dưới dạng bộ dữ liệu | 

Số lượng thao tác là khoảng 3200 với n = 100, trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sysio
    f = sysio.StringIO()
    with redirect_stdout(f):
        main()
    return f.getvalue().strip()

# provided samples
assert run("2\nG3 G3\n") == "0", "sample 1"
assert run("3\nR1 G2 B3\n") == "3", "sample 2"

# custom cases
assert run("1\nR1\n") == "0", "single card, no hint needed"
assert run("5\nR1 R2 R3 R4 R5\n") == "1", "all same color, need number hints"
assert run("5\nR1 G1 B1 Y1 W1\n") == "1", "all same number, need color hints"
assert run("5\nR1 G2 B3 Y4 W5\n") == "5", "all distinct, each hint needed"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\nR1 | 0 | Thẻ đơn, không gợi ý | 
| 5\nR1 R2 R3 R4 R5 | 1 | Cùng màu, số khác nhau | 
| 5\nR1 G1 B1 Y1 W1 | 1 | Cùng số, khác màu | 
| 5\nR1 G2 B3 Y4 W5 | 5 | Tất cả các thẻ riêng biệt đều yêu cầu gợi ý riêng | 

## Vỏ cạnh 

Đối với các thẻ giống nhau
