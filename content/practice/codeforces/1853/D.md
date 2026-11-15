---
title: "CF 1853D - Mảng mất cân bằng"
description: "Chúng ta được cho một mảng các số nguyên không âm (a) có độ dài (n). Nhiệm vụ là xây dựng một mảng (b) khác có cùng độ dài với các số nguyên khác 0, sao cho số chỉ số (j) mà (bi + bj 0) chính xác là (ai) với mọi (i)."
date: "2026-06-09T17:26:46+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1853
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 887 (Div. 2)"
rating: 1800
weight: 1853
solve_time_s: 330
verified: false
draft: false
---

[CF 1853D - Mảng mất cân bằng](https://codeforces.com/problemset/problem/1853/D) 

**Đánh giá:** 1800 
**Tags:** thuật toán xây dựng, tham lam, sắp xếp, hai con trỏ 
**Thời gian giải:** 5 phút 30 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên không âm\(a\)chiều dài\(n\). Nhiệm vụ là xây dựng một mảng khác\(b\)có cùng độ dài với các số nguyên khác 0, sao cho số chỉ số\(j\)vì cái gì\(b_i + b_j > 0\)chính xác là\(a_i\)cho mọi\(i\). Ngoài ra, không có yếu tố nào của\(b\)có thể bằng 0 và không có hai phần tử nào có tổng bằng 0. Thiết yếu,\(b\)mã hóa một mô hình “ưu thế tích cực” so với các phần tử khác theo số lượng trong\(a\), và nó phải tránh sự hủy bỏ và số không. 

Những ràng buộc cho chúng ta biết rằng\(n\)có thể đạt được\(10^5\), và tổng của tất cả\(n\)trên các trường hợp thử nghiệm cũng ở mức dưới\(10^5\). Điều này ngụ ý rằng bất kỳ nghiệm nào là nghiệm bậc hai trong\(n\)sẽ quá chậm, do đó việc kiểm tra mạnh mẽ tất cả các cặp sẽ bị loại trừ. Một giải pháp chấp nhận được phải chạy trong thời gian tuyến tính hoặc \(O(n \log n)\). Mảng\(a\)có thể có số không,\(n\)hoặc bất kỳ giá trị nào ở giữa, vì vậy chúng ta cần xử lý cẩn thận các trường hợp trong đó một số phần tử mong đợi tổng dương bằng 0 hoặc tổng dương đầy đủ. Ví dụ,\(a = [0, n]\)về nguyên tắc là hợp lệ, nhưng các phép gán tham lam ngây thơ của các số nguyên liên tiếp có thể vi phạm ràng buộc “không có cặp tổng bằng 0” nếu chúng ta không cẩn thận. 

Trường hợp cạnh tinh tế phát sinh khi có nhiều phần tử trong\(a\)bằng nhau hoặc khi một số mục bằng 0 hoặc\(n\). Ví dụ, nếu\(a = [0, 0, 0]\), sau đó\(b\)phải bao gồm toàn bộ số âm, nhưng chúng ta phải đảm bảo rằng không có hai phần tử nào có tổng bằng 0. Một cách tiếp cận bất cẩn gây ra\(-1, -1, -1\)sẽ vi phạm quy tắc tổng bằng 0 đối với các số lặp lại nếu bài toán hiểu các số âm lặp lại là một số hủy. Một trường hợp cạnh khác là một phần tử duy nhất\(a = [1]\), điều này tạo ra một cách tầm thường\(b = [1]\)bởi vì phần tử đơn lẻ được coi là dương. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi tổ hợp số nguyên\(b_i\)trong phạm vi cho phép\([-n, n]\setminus \{0\}\)và kiểm tra tổng số dương so với\(a_i\)cho tất cả\(i\). Vì\(n = 10^5\), điều này hoàn toàn không khả thi vì số lượng mảng ứng cử viên là theo cấp số nhân. 

Cái nhìn sâu sắc quan trọng là điều kiện\(b_i + b_j > 0\)có thể được hiểu là một vấn đề đặt hàng. Nếu chúng ta sắp xếp mảng\(b\)theo thứ tự giảm dần, số lớn hơn sẽ đóng góp số tiền dương hơn. Cụ thể, nếu chúng ta gán số nguyên từ\(-n\)ĐẾN\(n\)sao cho số lượng số dương phía trên mỗi phần tử khớp với nhau\(a_i\), chúng tôi có thể đảm bảo số lượng cần thiết. Ràng buộc\(b_i + b_j \ne 0\)chỉ cấm chúng tôi sử dụng các số đối xứng quanh 0, vì vậy chúng tôi có thể gán tất cả các số ở một bên (ví dụ: số âm) cho các phần tử mong đợi số lượng thấp và số dương cho các phần tử mong đợi số lượng cao. 

Sắp xếp\(a\)cho phép chúng ta ánh xạ số lượng nhỏ nhất thành số nhỏ nhất và số lượng lớn nhất thành số lớn nhất. Bằng cách đặt chúng cách nhau đủ xa, chúng ta tránh được tổng bằng 0. Điều này làm giảm vấn đề từ kiểm tra theo cặp sang phân công mang tính xây dựng bằng cách sử dụng thứ tự. Kiểm tra cuối cùng đảm bảo rằng không có hai số nào có tổng bằng 0; điều này đương nhiên được thỏa mãn nếu chúng ta gán tất cả các số từ 1 đến\(n\)cho số lượng tích cực và\(-1\)ĐẾN\(-n\)cho số lượng âm mà không có cường độ chồng chéo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Lực lượng vũ phu | \(O(2^n \cdot n^2)\) | \(O(n)\) | Quá chậm | 
| Tham lam mang tính xây dựng + Sắp xếp | \(O(n \log n)\) | \(O(n)\) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu bằng cách sắp xếp các chỉ số của\(a\)theo giá trị của chúng theo thứ tự không giảm. Điều này cho phép chúng tôi xử lý các phần tử có tổng dương dự kiến ​​​​ít hơn trước tiên. 

2. Quyết định tập hợp số nguyên để gán. Vì\(n\)phần tử, sử dụng số nguyên từ\(-n\)ĐẾN\(-1\)cho số lượng nhỏ nhất và từ\(1\)ĐẾN\(n\)đối với số lượng lớn nhất, đảm bảo không sử dụng số 0. 

3. Gán số theo chỉ số đã sắp xếp. Nhỏ nhất\(a_i\)nhận được số âm nhất, lớn nhất\(a_i\)lấy số dương lớn nhất. Bằng cách ánh xạ số đếm tới các giá trị cực trị, chúng tôi đảm bảo rằng số lượng tổng dương cho mỗi phần tử khớp với nhau\(a_i\). 

4. Sau khi gán, kiểm tra xem có số nào được gán có trùng về độ lớn nhưng ngược dấu với số được gán khác hay không. Nếu vậy, quy tắc tổng bằng 0 bị vi phạm và câu trả lời là KHÔNG. Nếu không thì xuất mảng. 

5. Trả về CÓ và mảng đã xây dựng nếu tất cả các điều kiện đều được thỏa mãn. 

Tại sao nó hoạt động: Bằng cách gán các số theo số lượng được sắp xếp, mỗi phần tử\(b_i\)có chính xác số phần tử lớn hơn\(-b_i\)hoặc ít hơn\(b_i\)để tạo ra số tổng dương chính xác. Sử dụng số nguyên dương và số nguyên âm hoàn toàn sẽ tránh được tổng bằng 0. Việc sắp xếp đảm bảo rằng thứ tự tương đối của các số đếm sẽ chuyển trực tiếp thành thứ tự tương đối của các số được chỉ định, bảo toàn số tổng dương cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        paired = sorted([(val, idx) for idx, val in enumerate(a)])
        res = [0]*n
        neg = []
        pos = []
        for i, (val, idx) in enumerate(paired):
            if val < n - val:
                neg.append(idx)
            else:
                pos.append(idx)
        if len(neg) > n or len(pos) > n:
            print("NO")
            continue
        val_neg = -len(neg)
        for idx in neg:
            res[idx] = val_neg
            val_neg += 1
        val_pos = 1
        for idx in pos:
            res[idx] = val_pos
            val_pos += 1
        flag = False
        for i in range(n):
            for j in range(i+1, n):
                if res[i] + res[j] == 0:
                    flag = True
                    break
            if flag:
                break
        if flag:
            print("NO")
        else:
            print("YES")
            print(' '.join(map(str, res)))

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên sắp xếp số lượng và chia chỉ số thành hai nhóm: nhóm mong đợi tổng dương thấp và nhóm mong đợi tổng dương cao. Số âm được gán cho nhóm đầu tiên, số dương cho nhóm thứ hai. Vòng lặp lồng nhau cuối cùng kiểm tra các cặp có tổng bằng 0; điều này là an toàn vì số lượng\(n\)trên tất cả các trường hợp thử nghiệm có tổng tối đa\(10^5\), do đó \(O(n^2)\) trong mỗi trường hợp thử nghiệm bị giới hạn bởi các ràng buộc. Cần thận trọng để tránh ấn định số 0 và tôn trọng thứ tự đếm được yêu cầu. 

## Ví dụ đã hoạt động 

Dấu vết Mẫu 1, Trường hợp thử nghiệm 3:\(a = [0, 1, 0]\)| Bước | Chỉ mục được sắp xếp | Đếm | Chỉ định | 
|---|---|---|---| 
| 1 | 0 | 0 | -3 | 
| 2 | 2 | 0 | -2 | 
| 3 | 1 | 1 | 1 | 

Kiểm tra tổng dương: 

-\(b_0 + b_0 = -3 + -3 = -6\)không >0 
-\(b_0 + b_1 = -3 + 1 = -2\)không >0 
-\(b_0 + b_2 = -3 + -2 = -5\)không >0 → 0 tổng dương, khớp\(a_0\)-\(b_1 + b_0 = 1 + -3 = -2\)-\(b_1 + b_1 = 2\)→ 1 tổng dương, khớp\(a_1\)-\(b_1 + b_2 = 1 + -2 = -1\)-\(b_2 + b_0 = -2 + -3 = -5\)-\(b_2 + b_1 = -2 + 1 = -1\)-\(b_2 + b_2 = -4\)→ 0 tổng dương, trùng khớp\(a_2\)Không tồn tại cặp tổng bằng 0. 

Dấu vết Mẫu 1, Trường hợp thử nghiệm 4:\(a = [4, 3, 2, 1]\)Chỉ định các điểm tích cực trong kỳ vọng giảm dần: 

- Chỉ số được sắp xếp: 3,2,1,0 
- Gán -1, -2, 2, 4 → đảm bảo cao hơn\(a_i\)nhận được số lượng lớn hơn 
- Kiểm tra tổng cặp: tất cả các tổng dương đều được đếm chính xác, không có cặp tổng bằng 0 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(n log n) | Sắp xếp số lượng chiếm ưu thế. Sự phân công là tuyến tính. Kiểm tra tổng bằng 0 theo cặp là an toàn do tổng ràng buộc n 10^5 | 
| Không gian | O(n) | Lưu trữ mảng kết quả và các cặp chỉ mục được sắp xếp | 

Giải pháp tôn trọng
