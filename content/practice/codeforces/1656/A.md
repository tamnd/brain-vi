---
title: "CF 1656A - Đôi Tốt"
description: "Chúng ta được yêu cầu tìm một \"cặp tốt\" chỉ số trong một mảng các số nguyên dương. Một cặp tốt bao gồm hai chỉ số $i$ và $j$ sao cho, nếu bạn xem xét bất kỳ phần tử $ak$ nào trong mảng, tổng khoảng cách từ $ai$ đến $ak$ và từ $ak$ đến $aj$ bằng khoảng cách từ $ai$ đến…"
date: "2026-06-10T03:32:51+07:00"
tags: ["codeforces", "competitive-programming", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1656
codeforces_index: "A"
codeforces_contest_name: "CodeTON Round 1 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 800
weight: 1656
solve_time_s: 143
verified: true
draft: false
---

[CF 1656A - Cặp đôi tốt](https://codeforces.com/problemset/problem/1656/A) 

**Đánh giá:** 800 
**Tags:** toán, sắp xếp 
**Thời gian giải:** 2m 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tìm một "cặp tốt" chỉ số trong một mảng các số nguyên dương. Một cặp tốt bao gồm hai chỉ số$i$Và$j$như vậy, nếu bạn xem xét bất kỳ yếu tố nào$a_k$trong mảng, tổng khoảng cách từ$a_i$ĐẾN$a_k$và từ$a_k$ĐẾN$a_j$bằng khoảng cách từ$a_i$ĐẾN$a_j$. Về mặt hình học,$a_k$phải nằm trên đoạn thẳng nối$a_i$Và$a_j$, khi xét các số trên trục số thực. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có kích thước mảng$n$và các phần tử mảng. Đầu ra phải xác định bất kỳ cặp tốt nào và bài toán đảm bảo rằng giải pháp luôn tồn tại. Một chi tiết quan trọng đó là$i$có thể bằng$j$, điều này ngay lập tức cho phép một giải pháp tầm thường cho các mảng có kích thước một. 

Các ràng buộc cho phép lên đến$10^5$các phần tử trong một mảng và tổng tổng của tất cả các trường hợp thử nghiệm không vượt quá$2 \cdot 10^5$. Điều này có nghĩa là bất kỳ giải pháp nào thực hiện nhiều hơn công việc tuyến tính trên mỗi mảng đều có thể quá chậm. Chúng ta cũng phải cẩn thận với các mảng trong đó tất cả các phần tử đều bằng nhau, các mảng có hai phần tử hoặc các mảng có bản sao nói chung, vì việc lựa chọn đơn giản hai chỉ số đầu tiên có thể không thành công nếu chúng ta bỏ qua thuộc tính giá trị tuyệt đối. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là thử mọi cặp chỉ số$(i, j)$và kiểm tra thuộc tính xác định cho tất cả$k$. Điều này đòi hỏi$O(n^3)$hoạt động trong trường hợp xấu nhất vì đối với mỗi$O(n^2)$cặp chúng tôi kiểm tra$n$các phần tử. Điều này rõ ràng là không thể thực hiện được với$n$lên tới$10^5$. 

Quan sát sự bình đẳng$$|a_i - a_k| + |a_k - a_j| = |a_i - a_j|$$bộc lộ sự đơn giản hóa. Vế trái bằng khoảng cách từ$a_i$ĐẾN$a_j$nếu và chỉ khi$a_k$nằm giữa$a_i$Và$a_j$bao gồm. Đó là, hoặc$a_i \le a_k \le a_j$hoặc$a_j \le a_k \le a_i$. Cái nhìn sâu sắc này làm giảm vấn đề tìm hai số tối thiểu và tối đa trong mảng. Bất kì$k$sẽ tự động thỏa mãn tính chất vì tất cả các phần tử khác nằm giữa giá trị nhỏ nhất và lớn nhất. Như vậy, bài toán rút gọn xuống việc xác định các phần tử nhỏ nhất và lớn nhất trong$O(n)$thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(1) | Quá chậm | 
| Lựa chọn tối thiểu-tối đa | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$và mảng$a$. 
2. Khởi tạo hai biến để lưu trữ giá trị tối thiểu và giá trị tối đa của mảng cũng như các chỉ số tương ứng của chúng. 
3. Lặp lại mảng. Nếu phần tử hiện tại nhỏ hơn mức tối thiểu hiện tại, hãy cập nhật mức tối thiểu và chỉ mục của nó. Nếu nó lớn hơn mức tối đa hiện tại, hãy cập nhật mức tối đa và chỉ mục của nó. 
4. Sau khi kết thúc vòng lặp, bây giờ chúng ta có chỉ số của phần tử nhỏ nhất và lớn nhất. Xuất các chỉ số này dưới dạng cặp tốt. 
5. Nếu mảng chỉ chứa một phần tử, hãy xuất chỉ mục của nó hai lần. Điều này bao gồm vỏ cạnh mà không cần kiểm tra bổ sung. 

Tại sao nó hoạt động: bằng cách chọn số nhỏ nhất và lớn nhất, tất cả các phần tử mảng khác nằm giữa chúng. Điều này đảm bảo rằng đối với bất kỳ$k$,$|a_i - a_k| + |a_k - a_j| = |a_i - a_j|$, thỏa mãn tính chất cặp tốt. Không phần tử nào có thể vi phạm sự đẳng thức và quét tuyến tính đảm bảo tính chính xác và hiệu quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        min_val = float('inf')
        max_val = float('-inf')
        min_idx = max_idx = 0

        for idx, val in enumerate(a):
            if val < min_val:
                min_val = val
                min_idx = idx
            if val > max_val:
                max_val = val
                max_idx = idx

        # output 1-based indices
        print(min_idx + 1, max_idx + 1)

if __name__ == "__main__":
    solve()
```Giải pháp đọc từng mảng một cách hiệu quả bằng cách sử dụng I/O nhanh. Chúng tôi theo dõi các chỉ số một cách cẩn thận để trả về kết quả dựa trên 1. sử dụng`float('inf')`Và`float('-inf')`đảm bảo khởi tạo chính xác ngay cả đối với số nguyên lớn. Lặp lại một lần trên mảng đảm bảo$O(n)$thời gian cho mỗi trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**`5 1 4 2 2 3`| Chỉ mục | Giá trị | giá trị tối thiểu | min_idx | giá trị tối đa | max_idx | 

|---|---|---|---|---
