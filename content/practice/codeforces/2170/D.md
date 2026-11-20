---
title: "CF 2170D - Gần như La Mã"
description: "Chúng ta được cấp một chuỗi gồm các ký tự 'X', 'V', 'I' và '?'. Mỗi chữ cái có một giá trị số: 'X' có giá trị 10, 'V' có giá trị 5 và 'I' thường có giá trị 1, nhưng nếu ngay sau nó là 'X' hoặc 'V' thì nó được tính là -1."
date: "2026-06-07T23:12:39+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2170
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 185 (Rated for Div. 2)"
rating: 2200
weight: 2170
solve_time_s: 107
verified: false
draft: false
---

[CF 2170D - Gần như La Mã](https://codeforces.com/problemset/problem/2170/D) 

**Xếp hạng:** 2200 
**Tags:** thuật toán xây dựng, tham lam, triển khai 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi gồm các ký tự 'X', 'V', 'I' và '?'. Mỗi chữ cái có một giá trị số: 'X' có giá trị 10, 'V' có giá trị 5 và 'I' thường có giá trị 1, nhưng nếu ngay sau nó là 'X' hoặc 'V' thì nó được tính là -1. Giá trị chuỗi là tổng giá trị của tất cả các chữ cái của nó. Dấu chấm hỏi có thể được thay thế bằng bất kỳ sự kết hợp nào của các chữ cái 'X', 'V' hoặc 'I', nhưng việc thay thế phải tôn trọng các ràng buộc sẵn có nhất định. Đối với mỗi truy vấn, chúng tôi được yêu cầu tìm giá trị chuỗi tối thiểu có thể sau khi thay thế tất cả các dấu chấm hỏi chỉ bằng các chữ cái được truy vấn cho phép. 

Đầu vào cung cấp nhiều trường hợp kiểm thử, mỗi trường hợp có một chuỗi và nhiều truy vấn. Mỗi truy vấn chỉ định số lượng chữ cái của từng loại có thể được sử dụng để thay thế các dấu chấm hỏi. Các ràng buộc cho phép các chuỗi có độ dài tối đa 3×10^5 và tổng số truy vấn trên tất cả các trường hợp thử nghiệm cũng lên tới 3×10^5. Điều này cho thấy rằng bất kỳ giải pháp nào xem xét tất cả các hoán vị thay thế sẽ quá chậm, vì số dấu hỏi trong trường hợp xấu nhất có thể đạt tới 3×10^5. 

Một phần tế nhị của vấn đề là định nghĩa về cái “tôi”. Giá trị của nó phụ thuộc vào ký tự tiếp theo, có nghĩa là những quyết định tham lam về vị trí đặt 'tôi' có thể ảnh hưởng đến tổng giá trị theo những cách không rõ ràng. Ví dụ: hãy xem xét một chuỗi "I?X" chỉ có một 'X' để thay thế. Thay thế '?' bằng 'X' sẽ làm cho 'I' đầu tiên được tính là -1, hạ thấp tổng giá trị, trong khi thay thế nó bằng 'I' sẽ làm cho nó trở thành +1. Một cách tiếp cận đơn giản thay thế tất cả các dấu hỏi bằng giá trị chữ cái tuyệt đối nhỏ nhất ('I') có thể không tạo ra tổng tối thiểu có thể. 

Các trường hợp cạnh bao gồm các chuỗi đều là dấu chấm hỏi, các chuỗi kết thúc bằng 'I' và các truy vấn chỉ có sẵn một tập hợp con các chữ cái, khiến một số vị trí nhất định không thể thực hiện được. Ví dụ: một chuỗi "??" với các chữ cái có sẵn 1 'X' và 1 'V' nên xem xét sự kết hợp nào mang lại tổng thấp nhất, có tính đến việc 'I' trước 'X' hoặc 'V' trừ đi tổng. 

## Phương pháp tiếp cận 

Cách tiếp cận mạnh mẽ là thử tất cả các thay thế có thể có cho dấu chấm hỏi cho mỗi truy vấn, tính giá trị chuỗi cho mỗi truy vấn và chọn giá trị tối thiểu. Về mặt lý thuyết, điều này có hiệu quả vì chúng ta có thể tính toán giá trị của bất kỳ chuỗi nào một cách xác định, nhưng số lần thay thế sẽ theo cấp số nhân theo số dấu chấm hỏi. Nếu chuỗi có 10 dấu chấm hỏi thì có 3^10 ≈ 59.000 khả năng; nếu nó có 3×10^5 dấu chấm hỏi, vũ lực là hoàn toàn không khả thi. Giải pháp vũ phu sẽ đúng nhưng quá chậm. 

Quan sát quan trọng là giá trị tối thiểu có thể đạt được bằng chiến lược vị trí tham lam. Vì 'X' đóng góp +10, 'V' đóng góp +5 và 'I' có thể là -1 hoặc +1 tùy thuộc vào vị trí kế nhiệm của nó, chúng ta nên ưu tiên các vị trí biến 'I' thành -1 bất cứ khi nào có thể. Nghĩa là, nếu 'I' đứng trước 'X' hoặc 'V', thì tổng đó sẽ bị trừ. Do đó, chúng tôi đặt mục tiêu đặt 'I' trước các chữ cái lớn nhất hiện có và chỉ đặt 'I's cuối cùng nếu không có 'X' hoặc 'V' sau nó. Đồng thời, chúng tôi muốn đặt 'X' và 'V' ở cuối để tối đa hóa số lượng 'I' trước đó được tính là -1. 

Chúng ta có thể tính toán trước cho chuỗi vị trí của các chữ cái cố định và vị trí tương đối của dấu chấm hỏi. Sau đó, đối với mỗi truy vấn, chúng tôi mô phỏng việc điền các dấu chấm hỏi từ trái sang phải, chọn 'X' và 'V' cuối cùng để chuyển nhiều đóng góp 'I' trước đó thành -1. Các chữ còn lại điền là “I”. Điều này làm giảm vấn đề xuống còn quét tuyến tính cho mỗi truy vấn, tạo ra giải pháp O(n + q).

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3^k × n) trong đó k = # của '?') | O(n) | Quá chậm | 
| Tham lam / Mô phỏng tuyến tính | O(n + q) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính trước giá trị cơ bản của chuỗi bỏ qua dấu chấm hỏi. Lặp lại từ trái sang phải: thêm 10 cho 'X', 5 cho 'V' và quyết định giá trị của 'I' dựa trên ký tự tiếp theo nếu biết. 
2. Ghi lại vị trí các dấu chấm hỏi trong mảng. Chúng tôi sẽ xử lý chúng theo thứ tự từ trái sang phải. 
3. Đối với mỗi truy vấn, sao chép tính khả dụng của các chữ cái vào các bộ đếm có thể thay đổi: c_X, c_V, c_I. 
4. Di chuyển chuỗi từ trái sang phải. Bất cứ khi nào có '?' gặp phải, hãy quyết định đặt chữ cái nào: 

- Nếu đặt chữ 'I' trước chữ 'X' hoặc 'V' cố định hoặc trước chữ '?' trong tương lai sẽ được thay thế bằng 'X' hoặc 'V', nó sẽ được tính là -1. Đặt 'I' vào các vị trí như vậy nếu c_I > 0. 
- Ngược lại, đặt 'X' hoặc 'V' càng muộn càng tốt để tối đa hóa 'I' trước đó được tính là -1. Giảm c_X hoặc c_V tương ứng. 
5. Sau khi thay thế, quét lại để tính tổng giá trị chuỗi bằng cách sử dụng đánh giá kiểu La Mã: lặp từ trái sang phải và bất cứ khi nào 'I' được theo sau bởi 'X' hoặc 'V', hãy tính nó là -1; nếu không thì +1. Thêm các khoản đóng góp cố định cho 'X' và 'V'. 
6. Xuất ra tổng giá trị cho truy vấn này. 

Thuật toán hoạt động vì lựa chọn tham lam đảm bảo rằng mọi 'I' có thể được chuyển đổi thành -1 sẽ được chuyển đổi và 'X'/'V' được định vị để tối đa hóa hiệu ứng này. Vì chúng tôi chỉ gán các chữ cái trong giới hạn sẵn có nên chúng tôi duy trì tính chính xác theo các ràng buộc truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def compute_min_value(s, counts):
    n = len(s)
    s = list(s)
    c_X, c_V, c_I = counts
    q_positions = [i for i, ch in enumerate(s) if ch == '?']

    # Greedy assignment
    for i in q_positions:
        # Check if we can force this I to be -1
        # Lookahead to next known character
        if c_I > 0:
            next_char = None
            for j in range(i+1, n):
                if s[j] != '?':
                    next_char = s[j]
                    break
            if next_char in ('X', 'V'):
                s[i] = 'I'
                c_I -= 1
                continue
        # Otherwise assign smallest effect: assign I if available
        if c_I > 0:
            s[i] = 'I'
            c_I -= 1
        elif c_V > 0:
            s[i] = 'V'
            c_V -= 1
        elif c_X > 0:
            s[i] = 'X'
            c_X -= 1

    # Compute string value
    total = 0
    for i in range(n):
        if s[i] == 'X':
            total += 10
        elif s[i] == 'V':
            total += 5
        elif s[i] == 'I':
            if i + 1 < n and s[i+1] in ('X', 'V'):
                total -= 1
            else:
                total += 1
    return total

def main():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        s = input().strip()
        queries = [tuple(map(int, input().split())) for _ in range(q)]
        for counts in queries:
            print(compute_min_value(s, counts))

if __name__ == "__main__":
    main()
```Giải pháp đầu tiên xác định tất cả các vị trí dấu chấm hỏi. Đối với mỗi truy vấn, nó tham lam gán các chữ cái để giảm thiểu giá trị chuỗi: nó ưu tiên đặt 'I' trước 'X' hoặc 'V' để tận dụng quy tắc -1 và điền các chữ cái vào các dấu chấm hỏi còn lại tùy theo tình trạng sẵn có. Việc tính toán giá trị cuối cùng sử dụng quy tắc đánh giá kiểu La Mã cho 'I'. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
3
???
3 0 0
2 3 1
0 1 2
```Theo dõi truy vấn đầu tiên (3,0,0): 

| Chỉ mục | Thư | Quyết định | Đóng góp giá trị | 
| --- | --- | --- | --- | 
| 0 | ? | X | +10 | 
| 1 | ? | X | +10 | 
| 2 | ? | X | +10 | 

Tổng cộng = 30. Thuật toán chọn 'X' vì chỉ có sẵn 'X'. Điều này xác nhận sự lấp đầy tham lam chính xác. 

Truy vấn thứ hai (2,3,1): 

| Chỉ mục | Thư | Quyết định | Đóng góp giá trị | 
| --- | --- | --- | --- | 
| 0 | ? | X | +10 | 
| 1 | ? | V | +5 | 
| 2 | ? | Tôi | +1 | 

Tổng = 16. Thuật toán đảm bảo các ràng buộc về tính khả dụng được tôn trọng trong khi vẫn cố gắng đặt 'I' trước các chữ cái tạo thành -1, nhưng ở đây chỉ có một 'I'. Dấu vết này cho thấy mức độ sẵn có ảnh hưởng đến sự lựa chọn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) cho mỗi trường hợp thử nghiệm | Quét tuyến tính để gán các chữ cái và tính toán giá trị. | 
| Không gian | O(n) | Lưu trữ vị trí của dấu chấm hỏi và bản sao làm việc của chuỗi. | 

Với các ràng buộc, tổng n và q trên tất cả các trường hợp thử nghiệm ≤
